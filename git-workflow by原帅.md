Git workflow </br>
</br>![](https://www.atlassian.com/dam/jcr:25d06843-2468-4e00-8ae7-11d4164f8995/hero.svg)
====
by 原帅 516030910222
# 1. what is git workflow
 A Git Workflow is a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. Git workflows encourage users to leverage Git effectively and consistently. Git offers a lot of flexibility in how users manage changes. Given Git's focus on flexibility, there is no standardized process on how to interact with Git. When working with a team on a Git managed project, it’s important to make sure the team is all in agreement on how the flow of changes will be applied. To ensure the team is on the same page, an agreed upon Git workflow should be developed or selected.

 # 2. why use it
 An appropriate workflow serves as a good pattern to manage development. It makes clear rules to deal with version control, merge conflicts.For teamwork, it makes development more effective, manageable and consistent.

 # 3.how to use it
 There are several  popular patterns as follows :

 ## 1.Centralized Workflow </br></br>
![](https://www.atlassian.com/dam/jcr:0869c664-5bc1-4bf2-bef0-12f3814b3187/01.svg)

The Centralized Workflow uses a central repository to serve as the single point-of-entry for all changes to the project. The default development branch is called master and all changes are committed into this branch. This workflow doesn’t require any other branches besides master.  
#### Initialize
 First, someone needs to create the central repository on a server, which should always be bare repositories (they shouldn’t have a working directory). It can be created as follows:
```
ssh user@host git init --bare /path/to/repo.git
```
  Be sure to use a valid SSH username for user, the domain or IP address of your server for host, and the location where you'd like to store your repo for /path/to/repo.git. Note that the .git extension is conventionally appended to the repository name to indicate that it’s a bare repository.

#### Clone the central repository
Next, each developer creates a local copy of the entire project. This is accomplished via the git clone command:
```
git clone ssh://user@host/path/to/repo.git
```
When you clone a repository, Git automatically adds a shortcut called origin that points back to the “parent” repository, under the assumption that you'll want to interact with it further on down the road. 
#### Make changes and commit
Once the repository is cloned locally, a developer can make changes using the standard Git commit process: edit, stage, and commit. If you’re not familiar with the staging area, it’s a way to prepare a commit without having to include every change in the working directory. This lets you create highly focused commits, even if you’ve made a lot of local changes.
```
git status # View the state of the repo
git add <some-file> # Stage a file
git commit # Commit a file</some-file>
```
#### Push new commits to central repository
Once the local repository has new changes committed. These change will need to be pushed to share with other developers on the project.
```
git push origin master
```
#### Managing conflicts
The central repository represents the official project, so its commit history should be treated as sacred and immutable. If a developer’s local commits diverge from the central repository, Git will refuse to push their changes because this would overwrite official commits.
![](https://www.atlassian.com/dam/jcr:5165668f-b62d-4417-95e6-fde8ed97ec60/11.svg)
Before the developer can publish their feature, they need to fetch the updated central commits and rebase their changes on top of them. 

## 2. Git Feature Branch Workflow
![](http://img.blog.csdn.net/20151230174528657)

The Git Feature Branch Workflow is a composable workflow that can be leveraged by other high-level Git workflows.The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the master branch. It also means the master branch will never contain broken code, which is a huge advantage for continuous integration environments.   

Aside from isolating feature development, branches make it possible to discuss changes via pull requests. Once someone completes a feature, they don’t immediately merge it into master. Instead, they push the feature branch to the central server and file a pull request asking to merge their additions into master. This gives other developers an opportunity to review the changes before they become a part of the main codebase.
#### Start with the master branch
All feature branches are created off the latest code state of a project. This guide assumes this is maintained and updated in the master branch.
```
git checkout master
git fetch origin
git reset --hard origin/master
```
This switches the repo to the master branch, pulls the latest commits and resets the repo's local copy of master to match the latest version.
#### Create a new-branch
Use a separate branch for each feature or issue you work on. After creating a branch, check it out locally so that any changes you make will be on that branch.
```
git checkout -b new-feature
```
This checks out a branch called new-feature based on master, and the -b flag tells Git to create the branch if it doesn’t already exist.
#### Update, add, commit, and push changes
On this branch, edit, stage, and commit changes in the usual fashion, building up the feature with as many commits as necessary. Work on the feature and make commits like you would any time you use Git. When ready, push your commits, updating the feature branch on Bitbucket.
```
git status
git add <some-file>
```
#### Push feature branch to remote
It’s a good idea to push the feature branch up to the central repository. This serves as a convenient backup, when collaborating with other developers, this would give them access to view commits to the new branch.
```
git push -u origin new-feature
```
#### Merge your pull request
Before you merge, you may have to resolve merge conflicts if others have made changes to the repo. When your pull request is approved and conflict-free, you can add your code to the master branch. 

## 3.Gitflow Workflow
![](https://www.atlassian.com/dam/jcr:2bef0bef-22bc-4485-94b9-a9422f70f11c/02%20(2).svg)
Gitflow is ideally suited for projects that have a scheduled release cycle. It assigns very specific roles to different branches and defines how and when they should interact. In addition to feature branches, it uses individual branches for preparing, maintaining, and recording releases. 
#### Feature Branches
Each new feature should reside in its own branch, which can be pushed to the central repository for backup/collaboration. But, instead of branching off of master, feature branches use develop as their parent branch. When a feature is complete, it gets merged back into develop. Features should never interact directly with master.
![](https://www.atlassian.com/dam/jcr:b5259cce-6245-49f2-b89b-9871f9ee3fa4/03%20(2).svg)

#### Release Branches
Once develop has acquired enough features for a release (or a predetermined release date is approaching), you fork a release branch off of develop. Creating this branch starts the next release cycle, so no new features can be added after this point—only bug fixes, documentation generation, and other release-oriented tasks should go in this branch. Once it's ready to ship, the release branch gets merged into master and tagged with a version number. In addition, it should be merged back into develop, which may have progressed since the release was initiated.
![](https://www.atlassian.com/dam/jcr:a9cea7b7-23c3-41a7-a4e0-affa053d9ea7/04%20(1).svg)
 
 #### Hotfix Branches
 Maintenance or “hotfix” branches are used to quickly patch production releases. Hotfix branches are a lot like release branches and feature branches except they're based on master instead of develop. This is the only branch that should fork directly off of master. As soon as the fix is complete, it should be merged into both master and develop (or the current release branch), and master should be tagged with an updated version number.
 ![](https://www.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%20(2).svg)

---
### Main resouces from:
[https://www.atlassian.com/git/tutorials/comparing-workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)



