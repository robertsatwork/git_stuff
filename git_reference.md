#### Git & GitLab Reference Guide

## Overview

**Git** is a code-tracking and version-control system originally designed for linux development.
It is decentralized and allows development separate from others' work, with syncing on command.

See [Source Code Management with Git (PDF)](http://ice.uchicago.edu/2012_presentations/Faculty/Skrainka/Talk_Git.pdf)
for a nice, accessible overview of using git.

**GitLab** and **GitHub** are more centralized websites with additional features 
built on top of git or to support git-centric workflows.
GitLab adds basic issue-tracking and other features,
and it acts as a central hub for a code repository (somewhat distinct from the core functionality of git).

# Disclaimer
Be sure you understand Deloitte's and your project's / client's policies regarding the use of git before you use it for Deloitte-related work.
* Clients might use GitLab, TFS, GitHub, or roll their own custom configuration based on git/mercurial/CVS, etc. They might have all manner of restrictions in place, or they might not know enough to instruct contractors how to use it wisely. Be prudent.
* Deloitte has a locked down [GitHub group](https://developer.deloitte.com/enterprisecoderepository/) that you can apply for permission to use, subject to [restrictions](https://developer.deloitte.com/ecr-docs/rules/) including: 
  - Only source code for internal projects can be managed in GitHub. Source code developed for a specific client is not permitted.
  - Only GitHub user accounts with an active deloitte.* email address associated with them are permitted access to the ECR (i.e. outside collaborators are not permitted).


[[_TOC_]]


## Installation / Access instructions

* Insert instructions link for Deloitte github
* Insert instructions for installing gitbash (Windows)
* Insert instructions for installing github app


**Note:** Most comments below are written from a linux user's perspective, 
but they should apply well to git bash usage on windows. 
The concepts are analogous to GUI-based commands you'll find in git-centric applications as well, 
even though the visual interface will be different.


## Gtilab workflows & general best practices

The gitlab website offers a means to view git repository files, edit in-browser, open issues and link to branches, submit and process merge requests, etc.
It's a very user-friendly method of interacting with a git repository overall.
The primary downside is that syncing edits made in a web page to an application where files are being run/edited is manual and prone to copy/paste errors. 
A preferred approach is using a git-aware or version-control-aware development application
(such as Notepad++, DBeaver, RStudio, VSCode, etc.) that detects changes made outside its own processes.
Understanding that limitation can help you make best use of the website and command-line applications in parallel.

When you're using gitlab to track code revisions, maintaining a general framework for where & how to edit files can be helpful:
* When new team members join a project or activity:
  - Immediately grant them access to the project repository.
  - Ensure they know team-level expectations for how to use and interact with the project.
  - Optionally, follow a principle of increasing access as necessity dictates per user. (Wait until access-denied issues arise before increasing access.)
* Maintain a primary branch that is stable and rarely/never edited directly. (This might be the `master` branch, for example.)
  - Maintain a secondary branch where all changes are integrated for testing, before then being merged into the primary branch infrequently once stable. (This might be your 'dev' or 'development' branch.)
  - Generate a variety of other branches for changes that are more topic-specific. These will branch off the develpment branch, absorb edits from ongoing work, and then be merged back into development before being deleted.
  - As appropriate, branch further off the above branches for efver more specific work. Use descriptive branch names rather than default branch names like "patch-5."
  - As an alternative model, explore [trunk-based development](https://trunkbaseddevelopment.com/) for a simplified branching policy designed to reduce the difficulty of merging new code.
* Generate an issue that describes a particular task to be performed, whose end result is shown in changed code.
  - Describe the issue and expected work in concrete detail to the extent possible.
  - From the individual issue page, generate a branch focused on that issue.
  - Perform work to resolve the issue on that branch.
  - Ideally engage in peer review or team lead review, such that code changes are reviewed by someone other than the person who generated the changes.
  - Submit a merge request for that branch (back into `development` (or appropriate source) once work is complete and tested/reviewed.
  - Close the issue once the work is merged.


## Gitlab and Jupyter Notebooks
The gitlab website can display jupyter notebooks, but it is not great about showing version changes.
Be careful not to leave PII or large output data sitting in the output cells of a Jupyter notebook whne it's saved into your repository.


## Command line instructions

You can sync Gitlab files to your computer or server environment using git commands.
If you've never used git before, read up or follow other instructions to get acquainted.
Then customize and run the global setup commands below.
Each command should be typed into a terminal window such as git bash or a server view of the unix console.


### Git global setup
If you've never synced files using git before,
find some Unix instructions for generating an SSH key. 
Doing so will involve pasting a long string of characters into your
github or gitlab profile.

Once you have SSH keys set up,
customize and run the commands below once.

```sh
git config --global user.name "John Q. Public"
git config --global user.email "johnpublic37@deloitte.com"
git config --global core.fileMode false
```

The last command above is useful for certain situations where a managed server environment will rewrite file modes (read/write/execute flags) without user intervention.
The command instructs git not to track those often trivial changes in version history.


### Create a new local copy of a git repository

After you have git set up and authenticated for gitlab or github,
use some version of the following command:

```sh
git clone git@gitlab.com/userid/project_name.git   # general format for personal projects
git clone git@gitlab.com/group_name/project_name.git   # general format for group-owned projects
```

Doing so will create a directory named with the project name,
containing any files that were already hosted in the repository.
That directory can synchronize the git-tracked copy of this project's files.

```sh
cd project_name
ls -lha
```

The 'cd' command changes directory so that you're working inside the project folder.
The 'ls' command lists the contents of the directory. If this is a new project, you won't see much.
If you cloned an existing project, then you should see the files that exist on some branch of the repo.


### Always stay in sync with others: use `git fetch` or `git pull`

When you return to working with gitlab code after a break, it's useful to 
sync up your codebase with what others have posted into the gitlab projectd.

Run this "git pull" command habitually when starting a work session:
```sh
git fetch origin                   # fetches the changes from remote server; allows using git diff afterward
git pull                           # fetches changes and auto-merges them
git pull origin master             # fully-specified form for the default "master" branch
git pull remote_name branch_name   # general form
git pull --rebase                  # Attempt to replay local changes onto the end of the remote branch's most recent change list.
```

Doing so will attempt to synchronize the contents of your "local" codebase (the files in your device or folder)
with the remote repository's codebase (including recent changes by any others).
A clean result from a `git pull` wis a good prerequisite for easily working with git-tracked code.

The output can sometimes be a lot. Read it anyway to be sure the command succeeded before continuing.


### General process for updating files safely: Use *branches*

A brand new repo contains the "master" branch only.
It's considered best practice to work in other branches,
then only merge well-tested and stable code into the master branch.

**Here's how you can do that:** create a branch, switch to the branch,
do your work (editing files), and then commit changes within that new branch.

```sh
git branch my_new_branch                # makes a new branch
git checkout my_new_branch              # switches to that new branch
# ...(make changes, add/edit files, etc.)
git status                              # lists file changes relative to what has been previously "committed"
git add edited_file.ext                 # "stages" a file to be committed to the repo
# Next command commits files to version history
git commit -m "Updated edited_file.ext to include better code and comments"
git push origin my_new_branch           # propagate committed changes to the remote repository
```

Only later, once you're happy with the edited contents (i.e., the changes are stable and/or complete), 
should you try merging into a different branch.
(You don't even need to merge into 'master'! You can make lots of branches,
merge them as seems appropriate, and leave it all outside of master for a long time. That can make things pretty complicated, though.)


#### Variations on the branch checkout command

```sh
git checkout existing_branch            # Switch to a specific branch
git checkout -b new_branch_name         # Create and then switch to a new branch
git checkout {hash of earlier commit}   # Switch focus to a specific commit as if it were a branch (but it's not)
```
Underneath the hood, git is actually doing a lot when you switch branches: it's systematically swapping all the files you had out for files as they exist in that other branch. It'll stop & fail if you have uncommitted changes that would be overwritten, so you need to pay attention to what happens after issuing the checkout command.



#### Adding and editing a file within a server environment
```sh
touch new_markdown_file.md              # create an empty file, or update the modification date of an existing file
```

Once you've created that file using the 'touch' command, try a simple terminal-based editor:
```sh
nano new_markdown_file.md
```

A few command codes are useful in nano: Ctrl+X is exit (and you'll be prompted to save).
Check the others displayed at the bottom of the screen when using nano.


#### Adding file(s) to "stage" them for a commit, then commit, then push to remote

```sh
git add new_markdown_file.md     # Stage a specific file so that it can be committed
git add *.sql                    # Stage all files of a specific type
git add .                        # Stage all files
git commit -m "updated a markdown file, and several .sql files, and just everything"
```

When you want to sync your committed changes with the a remote repository,
you should "push" your changes using git commands:

```sh
git push                         # Send changes from your home system to the remote project
git push -u origin               # force update to the remote destination
git push origin my_new_branch    # fully specify the destination & branch to push
```

For historical reasons, this is similar to what's referred as a "pull request",
which refers to sending a request that the remote repository pull in your pushed changes.

The sequences above seem like many more steps 
than what people new to version control or transitioning
from pure gitlab/github usage will be used to.
Using the gitlab website directly avoids a lot of this complexity,
and gives the sense of being perfectly in sync all the time.

* **Remember** that using git to sync with gitlab is a voluntary, user-initiated process.
* If you don't `git pull` and `git push` properly, you're not syncing with gitlab.
* If you're not syncing with gitlab, others aren't seeing your work.
* If you somehow manage to start syncing your work against a different project than what others are using (a common error with access-restricted users), others aren't seeing your work.


### Merging your changes back into the main code
So you've created a branch and made some nice improvements to the code. Now what?
Well, you want to instruct git to incorporate those changes.
You need to use the `git merge` command to merge the contents of your new working branch into the other branch (usually the branch it diverged from but not always).
Merging requires analyzing what changes occurred in both the source and the target branches to determine whether those changes are compatible or potentially produce conflicts.
As an example, if you and your coworker both changed the exact same line of code since your branch diverged, then when you merge the work together, there's no clear decision for which change git should use. And it's not quite smart enough to just program up an arbitrary average of the two.


### Merge conflicts

Merge conflicts can be a real headache. Prevent where possible, undo where possible, resolve when reasonable, nuke & restart when necessary.

#### Preventing merge conflicts

* Use `git pull` and `git push` frequently
* Factorize your code appropriately
* Use different branches for different work
* Split long code segments across multiple lines

(Other options: `git fetch` then extra local work to merge, `git pull --rebase`, etc.)

#### Resolving merge conflicts

The core of resolving a merge conflict is digging into the differences between the two files,
and manually editing until you have one version that you're happy with.
At its simplest level, you can just obliterate one version and keep the other.
A temporary wayu to set aside all local work conflicts is to use the `git stash`  command.
A more permanent way is to use `git checkout the_file_I_want_to_reset.txt` to obliterate local changes.

#### Nuke & restore

If preserving the local changes just aren't worth it but you've still gotten things too messed up to dig yourself out,
delete the git repo from your local filesystem, then fully sync again.
It's the "nuclear option" for a reason, but it can be a quick way to end the battle.


