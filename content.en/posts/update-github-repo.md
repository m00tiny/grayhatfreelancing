---
author: "m00tiny"
date: 2022-12-28
title: Updating a github repository after cloning
---


## Updating Github Repos
println("Hamming difference of: ${count}")It's been a nuisance to create your own or fork another, clone it, update the remotes for SSH and then get back to business. This reference is just a quick 1, 2, 3... to-do/check list of steps for ease of reference!

```
Create repository on Github
 - Go to the website and just create one using their form, get the empty skeleton
Clone repository
 - git clone <your repo>
Remove remotes
 - git remote remove origin
Add SSH remotes
 - git remote add origin git@github.com:m00tiny/todo-android
   - You can combine the above two by using 'git remote set-url origin get@github.com:m00tiny/todo-android'
Quick commit and push
 - git add .
 - git commit -m '.'
 - git push
```