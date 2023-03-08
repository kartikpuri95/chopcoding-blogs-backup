---
title: "Git Beyond the Basics: 5 Advanced Commands for Power Users"
datePublished: Wed Mar 08 2023 06:29:45 GMT+0000 (Coordinated Universal Time)
cuid: clezavly2000109l90uqu0yn0
slug: git-beyond-the-basics-5-advanced-commands-for-power-users
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677555613322/96343d47-212d-4764-8ad0-864df6ae2678.png
tags: github, git, coding, gitlab

---

Git is a powerful version control system that can help you manage your projects effectively. It has many useful commands that can save you time and increase your productivity. Here are some of the top Git commands that you should know:

## Updating the last commit message

If you want to update the message of your last commit, you can use the following command to edit it:

```bash
git commit --amend -m "Updated message"
```

## Checking the total number of commits

To check the total number of commits on a particular branch, you can use the following command:

```bash
git rev-list --count <branch-name>
```

For example, if you want to check the number of commits on the main branch, you can use the following command:

```bash
git rev-list --count main
```

## Staging and committing changes in a single command

Instead of using `git add` and `git commit` separately, you can use the following command to stage and commit changes in a single step:

```bash
git commit -am "message"
```

## Checking files from a different branch

If you want to view files from a different branch while working on a different branch, you can use the following command:

```bash
git show <branch-name>:<file-name>
```

For example, if you are working on the test branch and want to see the [README.md](http://README.md) file of the main branch, you can use the following command:

```bash
git show main:README.md
```

## Blank Commit

Sometimes you may need a blank commit, such as when you want to initialize a repository or trigger an action. You can use the following command to create a blank commit:

```bash
git commit --allow-empty -m "blank commit message"
```

This command creates a commit with no changes added or deleted.

%[https://twitter.com/chopcoding/status/1630092913739960320]