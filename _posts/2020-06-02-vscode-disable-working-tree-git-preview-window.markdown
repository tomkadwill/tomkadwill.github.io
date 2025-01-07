---
layout:     post
title:      "VS Code Disable 'Working Tree' Git Preview Window"
date:       2020-06-02 07:46:00 +0000
permalink:  'vscode-disable-working-tree-git-preview-window'
---

VS Code's built in source control support is fantastic. However, the working tree preview feature annoys me. When you click on a file, from the source control sidebar, you'll see a diff preview like this:

![Working Tree Diff](/assets/vscode-disable-working-tree-git-preview-window/working-tree-diff.png)

This slows me down because I use the source control sidebar for navigation. I like to jump to edited files and tidy them up before committing to GitHub.

You can disable this feature by searching for `openDiffOnClick` in settings and un-checking it:

![openDiffOnClick](/assets/vscode-disable-working-tree-git-preview-window/open-diff-on-click.png)

With `openDiffOnClick` disabled you'll jump straight to files for editing. Just like you would when navigating via the file explorer.