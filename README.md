# Git Repository Join

Combines separate git repositories into a single monolithic git repository, prepending the source repo name to every commit message. It only includes one selectable branch ('master' by default) from each repository, but otherwise complete version history is maintained. Mainly just a convenient bash wrapper around git commands.

## Usage

Run the script from inside an **empty** git repository, for example:

```bash
#Describe the source repositories, see below for file format
$EDITOR repo_data.txt

#make an empty repository
git init $repo_name
cd $repo_name

#Run the merge/join tool
~/path/to/git-join-repos ../repo_data.txt

#Add the monolithic remote, this is just an example, plug in your actual remote:
#Be sure to run this step *after* the join tool.
git remote add origin git@github.com:username/monolithic.git

#Push it when you're ready
git push origin master
```

where repo_data.txt is in the format:

```
repo1 git@github.com:username/repo1.git
newname git@github.com:username/repo2.git
repo3 /path/locally/repo3
repo4 /path/locally/repo3 custom-branch
```

each row is separated by a newline and contains the name, the url and optionally a branch name separated by a space character (tabs not supported!).

## Expected Result

Your new top-level working directory will contain:

```
repo1
newname
repo3
repo4
```

Where each subfolder contains the HEAD commit of the selected branch from the corresponding source repository. n.b. old commit ids are lost in this process - so don't count on being able to reference your new repository with old commit ids.

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

## Contributions

Pull requests are welcome
