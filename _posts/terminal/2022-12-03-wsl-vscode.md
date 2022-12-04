---
title: 'VS Code에서 WSL 터미널 설정하기'
last_modified_at: 2022-12-03T23:36
categories:
  - Terminal
tags:
  - wsl
  - vs code
  - bash
  - terminal
  - ubuntu
toc: true
toc_sticky: true
---


# 개요
wsl 로 Ubuntu 20.04 LTS 설치하고, vs code에서 default terminal을 wsl 로 설정하는 법을 배웁니다.
wsl 설치 전 저는 Git Bash를 default 터미널로 썼습니다. 


# 방법
1. visual studio code 에 wsl 터미널 profile을 넣어 줍니다. 
2. visual studio code에서 wsl 터미널을 default 터미널로 설정합니다.

## 1. wsl 터미널 프로파일 넣기
- visual studio code를 킵니다. 
- `CTRL+SHIFT+P`를 눌러서 Command Palette을 킵니다. 
- `Preferences: open User settings(JSON)`을 치고 선택합니다.
![Preferences: open User settings(JSON)]({{"/assets/images/posts/20221203_wsl_vscode_1.png"| relative_url}})

- VS Code 의 settings.json이 켜지면 파일 안에 다음과 같은 항목을 넣어줍니다. 


```json
{
    "terminal.integrated.profiles.windows": {
        "Debian (WSL)": {                   
            "path": "C:\\Windows\\System32\\wsl.exe",
        }   
    }
}
```
`path` 값 위치에(`"C:\\Windows\\System32\\wsl.exe"`)  `wsl.exe` 이 있는지 확인합니다.
{: .notice}

## 2. wsl 터미널을 vs code default 프로파일로 설정하기
- `CTRL+SHIFT+P`를 눌러서 Command Palette을 킵니다. 
- `Terminal: Select Default Profile`을 치고 선택합니다.
![Terminal: Select Default Profile]({{"/assets/images/posts/20221203_wsl_vscode_2.png"| relative_url}})
- `Debian (WSL) C:\\Windows\\System32\\wsl.exe`을 선택합니다.


# 확인
제대로 wsl 터미널이 세팅되었는지 확인해봅니다.
- VS Code 창 아래에 TERMINAL 터미널 창이 뜨지 않았다면, `CTRL+``을 눌러서 킵니다.
- VS Code TERMINAL 창 우상단 `+`를 클릭하거나, `CTRL+ SHIFT + ``를 눌러서 새 terminal 창을 킵니다. 짜잔🥳
![VS Code WSL Terminal]({{"/assets/images/posts/20221203_wsl_vscode_3.png"| relative_url}})



