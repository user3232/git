<!-- 
vim: set tabstop=2:
-->
<!--
to view this in console:
$ pandoc -t plain git.md
-->

% Git notes

# Key things

* git repository is always created locally
* it can be accessed remotely if created on server

# Global configuration

To list/edit user configuration:

```console
$ git config --list --show-origin
file:/home/mk/.gitconfig        user.email=kolodziej.michal@gmail.com
file:/home/mk/.gitconfig        user.name=Michal Kolodziej
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









