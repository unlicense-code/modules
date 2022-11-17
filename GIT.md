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
  
## Updated grafts method

Say you have two git branchs you want to graft:

(a)-(b)-(c) (d)-(e)-(f)

(d) be parent of (c). replacement for (c) with (c1). each ref is a commit hash.

To create the new commit:
```
git checkout d && git rm -rf . 
# create replacement commit with original info
c1=$(git checkout c -- . && git commit -a -C c)
git replace c c1
```
(c1) which has the correct parent (d).  replace the existing (c) with (c1):
Now your history looks like this:
(a)-(b)-(c1)-(d)-(e)-(f)
