### Checkpoint2 Submission

- **COURSE INFORMATION: CSP451NIA
- **STUDENT’S NAME: Thuan Le
- **STUDENT'S NUMBER: 125180182
- **GITHUB USER ID: 125180182-myseneca
- **TEACHER’S NAME: Atoosa Nasiri

----

### Questions
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

The best practices in working with release branches is that it should always be publicly available to other collaborators, must be branched from develop, restricted to only merge into master and develop, and it requires a naming convention that follows semantic versioning.


