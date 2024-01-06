# Introduction to version control with Git

Some introductory notes to Git - mainly for own reference to look up the commands later because I tend to forget them. Hopefully these notes will be helpful to others, too.

If you want to learn Git using these notes, a good idea is to create your own version of this file README.md while trying out the different commands - learning by doing :D


## Basic shell commands

The starting point for this tutorial is to create a working folder with one or more files in it, using the following commands:

pwd - print working directory
ls - list files and directories in current directory
ls -a - shows also hidden files
cd directory, cd .. - change directory to archive, one level up
cat file.txt - show file.txt
nano file.txt - small terminal editor
echo "text" > test.txt - writes "text" to file (creates file if it does not exist, otherwise overwrites it)
echo "text2" >> test.txt - appends "text2" to file (error, if file does not exist)


## First git commands

git --version - displays version of git
git (without any arguments) - prints a helpful starting point (common Git commands used in various situations), for example try: `git help everyday` to see a Git guide on an example of everyday Git usage.

Git workflow: 

- Modify a file
- Save the draft
- Commit the updated file
- Repeat

git add file - add a single file to the staging area (this will only work after you did `git init to initialize the current folder as git directory)
git add . - add all (modified) files in current location
git commit -m "Making a commit with a commit message"
git status - see status of directory, e.g., non-tracked or modified files
git log - see (entire) history of commits. `git log -3` 3 most recent commits only. `git log --help` for (many) more ways of filtering the commit history.



## Comparing files

git diff file - compares changes since last staging of file to last staged version (if file has been staged with `git add file`, but not modified afterwards, nothing will be shown)
git diff -r HEAD file - compares all changes to file since last commit to last commit (irrespective of staging)
git diff -r HEAD - same, but for all files
`git diff` without any arguments shows any changes that have not yet been staged

git show first-few-characters-from-commit-hash - shows the commit (including diff of that commit). The identifier (hash) of the commit is visible with `git log`.

HEAD - most recent commit
HEAD~n - (n+1)th most recent commit
git show HEAD~1 - shows 2nd most recent commit

git diff hash1 hash2 - compares two commits with hashes hash1 and hash2, respectively. The first few characters of the hashes are sufficient.
git diff HEAD~2 HEAD~3 - compares the 3rd and 4th most recent commits

git annotate file - line-by-line information of commit hash, author and time


## Resetting and undoing changes

git reset HEAD file - unstaging a single file
git reset HEAD - unstaging all files
git checkout -- file - undoes changes to non-staged file (this command is potentially dangerous? If a file was saved as normal, but not staged or committed, then this command will delete the progress since last commit.) The double dash is a place holder for the last commit. Can do `git checkout commithash file` where `commithash` are a few (around 8) first characters of a specific commit. Alternatively, `git checkout HEAD~1 file` if the commit you are looking for is relative to the last commit.
git checkout . - undoing changes to all unstaged files since last commit
git checkout commithash
git checkout HEAD~1 - both commands will reset the entire repo to a specific commit.

To restore the entire repository to the state of the previous commit, use a combination of these commands:

    git reset HEAD
    git checkout .
    git add .
    git commit -m "Reset repo to last commit"


## git log customization

git log -3 - only 3 last commits
git log file - only commits modifying the given file (can be combined with previous command)
git log --help - overview of what customization is possible (a lot!)
git log --since='Jan 5 2024' - all commits after January 5th (not included), 2024. This can be combined with --until='date in the same format' to restrict the end date (inclusive)


## Cleaning a repository

git clean -n - show which files in the directory are not currently tracked (the `-n` flag means that nothing will be deleted just yet, it will be printed which files would be deleted. You may need to use the `-f` flag to force a deletion, unless the configuration `clean.requireForce` is true, which you can check with `git config --list`.)


## Configuring Git

git config --list - settings. Additionally append `--local`, `--global`, or `--system` for different levels of settings (for one project, all projects, or all users).
git config --global setting value - change a setting (for example user.name to a given value. Note, user.name is a global setting, `git config --local user.name value` will add an additional local user.name). Use single quotes '' for values with space.


### Interlude: Setting your user name and email address of an existing commit

When doing the first commit, you may get this message:

Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author


The command `git config --global --edit` will open the vi editor showing you a file similar to:

 Please adapt and uncomment the following lines:
        name = xxx
        email = xxx

To edit the file you need to hit Enter (edit mode). Then hit Esc (command mode) and type `:q!` to exit without saving, or `:wq` to save and exit.

After `git commit --amend --reset-author` you can check the user name and email address are correctly updated with `git config --list`.


### Command aliases

git config --global alias.cm 'commit -m' - you can now execute `git cm "commit message"` instead of `git commit -m "commit message"`


## Branches

`git branch` shows existing branches, the current branch is indicated with `*`

`git checkout -b branchname` creates new branch and switches to it (fails, if a branch with that name already exists)

`git switch branchname` switch to different branch

`git branch branchname` creates new branch (fails, if the name is already taken), but does not switch to the new branch

`git diff branch1 branch2` compares two branches (remember, before we used `git diff` to compare different commits in the same branch!)















