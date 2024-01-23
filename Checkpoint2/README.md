### Checkpoint2 Submission

- **COURSE INFORMATION: CSP451NIA
- **STUDENT’S NAME: Thuan Le
- **STUDENT'S NUMBER: 125180182
- **GITHUB USER ID: 125180182-myseneca
- **TEACHER’S NAME: Atoosa Nasiri

----
### Table of Contents
1. [Part A - Adding Files - Local Repo Workflow](#Part-A)
2. [Part B - Inspecting Local Repo with `git status` and `git log`](#Part-B)
3. [Part C - Creating & Merging Branches](#Part-C)
4. [Part D - Git Branching Strategy Review Question](#Part-D)
----

### Part A

#### Untracked
[git_status_untracked.txt](git_status_untracked.txt)
```bash
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	Checkpoint2/

nothing added to commit but untracked files present (use "git add" to track)
```
![git-untracked](images/untracked.png)

#### Uncommitted
[git_status_uncommitted.txt](git_status_uncommitted.txt)
```bash
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   Checkpoint2/README.md
	new file:   Checkpoint2/git_status_untracked.txt
	new file:   Checkpoint2/untracked.png

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	Checkpoint2/committed.png
	Checkpoint2/git_status_uncommitted.txt
```
![git-committed](images/uncommitted.png)

#### Committed
[git_status_committed.txt](git_status_committed.txt)
```bash
On branch main
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	git_status_committed.txt

nothing added to commit but untracked files present (use "git add" to track)
```
![git-uncommitted](images/committed.png)


#### Git log -n 5
![git-log-n-5](images/git-log-n-5.png)

--------

### Part B

<p>These two commands differ in function. "Git Status" tells us if the branch is up to date, of files that are untracked, staged, or unstaged, files that are modified, created or deleted, and whether if there's anything to commit. For an example if you created a file, the file will be mentioned in the output of Git Status as an untracked file and that it can be added(staged).

"Git Log" on the other hand gives you an output of what commits has been done on the repository. For example where you are dealing with a project, you can use the "Git Log" command to check the commit history of that project. This allows you to see the changes and when it was made which you or the collaborator(s) have made on the project. </p>

### Part C

![BranchPush](images/branchpush.png)
![Branches](images/branches.png)

[git_log_output.txt](git_log_output.txt)
```bash
commit 15fcf874ccb482b5510c1c9cc5401c6f2704e908
Author: Thuan Le <tle53@myseneca.ca>
Date:   Tue Jan 23 14:47:36 2024 -0500

    Added images

commit 21657171226f5ef232d5e6b81b743df12d23ff56
Author: Thuan Le <tle53@myseneca.ca>
Date:   Tue Jan 23 14:28:50 2024 -0500

    adds emojis to feat-emojis branch

commit bf3a7f33aaf20ba7c6f2e94bf4991e1e6bc4099f
Author: Thuan Le <tle53@myseneca.ca>
Date:   Tue Jan 23 14:26:15 2024 -0500

    adds footnotes folder

commit 47a702843b9ce54adcbd1d388df7d674db46cf04
Author: Thuan Le <tle53@myseneca.ca>
Date:   Tue Jan 23 14:22:12 2024 -0500

    Added Tables of Contents and Part A

commit aca2c561b80fb76d611133ec4d231edbef51233c
Author: Thuan Le <tle53@myseneca.ca>
Date:   Tue Jan 23 14:15:32 2024 -0500

    added git_status_committed.txt and img
```
![Gitlog-n5](images/gitlog-n.png)

### Part D

1. What are the differences between develop branch and main branch?

    <p> The difference between the develop branch and the main branch is that the main branch contains the source code which is intended to be deployed to production. The develop branch is meant for developers. Developers will branch off the develop branch to create updates and features which they then merge into the develop branch.</p>

2. What are the three supporting branches? Briefly describe the function of each of these supporting branches.

    <p> The three supporting branches are:</p>

    ### Feature/Bugfix branches 
     1. The Feature/Bugfix branch is used by developers to create new features or to fix bugs. This branch branches off the develop branch. When using this branch, it is important to verify the commits to the develop branch as there are other collaborators. If there are commits to the develop branch, the changes of develop and the feature branch are required to merge, meaning the feature branch needs to be up to date with the develop branch before merging back with the develop branch.

    ### Hotfix branches
     2. The Hotfix branch is utilized for immediate improvement or changes to a production version which went wrong. It branches from the master branch so it doesn't affect the develop branch. This branch is necessary because at the time when a situation occurs and a hotfix is needed, the develop branch does not represent production as it may have commits from developers.

    ### Release Branches   
     3. The Release branch is used for preparation before release. It is used when the features and bug fixes that are planned for the release is completed and merged with the develop branch.
     
     
3. What are the best practices in working with release branches?

    <p>The best practices in working with release branches is that it should always be publicly available to other collaborators, must be branched from develop, restricted to only merge into master and develop, and it requires a naming convention that follows semantic versioning.</p>

