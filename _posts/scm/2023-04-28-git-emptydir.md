---
title: '빈 폴더 git 커밋 '
last_modified_at: 2023-04-28T01:10
categories:
  - scm
tags:
  - git
  - empty folder
toc: true
toc_sticky: true
---


## 개요
생성한 빈 디렉토리를 git commit 하는 법을 알아본다.

## .gitkeep 파일 생성 
```shell
# 'new_dir_name'으로 새로운 폴더를 생성한다.
mkdir new_dir_name
```
이 상태에서 `git status`를 해봐도 새로운 폴더는 뜨지 않는다. Git은 빈 폴더를 추가하지 않는다.
이를 해결하기 위한 trick으로, `.gitkeep`이란 파일을 새로 생성한 폴더 안에 만든다.[^fn1]

```shell
touch new_dir_name/.gitkeep
```

이 파일을 생성한 후에는 `git status`에 `new_dir_name/`이 뜨는 것을 확인할 수 있다. 
이 후는 보통 커밋과 같은 프로세스다. 

```shell
git add new_dir_name/
git commit -m "new_dir_name라는 새로운 폴더 추가"
```

사실 `.gitkeep`말고도 `.anyname`, `.empty`, `.keep`와 같은 다른 이름으로 해당 폴더안에 생성해도 뜬다. `.gitkeep`은 Git이 `.gitignore`같은 파일처럼 뭔가 처리하는 파일은 아니다. 근데 상징적으로, 많은 사람들이 빈 폴더를 깃 커밋할 때 생성하는 파일 이름을`.gitkeep`이라는 이름으로 생성하는 듯 하다.[^fn2]



# References
[^fn1]: [digitalocean: how to add and commit an empty directory in my git repository](https://www.digitalocean.com/community/questions/how-to-add-and-commit-an-empty-directory-in-my-git-repository){:target="_blank"}
[^fn2]: [educative ](https://www.educative.io/answers/what-are-the-differences-between-gitignore-and-gitkeep){:target="_blank"}
