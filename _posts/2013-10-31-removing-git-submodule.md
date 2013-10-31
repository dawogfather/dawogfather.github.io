---
layout: post
title: "Removing a git submodule"
description: "A link I found regarding removing a git submodule"
tags: [git, submodule, remove, link post]
comments: true
link: http://davidwalsh.name/git-remove-submodule
share: true
---

Git submodules are super handy and awesome. But quickly become a pain if you dont have them setup right. 
This link by David Walsh covers how to remove a git submodule on his site, but I'll put it here to look it up quickly. 

Delete the relevant section from the .gitmodules file.  The section would look similar to:
    [submodule "vendor"]
    path = vendor
    url = git://github.com/some-user/some-repo.git
Stage the .gitmodules changes via command line using: ```git add .gitmodules```
Delete the relevant section from .git/config, which will look like:
    [submodule "vendor"]
    url = git://github.com/some-user/some-repo.git
Run ```git rm --cached path/to/submodule``` .  Don't include a trailing slash -- that will lead to an error.
Run ```rm -rf .git/modules/submodule_name```
Commit the change:
Delete the now untracked submodule files ```rm -rf path/to/submodule```