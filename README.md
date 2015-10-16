# Git Repository Join

Combines separate repositories into a single monolithic repository. It only includes master branches from each repository, but complete version history is maintained. Mainly just a convenient wrapper around git commands.

## Usage

Run this command from inside of an **empty** git repository - this can either be a `git clone` from github or a local `git init`.

`git-join-repos datafile.txt`

where datafile.txt is in the format:

```
repo1 git@github.com:username/repo1.git
newname git@github.com:username/repo2.git
repo3 /path/locally/repo3
```

each row is separated by a newline and the name and url are separated by a space character (tabs not supported!).

## Expected Result

Your new TLD working directory will contain:

```
repo1
newname
repo3
```

Where each subfolder contains the HEAD master commit from the corresponding source repository. n.b. old commit ids are lost in this process - so don't count on being able to reference your new repository with old commit ids.

Each commit message will be retained, but prepended with the name of the source repository.

`git log --oneline --graph --decorate`

```
*   c91baa3 (HEAD -> master, origin/master) repo3: git-join-repos monolithic conversion merge
|\  
| * 106a74c repo3: add keyed message storage
| * c48e9f2 repo3: add build/.npmignore
| * 2766e11 repo3: fix build/.gitignore
| * dcef023 repo3: initial commit of repo3
*   b7530e7 newname: git-join-repos monolithic conversion merge
|\  
| * 02f3536 newname: update sublibrary to versionf 1.4
...
```
