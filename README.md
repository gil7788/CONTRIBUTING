# CONTRIBUTING

This file presents a few guidelines you need to follow, so that your contribution will utilize Firebase Hosting workflows and will be easy to merge with the contribution of others.

## Workflow
 ### GitHub Flow
 In our project, we use a structured branching strategy involving `release`, `master`, and feature branches. The `master` branch serves as the primary development branch where all the feature development takes place. Contributors create feature branches off of `master` for each new feature or bug fix. After completing the development, the changes are merged back into `master` through pull requests.

For releases, we use the `release` branch, which represents the production-ready state of our application. When it's time to deploy, the latest stable version of code from `master` is merged into `release`


 #### Updating the Master Branch:

 Here’s how to update the master branch using this workflow:

 1. **Create a Feature Branch:**
    - From  `master` branch, create a new branch. This is where you'll work on your changes.
    ```bash
    git checkout master
    git pull origin master
    git checkout -b your-feature-branch
    ```

 2. **Develop Your Feature:**
    - Make your changes in this feature branch. Commit these changes to the branch.
    ```bash
    git add .
    git commit -m "Your commit message"
    ```

 3. **Create a Pull Request (PR):**
    - Push your branch to the remote repository and open a pull request to the `master` branch.
    ```bash
    git push origin your-feature-branch
    ```
    - On GitHub, create a new PR from `your-feature-branch` to `master` 

 4. **Merge the PR:**
    - After Pull Request review
    - Choose either **"Squash and merge"** or **"Rebase and merge"**.

 #### Updating the Production (Prod) Branch: 

 For deploying to production, the workflow involves merging changes from the `master` branch into a `release` branch. 

 1. **Create a Pull Request to Prod Branch:** 
    - From the `master` branch, create a new pull request to the `release` branch. 
    ```bash 
    git checkout master 
    git pull origin master 
    git push origin master  # Ensure the remote master is up to date 
    ``` 
    - On GitHub, create a new PR from `master` to `release` 

 2. **Merge with a Merge Commit:** 
    - Select "Create a merge commit" while merging the pull request on GitHub.

 3. **Update Local Prod and Master Branches:** 
    - After merging, make sure to update both your local `release` and `master` branches. This ensures that both branches reflect the current state of the repository after the merge. 
    ```bash 
    # Update local release branch 
    git checkout release 
    git pull origin release 

    # Update local master branch 
    git checkout master 
    git merge origin release 
    git push origin master 
    ``` 
#### Managing conflicts
Here's how to manage a scenario, with `feature/one` already rebased and merged into `master`, and now needing to add changes from `feature/two` into `master`.

##### Managing Conflicts on Master Branch:
**Update Your Local master Branch:**
Before attempting to merge `feature/two`, ensure your local `master` branch is up to date with the remote repository. This includes the changes from `feature/one` that were recently merged.

    git checkout master
    git pull origin master

**Rebase feature/two Against the Updated master:**
Switch to your `feature/two` branch and rebase it against the updated `master` branch. This step is crucial as it applies your `feature/two` changes on top of the latest `master`, helping to identify and resolve conflicts outside the `master` branch.

    git checkout feature/two
    git rebase master

During the rebase process, git may pause and alert you to conflicts that need to be resolved manually.

**Resolve Conflicts:**
If there are conflicts, git will stop the rebase and list the files that need manual intervention. Open these files in your code editor, and you'll see sections marked with <<<<<<<, =======, and >>>>>>>, indicating conflicting changes. Resolve each conflict by editing the file to your desired final state.

After resolving each conflict in a file, add it to the staging area:

    git add <filename>

Once all conflicts are resolved and the changes are staged, continue the rebase:

    git rebase --continue

Repeat this process until all conflicts are resolved and the rebase is complete. If at any point you decide that the rebase should be aborted, you can do so with `git rebase --abort`.

**Finalize the Merge:**
After successfully rebasing `feature/two` onto the latest `master`, push your changes to the remote feature branch (you might need to use force push due to the rebase, but be cautious as this can overwrite history on the remote branch):

    git push origin feature/two --force

Then, create a pull request for `feature/two` into `master` as you did with `feature/one`. Since `feature/two` has been rebased, the pull request should only contain the changes unique to `feature/two` and be free of conflicts with `master`.

**Review and Merge the Pull Request:**
Have your changes reviewed through the pull request process. If there are no additional conflicts or issues, merge `feature/two` into `master` using your project's preferred merge strategy - either squash and merge, or rebase and merge.

#### Managing conflicts on Prod Branch
`release` branch should be updated only by a pull request from `master` branch. Using such workflow, will prevent conflicts at merge between `master` and `release` branches.
If there is a merge conflict between `master` branch and a `release` branch, you should raise a red flag and reconsider your workflow.


#### Best Practices for Conflict Management:
1. Keep Branches Short-Lived: The longer a branch lives separately from `master`, the higher the chances of conflicts. Try to merge feature branches back into `master` as soon as they're ready and tested.

2. Regularly Pull Changes from `master` into Your Feature Branches: This can help minimize conflicts by keeping your feature branches up-to-date with the latest changes in `master`.

3. `release` branch should be updated only by a pull request from `master` branch. Using such workflow, will prevent conflicts at merge between `master` and `release` branches.

4. Consider using feature toggles to shorten `release` branch update cycle.
