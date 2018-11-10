# devops.works blog site

## Creating a post

- create a new file in `_drafts/permalink-address.md`
- find a nice illustration for the thumbnail and the article:
  - thumbnail: 200x200
  - full size: 1280x256
- name the image using the same slug (`permalink-address.jpg` and `permalink-address-thumb.jpg`)
- start `jekyll server --drafts` to check post while working on it

When ready to publish, `git mv _drafts/permalink-address.md _posts/YYYY-MM-DD-permalink-address.md`

Commit & push to publish.

## Frontamtter

Here is an example frontmatter:

```yaml
---
layout: post
title: Laying out roles, inventories and playbooks
excerpt: Keeping your Ansible infrastructure code tidy.
categories: articles
permalink: ansible-files-layout
share: true
tags:
  - ansible
author: mb
image:
  path: /images/posts/ansible-files-layout.jpg
  thumbnail: /images/posts/ansible-files-layout-thumb.png
  caption: "Photo credit [Paramjeet](http://pixabay.com/fr/users/Paramjeet-66854/)"
---
```

## Categories

Please use one of:

- articles: for full blown articles
- quickies: for TIL style entries & quick tips & tricks
