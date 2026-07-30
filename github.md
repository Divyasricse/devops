**1.what is git**
Git is distributed version control system (DVCS) that helps to track code changes ,collaborate with teams,roll back to older versions and manage branching/merging

<img width="379" height="361" alt="image" src="https://github.com/user-attachments/assets/4816fdae-b708-4d23-b792-0a242d90658d" />

**2.what is repository**
A repository is a project directory managed by git,containing code,commit history and branches
#local repository ->hosted on ur machine(git init)
#Remote repository ->Hosted on github for team collaboration

**3.what is commit in git**
A commit is a cryptographic snapshot of your project files at a specific point in time,acting as a permanent save point in ur repository 's history

**4.what is a merge conflict?how do you solve it?**


git commit -am 'msg' as a shotcut for tracked files

**5/Diff btn git fetch and git pull**
git fetches only downloads remote changes without modifying your local working files,
git pull downloads those changes and immediately merges them into ur active local branch->fetch+merge into local

**6.what is gitstash

**7what is .gitignore**
a file that lists files/folders git should ignore(eg:logs,build files)

**8.what is difference btw fork and clone?**
fork ->copies repository to ur github account
clone->copies repository to ur local machine

**9.What is pr(pull request)?**
a pr is a request to merge code into another branch on github.it allows code review,discussion and approval before merging.


## Branching Strategy
### 1. Main Branch
The **Main Branch** always contains the production-ready code. Whatever code is currently running in the production environment should be available in the main branch.
### 2. Feature Branch
A **Feature Branch** is created whenever a new feature needs to be implemented or a bug needs to be fixed. All new development changes are done in the feature branch.
### 3. Snapshot Branch
A **Snapshot Branch** is created with a naming convention that includes the month, date, and version number. It is used for testing in the development environment. The code is copied from the feature branch into the snapshot branch for validation.
### 4. Release Branch
A **Release Branch** is similar to a snapshot branch but follows a naming convention that ends with `_rc` (Release Candidate), for example: `Jul29_2026_v1.0_rc`.
This branch is used for:

* Deployment to the testing environment.
* Performance testing.
* End-to-end validation and other testing activities.

Once all tests pass successfully, the release is deployed to production. The release then goes through a validation period (typically around 3 days). If no issues are identified during this period, the changes are merged into the **Main Branch**.
### 5. Hotfix Branch
If any issues are discovered during the validation period and can be resolved quickly, a **Hotfix Branch** is created to implement and deploy the fix.
If the issue cannot be resolved within the validation period:

* Revert the changes in production.
* Create a new feature branch.
* Implement the required fixes.
* Restart the release process.

***

## Important: Release Process

1. **Backup** – Take a backup of the existing production code before deployment.
2. **Deployment** – Deploy the new release to production.
3. **Fallback/Rollback Plan** – Have a rollback strategy ready to restore the previous version if any critical issues occur after deployment.
