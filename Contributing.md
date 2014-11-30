## Overview
We are happy to accept contributions to the project in the form of pull requests and reporting issues.  Please use the [GitHub Issues]() tool to submit bugs or request new features.  

For any bugs, please include as much detail as possible in your tickets including your operating system, Java version, StreamFlow version, and any other environmental details you think necessary to recreate the issue.  Even better, if you would like to contribute a fix for your issue or new feature please submit a pull request for review by the development team.

Before submitting any new code via pull requests, please consult the following documentation which describes the overall branching model and source code conventions our project uses.


## Versioning
StreamFlow uses [Semantic Versioning](http://semver.org/) when updating versions for a new release.


## Branching Model
StreamFlow uses the [GitFlow](http://nvie.com/posts/a-successful-git-branching-model/) branching model to create releases and merge new features into the codebase.  We highly recommend reading over the GitFlow blog post if you have never used the GitFlow branching model before.

In compliance with the GitFlow model, we have a master branch, a develop branch, and various feature, hotfix, and release branches.   

#### Master Branch
The `master` branch should contain the most stable release code and would be the best place to pull code if deploying to a production environment.  The `master` branch should **NEVER** be committed to directly and should only receive updates during the release process.

#### Develop Branch
The `develop` branch is assumed to be stable and contains all of the newest individually tested features.  All features in the develop branch should be tested.  The `develop` branch should **NEVER** be committed to directly and should only receive updates from merged feature branches.

#### Feature Branches
Feature branches should always be forked from the develop branch and should contain enhancements which are targeted for the next release.  All contributors looking to submit new features should do so in a feature branch.  Feature branches use the naming convention of `feature/my-new-feature` where you would replace `my-new-feature` with a short but descriptive name for your branch.  When you have completed testing of your new feature, please submit a new pull request with your feature branch as the source and the `develop` branch as the target.

#### Release Branches
Release branches support the preparation of a new production release.  The release branch should be created once all features planned for the release have been merged back into the `develop` branch.  Final testing and verification can be performed on the release branch while developers looking to push new features can do so in the `develop` branch independent of the release.  Release branches use the naming convention of `release/0.9.0` where `0.9.0` represents the target version of the new release.

#### Hotfix Branches
Hotfix branches are branches used to prepare for a new release which fixes any bugs from a previous release.  The hotfix branches should branch off of `master` and changes should be merged back into both `master` and `develop` when complete.  Multiple bugs can be addressed in a single hotfix branch and when complete should increment the patch component of the release version.  Hotfix branches use the naming convention of `hotfix/0.9.1` where `0.9.1` represents the target version of the new release.


## Using the JGit-Flow Plugin to Create Branches
The Atlassian JGit-Flow Maven plugin simplifies the process of creating feature, release, and hotfix branches.  While the Maven plugin can also be used to complete features, releases, and hotfix branches, please use the pull request system when merging branches. 

> **Note:** Use of the Maven plugin is not required and if you prefer to using traditional Git commands to create your feature branches, please feel free to do so.  Just make sure you keep to the naming conventions referenced in the above Branching Model section. 

If you would like to use the Maven plugin, execute the following commands from within the root folder of the StreamFlow source code.  Each command will prompt you for the name of your feature, release, or hotfix branch so enter the desired name for your branch when prompted.

#### Creating a Feature Branch

    mvn jgitflow:feature-start

#### Creating a Release Branch

    mvn jgitflow:release-start

#### Creating a Hotfix Branch

    mvn jgitflow:hotfix-start


## Source Code Conventions

When submitting new source code to the StreamFlow project please use the following code conventions:

##### Tabs and Indent
* Convert tabs to **4** spaces
* Max code width: 100 characters
