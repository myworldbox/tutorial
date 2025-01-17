# Git
All tutorial notes related to Git

## Discard commits
```bash
# n is an integer
git reset -m HEAD~n
```

## Commit insertion
```bash
# Create a Temporary Branch: Create a temporary branch from commit A:
1. git checkout -b temp <commit-hash-of-A>

# Make Your Changes: Make the changes you want to include in the new commit and commit them:
2. git add .
3. git commit -m "Your new commit message"

# Rebase the Remaining Commits: Rebase the commits that come after commit A onto your new commit:
4. git checkout <original-branch>
5. git rebase --onto temp <commit-hash-of-A> <original-branch>

# Delete the Temporary Branch: Once the rebase is complete, you can delete the temporary branch:
6. git branch -d temp
```

## Squash multiple commits into fewer commits
```bash
# Start Interactive Rebase: Open your terminal and navigate to your repository. Then, run:
1. git rebase -i HEAD~6

# Edit the Rebase Instructions: In the editor, change 'pick' to 'squash' (or 's') for the commits you want to combine.
# For example, to squash the first 3 commits into one and the last 3 commits into another:
# pick <commit-hash> Commit message 1
# squash <commit-hash> Commit message 2
# squash <commit-hash> Commit message 3
# pick <commit-hash> Commit message 4
# squash <commit-hash> Commit message 5
# squash <commit-hash> Commit message 6

# Save and Close the Editor: Save the file and close the editor. In Vim, you can do this by typing ':wq' and pressing Enter.

# Edit Commit Messages: Git will open another editor for you to edit the commit messages for the squashed commits. Combine the messages as needed, then save and close the editor.

# Complete the Rebase: Git will reapply the commits with the new structure. If there are no conflicts, the rebase will complete successfully.

# Force Push (if necessary): If you've already pushed these commits to a remote repository, you'll need to force push:
6. git push --force-with-lease
```

## Squash all commits into one commit
```bash
# Create a Temporary Branch: Create a temporary branch from your current branch.
1. git checkout -b temp-branch

# Reset to the First Commit: Reset this new branch to the first commit.
2. git reset --soft $(git rev-list --max-parents=0 HEAD)

# Commit All Changes: Stage all changes and create a new commit.
3. git add --all
4. git commit -m "Squashed all commits into one"

# Force Push (if necessary): If you've already pushed the original branch to a remote repository, you'll need to force push the new branch.
5. git push --force-with-lease origin temp-branch:main
```

## Cherry picking multiple commits into another branch
```bash
# Checkout the Target Branch: Switch to the branch where you want to apply the commits.
1. git checkout <target-branch>

# Cherry Pick Multiple Commits: Use the 'git cherry-pick' command with a range of commits.
# For example, to cherry-pick commits from A to B (inclusive):
2. git cherry-pick <commit-hash-A>^..<commit-hash-B>

# Resolve Conflicts (if any): If there are conflicts, resolve them and continue the cherry-pick process.
3. git cherry-pick --continue

# Commit the Changes: If there are no conflicts, the commits will be applied to the target branch.
4. git push
```