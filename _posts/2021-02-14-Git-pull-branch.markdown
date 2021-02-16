---
layout: post
title: "Pull해오기"
date: 2021-02-14 23:13:41 +0530
categories: Git
---

# pull이란?

원격 저장소에 올라온 변경 사항을 로컬 저장소로 가져온다.

fetch + merge

## master의 최신 내용 pull해오기

---

git checkout master  
git pull origin master  
git checkout 내 브랜치명  
git merge master

master 브랜치로 이동 후 원격 저장소 내용을 pull해온다.
내 브랜치로 이동 후 master브랜치를 merge해준다.
