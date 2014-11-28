# Description
This repository is thought for learning git and therefore contains the most important commands for git in a very basic way.
The author most often doesn't use prosa text but heavily uses some basic git commands in favour of providing lean examples.

# Configuration
1. Set the user name locally (for this repository)
2. Set the email address locally (for this repository)
3. Set the default editor globally (for the current user)
4. List all configurations

```sh
# details: man git-config
1> git config --local user.name "John Doe"
2> git config --local user.email john.doe@email.com
3> git config --global core.editor emacs
4> git config --list
```

# Getting a Repository
1. Initialize the working directory as an empty git repository
2. Clone the repository _git\_training_ into the subfolder _git\_training_
3. Clone the repository _git\_training_ into the subfolder _training_
4. Clone the branch _my\_branch_ of the repository _git_training_ into the subfolder _git\_training_ (the _--branch_ switch also allows to clone tags)

```sh
# details: man git-init
1> git init
# details: man git-clone
2> git clone git@github.com:lgeorgieff/git_training.git
3> git clone git@github.com:lgeorgieff/git_training.git training
4> git clone --branch my_branch git@github.com:lgeorgieff/git_training.git
```

# Working with Files
1. Request the current status of the repository
2. Request the current status of the repository in short form
3. Add the file _README.md_ to the repository stage the file _README.md_ and add its last changes to the next commit
4. Check unstaged changes
5. Check all staged changes that will go into the next commit
6. Identify whitespace errors (whitespace errors are defined by _core.whitespace_)
7. Remove the file _README.md_ from the repository and the file system
8. Remove the file _README.md_ from the repository but leave it on the file system
9. Moves the file _README.md_ to _README.txt_ in the repository and on the file system
10. Clean the working directory and all sub-directories except files matched by _.gitignore_
11. Show what would be done if `git clean -d` is executed
12. Clean the working directory and all sub-directories inclusive files matched by _.gitignore_

```sh
# details: man git-status
 1> git status
 2> git status -s
# details: man git-add
 3> git add README.md
# details: man git-diff
 4> git diff
 5> git diff --staged
 6> git diff --check
# details: man git-rm
 7> git rm README.md
 8> git rm --cached README.md
# details: man git-mv
 9> git mv README.md README.txt
# details: man git-clean
10> git clean -d
11> git clean -d -n
12> git clean -d -n -x
```

## .gitignore
1. Ignore all files that ends with _~_
2. Ignore the folder _/tmp_
3. Ignore the file _/TODO_
4. Ignore all files with the name _TODOs_
5. Ignore all _.docx_ files in the folder _/doc/*/_
6. Track the file _important.docx_. This will also track files with the name _important.docx_ in _/doc/*/_.

```sh
# details: man gitignore
1> *~
2> /tmp/
3> /TODO
4> TODOs
5> /doc/*/*.docx
6> !important.docx
```

# Committing Changes
1. Commit all staged changes to the local repository
2. Stage all unstaged changes and commit it along with the comment _"..."_
3. Show the log of the repository
4. Show the log of the repository in the form of a commit per line
5. Show the log of the repository including a diff for each commit
6. Show the log of the repository including a compact diff for each commit
7. Show the log of the repository formatted as "short hash - author name, author date : subject"
8. Show the log of the repository in the form of a commit per line and displaying the branches as a graph
9. Show the log of the repository for the last two days
10. Show all logs with a short SHA-1 value
11. Show the log message of the commit _940c980_

```sh
# details: man git-commit
 1> git commit
 2> git commit -a -m "..."
# details: man git-log
 3> git log
 4> git log --oneline
 5> git log -p
 6> git log --stat
 7> git log --pretty=format:"%h - %an, %ar : %s"
 8> git log --oneline --graph
 9> git log --since=2.days
10> git log --abbrev-commit
# details: man git-show
11> git show 940c980
```

# Changing the History
1. Change the last commit
2. Reset all unstaged changes of the file _README.md_ to the version of the last commit
3. Reset the local repository to the commit _36b01dc_
4. Checkout the last version of _README.md_ and replace the local file on the file system by it
5. Change the history (commits) on top of the commit _36b01dc_
6. List all SHA-1 values for commits where the _HEAD_ and branch references pointed to. This can be very helpful after rewriting the history by using _git rebase_, since the overwritten references are still tracked in the _reflog_
7. Show the hash of the commit on which the master reference is pointing
8. Create a new commit that undoes the changes made in the commit _2c1e336_

```sh
# details: man git-commit
1> git commit --amend
# details: man git-reset
2> git reset HEAD README.md
3> git reset 36b01dc --hard
# details: man git-checkout
4> git checkout -- README.md
# details: man git-rebase
5> git rebase -i 36b01dc
# details: man git-reflog
6> git reflog
# details: man git-rev-parse
7> git rev-parse master
# details: git-revert
8> git revert 2c1e336
```

## Rebasing
Rebasing allows to integrate two branches into a "single line" (after the rebase it seems that there existed only one branch). The results of rebasing are similar to merging except that there is only one final branch. An example rebase works as follows:

1. Checkout the branch _my\_branch_ which changes should be put on top of the branch _master_
2. Rebase the current branch on top of the branch _master_
3. Checkout the rebased branch _master_
4. Perform a _fast-forward_ merge (since the _master_ pointer is behind the _my\_branch_ pointer)

```sh
1> git checkout my_branch
2> git rebase master
3> git checkout master
4> git merge
```

## Interactive Rebasing
Interactive rebasing allows to change the entire history of the git repository.
With _git rebase -i <commit hash>_ it is possible to select the start point from which the history is changed, i.e. the first changeable commit is the commit that follows after the specified one.
Afterwards the default editor appears with all commits listed (on top of the specified one). It is possible to
* __reorder__: by changing the order of the commits
* __pick__: (default) each commit that is annotated as _pick_ is used in the history
* __reword__: the commit message is changed
* __edit__: the entire commit can be changed (sources and commit message)
* __squash__: the commit is melt into the previous commit, the commit message can be changed
* __fixup__: the commit is melt into the previous commit, but the commit message of the current commit is discarded
* __exec__: the specified command at the end of the line is executed

the commits.

Note the history is rewritten, i.e. there are used different commit IDs and usually a _git push --force_ is required to write the changes into the remote repository. Also take into account, that others working with the current version of the repository may get in real trouble since their version of the repository won't exist after you commit the rebased changes.

1. Start the rebase process on top of the commit _9fc7ad5_
2. Stage the file _README.md_
 * Add the latest changes of the file _README.md_ of the current commit to the staging area
 * Or resolve a conflict of the file _README.md_ caused by the changes already done and the next commit
4. Change the currently selected commit in the rebase process
5. Go to the next commit during the rebase process (is required after amending a commit or resolving a conflict)
6. Abort the entire rebase process and revert all done changes during the rebase process

```sh
1> git rebase -i 9fc7ad5
2> git add README.md
3> git commit --amend
4> git rebase --continue
5> git rebase --abort
```
