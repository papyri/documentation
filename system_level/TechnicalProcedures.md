---
layout: default
title: PN/PE Technical Procedures
---

PN/PE Technical Procedures
==============================

PN Processes
------------

1. **PN Code Modification**
    1. TODO: Need to get from Hugh
2. **Peers: We change API and hurt someone**
    * Solr API
    * Trismegistos
    * ATOM Feed

PE/SoSOL Processes
------------------

1. **SoSOL Code Modification**

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

2. **GUI Help Menu Change**

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

3. **UI and CSS**
   
    Navigator (PN) and Editor(PE) use similar but different css. master.css is used by both. master_additions.css contains additions and alterations to master.css for the PN side.
   
    PE uses many additional stylesheets for various pages.

    Header/Footer views depend on user being logged in. Since a user logs into PE,  PE is largely responsible for creating views that depend on the user being logged in. PE creates header/footer parts, or the complete header and footer. These are just the HTML (without CSS links) and can be accessed by:

     * `siteurl/cross_site/header` for the complete header
     * `siteurl/cross_site/footer` for the complete footer
     * `siteurl/cross_site/sign_in_out` for just the sign in out links
     * `siteurl/cross_site/advanced_create` for just the advanced create link

4. **Leiden+ Change (text/translation)**
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

5. **Provide users ability to download a publication**
   
    Added 'Download Copy' button to 'Overview' page and as 'Admin' function.

6. **Update `collection.rdf` during rename**
   
    Added new rename feature to SoSOL. On rename, if string is unknown to collection.rdf returns 'no-go' and invite entry of new human readable series (O.Abu Mina) and ddb title (e.g. o.abu.mina;;1). Uses entry to populate collection.rdf.

7. **Batch rename series**

    When papyri.info already has records for a volume such as O.Stras. where the ddb-hybrid idno is e.g. o.stras;;23, the construction of the idno indicates that there is no volume number (subsequent volumes not expected). When, however, volume two of O.Stras. does appear (as it did), then within the system all o.stras;;#### have to be changed to o.stras;1;####. XSLT to do this transformation has to be created case by case. Thereafter commit to github. Then rerun of Numbers Server necessary. 

Validation/Display Processes
----------------------------

1. **EpiDoc RNG Schema Change**

    TODO: Need to link to EpiDoc community documentation on doing schema changes and publications.

    1. Discuss and coordinate proposed changes with the wider EpiDoc community via the Markup list.
    2. Coordinate with any other EpiDoc projects that are actively modifying the schema and then update EpiDoc RNG schema – located at http://epidoc.svn.sourceforge.net/viewvc/epidoc/trunk/schema/tei-epidoc.rng - to implement the desired changes. Methods for making these changes are provided in the README.txt file in the same SVN folder. The result will become the latest development version, which you should upload to http://www.stoa.org/epidoc/schema/dev/tei-epidoc.rng. A concurrent notice must be sent to the Markup list outlining the changes and reporting the update to the public dev version of the schema.
    3. Once stable/tested, copy changes made to EpiDoc RNG to primary public server – located at http://www.stoa.org/epidoc/schema/latest/tei-epidoc.rng - current authorized editors are Gabriel Bodard, Hugh Cayless, Tom Elliott or Noel Fiser - also related to #713
    4. Update EpiDoc Guidelines (coordinate with wider EpiDoc community) – located at http://www.stoa.org/epidoc/gl/dev/toc.html - current authorized editors are Gabriel Bodard, Tom Elliott, Ryan Baumann – are these right?
    5. Alert Gabriel Bodard that EpiDoc Cheatsheet may need to be revised. It is located at http://epidoc.svn.sourceforge.net/viewvc/epidoc/trunk/guidelines/msword/cheatsheet.doc - current 
    6. Does this affect the PN/SoSOL XSLT Display, indexing, or mapping stylesheets?  If so coordinate and carry out changes to the example EpiDoc stylesheets and propagate them through the PN/SoSOL system. See the PN/SoSOL XSLT Display Stylesheet Modification process.
    7. Does this likely affect the Leiden+ Syntax/Grammar? Coordinate and execute any needed updates in collaboration with the Editorial board(s) and development team: Leiden+ Change (text/translation).
    8. Does this affect the Leiden+ Syntax Documentation? If so, coordinate with Editorial board so that they update the documentation using the Leiden+/Translation Leiden+ Documentation Change process if it does
    9. Does this affect the Leiden+ Helpers? If so, update the Helpers using the GUI Help Menu Change process if it does
    10. Does this affect the "new document" templates in SoSOL? If so, update. Ryan see comment.
    11. Notify Heidelberg of the change so that they can check and, if necessary, carry out the Modify Reverse Crosswalker/Crosswalker process if needed.
    12. Notify the Editorial board so that they can publish appropriate (their judgment) change notice(s) to active users 
    13. Do any mass updates needed to the Git Repository XML using the Global XML Updates process.  
    14. Make changes to the SoSOL preprocess XSLT to apply these same 'mass updates' to 'user' files using the XML Updates via SoSOL XSLT process.
    15. Does this likely affect core 'papyrological conventions'? If so, alert editorial board to update.

