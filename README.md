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
