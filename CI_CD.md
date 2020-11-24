# CI/ CD 
## 1 ***PIPELINE DESCRIPTION***

### **Release schedule variations**
Once in 2-3 month if there are big features.
Once in 1-2 week if it is small features.

### **Enviroments**
Local, CI, Integration, QA, Staging, Prod.
![stages](img/stages.png)
### **Quality gates, testing**
Checking:
- Is the code fully usable for all use cases described in the code requirements
- Do the new automated tests cover the added code enough
- Does the code meet the requirements of the existing styling guidelines
- Working with DB
- Security

Used testing types: unit testing, integration testing, smoke testing, gui testing.
Test coverage must be more than 50% and near 90% for unit tests
### **Tools for CI/CD**
Source version control - github

CI server - Jenkins

code quality checking - SonarQube

documentation - swagger

## 2 ***Branching strategy***
**ONE FLOW** branching strategy is used.
OneFlow’s branching model is exactly as powerful as GitFlow’s. There is not a single thing that can be done using GitFlow that can’t be achieved (in a simpler way) with OneFlow. The description below goes into more detail.
Maintaining a single long-lived branch simplifies the versioning scheme and day-to-day operations that developers have to perform considerably.
The main condition that needs to be satisfied in order to use OneFlow for a project is that every new production release is based on the previous release (GitFlow has exactly the same requirement). 
It also makes the project history cleaner and more readable, and thus more useful.
The main branch
Like was explained before, the workflow uses only one eternal branch. The name doesn’t matter, and can be anything you want. We will use ```master``` in this description, as it’s probably the most common name.

Feature branches (also sometimes called topic branches) are where the day-to-day development work happens – hence, they are by far the most common of all the support branches. They are used to develop new features and bugfixes for the upcoming release. They are usually named similarly to ```feature/my-feature.```

Release branches are created to prepare the software for being released. Obviously, what exactly that means varies on a project-per-project basis. This could be as simple as bumping the version number in the configuration, or involve things like code freezes, producing Release Candidates, and having a full QA process. The important thing is all that happens on a separate branch, so that day-to-day development can continue as usual on master.

The naming convention for these is ```release/<version-number>```.
  
  Hotfix branches are very similar to release branches – they result in a new version of the project being released. Where they differ is their intentions – while release branches signify a planned production milestone, hotfix branches are most often an unwanted but necessary exception to the usual release cadence, typically because of some critical defect found in the latest release that needs to be fixed as soon as possible.

They are named ```hotfix/<version-number>```. Note that if you use Semantic Versioning, regular releases bump either the Major or Minor number, while hotfixes bump the Patch number.


## 3 ***Scenarios decription***
### **Path to production**
![release](img/release.png)
![feature](img/feature.png)
Feature branches often exist only in the developer’s repository, and are never pushed – however, if there are multiple people working on one feature, or if the feature will take a long time to develop, it’s typical to push them to the central repository (if only to make sure the code isn’t lost with a single disk failure).

Once work on the given feature is done, it needs to be integrated back into master. There are several ways this can be accomplished.

Note: the choice of the feature branch integration method is immaterial as far as the workflow is concerned. It should be based on personal or team preference, however the branching model will work exactly the same, regardless of which option is chosen. My personal recommendation is to use option #1.

merge –no-ff Reverting the entire feature requires reverting only one commit (the merge commit).

Release branches also start from master, however they often don’t start from the tip – instead, they have their origin in whatever commit on master you think contains all of the features that you want to include in the given release.
  Once whatever process you use for releasing is finished, the tip of the branch is tagged with the version number. After that, the branch needs to be merged into master to be versioned permanently
  
### **Hot-fixes**
![hotfix](img/hotfixes.png)
  Hotfix branches are cut from the commit that the latest version tag points to.
  Finishing a hotfix branch is pretty much the same as finishing a release branch: tag the tip, merge it to master, then delete the branch.
  There is one special case when finishing a hotfix branch. If a release branch has already been cut in preparation for the next release before the hotfix was finished, you need to merge the hotfix branch not to master, but to the release branch. Otherwise, the new release will bring back the original bug that the hotfix corrected. The fix will eventually get to master – when the release branch is merged back to it.

As always, if the hotfix branch was pushed to the central repository, you need to remove it now
### **Path to another enviroments**
![env](img/env.png)
