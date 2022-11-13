# GIT Modules


## posix - Add Empty Commit before existing commits
All Repos with the same Message for the Genesis will be related.
you can repeat that to make many repos compatible and clean the history incremental.

git 2.34 defaults to create a Root-Commit branch when you do

```
git commit --allow-empty -m 'The Unlicense'
```

```shell
OLDBRANCH=$(git branch --show-current)

git checkout --orphan newbranch && git rm -rf .
GENESIS="The Unlicense"
REPO_INIT="git init && git commit --allow-empty -m 'The Unlicense'"
# deprecated.
echo "$(git rev-list --max-parents=0 HEAD) $(git rev-list HEAD)" > .git/info/grafts

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
