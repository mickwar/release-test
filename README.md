# refer to [git flow](https://nvie.com/posts/a-successful-git-branching-model/)

# commands for release-test
git clone git@github.com:mickwar/release-test.git
cd release-test
touch file
git add .
git commit -m 'initial commit'
git push

git checkout -b dev
git push -u origin dev 

echo line1 >> file
git add .
git commit -m 'add line'
git push

## via release branch
git checkout -b release-1
# do things and commit changes

git checkout master
git merge --no-ff release-1
git tag -a 1.0 -m 'first release'
git push

git checkout dev
git merge --no-ff release-1 # everything up-to-date

git branch -d release-1

# need to push the tag
# makes the release visible, no verified showing up
git push origin 1.0
# also: `git push origin --tags` to push all tags


## doing the merge through a pull request on github
git checkout dev
echo 'for second release' >> file
git add .
git commit -m 'r2'
git push

# merge dev into branch
# on github, go to pull requests -> new pull request -> Change to "base: master <- compare: dev"
# then click "Create pull request"
# (or click "Compare & pull request")
# (or change base branch to "Branch: dev" and click "New pull request")
# (Note, github uses `git merge --no-ff` by default)

# go to Releases
# click Draft New Release
# use tag 1.1 @ Target: master
# title: second release
# description: notice how the dev branch is now behind the master branch
# publish the release

# this release now has the "Latest release" label next to it along with "Verified"

# pull the changes
git checkout master
git pull

# tag 1.1 is now showing
git tag

# nothing shows up for
git diff dev
# yet dev is behind master by two commits (see `git log`), one for each of the --no-ff merges

# can bring dev up-to-date with master with
git checkout dev
git merge master    # this gets the two commits from the merges into master
git push

# you could also delete the dev branch and just start again
git checkout master
git branch -d dev
git checkout -b dev
# but this might not be reasonable if dev is a branch many people are pulling from

# in either case, the dev branch is now equivalent to the master branch
