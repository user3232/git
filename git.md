<!-- 
vim: set tabstop=2:
-->
<!--
to view this in console:
$ pandoc -t plain git.md
-->

% Git notes

# Key things

* every git local repository is main repository
	(git repositiores system is distributed)
* git repository is always created locally
* it can be accessed remotely if created on server

# Global configuration

To list/edit user configuration:

```console
$ git config --list --show-origin
file:/home/mk/.gitconfig        user.email=26368212+user3232@users.noreply.github.com
file:/home/mk/.gitconfig        user.name=MK
file:/home/mk/.gitconfig        core.editor=vim
$ git config --global core.editor vim
$ git config --global core.editor
vim
$
```

# Getting help

All fallowing commands will load man page for `config` git command:

```console
$ git help config
$ git config --help
$ man git-config
```

Short help is by using `-h`:

```console
$ git add -h
użycie: git add [<options>] [--] <pathspec>...

    -n, --dry-run         dry run
    -v, --verbose         be verbose

    -i, --interactive     interactive picking
    -p, --patch           select hunks interactively
    -e, --edit            edit current diff and apply
    -f, --force           allow adding otherwise ignored files
    -u, --update          update tracked files
    --renormalize         renormalize EOL of tracked files (implies -u)
    -N, --intent-to-add   record only the fact that the path will be added later
    -A, --all             add changes from all tracked and untracked files
    --ignore-removal      ignore paths removed in the working tree (same as --no-all)
    --refresh             don't add, only refresh the index
    --ignore-errors       just skip files which cannot be added because of errors
    --ignore-missing      check if - even missing - files are ignored in dry run
    --chmod <(+/-)x>      override the executable bit of the listed files
$
```

# Marking folder gittable

Two options are common:
1. Make existing local forder git enabled or
2. Clone remote repository at specified folder.

## Marking folder git enabled

```console
$ git init # at folder root
```

## Cloning remote repository

```console
$ cd github/js/
$ ~/github/js$ git clone https://github.com/user3232/charting
Cloning into 'charting'...
remote: Enumerating objects: 12, done.
remote: Total 12 (delta 0), reused 0 (delta 0), pack-reused 12
Unpacking objects: 100% (12/12), done.
$
```

# Adding files to consider (staging)

To add files to be monitored by git:

```console
$ # to add:
$ git add git.md
$ # to selectively add:
$ git add -i
           staged     unstaged path
  1:       +68/-0      nothing git.md

*** Commands ***
  1: status	  2: update	  3: revert	  4: add untracked
  5: patch	  6: diff	  7: quit	  8: help
What now> q
Bye.
$ # to see what is going on:
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	nowy plik:     git.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.git.md.swp

$
```


# Tracked and untracked files

* Tracked files are those who:
	* were in last snapshot or
	* are in stageing area (marked with `git add somefile.x`).
* Untracked files are all other files.
* Modified files are files that were in last snapshot but
  have different content.
* Staged files are snapshot of files taken when using `git add ...`


Status of all files can be checked using `git status` command.

## git add command

This command can be thought as a way to:
* begin tracking new file,
* stage tracked modified files (marks they exact content
  at add moment to be candidates for next folder snapshot),
* mark merge conflicted files as resolved.


## Modifying staged files

Files marked as staged are snapshoted at time of marking (`git add`),
further changes to those files will not be candidates
to commit, but this information will be displayed by `git status`.

**Solution is simple, stage those files one more time.**

```console
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	nowy plik:     git.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	zmodyfikowany: git.md

$ git add git.md
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	nowy plik:     git.md

$ git status -s
A git.md
```


## .gitignore

To point files to be ignored `.gitignore` file
can be created. 

Synthax of this file is:

* Blank lines or lines starting with `#` are ignored.
* Standard glob patterns work, and will be applied 
	recursively throughout the entire working tree (`*`, `**`, `[ab]`, `?`).
* You can start patterns with a forward slash (`/`) to avoid recursivity.
* You can end patterns with a forward slash (`/`) to specify a directory.
* You can negate a pattern by starting it with an exclamation point (`!`).


Patterns are applied relatively to `.gitignore` file path.

Examples can be found at https://github.com/github/gitignore

Example:
```.gitignore
# ignore vim swap files
**.*.swp
```

## Pecisely figuring out changes

`git diff` command compares tracked (and staged) files
with their modified versions. 
* Oryginal files content are reffered by `-` (minus sign)
* Modified contend is reffered by `+` (plus sign)


## Removing files

```console
$ git rm someting.to.remove
```

it removes file and stages this file removal.

## Moving files

