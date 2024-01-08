# Introduction to version control with Git

Some introductory notes to Git - mainly for own reference to look up the commands later because I tend to forget them. Hopefully these notes will be helpful to others, too.

If you want to learn Git using these notes, a good idea is to create your own version of this readme file while trying out the different commands - learning by doing :D


## Basic shell commands

The starting point for this tutorial is to create a working folder with one or more files in it, using the following commands:

`pwd` - print working directory

`ls` - list files and directories in current directory

`ls -a` - shows also hidden files

`cd directory`, `cd ..` - change directory to archive, one level up

`cat file.txt` - show file.txt

`nano file.txt` - small terminal editor

`echo "text" > test.txt` - writes "text" to file (creates file if it does not exist, otherwise overwrites it)

`echo "text2" >> test.txt` - appends "text2" to file (error, if file does not exist)


## First git commands

`git --version` - displays version of git

`git` (without any arguments) - prints a helpful starting point (common Git commands used in various situations), for example try: `git help everyday` to see a Git guide on an example of everyday Git usage.

`git add file` - add a single file to the staging area (this will only work after you did `git init` to initialize the current folder as git directory)

`git add .` - add all (modified) files in current location

`git commit -m "Making a commit with a commit message"`

`git status` - see status of directory, e.g., non-tracked or modified files

`git log` - see (entire) history of commits. `git log -3` 3 most recent commits only. `git log --help` for (many) more ways of filtering the commit history.


## Basic Git workflow (local repository)

- Modify a file
- Save the draft: `git add file`
- Commit the updated file: `git commit -m "message"`
- Repeat


## Comparing files

`git diff file` - compares changes since last staging of file to last staged version (if file has been staged with `git add file`, but not modified afterwards, nothing will be shown)

`git diff -r HEAD file` - compares all changes to file since last commit to last commit (irrespective of staging)

`git diff -r HEAD` - same, but for all files

`git diff` without any arguments shows any changes that have not yet been staged

`git show first-few-characters-from-commit-hash` - shows the commit (including diff of that commit). The identifier (hash) of the commit is visible with `git log`.

`HEAD` - most recent commit

`HEAD~n` - (n+1)th most recent commit

`git show HEAD~1` - shows 2nd most recent commit

`git diff hash1 hash2` - compares two commits with hashes hash1 and hash2, respectively. The first few characters of the hashes are sufficient.

`git diff HEAD~2 HEAD~3` - compares the 3rd and 4th most recent commits

`git annotate file` - line-by-line information of commit hash, author and time


## Resetting and undoing changes

`git reset HEAD file` - unstaging a single file

`git reset HEAD` - unstaging all files

`git checkout -- file` - undoes changes to non-staged file (this command is potentially dangerous? If a file was saved as normal, but not staged or committed, then this command will delete the progress since last commit.) The double dash is a place holder for the last commit. Can do `git checkout commithash file` where `commithash` are a few (around 8) first characters of a specific commit. Alternatively, `git checkout HEAD~1 file` if the commit you are looking for is relative to the last commit.

`git checkout .` - undoing changes to all unstaged files since last commit

`git checkout commithash`, `git checkout HEAD~1` - both commands will reset the entire repo to a specific commit.

To restore the entire repository to the state of the previous commit, use a combination of these commands:

    git reset HEAD
    git checkout .
    git add .
    git commit -m "Reset repo to last commit"


## git log customization

`git log -3` - only 3 last commits

`git log file` - only commits modifying the given file (can be combined with previous command)

`git log --help` - overview of what customization is possible (a lot!)

`git log --since='Jan 5 2024'` - all commits after January 5th (not included), 2024. This can be combined with `--until='date in the same format'` to restrict the end date (inclusive)


## Cleaning a repository

`git clean -n` - show which files in the directory are not currently tracked (the `-n` flag means that nothing will be deleted just yet, it will be printed which files would be deleted. You may need to use the `-f` flag to force a deletion, unless the configuration `clean.requireForce` is true, which you can check with `git config --list`.)


## Configuring Git

`git config --list - settings`. Additionally append `--local`, `--global`, or `--system` for different levels of settings (for one project, all projects, or all users).

`git config --global setting value` - change a setting (for example `user.name`) to a given value. Note, `user.name` is a global setting, `git config --local user.name value` will add an additional local `user.name`). Use single quotes '' for values with space.


### Interlude: Setting your user name and email address of an existing commit

