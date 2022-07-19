# Git Commands

|Command  |Purpose  |
|:----------|:--------------------|
| `git config --list` | Show the configuration options git has encoded for your local system.|
|`git clone <https or ssh URL>`| Copies the entire repository into your local environment (your computer or your server account). You typically wouldn't use this more than once per project.|
|`git ls-remote --heads origin` | This command lists all branches on the remote machine (typically a central repo).|
|`git checkout <existing branch name>` | This command allows you to "checkout" or start using an existing branch, as long as it already exists in the repo.|
|`git checkout -b <new branch name> <branch to track against>`| Create a new branch that tracks against what's in the existing branch you specify. The latter branch name is optional -- if absent, your current branch is used.|
|`git checkout -b <existing remote branch name> origin/<existing remote branch name>` | Create a local branch that tracks a pre-existing remote branch. If you've previously interacted with the repository and have git pulled, then you can also just list the remote branches and issue a `git checkout <branch name>` command.|
|`git checkout -- <file name>` | Revert the file to the last commit.|
|`git pull` | This command pulls the latest changes to the repo, from the remote/server. It is best practice to "git pull" before you begin making changes locally, to ensure you won't have any version conflicts when you push your changes. A default git pull is equivalent to a git fetch + git merge. Alternately, you can check out `git pull --rebase`.|
|`git status` | List the current state of files in your project relative to the most recent committed state. (Identifies changes files, deleted files, and new/untracked files.) | 
|`git rm <filename>` | Delete a file and instruct git to track/acknowledge the deletion.|
|`git checkout <branch to copy from> <filename>` | While in a branch, copy a file form another branch to your current branch (will copy to the same folder on the new branch).|
|`git rm <file name>` | Delete (`rm`) a file and stage the deletion to be commited so that it can be pushed to the repo.|
|`git mv <original path/filename> <new path/filename>` | Move (`mv`) or rename a file in a way that maintain's git's awareness of the relationship between the new filename and the old (better for tracking changes over time). By comparison, a simple `mv` command without the `git` part will be treated as deleting one file and creating an entirely separate file in the repository. |
|`git diff <filename> <commit or branch name>` | Display the file-level differences between two versions or checkpoints in the repository. If filename is unspecified, this shows all differences across all files. If the commit or branch name is unspecified, this shows differences from what was last committed to the current branch. |
|`git add <file names or .>`  | This command *"stages"* any changes you've made to the files in your repo in prepareation to be committed. When you type "`git add .`" you add everything in the folder. You can also specify individual files, but best practice is to add all changes.|
|`git commit -m "<insert commit message here>"` | This command commits the changes you've staged to the git tracking system. This commit is only stored locally, until it's pushed to the server. **Design your commit messages to be useful for others.** Your team (and future self) will thank you!|
|`git push` | This command pushes the committed changes to server and will be available for everyone else to access. For the first push, you may need to use an expanded form: `git push --set-upstream origin <branch name>` |
|`git push --force` | **Don't do this.** In a multi-user environment, this says "I don't care what you did while I wasn't looking. Make the files look like mine instead." |
|`git branch -d <branch name>` | Remove local branch (do this after merging your branch.|
