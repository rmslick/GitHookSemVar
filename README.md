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

1. pre-commit - Pre-commit hook will sync all commit's made on the develop branch with the remote develop branch and make the appropriate change to the version based on the most recent merge into develop. If pre-push fails with a version mismatch error, run ```git commit -m "Any message"``` to sync version and successfully push.
3. pre-commit - Prevents any commit from being made if branch name is not in convenction listed above. 
4. pre-push - Final check which verifies the integrity of the version.txt file. For mismatch errors in push, see (2) above.


### Testing
1. Clone repo
2. Run: ```python3 buildscript.py``` to configure hooks in hooks/ to hooks in .git/hooks```
3. Create a branch. Using naming conventions to test versioning or arbitrary name to test branching pre-commit check on non-conforming branch names.
4. Commit change from branch, merge into develop.
5. Add, commit and push changed to origin/develop
6. Verify Maj.Min.Patch number changed 
Example:
```bash
git checkout -b feature-myfeature
echo SomeCode > bigimportantlib.cpp
git add .
git commit -m "My big important feature"
git checkout develop
git merge feature-myfeature
git add .
# Note change to local version.txt before and after next step
git commit -m "Merged feature from my feature" 
# if error then remote version changed between commit above. Commit again and retry
git push origin develop 
```
