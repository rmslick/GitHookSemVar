# A system for automated semantic versioning based on branch name

## Semantic versioning rules
### Adapted from [SemVar Official](https://semver.org) 
1. MAJOR version when you make incompatible API changes,
2. MINOR version when you add functionality in a backwards compatible manner, and
3. PATCH version when you make backwards compatible bug fixes.

### Patch Rule
Patch version Z (x.y.Z | x > 0) MUST be incremented if only backwards compatible bug fixes are introduced. A bug fix is defined as an internal change that fixes incorrect behavior.

### Minor Rule
Minor version Y (x.Y.z | x > 0) MUST be incremented if new, backwards compatible functionality is introduced
### Major Rule
MUST be incremented if any backwards incompatible changes are introduced to the public API. It MAY also include minor and patch level changes. 

**NOTE:** Patch and minor version are reset to 0 when major version is incremented.
### Branch Types
All branch names must follow the follwing convention [BranchType]-[DescriptiveName]. BranchType's not conforming to the naming policy will be blocked from making a commit. BranchType's are listed below and correspong to the rules of semantic versioning. Decriptive name should describe the content assosciated with the branch type.

**NOTE:** Maj.Min.Fix Version numbers will not be changed until after a successful merge. Thus changes may be made to the branch names prior to the merge commit. 

1. realease-* : A release-* branch name will increment the MAJOR number
2. feature-* : Added functionality that has backward compatibility 
3. patch-* : Backward compatible bug fix in end-user code
4. private-* : If changes to the private code do not provide new functionality for the user and have backward compatibility they may be marked private. If a branch is of type private then it will have no change on the version number.
### Git Hooks
Located in the hooks folder there are three hooks: pre-merge-commit, pre-commit and post-merge.

1. pre-merge-commit - Prevents overwriting version.txt while merging into develop. When merging develop into master, version.txt is merged from develop into master.
2. pre-commit - Prevents any commit from being made if branch name is not in convenction listed above. 
3. post-merge - Invoked after successful merge of dev branch into develop. Will update the maj.min.patch version based on dev branch name.

**NOTE:** Post merge leaves uncommitted changes after the merge of a dev branch into develop.  This leaves a final check for the version change which can then be committed before merge into master.

### Usage
1. Clone repo
2. Run: ```bash python3 buildscript.py to configure hooks in hooks/ to hooks in .git/hooks```
3. To see example branches, run: ```git branch -a```
4. Take note of version in version.txt on develop.
5. Checkout dev branch, make change, commit, merge into develop. Verify appropriate change to version.txt.
Example:
```bash
git checkout feature-testing
echo "Myfeaturechange" > bigimportantlib.cpp
git add .
git commit -m "big important feature"
git checkout develop
git merge feature-testing
git add .
git commit "Bumped version"
git checkout master
git merge develop
```