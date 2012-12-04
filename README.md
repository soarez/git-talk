# Intro

##### Problems

- **Traveling back in time**
	- Revert files back to a previous state
	- Revert the entire project back to a previous state
	- Compare changes over time
	- See who last modified something that might be causing a problem
	- Who introduced an issue and when
- Sharing
- Collaborating
- Paralelizing your work
	- Solving multiple issues separately
	- Make a quick fix in the midst developing a feature

### Version Control Systems (VCS)

#### Frequently making a backup

  - The caveman solution
  - Muggles still use this use this everyday
	- Designers
	- Managers
	- Lawyers
	- Doctors
	- …

- Good
	- Pretty straightforward
- Bad
	- CHAOTIC

##### Local VCS

- rcs
- Good:
	- ???
- Bad:
	- Hard to manage over time
	- Hard to collaborate
	- Single point of failure

##### Centralized VCS

  - CVS, Subversion, Perforce
  - Standard solution for many years and still used by many
  - Good
    - Easier to collaborate
    - Not too hard to understand
    - Easier to administer a CVCS than it is to deal with local databases on
      every client
  - Bad
    - Single point of failure
    - Hard to work without access to the server

##### Distributed VCS

  - Git, Mercurial, Bazaar
  - Clients fully mirror the repository
  - No single point of failure
  - Best for collaboration - Multiple remotes

##### Git

 - Designed for:
 	- Speed
	 - Simple Design
 	- Non-linear development (thousands of parallel branches)
 	- Fully distributed
  	- Scale - Able to handle large projects like the Linux kernel efficiently
    (speed and data size)
 - Different
  	- While other VCSs represent commits as file based changes, git takes
    snapshots of the files.
    Snapshots, not differences
  	- Git stores data as snapshots of the project over time
  	- Nearly every operation is local
	- Local history
	- Database is replicated when cloning
  	- You can commit without network connection
	- Integrity is built-in

O rly? Who is using dat?

- Linux
- Rails
- Google
- Facebook
- Microsoft
- Twitter
- LinkedIn
- …

**If you don't use Git, you better have a damn good reason not to.**


###### Why you should NOT use a GUI for Git

1. **Git is NOT famous for its interface**
2. It is really HARD to change #1. It is a very likely unreliable abstraction.
3. You're a programmer, not a manager or tech dude
4. GUIs are slow


# Setting up

##### Configuration

  - `/etc/gitconfig` - system
  - `~/.gitconfig`   - global
  - `.git/config`

Windows: `C:\Documents and Settings\$USER\.gitconfig`

##### Identity

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

##### Editor

    git config --global core.editor vim

##### Merge tool -- don't change this if you're not sure

    gif config --global merge.tool vimdiff

##### Check current settings

    git config --list # show all config values, last ones have priority
    git config user.name # show config value

# Help

    git help <verb>
    git <verb> --help
    man git-<verb>

Example:

    git help config

IRC - irc.freenode.net

  - \#git
  - \#github

Free book

  - ProGit
  
# Git Basics

### The 3 states for **tracked files**

  - Commited
    The data is safely stored in the database.
  - Modified
    Changed, but not commited.
  - Staged
    Marked modified file in its current version to go into the next commit

### The 3 sections

  - Git directory
    Where Git stores the metadata and object database. This is what is copied
    when a project is cloned
  - Working directory
    A single checkout of one version of the project, pulled from the database
    to use or modify.
  - Staging area - aka index
    A file contained in the Git directory that stores information about the
    next commit

##### Initializing a repository in an existing directory

    git init

##### Cloning an existing repository

    git clone <repo>

##### Checking the status of your files

    git status

##### Start tracking new files or staging modified files

    git add <filename>
    
    # interactive
    git add -i <filename>

A file is staged in its current version. If you make changes to a file after
you staging it, the new changes will not go into the new commit unless you
stage the file again before commiting.

##### Ignoring files

    $EDITOR .gitignore

##### See what is changed

    git diff

##### See what is staged

    git diff --cached

##### Commit!

    git commit
    git commit -m 'Add speech module #432'

    # Thor-style commits
    git commit -a -m 'fix stuff'
    
Preferably, commit messages:

- Are in the present tense
- Start with a capitalized letter
- Have a subject that does not exceed 50 chars
- Have a thourough message body that explains in detail what was changed and why. This message is linewrapped at 72 chars.

##### Change last commit message

    git commit --amend

##### Remove files

    git rm <filename>
    git rm <filename> --cached

##### Git accepts file-glob patterns

    git add test/\*.js
    
Because bash does its own globbing, `*` is escaped.

##### Moving files

    mv <old> <new>
    git rm <old>
    git add <new>

    # or

    git mv <old> <new>

##### View the commit history

    git log

##### Removing file form staging area

    git reset <filename>

##### Undo file changes

    git checkout <filename>

##### Tagging

    git tag <tagname>     # simple
    git tag -a <tagname>  # annotated

# Remotes

##### List remotes

    git remote -v

##### Add a remote

    git remote add <name> <url>

##### Fetch remote database

    git fetch <remote>

Does not change any of your own branches nor your working copy.

##### Fetch + merge with current branch

    git pull <remote> master

##### Pushing

    git push <remote> master

# Branching

Branching means diverging the from the current line of development.

To understand branching, you need to understand how commits are stored. A commit consists of:

- message
- author
- commiter
- date
- pointer to a tree (snapshot), identified by its checksum
- pointer to previous commits

