#### Sometimes it’s hard to understand a few Git commands. What do they actually do and how do they differ when certain flags are used with the commands?

***********

How Does Git Commit Work?
When we modify a file in a working directory, the changes are initially unstaged (not added to index).
We have to add it to the index (staging area) by using git add filename in order to commit the changes.
When we commit using git commit -m “message” all the changes that are added to the index will be committed (saved to the local Git repository).
Finally, the current branch and HEAD will be pointing to the last commit (i.e. the most recent commit).
Before we jump in, take a look at the pictorial representation for the different flags. Refer to this diagram throughout the article whenever needed.

## See the git_reset.png image for this concept