When doing the first commit, you may get this message:

> Your name and email address were configured automatically based on your username and hostname. Please check that they are accurate. 

You can suppress this message by setting them explicitly. Run the following command and follow the instructions in your editor to edit your configuration file: `git config --global --edit`

After doing this, you may fix the identity used for this commit with `git commit --amend --reset-author`

The command `git config --global --edit` will open the vi editor showing you a file asking you to change user name and email address.

To edit the file you need to hit Enter (edit mode). Then hit Esc (command mode) and type `:q!` to exit without saving, or `:wq` to save and exit.

After `git commit --amend --reset-author` you can check the user name and email address are correctly updated with `git config --list`


### Command aliases

`git config --global alias.cm 'commit -m'` - you can now execute `git cm "commit message"` instead of `git commit -m "commit message"`


## Branches

### Creating and changing branches

`git branch` - shows existing branches, the current branch is indicated with `*`

`git checkout -b branchname` - creates new branch and switches to it (fails, if a branch with that name already exists)

`git switch branchname` - switch to different branch

`git branch branchname` - creates new branch (fails, if the name is already taken), but does not switch to the new branch

`git diff branch1 branch2` - compares two branches (remember, before we used `git diff` to compare different commits in the same branch!)

`git checkout branchname` without the `-b` flag will change to the given branch. You need to commit any changes in your current branch before you can change the branch. Trying to check out a nonexistent branch fails (without the `-b` flag it will not be created)


### Merging branches

`git merge sourcebranch destinationbranch` - will merge two branches. For example `git merge currentbranch main` will merge your current branch to the main branch

There are different merge types in Git, see https://lukemerrett.com/different-merge-types-in-git/. For example, adding this line in an extra branch, but not making any changes in the main branch before merging will create a "fast forward merge". (For some reason, the command `git diff fast-forward-merge-test main` only worked in the main branch, not in the fast-forward-merge-test branch where the commit was made.)


### Tiny merge conflict example

Try to create a tiny merge conflict example:

1. Create a new file with 3 lines
2. Create a new branch and remove one line
3. Back in main, delete a different line
4. Try to merge into the main branch
5. Examine the file after the failed merge
6. Edit the file in main to produce the definitive version
7. Repeat the merge command and examine the file again

Possible solution:

1.  `echo -e "Line 1\nLine 2\nLine 3" > conflict.txt` 
    
    `git add conflict.txt`
    
    `git commit -m "adding conflict.txt with 3 lines"`
    
2.  `git checkout -b merge-conflict-example`
   
    `echo -e "Line 1\nLine 3" > conflict.txt`
   
    `git add conflict.txt`
   
    `git commit -m "removing line 2 from conflict.txt in merge-conflict-example branch"`
    
3.  `git checkout main`
    
    `echo -e "Line 2\nLine 3" > conflict.txt`
    
    `git add conflict.txt`
    
    `git commit -m "removing line 1 from conflict.txt in main"`
    
4.  `git merge merge-conflict-example main`

5.  `cat conflict.txt`

6.  `echo "Line 3" > conflict.txt`
    
    `git add conflict.txt`
    
    `git commit -m "only line 3 remains in conflict.txt after merge conflict"`
    
7.  `git merge merge-conflict-example main`


## Creating repositories

`git init` - creates new repository out of the current folder

`git init new-folder-name` - creates new sub-folder and makes a repository out of that new folder (if the folder already exists, and is already a Git repository, the message "Reinitialized existing Git repository" appears - I don't know what it does")

`git clone path-to-repository` - clones a local repository. Optionally, `git clone path-to-repository new-name` gives the newly created copy a different name than the original repository

`git clone URL` - to clone a remote repository. For example, `git clone https://github.com/adrische/git-tutorial.git` to clone this tutorial

`git remote -v` - display information about the original repository that was cloned from


## Working with remote repositories

`git fetch remote-name local-branch`, for example `git fetch origin main` - fetches the remote branch main

`git merge origin main` - will locally merge the main branch of origin into your main branch

`git pull origin main` - combined fetch & merge command

`git push remote-name local-branch`, for example `git push origin main` - pushes your local main branch to the remote main branch (what happens if the local branch does not exist in the remote repository?)

What if between pull and push more changes have been made to the remote repository? You would usually work in your own branch, and never actually directly commit to the main branch, but open a pull request.

Not covered: `git stash` (and much more).










