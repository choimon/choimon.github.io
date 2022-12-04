---
title: '커스텀 bash 쉘 - 작업 폴더 길이 줄이기'
last_modified_at: 2022-12-04T15:50
categories:
  - Terminal
tags:
  - bash
  - terminal
  - wsl
  - ubuntu
  - working directory
  - Customize Bash Shell Prompt to show only current directory
toc: true
toc_sticky: true
---

# 개요
wsl로 Ubuntu 20.04 LTS를 설치하고, WSL bash 터미널을 사용해서 작업 중인데, WSL 같은 경우 `/mnt/c/Users/{usernmae}` 이런식으로 시작된다. \
작업하는 프로젝트 path가 길어지게 되면, 터미널 prompt에 working directory(작업 폴더/작업 공간) 경로가 너무 길게 보인다. 
working directory path가 너무 길어서 wsl bash shell에서 어떻게 못 줄이나 알아보다가 찾았다. 




# bash 쉘 working directory path 줄이기 

- 줄이기 전은 다음과 같이 workding directory path가 길게 보인다. 
![bash shell long working dir path]({{"/assets/images/posts/20221204_bash_pathdir_1.png"| relative_url}})


- bashrc 파일을 text editor 로 열어서 다음과 같이 수정해준다. 예제에서는 vim을 사용했다. 
- `vim ~/.bashrc`
```shell
#마지막 줄에 다음을 추가해준다. 
PROMPT_DIRTRIM=1
```

- 새로운 bash 터미널창을 키거나 현재 터미널창에서 `source ~/.bashrc` 커맨드를 날려 bashrc파일을 실행해준다. 
- 쨔쟌🥳 다음과 같이 현재 경로에서 마지막 폴더명만 보여지게 된다.
![bash shell curr working dir path only]({{"/assets/images/posts/20221204_bash_pathdir_2.png"| relative_url}})

- vs code 터미널 창에서 특히 유용하게 사용한다. 
- vs code wsl 터미널 전과 후 
![bash shell curr working dir path only]({{"/assets/images/posts/20221204_bash_pathdir_3.png"| relative_url}}) <br/><br/>
![bash shell curr working dir path only]({{"/assets/images/posts/20221204_bash_pathdir_4.png"| relative_url}})