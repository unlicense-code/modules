# GIT Modules


posix - Add Empty Commit before existing commits
```shell

OLDBASE=$(git rev-list --max-parents=0 HEAD)

# create a new orphan branch and switch to it
git checkout --orphan newbranch
git rm -rf .
# All Repos with the same Message for the Genesis will be related.
# you can repeat that to make many repos compatible and clean the history incremental.
REPO_INIT="git init && git commit --allow-empty -m 'First Commit: Genesis'"
git commit --allow-empty -m "${REPO_INIT}"
NEWBASE=$(git rev-list HEAD)

OLDBRANCH="master"
echo "$OLDBASE $NEWBASE" > .git/info/grafts
git checkout $OLDBRANCH
git filter-branch

# NOTE: For all additional branches in process
# git-filter-branch -f <BRANCH>


rm .git/info/grafts
```

Although this procedure is a bit involved, the end result is an empty base commit that can serve as the <start-point> 
for any new "clean-slate branch"; all I'd need to do then is

```
git checkout -b init $(git rev-list --max-parents=0 HEAD)
git checkout -b from_init init
```

In the future always create new repositories like this:

git init && git commit --allow-empty -m 'The Unlicense'

so that the first commit is empty, and always available for starting a new independent branch.
