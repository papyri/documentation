---
layout: default
title: PN/PE Technical Procedures
---

PN/PE Technical Procedures
==============================

PN Processes
------------

1. PN Code Modification
    1. TODO: Need to get from Hugh
2. Peers: We change API and hurt someone
    * Solr API
    * Trismegistos
    * ATOM Feed

PE/SoSOL Processes
------------------

1. SoSOL Code Modification

    See also `doc/README_FOR_APP`.

    1. Set up a local repository for SoSOL to make modifications in using the Create Local Git Repository for XSugar/SoSOL/IDP data process. 
    2. Use your favorite text editor/IDE to make changes to file(s) in your local directory (yourdirectory/protosite)
    3. Test your changes by running the application on your local machine
    4. Are you happy/done with your changes?
        * Yes – continue to next step
        * No – go back to step 2
    5. Run dependency/automated tests by performing the  `jruby rake` command from a terminal window from your local directory (yourdirectory/protosite) 
    6. Were there any failures? 
        * Yes – return to step 2 and make changes needed
        * No – continue to next step
    7. Should be ready for the next execution of the Release Process.

2. GUI Help Menu Change
    1. Files in the SoSOL project that contain code used specifically to create or link from the Text Help Menu are:
        * Controllers - helper_controller.rb, leiden_controller.rb
        * Model - leiden.rb
        * Views – under ddb_identifiers directory = edit.haml, _leiden_helpers.haml
        * Views – under helper directory = all (except insertFootnote.haml, insertLinkBiblio.haml, insertLinkPN.haml, insertlink.haml ) = the different pop up windows
        * Javascripts - helper.js, leiden.js, confirm.js, menu-for-applications.js
        * Stylesheets – helper.css, menu-bar.css, menu-item.css 
    2. Files in the SoSOL project that contain code used specifically to create or link from the Translation Help Menu are:
        * Controllers – translation_helper_controller.rb, translation_leiden_controller.rb
        * Model - translation_leiden.rb
        * Views – under hgv_trans_identifiers directory = edit.haml, _translation_leiden_helpers.haml
        * Views – under translation_helper directory = all = the different pop up windows
        * Javascripts - translation_helper.js, translation_leiden.js, confirm.js, menu-for-applications.js
        * Stylesheets – helper.css, menu-bar.css, menu-item.css 
    3. Files in the SoSOL project that contain code used specifically to create or link from the Commentary Help Menu are:
        * Controllers – none
        * Model - none
        * Views – under ddb_identifiers directory = commentary.haml
        * Views – under helper directory = insertFootnote.haml, insertLinkBiblio.haml, insertLinkPN.haml, insertlink.haml = 4 pop up windows
        * Javascripts – under ddb_identifiers directory =  commentary.haml - contains javascript besides the Rails view code - functions generateFrontmatterCommentaryForm and generateCommentaryForm dynamically build the menu when needed
        * Javascripts - commentary.js, confirm.js, menu-for-applications.js
        * Stylesheets – helper.css, menu-bar.css, menu-item.css 
    4. Any changes made to the menu-for-applications.js file that was originally downloaded from http://www.dhtmlgoodies.com DHTML Suite/menu bar is marked with `jfox` and commented as to why
    5. Follow the SoSOL Code Modification process to change the above files and release them to the appropriate environments.

3. UI and CSS
   
    Navigator (PN) and Editor(PE) use similar but different css. master.css is used by both. master_additions.css contains additions and alterations to master.css for the PN side.
   
    PE uses many additional stylesheets for various pages.

    Header/Footer views depend on user being logged in. Since a user logs into PE,  PE is largely responsible for creating views that depend on the user being logged in. PE creates header/footer parts, or the complete header and footer. These are just the HTML (without CSS links) and can be accessed by:

     * siteurl/cross_site/headerfor the complete header
     * siteurl/cross_site/footerfor the complete footer
     * siteurl/cross_site/sign_in_outfor just the sign in out links
     * siteurl/cross_site/advanced_create for just the advanced create link