```console
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

above is eqivalent to:

```console
$ mv README.md README
$ git rm README.md
$ git add README
```


# Creating next snapshot (commit)

To save work described by stageing area `git commit`
command is used.:


```console
$ git commit
```

Now default system or set (using `git config --global core.editor`) 
editor will be called to create commit message. It will open
some file which should be edited and saved. After editor process
termination message from saved file will be used as commit message.


After successful commit repo status should be as fallows:

```console
$ git status
On branch master
nothing to commit, working tree clean
$
```


# History

History can be shown using:

```console
$ git log
commit 55c896e52f51a68e68be95bfaf5dec5e607f84dc (HEAD -> master)
Author: Michal Kolodziej <kolodziej.michal@geemail.com>
Date:   Tue Feb 16 23:37:11 2021 +0100

    Comments on viewing snapshots history.

commit 335faa0617e94467393971302be4b9f93198293f
Author: Michal Kolodziej <kolodziej.michal@gmail.com>
Date:   Tue Feb 16 22:23:12 2021 +0100

    In git.md added fundamental info about git version control system
    philosopy and primary commands:
    * add for marking changes (at stageing area)
    * rm mv for marking remove/move changes
    * status for repository changes in staged and unstaged areas
    * diff for viewing inside files changes
    
    In .gitignore swap files are marked to be ignored.
$
```

## Observing changes inside files (diff)

* `--patch` (`-p`) option causes to display diff output
* `-3` option causes to show 3 last snapshots

```console
$ git log --patch -3
```

## Observing status changes

```console
$ git log --stat
```

## Controlling what to show

Entries description can be formatted using

```console
$ git log --pretty=format:"format string"
$ # info about format can be found at
$ # PRETTY FORMATS section of man
$ # (search section using /PRETTY FORMATS)
$ # (last search can be repeated using / and Enter)
$ man git-log
```

## Branch and merge viewing (graph)

```console
$ git log --graph

## Snapshots filtering

Commits can be filtered using:
* `--author` - filters commits to those of particular author
* `--grep` - output containing something grepped
* `-S` - ("pickaxe option") filters to commits changing number
  of occuriences of specified string, for example:

	```console
	$ git log -S function_name
	```

## History for subdirectories

```console
$ git log -- path/to/search/within
```

## Other examples

```console
$ git log --pretty=oneline
$ git log --pretty=format:"%h %s" --graph --since=2.weeks


# Correctiong commits


## Correcting commit message + minor changes (amend (rebase))

To change commit message or content one may use
`git commit --amend` command.

```console
$ git status
$ # forgot something
$ git add git.md 
$ # same commit as before but with added forgotten content
$ git commit --amend 
```

## Unstage file (reset)

Following command removes file from stageing area
but it will not lost any changes:

```console
$ git add *
$ git reset HEAD not_intended_to_stage_file
```

## Delate changes (checkout)

To delate all changes made to file (from point of view of stageing):

```console
$ git checkout -- changes_is_staged_file_snapshot
```


# Remote repositiories


Remote repositiories can be configured 
by adding and naming, or implicitly by cloning:

```console
$ git remote add origin ssh://github.com/user3232/charging.git
$ git clone ssh://github.com/user3232/charging.git
```

Then they can be refered by name (e.g. "origin" for above example).


# Branching

Git encourages workflows that branch and merge often, 
even multiple times in a day.

When you make a commit, Git stores a commit object that contains
* a pointer to the snapshot of the content you staged. 
* This object also contains the author’s name and 
* email address, 
* the message that you typed, 
* and pointers to the commit or commits that directly came before this commit 
  (its parent or parents): 
	* zero parents for the initial commit, 
  * one parent for a normal commit, and 
  * multiple parents for a commit that results from a merge 
		of two or more branches.

## Object stored by git

* commit object - containing:
	* author,
	* committer and
	* tree object pointer
	* parent commit object pointer
* tree object - containing:
	* list of blob object pointer
* blob object - containing:
	* representation of file content


## Branch and names

Branch:

* A branch is just movable pointer to commit object.
* Branch in Git is actually a simple file that contains the 40 
	character SHA-1 checksum of the commit it points to, branches 
	are cheap to create and destroy. Creating a new branch is as 
	quick and simple as writing 41 bytes to a file (40 characters 
	and a newline).

Default branch:

* A default branch name in git is `master`
* A default branch name at github is (currently 2021) `main`
* `master` points to last commit
* Default branch does not differ from any other branch

Branch I'm currently on:

* in named HEAD
* HEAD is pointer to the local branch you’re currently on
* To display: `git log --oneline --decorate`
* To display history of particular branch e.g.: `git log testing`
* To display history of all branches: `git log --all`



## New branch

New branch creates a new pointer to the same commit you’re currently on.

```console
$ git commit -am "Start notes on branching."
[master 0d44ae3] Start notes on branching.
 1 file changed, 8 insertions(+), 4 deletions(-)