2. **PN/SoSOL XSLT Display Stylesheet Modification**
    1. Update the portion(s) of the EpiDoc XSLT Display Stylesheet to modify the way the EpiDoc XML is displayed within PN/SoSOL.  This is located at https://epidoc.svn.sourceforge.net/svnroot/epidoc/trunk/example-p5-xslt/ - current authorized editors are Gabriel Bodard, Hugh Cayless, Tom Elliott or Noel Fiser. 
    2. Create a new tag for the modification made at https://epidoc.svn.sourceforge.net/svnroot/epidoc/tags/ 
    3. Let the PN and SoSOL development teams know what the new tag to include in the next production release.
    4. SoSOL team switches to tag, diffing start-edition.xsl against the previous tag to see if any includes from the calling templates at https://github.com/papyri/sosol/blob/master/data/xslt/pn/start-div-portlet.xsl and https://github.com/papyri/sosol/blob/master/data/xslt/pn/start-divtrans-portlet.xsl need to be changed.
    5. Merge changes into pn-xslt - notify Hugh/Tim that this needs to be done.

Data Processes
--------------

1. **Data Modification**

    **XML Updates via SoSOL XSLT**

    1. Changes applied via the SoSOL application before saving a text or translation XML file.
    2. These changes are applied to the XML file via a XSLT stylesheet maintained as part of the SoSOL application.  These stylesheets can be found in the SoSOL code directory at: 
        * text = data/xslt/ddb/preprocess.xsl 
        * translation = data/xslt/translation/preprocess.xsl
    3. Any changes needed may be added to the file(s) above and released to the production environment using the normal Release Process.
    4. Many changes found here are first applied via the Global XML Updates process and then added here to catch any 'user' files that were not in the canonical repository when the 'global' change was made.
    5. In-process texts will get this XSLT applied automatically whenever the set_xml_content method is called, which for translations and texts is what calls the pre-processing. Since part of automatically updating the revisionDesc during finalization now does this, this should happen transparently for finalized texts.
    6. *What's the regime for sun-setting such patches so they're not left hanging around in the save step?*
    7. *There was a proposed mechanism for notifying users of such modifications, but where this should go is ambiguous. At one point the Digital Papyrology blog and/or some sort of "news" on the papyri.info home page was proposed. Josh: still desired?*

    **Global XML Updates**

    1. Global change applied to all canonical XML files that meet the criteria – must have a comment added into the change history in the XML file (`<change>` tag inside `<revisionDesc>` tag)
    2. Usually applied using an XLST style sheet ran against the canonical repository XML files.
    3. If you have not done it before, use the Create Local Git Repository for XSugar/SoSOL/IDP data process to create a local copy of the `idp.data` repository.
    4. Make sure you are on the `master` branch and it is up to date (`git checkout master` and then `git pull`).
    5. Create a new branch to apply your changes to from the `master` branch (`git checkout –b makechanges`.
    6. Apply the XSLT to XML files you need to update in the appropriate working directory
        * Text files = DDB_EpiDoc_XML directory
        * Translation files = HGV_trans_EpiDoc directory
        * HGV = HGV_meta_EpiDoc directory
    7. Validate the XSLT performed the changes you wanted and no other extraneous ones occurred.
    8. Return to the `master` branch and merge the branch with the changes into the `master` branch (`git checkout master` and `git merge makechanges` based on example above).
    9. Use the Re-publish PN/PE Data process to make the changes visible via PN and newly checked out PE files.
    10. Follow the XML Updates via SoSOL XSLT process to ensure the changes are applied to 'user' files that were not in the canonical repository when this 'global' change was made. 

2. **Modify Reverse Crosswalker/Crosswalker**

    **Modify Crosswalk / What has to happen after the Crosswalker has been called to action?**

    * Commit recently crosswalked data to master branch of idp.data repository
    * Synchronise all existing mirrors of idp.data repository
    * Call number server to index the new stuff → Hugh

    The crosswalker applications are tools that need to be modified if an interface on either end (input or output) has changed....

3. **Detect and Resolve Git Update Conflicts**

    The automated sync process will fail in the case of a merge conflict, and sometimes merge conflicts occur during manual sync. These are flagged in the files, and the merge conflict must be resolved manually and committed.

4. **Add Data to Numbers Server**

    This assumes GitHub and canonical data have been merged.

    1. `cd /data/papyri.info/git/navigator/pn-mapping`
    2. `lein run map-all`

5. **Re-index PN Canonical Data**

    1. changed sic/corr to orig/reg caused search to not work correctly. Hugh needed to change something and do a re-index. `^καγω^` 11 hits and `"και εγω"` 301 hits
    2. `cd /data/papyri.info/git/navigator/pn-indexer`
    3. `lein run`

6. **Re-publish PN/PE Data**

    TODO: Details – believe this entails copying/cloning/merging/??? data from production SoSOL canonical to PN canonical, updating numbers server, and re-indexing for searching. (????)

    1. Take SoSOL offline so no new changes can be made
    2. cd /data/papyri.info/idp.data
    3. sudo su tomcat
    4. git pull canonical master
    5. git pull github master
    6. There may be merge conflicts, if so, they need to be resolved and committed
    7. git push canonical (may be slow if it triggers a garbage collection)
    8. git push github
    9. cd /data/papyri.info/sosol/repo
    10. run git --git-dir=canonical.git gc --no-prune
    11. cd boards
    12. find . -name "*.git" -exec git --git-dir="{}" gc --no-prune \;
    13. cd ../users
    14. find . -name "*.git" -exec git --git-dir="{}" gc --no-prune \; (this last may take a very long time—sometimes I just do Josh, James, and other board members)
    15. Update numbers server with new data just merged ( follow Add Data to Numbers Server )
    16. Re-index PN canonical data so new data will be searchable, etc.(follow Re-index PN Canonical Data)
    17. Bring SoSOL back online

7. **SoSOL Broken Publication**

    1. Withdraw button available on board member's publication overview removes **all copies** of the submitted publication **for all boards**, including finalizing copies. This does not affect any commits to canonical that have already been finalized. This, in effect, clears out any inconsistent states that may arise in a publication and returns it to being unsubmitted.
    2. For publications this doesn't fix, see SoSOL maintenance.

Release Processes
-----------------

1. **Release Staging/Tagging**

    SoSOL normally uses a release_staging branch for tagged releases. This branch allows you to keep a branch which has the development branch(es) merged onto it, then external dependencies can be frozen on the release staging branch. Individual fixes from development branches can also then be cherry-picked during the release testing process. Don't merge release_staging back onto your development branch, unless you also want externals frozen there.

    1. Check out the release_staging branch, merge your development branch with e.g. `git merge integration`.
    2. Freeze external dependencies by editing config/externals.yml. You can then update them on your local checkout with `cap local externals:setup`. Commit this change to the release_staging branch.
    3. Use `git log -1` to get the commit SHA of the most recent revision on the release_staging branch. Use this to tag a release e.g.: git tag -m 'SoSOL v1.0.17-rc4' v1.0.17-rc4 5956ce86b39f5f73fc1f209e5575a72631ab4032
    4. If incrementing version number instead of release candidate number (rc), tag last rc of previous version to be the final release for that version, e.g. `git show v1.0.17-rc4` to find the commit SHA, then: git tag -m 'SoSOL v1.0.17' v1.0.17 5956ce86b39f5f73fc1f209e5575a72631ab4032
    5. Push the commit with `git push`, then the tag with `git push --tags`.

2. **Release Process**

    1. Make code, environment file, XSLT, stylesheet, etc. changes
    2. Release to Development (go to https://github.com/papyri/sosol/tree/release_staging and look at the "switch tags" dropdown to see the latest tag) (NYU http://dev.papyri.info)(follow Development Release Process – will include any environmental changes unique to the Development environment).
    3. Papyrologists (and team) test the new functionality and regression test also – approximately 3 days
    4. Changes OK?
        * Yes – continue to next step
        * No – return to step 1 (fix whatever the problem is and start release from the beginning)
    5. Release to Production (NYU http://papyri.info) (follow Production Release Process – will include any environmental changes unique to the Production environment)
    6. Papyrologists (and team) test the new functionality and regression test also
    7. Changes OK?
        * Yes – continue to next step
        * No – return to step 1 (fix whatever the problem is and start release from the beginning)
    8. Publish release/change notifications to ????? papylist? Blogs?
   
3. **Development Release Process**

    1. ssh to dl-papyri, get a mysql dump of the idp database, copy it to dev-dl-papyri
    2. ssh to dev-dl-papyri
    3. /etc/init.d/dlib_tomcat_pi stop-tc (stop sosol)
    4. /etc/init.d/dlib_tomcat_pi stop-xs (stop xsugar)
    5. reload the database
    6. cd /data/papyri.info/sosol
    7. remove the repo/ dir and rsync a copy from prodo
    8. chown repo to tomcat
    9. Build SoSOL
    10. cd /data/papyri.info/idp.data
    11. sudo su tomcat
    12. git pull github master (this assumes github is up-to-date)
    13. cd /data/papyri.info/git/navigator
    14. git fetch
    15. git merge <release tag>
    16. cd pn-mapping
    17. lein run map-all
    18. If there are solr schema changes, we might need to restart solr: /etc/init.d/dlib_tomcat_pi stop-solr / start-solr
    19. cd ../pn-indexer
    20. lein run
    21. cd ../pn-dispatcher
    22. mvn clean package
    23. sudo mv target/dispatch.war /usr/local/tomcat-solr/webapps/
    24. start sosol and xsugar

4. **Build SoSOL**

    1. cd editor/
    2. *not sure - but may need something added somewhere in here to  Pull In Metadata Changes - used to be done during the "test" release but that is going away.  So will need to pull in here or maybe work from the same branch - depends on how as to whether some "merge" needs to take place in this process I think.*
    3. git fetch
    4. git merge <tag name>
    5. cap local externals:setup
    6. chown tomcat the xsugar standalone directory
    7. (possibly rake db:migrate if there are databases changes)
    8. jruby -S warble war
    9. sudo rm /usr/local/tomcat/webapps/editor*
    10. sudo mv editor.war /usr/local/tomcat/webapps/

5. **Production Release Process**

    (This is a typical release, sometimes there are variations. Hugh likes to write up a release plan.)
   
    1. put up maintenance page (edit pi.conf file and restart apache)
    2. stop sosol and xsugar
    3. follow Re-Publish PN/PE Data
    4. cd /data/papyri.info/sosol/editor
    5. git fetch
    6. git merge <release tag>
    7. Build SoSOL
    8. cd /data/papyri.info/git/navigator
    9. git fetch
    10. git merge <release tag>
    11. Re-Publish PN/PE Data
    12. cd pn-dispatcher
    13. mvn clean package
    14. sudo mv target/dispatch.war /usr/local/tomcat-solr/webapps/
    15. start xsugar and sosol

6. **Pull In Metadata Changes**

    1. may need tweaked to be part of "development" release or totally removed if decide to work from same branch
    2. You now need to merge updates from the "master" branch with updates from "metadata" branch (maintained via the HGV support team).  There is a "metadata_integration" branch on the Halsted repository for this purpose.
    3. git checkout metadata_integration - to prepare for integrating the 2 branches and testing.  Doing `grb track metadata_integration` will create the tracking branch for you if you had not previously set it up.
    4. git pull – to make sure the branch is up to date
    5. git merge ghs/metadata – to merge in changes from the GitHub "metadata" branch.  Resolve any merge conflicts that occur.  (You may have a local "metadata" branch and may merge from there if you have it up to date and prefer that method).
    6. git merge master – to merge in changes from "master" branch.  Resolve any merge conflicts that occur.
    7. Stop the servers and do jruby –S rake – this will run all the automated regression tests.  Fix any errors that may occur.
    8. Start up the servers to run the SoSOL application and do a cursory test to make sure it runs OK and changes you expect to appear are there, etc.
    9. Once you are satisfied with the changes and the way the application is running, the updated "metadata_integration" branch need to be "pushed" out.  There are options here:
        1. git push public – will attempt to push all local branches with matching names in the "public" repositories
        2. git push public metadata_integration – will attempt to push only the "metadata_integration" local branch to the "public" repositories (Halsted and GitHub)
    10. git checkout master - to prepare for merging in the "metadata_integration" branch 
    11. git merge metadata_integration – to merge in changes from the "metadata_integeration" branch.  Resolve any merge conflicts that occur.  This in effect, merges in any changes from the "metadata" branch into the "master" branch.
    12. The updated "master" branch needs to be "pushed" out.  There are options here:
         1. git push public – will attempt to push all local branches with matching names in the "public" repositories.
         2. git push public master – will attempt to push only the "master" local branch to the "public" repositories (Halsted and GitHub).

Miscellaneous Processes
-----------------------

1. **Git Repository Maintenance**
    1. Run `git fetch --all` on canonical repository. This should fetch all objects from remote repositories.
    2. Run git repack on canonical repository – command is `git repack -a -l`. See `git help repack`.
    3. Garbage collection on repositories – command is `git gc --no-prune`. See `git help gc`.
    4. The `git fsck` command can be used to verify repository integrity. This also outputs a number of warnings due to zero-padded modes, which can be ignored.
    5. When merging a branch like textchange or xwalk for testing use "git merge -s recursive -Xtheirs remote/branchname" - makes things easier (should always pick their side of the merge)

2. **SoSOL Maintenance**
    
    Sometimes you may need to perform different kinds of maintenance on a production instance of SoSOL - changing or fixing things that there`s no UI for, for example. This can often be done through a using a production console, or interacting with the production MySQL database and data repositories directly.

    **Production Console**

    1. Switch to the directory the production SoSOL instance is running from (or where the production .war was built from). For NYU, this is done with `cd /data/papyri.info/sosol/editor/`.
    2. Run `jruby script/console production`
    3. You now have an interactive Ruby shell (a la `irb`) with access to the full SoSOL production environment - including data models and database access. Be cautious, particularly as the user you're logged in as may not have write permission to the production Git repositories.

    **MySQL Database**

    MySQL credentials and database info can be found under the "production" section of config/database.yml for a production SoSOL instance. Use these credentials to connect with your favorite MySQL client - you may need to use SSH tunneling as well.

    **Data Repositories**

    SoSOL uses a number of Git repositories to store data. You may need to look in these to check branches, commits, repository integrity, etc. The root directory for this is set by the REPOSITORY_ROOT constant in SoSOL (usually in config/environment.rb), defaulting to db/git. For NYU production, this is /data/papyri.info/sosol/repo. This will then have under it canonical.git (the canonical repository, and normally the only full one - all the other repositories are bare clones of this repository which refer to it using object/info/alternates), as well as users, boards, and communities. You can switch to an individual repository to list branches or show objects. Keep in mind these repositories are bare and have no checkout, so some operations may be difficult to perform. If you must, you may be able to clone the repository somewhere else to obtain a checkout and do work then push the changes back, but it is recommended that you take the production instance of SoSOL offline for such work.

2. **PN Maintenance**

    See https://github.com/papyri/navigator (README.md).

3. **NYU Preservation**

    * weekly snapshots
    * natural evolution of the data (snapshots activated by the passing of certain defined change thresholds)

4. **DDbDP/HGV SoSOL Records Relationship Management**

    TODO: How to verify data points between records stay in sync – not sure what this is – need help. Papies describe idnos, HC describe how Numbers Server aggregates.

5. **GitHub Account/Access**

    1. Open a browser and go to the GitHub website (https://github.com/).
    2. Create an account on GitHub.
    3. Ask one of the "papyri.info" project GitHub administrators (currently Ryan Baumann or Hugh Cayless) to add your GitHub ID to the papyri organization/team on GitHub.
    4. Set up `ssh` access to the GitHub repositories by following either http://help.github.com/mac-set-up-git/, http://help.github.com/linux-set-up-git/, or http://help.github.com/win-set-up-git/ depending on your operating system.

6. **Create Local Git Repository for XSugar/SoSOL/IDP Data**

    1. Load Git on your machine.  See http://git-scm.com/ for the latest.
    2. Create a directory where you would like to store your local copy of the Git repository (ex. `mkdir sosol` in terminal window or use your favorite file managing GUI interface)
    3. Open a terminal window and go to the directory you created for this Git Repository (ex. `cd sosol`
    4. The following steps are documented in a way to set up the Halsted repository as the default (origin) remote repository and adding the GitHub repository as an alternative remote ("ghs" in the examples).  You may set up to the GitHub repository as the default (origin) remote if that is your desire by "cloning" from there instead.  The Halsted repository for "idp.data" is set up to automatically pull updates from the GitHub repository.
    5. Use Git command line to clone the repository you want. (ex. `git clone ssh://halsted.vis.uky.edu/srv/git/protosite.git` will clone the SoSOL code repository from the Halsted server).  Substituting "xsugar" for "protosite" will get the xsugar repository and "idp.data" for "protosite" will get the IDP data repository.  This will create a default "master" branch pointing to the default "origin/master" remote branch on Halsted.  This process will also create another directory level in the directory you issued the command in (ex. "sosol/protosite").  This sets up the following default remote and branch in your project directory ".git/config" file for syncing your local and remote repositories.

             [remote "origin"] 
               fetch = +refs/heads/*:refs/remotes/origin/*
               url = ssh://jfox@halsted.vis.uky.edu/srv/git/protosite.git

             [branch "master"]
               remote = origin
               merge = refs/heads/master

    6. Go to the new directory created based on which repository you cloned – xsugar, prototsite, or idp.data (ex. `cd protosite`).
    7. git config --global user.name "Your Name Comes Here" - to set up your name as it will be seen when you do Git commands that modify the repository
    8. git config --global user.email "you@yourdomain.com" - to set up your email as it will be seen when you do Git commands that modify the repository
    9. If using Windows - git config --global core.autocrlf false – to keep Git from automatically converting line endings to CRLF in Windows.  But you will need to change your text editor to use LF line endings to match Linux/Mac.  This keeps diffs from showing every line changing just because of line endings and makes file sharing across multiple platforms a little nicer.
    10. git config --global color.ui true - Git colors its output if the output goes to a terminal.  This is optional but will make output from some Git line commands more readable.
    11. git config -l – to confirm your configuration settings
    12. If you need to push changes to the GitHub repository, follow the GitHub Account/Access process to get write access to the GitHub repository and then execute this command – `git remote add ghs ssh://git@github.com/papyri/sosol.git` ("ghs" in this command can be anything you want it to be – it will be the name of the remote used in Git commands) - this adds a remote of the GitHub repository so you may "fetch" updates from this alternate public remote repository.  This sets up the following remote in your project directory ".git/config" file for retrieving updates from this remote repository (or other Git actions).
   
              [remote "ghs"]
                url = ssh://git@github.com/papyri/sosol.git
                fetch = +refs/heads/*:refs/remotes/ghs/*

    13. git remote – to confirm your remote is set up correctly (ex. should see "ghs" and "origin" where "ghs" is the remote name used above).

              jfox@jfox-laptop:~/ghs/protosite$ git remote
              ghs
              origin
   
    14. git fetch ghs  (where "ghs" is the remote name used above) – to fetch the GitHub repository data to your local machine.
    15. git remote show origin and git remote show ghs (where "ghs" is the remote name used above) – to see how Git views the status of both remote repositories via your local machine.  This command shows the HEAD branch, remote branches, local branches configured for "git pull", and local refs configured for "git push", and the "up to date" status of the local branches.  A "fast-forwardable" status means the local branch has changes that can and will be pushed to the remote repository next time a "git push" is performed on a branch and/or remote that points to that repository.  A "local out of date" status means the remote branch contains updates that the local branch does not have and a "fetch" of the remote repository needs to be performed ("git fetch ghs"  where "ghs" is the remote name) and then a merge of the "fetched" remote branch into your local branch ("git checkout metadata" and then "git merge ghs/metadata" would merge the "ghs" remote "metadata" branch into the local "metadata" branch).  An "up to date" status means your local branch matches the remote branch.
         Before the merge:
         After the merge:
    16. You can "push" updates to both the "origin" and "ghs" remote repositories with 1 command from any local branch with a matching name (ex. master, integration, etc.) by setting up a "public" remote (or whatever name you want to give it) in your project directory ".git/config" file.  Open the ".git/config" file in your favorite text editor and copy a current remote and change it to contain all the URL"s you want to be able to push to at the same time. (see picture below for ex. of Halsted and GitHub).  Now if you "push" to the "public" remote (ex. "git checkout integration" and then "git push public"), all your local branch updates will be pushed to the defined "public" remote repositories with a matching branch name.  If you want to push just the branch you are on (checked out) to a specific remote, you may do that by specifying the remote and branch in the command.  Example – "git push ghs integration" will push the "integration" branch to the "ghs" remote.
    17. This gives you the basic set up for setting up and maintaining data/code in a Git repository.  As you learn more about Git, you may discover other ways of doing some of the same things.

7. **Key Interfaces**

    * NYU Preservation Team
    * XSLT stylesheets
    * RNG Schema
    * Leiden+
    * XSugar Grammar
    * EpiDoc Standards
    * Documentation
        * On-Line Leiden+
        * On-Line Translation
        * On-Line Video
        * EpiDoc
        * System – PowerPoint and code
    * HGV
    * Search Interface
    * Peer-projects
    * User Notifications
    * Automated Testing
    * Regression Testing
    * Archived Snapshots
    * Repositories
