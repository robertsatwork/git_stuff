# Git MergeConflicts

Merge conflicts in git can be a real headache. Prevent where possible, undo where possible, resolve when reasonable, delete file or repo & replace when necessary.

## Preventing merge conflicts

Merge conflicts are really annoying,
but they're an inevitable consequence of git's intermittent sync process.
Take rasonable steps to avoid them whenever possible.
Here are some general tips to prevent merge conflicts:

* Use git pull` and `git push` frequently
* Factorize your code appropriately
* Use different branches for different work
* Split long code segments across multiple lines


Using `git pull` frequently will help prevent merge conflicts when 
multiple users are editing the same file. 
If `git pull` is unsuccessful, first try `git fetch`, and read about `git stash` below.

Another general approach to preventing merge conflicts is to carefully
"factorize" your code. That means splitting out different code purposes into
different files, and keeping similar code purposes in the same files.
The idea is to reduce the likelihood that multiple people (or multiple branches)
will wind up editing the same files.

Similarly, when editing your code, make and merge branches frequently 
and for reasonably narrow purposes. The more you use branch-edit-test-merge 
methods, the more separate a specific branch's work will be from other work, 
and the less likely you'll be to see a merge conflict.

Finally, split long code across multiple lines where appopriate. Doing so improves readability, which is a great reason on its own. But it will also help isolate elements of the logic into different lines.

Git versioning and merging works on a line-by-line basis. So when anything in a line changes, the whole line is consdiered to have been edited. Longer lines have more editable parts, thus incraesing potential for concurrent edits.


## Resolving merge conflicts

The core of resolving a merge conflict is digging into the differences between two files,
and manually editing until you have one version that you're happy with. 
at its simplest level, you can just obliterate one version and keep the other. 
A temporary way to set aside all local work conflicts is to use the `git stash` command.
A more permanent way is to use `git checkout the_file_I_want_to_reset.txt` to obliterate local changes.

A more complicated (but ideally preferred) approach is to integrate the differences in a useful way.
The gitlab or github website can help resolve some merge conflicts in a more visual way.
The gist of that approach is that you identify discrepancies between branches, 
make a commit on the branch whose code should change to have it match the other, 
then perform the merge without remaining conflicts.

See [this stackoverflow page](https://stackoverflow.com/questions/161813/how-to-resolve-merge-conflicts-in-git) 
or just google for additional resources.

See also this how-to: [Reset local repository to be just like remote](https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head)


## Using git stash`

Got some local changes (or even just differences from the repo) that you 
don't want to commit just yet? Stash them away with `git stash`.

Git stash is a *stack* of uncommitted changes. (see also "FIFO" ordering)
When you use the command, you add all unstaged, uncommitted changes onto the stack.
Doing so will allow you to, for example, perform a clean `git pull` or 
switch to a different branch to do other work without committing in-progress changes.

To retrieve the contents of your stash, use
`git stash apply` (for the top of your stash)
or 
`git stash apply stash@(2)` (for the 2nd-from-top of your stash).

See [this reference](https://git-scm.com/book/en/v1/Git-Tools-Stashing) for more info.