$ git status
On branch master
nothing to commit, working tree clean
$ git branch testing
```

## Switching Branches

To switch to an existing branch, you run the git checkout command. 
Let’s switch to the new testing branch:

```console
$ git checkout testing
M	git.md
Switched to branch 'testing'
```

Now lets commit in new branch:

```console
$ git commit --all --message "First commit in testing branch."
[testing 975c535] First commit in testing branch.
 1 file changed, 86 insertions(+), 1 deletion(-)
$
```


Lets switch bask to master branch:
* this will make HEAD to point to master
* **and also revert all files to master version**.


```
$ git checkout master
error: Your local changes to the following files would be overwritten by checkout:
	git.md
Please commit your changes or stash them before you switch branches.
Aborting
$ git commit --all --amend
[testing feaf387] First commit in testing branch.
 Date: Thu Feb 18 16:50:08 2021 +0100
 1 file changed, 130 insertions(+), 1 deletion(-)
$ git checkout master
Switched to branch 'master'
$ 
```

## Divergent branches

Any changes to master branch after changes in testing branch
will cause file history to be divergent.:

```console
$ git commit --all --message "Changes in master causing history diversion"
[master e21ddc6] Changes in master causing history diversion
 1 file changed, 162 insertions(+), 1 deletion(-)
$ 
```


To see divergence in history:

```console
$ git log --oneline --decorate --graph --all
* e21ddc6 (HEAD -> master) Changes in master causing history diversion
| * feaf387 (testing) First commit in testing branch.
|/  
* 0d44ae3 Start notes on branching.
* 04d22c8 Comments on viewing snapshots history. And modification of last snap (amend).
...
$
```


# Merging branches

So we are at master branch, and wish to merge
testing branch. We just do it:

```console
$ git merge testing
Auto-merging git.md
CONFLICT (zawartość): Merge conflict in git.md
Automatic merge failed; fix conflicts and then commit the result.
$ # can not automatically merge
$ # lets see what is going on:
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   git.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Now `merge ...` causes conflicted files to be added
to MERGE_HEAD as one file with tagged differences,
for example conflicted file will now contain:

```

common content...

<<<<<<< HEAD
content in head...
=======
content in testing...
>>>>>>> testing


common content...



<<<<<<< HEAD
another content in head...
=======
another content in testing...
>>>>>>> testing


common content...

```

Above file can be marked as resolved by:

* removing all tags (`<<<<<<<`, `=======`, `>>>>>>>`)
	leaving file with desired content,
* saving this file,
* and staging it by: `git add confilict_free_file`.


State of merging can be checked by `git status` command.

After above steps, commit will cause merge.
Usually merged branch will not be useful in future,
so it can be delated (it is just pointer to commit,
so removing this pointer will cause no important
information loss).

Continuing example:

```console
$ # remove tags leaving desired content
$ vim git.md
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

$ # resolved git.md was automaticaly added to staging area
$ # but it can be done more times:
$ git add git.md
$ git commit 
[master 5da1aac] Merge branch 'testing'
$ # testing branch wont be needed so delete it:
$ git branch -d testing
Deleted branch testing (was feaf387).
$
```


## Using `git mergetool`

Every conflict resolving process will end with
file containing desired content. So normal file
edit is ok, but there are also diff/merge tools
which "helps" with this.