What are branches:

- A branch in Git is simply a lightweight movable pointer to one of these commits. 
- When you commit, the branch moves forward, pointing to the new commit.
- A branch in Git is in actuality a simple file that contains the 40 character SHA–1 checksum of the commit it points to

Take note:

- **Branching is inexpensive.**
	- Creating a new branch is just creating another pointer!
	- Creating a new branch is as quick and simple as writing 41 bytes to a file (40 characters and a newline).
	- Branching is a LOCAL operation, no server communication is needed
- Switching branches changes the files in the working directory
- A special pointer called `HEAD` always points to the current branch. 

##### Create a branch

    git branch <branch_name>

##### Move to another branch

    git checkout <branch_name>

##### Create a branch, and switch to it

    git checkout -b <branch_name>

##### List branches

    git branch

The `*` charater prefixes the current branch (`HEAD`)

##### See the last commit on each branch

    git branch -v

##### Delete a branch:

    git branch -d <branch_name>


### Topic branches

You should branch everytime you do something new. Branch for:

- Fixes
- Features
- Experiments

### Long-running branches

##### If for some reason you do not care about versions. 

- Keep master stable

##### If you want to have releases with specific versions.

- Develop on master
- Branch to stable release versions
- Fix bugs on release versions branches and merge onto master
- Never merge master onto release version branches

See semver.

##### Multi-branded products

- Keep master stable
- Have a branch for each brand
- Merge features onto master, and only then, merge master onto the branded branches

### Remote branches

References to the state of branches on remote repositories. They're local branches (pointers, remember!) that you can't move (directly).

Form: `<remote>/<branch>`

##### Push a branch

    git push <remote> <branch>
    # or 
    git push <remote> <local_branch>:<remote_branch>
    
##### Checkout a remote branch

    git checkout -b serverfix origin/serverfix
    
##### Setup a pre-existing branch to track an existing server branch

    git checkout <local_branch>
    git checkout --track <remote>/<remote_branch>

##### Delete a remote branch

    git push <remote> :<remote_branch>


# Merging

##### Merge `<branch>` into HEAD

    git merge <branch>

### Fast forward merges

When the commit on the branch you’re on isn’t a direct ancestor of the branch you’re merging in, or, in other words, when you try to merge one commit with a commit that can be reached by following the first commit’s history, Git simplifies things by moving the pointer forward because there is no divergent work to merge together — this is called a “fast forward”.

### Three-way (or n-way)

Otherwise, Git does a simple three-way merge. Git identifies the common ancestor and uses as a base for the 3-way merge. A new snapshot is created that results from this three-way merge and automatically, a new commit is created that points to it.

#### Merge conflicts

A merge conflict can happen in three-way merges when the same part of a file is changed by both branches being merged. Git pauses the merge process until the conflics are resolved.

##### See which files are *unmerged*

    git status
    
##### Conflict example

    <<<<<<< HEAD:index.html    <div id="container">Awesome!</div>    =======    <div id="container">
		Wunderbar!
    </div>    >>>>>>> my-other-branch:index.html

**Remember:** `HEAD` is was what you had checked out when you ran your merge command

##### Mark file as resolved

    git add <filename>
    
##### Use the configured graphical tool to resolve the issues
    
    git mergetool
    
##### Continue the merge after all conflicts have been resolved

	git commit

By default, the commit on a merge will be `'Merge branch <branch>'`

##### See which branches haven't yet been merged

    git branch --no-merged

# Rebasing

Reapplying a diverging branch onto another.

    git checkout <diverging_branch>
    git rebase master
    
After rebasing, a merge of *master* with *diverging_branch* will fast-forward master.


How it works: Git …

1. finds the common ancestor of the two branches (base)
2. gets the diff of each commit of the branch you’re on, from the base
3. saves those diffs to temporary files
4. resets the current branch to the same commit as the branch you are rebasing onto
5. applies each change (diff) in turn

> Rebasing replays changes from one line of work onto another in the order they were introduced, whereas merging takes the endpoints and merges them together.

**The only difference between merging and rebasing is the resulting history.**

#### Attention

**!! Do not rebase pushed commits !!**

# Distributed workflows

### Centralized workflow

- Everyone can commit
- The first to push feels less pain
- The second developer must merge/rebase before pushing
- Common in centralized version control systems (CVCS)
- Very basic, very common

### Integration manager workflow

- 1 maintainer
- Everyone pulls from the maintainer
- Contributions are made through pull requests
- Maintainer pulls contributions from the colaborators' repos and merges them

### Dictator and Lieutenants workflow

- Good for big projects
- Dictator owns the main repo
- Lieutenants are responsible for components of the whole solution
- Lieutenants integrate contributions from contributors
- Dictator integrate pulling from his trusted Lieutenants

# Tips

Auto complete

    source ~/git-completion.bash

Colors!

    git config --global color.ui true

Log

    git config --global alias.l log --color --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%aN>%Creset'

Status

    git config --global alias.s status -s

Stage parts of a file

    git add -i <filename>

Also, **use bash alias'es.**

Use a bash prompt that tells you which branch you're currently on.

    # Append this to .bash_profile
    export PS1="\W\$(git branch 2> /dev/null | grep -e '\* ' | sed 's/^..\(.*\)/[\1]/')$ "
    
# For another day

- Cherry picking
- Commit squashing
- Changing history
- bisect
- Submodules
- merge strategies
- whitespace settings
- gitattributes
- hooks
- 

