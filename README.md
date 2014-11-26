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
