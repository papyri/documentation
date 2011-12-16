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

1. EpiDoc RNG Schema Change

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
    10. Does this affect the “new document” templates in SoSOL? If so, update. Ryan see comment.
    11. Notify Heidelberg of the change so that they can check and, if necessary, carry out the Modify Reverse Crosswalker/Crosswalker process if needed.
    12. Notify the Editorial board so that they can publish appropriate (their judgment) change notice(s) to active users 
    13. Do any mass updates needed to the Git Repository XML using the Global XML Updates process.  
    14. Make changes to the SoSOL preprocess XSLT to apply these same ‘mass updates’ to ‘user’ files using the XML Updates via SoSOL XSLT process.
    15. Does this likely affect core ‘papyrological conventions’? If so, alert editorial board to update.

2. PN/SoSOL XSLT Display Stylesheet Modification
    1. Update the portion(s) of the EpiDoc XSLT Display Stylesheet to modify the way the EpiDoc XML is displayed within PN/SoSOL.  This is located at https://epidoc.svn.sourceforge.net/svnroot/epidoc/trunk/example-p5-xslt/ - current authorized editors are Gabriel Bodard, Hugh Cayless, Tom Elliott or Noel Fiser. 
    2. Create a new tag for the modification made at https://epidoc.svn.sourceforge.net/svnroot/epidoc/tags/ 
    3. Let the PN and SoSOL development teams know what the new tag to include in the next production release.
    4. SoSOL team switches to tag, diffing start-edition.xsl against the previous tag to see if any includes from the calling templates at https://github.com/papyri/sosol/blob/master/data/xslt/pn/start-div-portlet.xsl and https://github.com/papyri/sosol/blob/master/data/xslt/pn/start-divtrans-portlet.xsl need to be changed.
    5. Merge changes into pn-xslt - notify Hugh/Tim that this needs to be done.

Data Processes
--------------

1. Data Modification

    **XML Updates via SoSOL XSLT**

    1. Changes applied via the SoSOL application before saving a text or translation XML file.
    2. These changes are applied to the XML file via a XSLT stylesheet maintained as part of the SoSOL application.  These stylesheets can be found in the SoSOL code directory at: 
        * text = data/xslt/ddb/preprocess.xsl 
        * translation = data/xslt/translation/preprocess.xsl
    3. Any changes needed may be added to the file(s) above and released to the production environment using the normal Release Process.
    4. Many changes found here are first applied via the Global XML Updates process and then added here to catch any ‘user’ files that were not in the canonical repository when the ‘global’ change was made.
    5. In-process texts will get this XSLT applied automatically whenever the set_xml_content method is called, which for translations and texts is what calls the pre-processing. Since part of automatically updating the revisionDesc during finalization now does this, this should happen transparently for finalized texts.
    6. *What’s the regime for sun-setting such patches so they’re not left hanging around in the save step?*
    7. *There was a proposed mechanism for notifying users of such modifications, but where this should go is ambiguous. At one point the Digital Papyrology blog and/or some sort of “news” on the papyri.info home page was proposed. Josh: still desired?*

    **Global XML Updates**

    1. Global change applied to all canonical XML files that meet the criteria – must have a comment added into the change history in the XML file (‘<change>’ tag inside ‘<revisionDesc>’ tag)
    2. Usually applied using an XLST style sheet ran against the canonical repository XML files.
    3. If you have not done it before, use the Create Local Git Repository for XSugar/SoSOL/IDP data process to create a local copy of the ‘idp.data’ repository.
    4. Make sure you are on the ‘master’ branch and it is up to date (‘git checkout master’ and then ‘git pull’).
    5. Create a new branch to apply your changes to from the ‘master’ branch (‘git checkout –b makechanges’.
    6. Apply the XSLT to XML files you need to update in the appropriate working directory
        * Text files = DDB_EpiDoc_XML directory
        * Translation files = HGV_trans_EpiDoc directory
        * HGV = HGV_meta_EpiDoc directory
    7. Validate the XSLT performed the changes you wanted and no other extraneous ones occurred.
    8. Return to the ‘master’ branch and merge the branch with the changes into the ‘master’ branch (‘git checkout master’ and ‘git merge makechanges’ based on example above).
    9. Use the Re-publish PN/PE Data process to make the changes visible via PN and newly checked out PE files.
    10. Follow the XML Updates via SoSOL XSLT process to ensure the changes are applied to ‘user’ files that were not in the canonical repository when this ‘global’ change was made. 

2. Modify Reverse Crosswalker/Crosswalker

    **Modify Crosswalk / What has to happen after the Crosswalker has been called to action?**

    * Commit recently crosswalked data to master branch of idp.data repository
    * Synchronise all existing mirrors of idp.data repository
    * Call number server to index the new stuff → Hugh

    The crosswalker applications are tools that need to be modified if an interface on either end (input or output) has changed....

3. Detect and Resolve Git Update Conflicts

    The automated sync process will fail in the case of a merge conflict, and sometimes merge conflicts occur during manual sync. These are flagged in the files, and the merge conflict must be resolved manually and committed.

4. Add Data to Numbers Server

    This assumes GitHub and canonical data have been merged.

    1. `cd /data/papyri.info/git/navigator/pn-mapping`
    2. `lein run map-all`

5. Re-index PN Canonical Data

    1. changed sic/corr to orig/reg caused search to not work correctly. Hugh needed to change something and do a re-index. `^καγω^` 11 hits and `"και εγω"` 301 hits
    2. `cd /data/papyri.info/git/navigator/pn-indexer`
    3. `lein run`

6. Re-publish PN/PE Data

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

7. SoSOL Broken Publication

    1. Withdraw button available on board member’s publication overview removes **all copies** of the submitted publication **for all boards**, including finalizing copies. This does not affect any commits to canonical that have already been finalized. This, in effect, clears out any inconsistent states that may arise in a publication and returns it to being unsubmitted.
    2. For publications this doesn‘t fix, see SoSOL maintenance.

Release Processes
-----------------
