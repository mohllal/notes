# Version Control with Git

By: Atlassian.
[Course Link](https://www.coursera.org/learn/version-control-with-git/).

Date Started: Tuesday, April 7, 2020
Date Finished: Saturday, April 25, 2020

## Week 1: Our First Repository

- Git is a ***distributed version control system***. This means that each user has a local copy of the repository and that repositories can easily be synchronized.
- A distributed version control system is a type of version control system. It usually has these characteristics. *Each user has a local copy of the complete history of the project*, which is known as a repository.
- Git is a *free* and *open source* software project meaning that the code that implements Git is publicly available. No single company owns Git, and anyone can make contributions to improve it. Git has a vibrant community of support and an ecosystem in which many other technologies are integrated with it.
- The ***staging area*** contains a list of files that are planned to be included in the next commit that you make.
- The ***project directory*** contains the working tree. The working tree contains the directories and files of a single commit or snapshot of the project.
- The ***remote repository*** contains the commits of the project, and is often considered the source of truth or official state of the project.
- The ***local repository*** contains all of the commits that have been made for the project. These commits represent the version history of the project.
- The ***working tree*** is the location on your computer that contains the directories and files of a single commit.
- The project directory also contains a *hidden directory* named ***.git***. This is where the staging area and local repository are located.
- Use `git status` to view the status of files in the working tree and staging area.
- To review, a remote repository is a bare repository that often serves as the source of truth for the project.
- Use `git clone` to create a local copy of a remote repository. The word clone can be used as a noun and a verb. As a noun, a clone is a local copy of a remote repository. As a verb, cloning is the process of creating a clone.

## Week 2: Branching and Merging I

- ***Acyclic*** means no cycles or non-circular.
- A graph model defines the relationship among the commits in a repository.
- A directed graph means that the nodes are connected in a certain direction. The direction is represented using arrows.
- The 40-character hexadecimal string is the result of a mathematical computation based on the content. Statistically speaking, the ***SHA-1 value is unique for a given piece of content***.
- An ***annotated tag*** is a reference to a specific commit.
- A ***tree*** is an object that contains a list of the filenames and directories inside of a directory. A blob is an object that stores the content of a file that is being managed by Git.
- ***Git object*** names are also known as ***Git IDs***. Git objects are named with SHA-1 values. Statistically speaking, SHA-1 values are unique for a given piece of content
- ***Git IDs*** are often shortened to the first four or more characters.
- Internally, Git uses objects to store four types of things. A commit object is a simple text file that contains information such as:
  - The commit user information.
  - The commit message.
  - A reference to the commit's parent or parents.
  - A reference to the root tree of the project.
- A Git ID is the name of a Git object. All of the objects stored by Git are named with a 40-character hexadecimal string. These strings are commonly known as Git IDs, but they are also known as object ***IDs***, ***SHA-1s***, ***hashes*** and ***checksums***.
- Commits can be associated with references. A reference is a user-friendly name that points to a commit's SHA-1 value or another reference. If a reference points to another reference, it's called a symbolic reference.
- ***HEAD*** is a reference that points to the current commit.
- Create ***tags*** to place labels on specific commits.
- The caret `^` character can be appended to IDs and references in git commands, primarily to refer to a specific parent in a merge commit.
- A branch label is a reference that points to the tip of the branch.
- HEAD is implemented in Git as a reference. *There's a single HEAD reference in a local repository* and it points to the current commit.
- Tags are not automatically pushed to a remote repository.
- In Git commands use `~` and `^` to conveniently refer to previous commits.
- A branch label points to the most recent commit in the branch. That commit is commonly called the tip of the branch. Branch labels are implemented as references.
- Dangling commits will eventually be garbage collected.
- Fix a ***detached HEAD*** by creating a branch. A detached HEAD reference points directly to a commit.
- Deleting a branch really just means that you're deleting a branch label. Deleting a branch label normally does not delete any commits, at least not right away
- Checkout involves updating the HEAD reference and updating the working tree.
- So a ***detached HEAD*** basically means that the HEAD reference is detached from a branch label. Git will warn you that if you continue you will be in a detached HEAD state.
- A branch is a set of commits starting with the most recent commit in the branch and tracing back to the project's first commit.
- A merge is ***fast-forwardable*** if no other commits have been made to the base branch since the branch was created. A fast-forward merge moves the base branch label to the tip of the topic branch.
- Combining the work of multiple commits may result in what's called a ***merge conflict***, if both branches change the same thing in different ways
- While executing the git merge command, git will open an editor showing a default merge message. This is the commit message for the merge commit, you can accept or modify the default merge message.
- A no fast-forward merge means that a *merge commit will always be created*, even if the merge is fast-forwardable.
- A ***merge commit*** combines the work of the tips of the feature and base branches and places the result into a single merge commit.
- There are four main types of merges:
  - Fast-forward merge.
  - Merge commit.
  - Squash merge.
  - Rebase.
- Merging combines the work of independent branches. Usually, this involves merging a topic branch, such as the *featureX* branch, into what is called a base branch, such as the master branch. The base branch is usually a longer running branch than the topic branch.

## Week 3: Branching and Merging II

- When attempting a merge, files with conflicts are modified by Git and placed in the working tree.
- Any text that isn't surrounded by conflict markers was cleanly resolved by Git. The first part of the conflict marker is labeled head.
- Git takes on the responsibility of combining the work of multiple branches and placing the result into a single merge commit. Git will try to do this automatically.
- The first commit involved with resolving a merge conflict is the commit at the tip of the current branch. We call this commit, ours or mine. The second commit involved with resolving emerge conflict is the commit at the tip of the branch to be merged. We call this commit, theirs. The final commit involved with resolving a two-branch merge conflict is the common ancestor of the two branches. We call this commit, the merge base.
- Tracking branches are local branches that represent remote branches. They are named `<remote>/<branch>` for example `origin/master`. They can become out of sync with local branches, and they are updated with network commands like `clone`, `fetch`, `pull`, and `push`.
- The default behavior is the same as the `ff` option. This well perform a fast-forward merge if possible, otherwise it will create a merge commit. The `no-ff` option will always pull using a merge commit. This create a merge commit for every pull that downloads commits. The `ff-only` option only accepts fast-forward merges. It will cancel the merge rather than perform a merge commit. You will then have to decide how to deal with the merge using a separate command.
- Use `git pull` is used to combine `git fetch` and `git merge FETCH_HEAD`.
- ***FETCH_HEAD*** is an alias for the tip of the tracking branch. First new objects and references from the remote repository are fetched. If new objects are added to the tracking branch, the tracking branch is merged into the local branch. The current branch is assumed if branch is not specified.
- The `git fetch` command retrieves new objects and references from another repository. It mainly updates all of your tracking branch information. If the repository argument isn't specified and you only have one remote repository set up locally, that remote depository will be used by default.
- A general rule is to fetch or pull before you do a push.
- The `git push` command is used to add commits to a remote repository. You are pushing the commits of your current branch
- ***Clone***, ***fetch***, ***pull*** and push commands communicate with a remote repository. Fetch updates tracking branch information. Pull combines a fetch and a merge. Push adds commits to the remote repository.
- There is a general rule related to rebasing. Do not rewrite history that has been shared with others. If you've been working locally or if you know that no one else has used your branch you can safely rebase it.
- Rebasing is a form of merge and may result in merge conflicts.
- There are two types of Rebase:
  - A regular Rebase.
  - An interactive Rebase.
- When rebasing, git applies the diffs to the new parent commit. This is called ***reapplying commits***.
- ***Rebasing pros***:
  - The pros are that you can incorporate changes from the parent branch, so if that branch has new features or bug fixes, you will see them.
  - The tests on your branch are using more current code and because you are keeping up with the changes on other branches the eventual merge into the base branch will be easier.
  - Rebasing is that it avoids unnecessary merge commits. You can then have a very well-defined and clean commit history.
- ***Rebasing cons*** is it's a form of merge, so merge conflicts may need to be resolved. We've also pointed out that if you've already shared your commits that can cause problems because the commit IDs change in a Rebase. And finally with the Rebase you are not preserving the commit history, you are rewriting the commit history.
- In addition to amending the most recent commit message, you can change the files of the commit. You do this by modifying the staging area and then again executing git commit with the amend option.
- Interactive rebase allows you to rewrite the history of a branch.
- To perform a squash merge, you check out the *master* branch and then execute `git merge` with the squash option and specify the *featureX* branch as the argument. This adds the merged content to the staging area and you execute git commit to create the squash merge commit.
- Interactive rebase lets you edit commits using commands. The commits can belong to any branch and can in fact belong to a single branch.
- Squash combines the commit with the older commit, creating a single commit. The work of both commits is included.

## Week 4: Git Workflows

- Pull requests enable team communication related to the work of the branch. Notifications, related to the pull request, can be sent to team members. Those team members can provide feedback or comments, and ultimately can a have a say in approving the content, this can act as a form of code review.
- Pull requests are opened using an online Git host such as ***Bitbucket*** of ***GitHub***. The ultimate goal of a pull request is to merge a branch, but they also facilitate team discussion and approval.
- You can open a pull request any time after creating the branch, you do not need to edit the pull request if you add a commit to the branch. You can merge the pull request using an online Git host or by pushing the merge from your local client.
- There are two basic repository configurations related to pull requests:
  - The first one is a ***single remote repository***, a pull request in a single repository configuration is a request to merge a branch of the repository. 
  - The second configuration involves ***two remote repositories***, in this configuration, a pull request is a request to merge a branch from a forked repository into the upstream repository. The fork approach is common if the submitter doesn't have write access to the upstream repository.
- A fork can also be used to create a different source of truth. In other words, it can be used to start a new line of development of the project that remains independent from the upstream repository.
- Pull requests can be made from forks and merged into the upstream repository.
- Forking generally means copying a remote repository to your own online account.
- In a feature branch workflow, the work of the project is done in feature, or topic branches. The work is then merged into a longer running branch.
- ***GitFlow workflows*** enable a continuous train of project releases using multiple types of branches.
- ***The forking workflow*** involves multiple remote repositories. One of the repositories is considered upstream from the other. The upstream repository is considered the source of truth for the project.
- ***The centralized workflow*** uses a single branch to accomplish the work of the project. Even though this workflow is very simple, you still gain many of the benefits of git.
- ***GitFlow*** is a workflow that allows safe continuous releases of the project. It allows work to continue even through releases and hotfixes.
- Forking a repository is a great way to work on a feature branch without sharing your branch. This provides a remote backup of your work, and allows you to safely rebase your local branch. A downside of this approach is that the two remote repositories can become out of sync. It's the responsibility of the forked repository to keep up to date with the upstream repository.