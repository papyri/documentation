---
layout: default
title: XSugar Rails Plugin Architecture
---

XSugar Rails Plugin Architecture
================================

This version of XSugar has some code hooks to facilitate including and using its transformation
helpers in a Rails application as a plugin. The core Rails hook is
[`init.rb`](https://github.com/papyri/xsugar/blob/master/init.rb),
which is loaded when Rails searches through its plugin directories.
For IDP SoSOL, we define the XSugar plugin through a Capistrano external in
[`config/externals.yml`](https://github.com/papyri/sosol/blob/master/config/externals.yml),
which loads the whole of the XSugar code into `vendor/plugins/rxsugar` in the
Rails root.

When [`init.rb`](https://github.com/papyri/xsugar/blob/master/init.rb)
is called during the Rails application boot, it does two things:

 * loads [`lib/jruby_helper.rb`](https://github.com/papyri/xsugar/blob/master/lib/jruby_helper.rb)
 * causes `ActiveRecord::Base` to include `RXSugar::JRubyHelper`‘s instance methods

`RXSugar::JRubyHelper` then uses [`self.included`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L32-34)
to extend methods defined in the [`RXSugar::JRubyHelper::ActMethods`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L78-96)
module as class methods on the class it is called from (in this case, `ActiveRecord::Base`).

`RXSugar::JRubyHelper::ActMethods` defines three methods:

 * `acts_as_x` - a generic method which will cause the class it is called from to get as class methods the methods in the class passed to it, and the instance methods in `RXSugar::JRubyHelper::InstanceMethods` (currently empty, but can still be used to gaurd against multiple inclusion)
 * `acts_as_leiden_plus` - calls `acts_as_x` with [`LeidenPlusClassMethods`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L250-260) as the class methods to be added
 * `acts_as_translation` - calls `acts_as_x` with [`TranslationClassMethods`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L262-269) as the class methods to be added

You can see that `LeidenPlusClassMethods` and `TranslationClassMethods` are specializations
of the base [`RXSugar::JRubyHelper::ClassMethods`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L100-248)
module, which define two additional methods:

 * [`transformer_singleton`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L251-253) - used to create an actual transformer singleton from the grammar when the standalone transformation server is not being used, i.e. the Rails app does the transformations itself
 * [`transformer_name`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L255-257)- used to define the transformer name passed to the standalone transformation server if it is being used

These are used by the methods defined in the `ClassMethods` module to do the actual transforms.
As the `ClassMethods` module does not have these methods defined itself, trying to perform a transform
with just the base `ClassMethods` module methods extended onto a class will result in an error.

So, with these defined, any class in the Rails app which inherits from `ActiveRecord::Base`
(i.e. any model class) can call the `acts_as` methods defined to get the transformation methods
defined in `ClassMethods` mixed into it as class methods.

The most important methods for transformation are:

 * [`xml2nonxml`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L160-188)
 * [`nonxml2xml`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L190-218)

Which transform from or to the XML and non-XML representations of content, respectively.
Both take a string as their only parameter and return the transformed string.

These act by [checking for the global constant](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L163-172) `XSUGAR_STANDALONE_ENABLED`, and, if defined and set to `true`, sending the transform
request to the standalone transformation server set by the global constant `XSUGAR_STANDALONE_URL`.
These are intended to be set by the Rails app including the plugin,
in e.g. `config/environments/development_secret.rb`.

If `XSUGAR_STANDALONE_ENABLED` is not defined or set to `false`, the transformation will
[use a transformer singleton to perform the transform itself](https://github.com/papyri/xsugar/blob/v1.0.13/lib/jruby_helper.rb#L172-185).
Transformer singletons are used here because initializing a transformer is extremely resource intensive.
These transformers are instances of the [`RXSugar`](https://github.com/papyri/xsugar/blob/master/lib/rxsugar.rb) class,
created by calling the [`rxsugar_from_grammar`](https://github.com/papyri/xsugar/blob/v1.0.13/lib/rxsugar_helper.rb#L10-13) helper method.
