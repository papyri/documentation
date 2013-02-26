Sometimes objects in the various git repos go missing. This usually isn't a disaster, but it may mean that a given repo has refs that are unusable, and you'll need to manually prune them. Warning: because of the way repos are connected, pruning broken refs from one may cause problems in another. E.g. fixing a user repo may break a board repo. So a bit of checking to see whether you can reconstruct missing data often pays off. Repos may be connected via alternates, which you can find in <repo>/objects/info/alternates. Note that these are absolute paths, so renaming/moving repos should be done with care.

Always start by making a copy of the broken repo, just in case.

Running a git fsck will git you some idea of what's wrong. The output tends to have a lot of garbage in them. Run it with nohup, like

    nohup git fsck --full & 

to get a file of the output you can poke around in. You're looking for output beginning with "broken link" usually.

Missing blobs (files) often exist in another repo, and can be recovered. If you can find the repo that has a copy of the file, something like (run in the broken repo):

    git --git-dir=/path/to/repo/containing/object.git cat-file blob <sha-1> | git hash-object -w --stdin

will read out the file from the good repo and write it as an object to the current, broken, one. Blobs are more likely to be recoverable than trees and commits. To recover a tree, if you can find a repo containing it, you can do 

    git ls-tree <sha-1>

in the repo containing the tree, and that will write out the tree's content to the console. Piping the output of that into git mktree will re-create the missing tree. If there are missing objects in the tree (and there often are), you will need to do something like

    git --git-dir=/path/to/repo/containing/tree.git ls-tree <sha-1> | git mktree --missing

and then go find the missing objects. Missing commits, if they can be found in another repo, may be loose objects under the objects/ directory. Commit objects are stored in directories named for the first two characters of the SHA-1 hash of the commit and the actual file name is the rest of the hash. If you can find a loose object matching your missing one, you can simply copy it over into the corresponding objects/xx directory in the broken repo. Objects are periodically packed, however, so if it's not in the other repo, but you can read it with a

    git cat-file -p <sha-1>

then it is possible to re-create the missing commit. Git commits consist of a header followed by the commit data and message. If you do something like 

    printf "commit `git cat-file -p $sha | wc -c`\000" > $sha
    git cat-file -p $sha >> $sha

where $sha contains the SHA-1 hash of the file, you'll get a copy of the uncompressed commit. You can verify its correctness by doing

    sha1sum $sha

and the hash should be the same as the filename. The file still needs to be compressed with zlib, then it can be copied into place.

The Git FAQ explains how to get a list of [broken refs](https://git.wiki.kernel.org/index.php/GitFaq#How_to_remove_all_broken_refs_from_a_repository.3F)

    git for-each-ref --format='%(refname)' | while read ref; do git rev-list --objects $ref >/dev/null || echo "in $ref"; done

but if you have thousands of refs, like the DDbDP board does, this could take days to run. A better approach is often to see if you can figure out what ref the breakage is in. 

    git cat-file -p <sha-1> 
  
may give you a hint, if it contains the file name or id. Then you can look for corresponding refs by doing

    git show-ref | grep <identifying info>

You can maybe narrow down the refs you want to target then, by limiting the scope of `git for-each-ref`:

    git for-each-ref --count=100 --sort=-authordate --format='%(refname)'
    
for example, will only target the last 100 refs, sorted in descending order by creation date. Once the offending ref has been found, you can nuke it by plugging in `git update-ref -d $ref` at the end of the command above.

