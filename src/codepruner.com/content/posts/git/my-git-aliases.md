---
title: "My git aliases to improve development speed"
date: 2021-05-14T6:08:58+01:00
draft: true
tags: ["git", "productivity", ]
---





[alias]
	rpf = "!f() { \
	git checkout master; \
	git pull; \
	git checkout $1; \
	git rebase master; \
	git push origin $1 --force-with-lease;	\
	}; f"