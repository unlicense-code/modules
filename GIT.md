# GIT Modules


## posix - Add Empty Commit before existing commits
preprends a empty Root-Commit with the Message "The Unlicense"
All Repos with the same Message for the Genesis will be related.
you can repeat that to make many repos compatible and clean the history incremental.

git 2.34 defaults to create a Root-Commit branch when you do

```
git commit --allow-empty -m 'The Unlicense'
```

```shell
OLDBRANCH=$(git branch --show-current)
OLDBASE=$(git rev-list --max-parents=0 HEAD)

git checkout --orphan newbranch && git rm -rf .
GENESIS="The Unlicense"
REPO_INIT="git init && git commit --allow-empty -m \"${GENESIS}\""

# deprecated. 2.34+
echo "${OLDBASE} $(git rev-list HEAD)" > .git/info/grafts

git checkout $OLDBRANCH
git filter-branch

# NOTE: For all additional branches in process
# git-filter-branch -f <BRANCH>
rm .git/info/grafts
```

the end result is an empty base commit that can serve as the <start-point> 
for any new Branch with "The Unlicense" as Inital Commit Message. Root-Commit
```
git checkout -b init $(git rev-list --max-parents=0 HEAD)
git checkout -b from_init init
```

In the future always create new repositories like this:
```
git init && git commit --allow-empty -m 'The Unlicense'
```
so that the first commit is empty, and always available for starting a new independent branch.