4. Leiden+ Change (text/translation)
    1. Open a terminal window and traverse to the `xsugar` directory (ex. `cd ~/gitsugar/xsugar`) where you have placed the XSugar Grammar file (see Create Local Git Repository for XSugar/SoSOL/IDP data for setting up a local repository that will sync with the Halsted and/or GitHub repositories).
    2. This process will be based on doing changes in the `master` branch.  As you get familiar with Git, there are other processes you may choose to use to create an `alternate` branch to make your changes in and then merge the `alternate` back into the `master`.  Example – `git checkout –b whatever` will create and checkout a new branch named `whatever` using the branch you are currently in (let`s say `master`) as a starting point.  You can make modifications in the `whatever` branch and test them.  Then `git checkout master` to return to the `master` branch.  A `git merge whatever` will then merge the changes from the `whatever` branch into the `master` branch`.
    3. Do a `git branch` command to see what branch you are in (`*` will be shown by active branch).  If you are not in the `master`, do a `git checkout master` to traverse there.
    4. Do a `git pull` command to ensure your local repository is up to date with the Halsted repository changes
    5. Do a `git fetch ghs` command (where `ghs` is the remote name) to retrieve changes from the GitHub repository
    6. Check for commits on GitHub that you do not have on your local branch (git rev-list --pretty=oneline ghs/master ^origin/master where `ghs` is the remote name)
    7. If any commits are listed, you will want to merge them into your local branch (`git merge ghs/master` where `ghs` is the remote name) so your local repository is in sync.
    8. Open up the `xsugar/epidoc.xsg` file in your favorite text editor and make the required change to the XSugar Grammar – add, delete, change
    9. Test the changes on your local machine
        * create a new file `testquick_grammar.rb` in the `xugar/test` directory where you have the XSugar Grammar
        * use `test_grammar.rb` as a model and create assert_equal_fragment_transform statements to test the new/changed Leiden+ in the new `testquick_grammar.rb` file
        * update the `xsugar/rakefile` file to point to the new `quick` test file – change `FileList["test/test_*.rb"]` to `FileList["test/testquick_*.rb"]` (approximately line 9)
        * in a terminal window at the `xsugar` directory use command line to run a `quick` test using just the assert statements for the new/changed Leiden+ with the new `quick` test file (ex. `jruby -S rake >> rakquick0311_v01.txt` executes a test with the results sent to file `rakquick0311_v01.txt` in the `xsugar` directory – leave off the `rake >> rakquick0311_v01.txt` and the results will shown in your terminal window)
        * view the results – if no errors, continue to the next step, otherwise make corrections and return to the previous step to run the test again until errors are resolved
        * update the regression testing file `test_grammar.rb` file with the assert_equal_fragment_transform statements added/changed in the `testquick_grammar.rb` file 
        * update the `rakefile` file in the `xsugar` directory to point to back to the regression testing file – change `FileList["test/testquick_*.rb"]` to `FileList["test/test_*.rb"]` (approximately line 9)
        * in a terminal window at the `xsugar` directory use command line to run a regression test (ex. `jruby -S rake >> rakregress0311_v01.txt` executes a test with the results sent to file `rakregress0311_v01.txt` in the `xsugar` directory – leave off the `rake >> rakegress0311_v01.txt` and the results will shown in your terminal window)
        * view the results – if no errors, continue to the next step, otherwise make corrections and return to the previous step to run the test again or possibly to the `quick` test step if you need to start over with new Leiden+
        * You may review website http://www.brics.dk/xsugar/examples.html to see examples of how to test in native java if you prefer – but the method above is based on files/processes used in the SoSOL application.
    10. Commit the changes to the grammar and regression test case files (ex. `git commit -a -m "commit message here" ` command will commit all non-committed changes in the directory with the message you enter)
    11. Push the changes out to the Halsted and GitHub repositories by doing the `git push public` command where `public` is a remote pointing to URL`s for both Halsted and GitHub.
    12. Does this affect the Leiden+ Helpers? – Update using the GUI Help Menu Change process if it does
    13. Test the changes by running the SoSOL application on your local machine and executing a `cap local externals:setup` command before starting the local server to ensure you have the current version of the `epidoc.xsg` file.
    14. Verify the changes are viewed as expected using XSLT Stylesheet by using the `preview` tab for the corresponding identifier (text or translation) in the SoSOL application.
    15. Leiden+ changes will be available at the next code release to the production environment when the `config/externals.yml` file on the `release_staging` branch is updated to point to the new XSugar Grammar commit containing your changes.

5. Provide users ability to download a publication 
   
    Added 'Download Copy' button to 'Overview' page and as 'Admin' function.

6. Update `collection.rdf` during rename
   
    Added new rename feature to SoSOL. On rename, if string is unknown to collection.rdf returns 'no-go' and invite entry of new human readable series (O.Abu Mina) and ddb title (e.g. o.abu.mina;;1). Uses entry to populate collection.rdf.

7. Batch rename series

    When papyri.info already has records for a volume such as O.Stras. where the ddb-hybrid idno is e.g. o.stras;;23, the construction of the idno indicates that there is no volume number (subsequent volumes not expected). When, however, volume two of O.Stras. does appear (as it did), then within the system all o.stras;;#### have to be changed to o.stras;1;####. XSLT to do this transformation has to be created case by case. Thereafter commit to github. Then rerun of Numbers Server necessary. 

Validation/Display Processes
----------------------------