[`git mergetool`](https://www.git-scm.com/docs/git-mergetool)
command will call (configured) 
diff/merge program and after this program termination
it will use this program output file and
add this file to staging area.

On Ubuntu one of such popular diff/merge GUI program
is Meld.

```console
$ sudo apt-get install meld
....
$ # lets call meld:
$ # git mergetool -t meld
$ # or select tool interactively:
$ git mergetool

This message is displayed because 'merge.tool' is not configured.
See 'git mergetool --tool-help' or 'git help config' for more details.
'git mergetool' will now attempt to use one of the following tools:
meld opendiff kdiff3 tkdiff xxdiff tortoisemerge gvimdiff diffuse diffmerge ecmerge p4merge araxis bc codecompare emerge vimdiff
Merging:
git.md

Normal merge conflict for 'git.md':
  {local}: modified file
  {remote}: modified file
Hit return to start merge resolution tool (meld): 
$ # default configuration specifies (un)fortuately
$ # to leave intermediate files :
$ git status 
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	git.md.orig
	git_BACKUP_20811.md
	git_BASE_20811.md
	git_LOCAL_20811.md
	git_REMOTE_20811.md

$ # this can be repaired by removing those files
$ # after successful merge:
$ git clean -i
Would remove the following items:
  git.md.orig          git_BASE_20811.md    git_REMOTE_20811.md
  git_BACKUP_20811.md  git_LOCAL_20811.md
*** Commands ***
    1: clean                2: filter by pattern    3: select by numbers
    4: ask each             5: quit                 6: help
What now> c
Removing git.md.orig
Removing git_BACKUP_20811.md
Removing git_BASE_20811.md
Removing git_LOCAL_20811.md
Removing git_REMOTE_20811.md
$ # another possibility is to configure above to be automatic:
$ git config --global mergetool.keepBackup false
$ # also choice of tool can be configured, e.g. for meld:
$ git config --global merge.tool meld
$
```


# Branch management


Examples:

```console
$ # what branches exists?
$ git branch 
* master
$ # new branch
$ git branch other
$ git branch 
* master
  other
$ # list all branches last commit
$ git branch -v
* master 5da1aac Merge branch 'testing'
  other  5da1aac Merge branch 'testing'
$ git branch --merged
* master
  other
$ # branches merged with current branch
$ git branch --merged
* master
  other
$ # rename branch (only locally)
$ git branch --move other experiment
$ # show not only local info (but also remotes
$ # which this repo lacks at this time)
$ git branch --all
  experiment
* master
$ 
```


# Synchronizing with remotes

## To have local version having remote version

Use command: `git fetch <remote>`

Example

```console
$ git checkout master
$ # this will make origin/master actual
$ git fetch origin
```


## To have remote version having local version

Use command: 

* `git push <to_remote> <local_branch>`, or
* `git push <to_remote> <local_branch_name> <corresponding_remote_branch_name`


Following commands are equivallent:

```console
$ git push origin serverfix
$ git push origin refs/heads/serverfix:refs/heads/serverfix
$ git push origin serverfix:serverfix
```

## To track remote branch locally

```console
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
$
```

Above:

* This gives you a local branch that you can work on that 
	starts where origin/serverfix is.
* Checking out a local branch from a remote-tracking branch 
	* automatically creates what is called a “tracking branch” 
	* (and the branch it tracks is called an “upstream branch”).
* If you’re on a tracking branch and type `git pull`, 
	Git automatically knows which server to fetch from 
	and which branch to merge in.
* When you clone a repository, it generally automatically 
	creates a `master` branch that tracks `origin/master`.

Tracking remote branches is very common task, there exists
shortcuts, following commands are equivalent:

```console
$ git checkout -b serverfix origin/serverfix
$ git checkout --track origin/serverfix
$ git checkout serverfix
```

## What is tracked? And tracking when having existing branches

```
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

```
$ git branch --set-upstream-to origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

To see what tracking branches you have set up:

```console
$ git fetch --all; git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] Add forgotten brackets
  master    1ae2a45 [origin/master] Deploy index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] Should do it
  testing   5ea463a Try something new
$
```

## Deleting Remote Branches

```console
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```

# Rebasing

Rebasing chenges but simplifies history.
It works as well as merge if:
* no one references that will be rebased
* which implays that rebase is ok when you work 
  on something privately.


**With the rebase command, you can take all the changes 
that were committed on one branch and replay 
them on a different branch.**

Examples:

```console
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
$ git checkout master
$ git merge experiment
```

## Rebase When You Rebase

When you rebase stuff, you’re abandoning existing commits 
and creating new ones that are similar but different. 
If you push commits somewhere and others pull them down 
and base work on them, and then you rewrite those commits 
with git rebase and push them up again, your collaborators 
will have to re-merge their work and things will get messy 
when you try to pull their work back into yours.




It turns out that in addition to the commit SHA-1 checksum, 
Git also calculates a checksum that is based just on the 
patch introduced with the commit. This is called a “patch-id”.

If you pull down work that was rewritten and rebase it on top 
of the new commits from your partner, Git can often successfully 
figure out what is uniquely yours and apply them back on top 
of the new branch.


Example:

```console
$ git fetch
$ git rebase teamone/master
```

or (almost) equivalently:

```console
$ git pull --rebase


# Tagging

Creates (constant?) branch.

```console
$ # Annotated Tags:
$ git tag -a v0.5 -m "Initial beta version 0.5"
$ # list existing tags:
$ git tag
v0.5
$ # delete last tag
$ git tag -d v0.5
$ # Lightweight Tags (does not add annotations)
$ # tag object only have name and pointer to commit
$ # -a (annotate) -m (message) can not be used
$ git tag v0.5
$ # push tag to remote (same as with branch):
$ git push origin v0.5
$ # push all tags to remote:
$ git push --tags
$ # viewing files of tag (same as branch):
$ git checkout v0.5
Commits here can be done but eventualy must end
in other branch name...
```


# GitHub CLI s

GitHub can be accessed by CLI tools:

* (official) [GitHub CLI](https://github.com/cli/cli) (`$ gh`) 

  * [manual](https://cli.github.com/manual/),

	* [Linux install instructions](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)

* (git CLI extension) [hub](https://github.com/github/hub) (`$ hub`)

	* [examples](https://hub.github.com/#developer),

	* [manual](https://hub.github.com/hub.1.html), probably `$ man hub` will
    also work.









