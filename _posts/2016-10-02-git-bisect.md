---
layout: post
title: Git - Using Bisect to find broken commits
subtitle: Why Git Bisect is pretty awesome
preview: As almost every single project I've worked on has used Git for version control I've come to quickly realise just how powerful - and confusing - it can be. Recently at work a bug was found which was not present in the previous release, our test suite didn't detect the issue and we weren't aware of any changes that could have broken that functionality. 
---

As almost every single project I've worked on has used [**Git**](https://git-scm.com/) for version control I've come to quickly realise just how powerful - and confusing - it can be. Recently at work a bug was found which was not present in the previous release, our test suite didn't detect the issue and we weren't aware of any changes that could have broken that functionality. Instead of having to manually `checkout` previous commits we were instead able to use the `git bisect` command to find exactly when and why the bug was introduced which can (and did) help towards fixing. 

We need to tell the `git bisect` command which commit is bad (has the issue) and which commit is good (does not have the issue) and it will then perform a [**binary search**](https://en.wikipedia.org/wiki/Binary_search_algorithm) on our history by checking out a commit in the middle allowing us to perform a test and say whether the bug is present or not. It will repeat this process of checking out the commit between good and bad until it has found the exact commit that introduced the bug. 

### How to use
To start bisecting your projects history simply run `git bisect start` and then give it the commit hash of where the bug is present using `git bisect bad <hash>` and then the commit hash of where the bug is not present using `git bisect good <hash>`. Git will then checkout the commit in the middle of the history and you can then test to see if the bug is present or not using `git bisect bad` or `git bisect good` respectively. 

If for some reason you are unable to confirm if the bug is present or not in this version (perhaps another bug is preventing you from testing) you can run `git bisect skip` which will tell Git to ignore this version, however if the skipped commit is adjacent to the bad commit we are looking for Git will be unable to tell us exactly which one is the broken commit. 

The number of commits between your initial `good` and `bad` will determine how many revisions you have to go through, but it will almost always be faster than doing this process manually. If you know the area of the codebase that will cause the bug then you can further speed up the bisection process by starting with `git bisect start -- <folder path> <another folder>` which will tell Git to only bisect commits that have changed files in this folder. 

To check on your progress in the history you can use `git bisect log`. 

Once you're done simply run `git bisect reset` to go back to your HEAD or `git bisect reset <hash>` to checkout a specific commit. 

### Automate it
Now we have a method for finding the broken commit it could still be time consuming to complete if to reproduce the bug you have to run numerous steps or perhaps initiate some setup between the commits. 

If the bug can be reproduced by an automated test (which you should now be writing as it's highlighted a hole in your current test suite!) you can tell Git to run the script after checking out the new commit by using `git bisect run` after giving it the `good` and the `bad` commit, for example `git bisect run python tests/find_bad_commit.py`. 

Your test script should have an exist status of 0 if the test passes, otherwise a status of 1 to 127 inclusive, except status 125 which will indicate the test cannot be completed and this commit should be skipped. 
