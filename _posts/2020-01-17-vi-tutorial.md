---
layout: post
title: vi tutorial
description: >
  1/17 vi tutorial
tags: [study]
comments: true
---

# 1/17 Vi Tutorial

### vi 명령어 정리

- Cursor

  - 상하좌우 이동 : h(좌), j(하), k(상), l(우)
  - 단어 단위 이동 : w(단어 앞), e(단어 끝), b(전 단어)
  - 문장 맨 앞, 뒤로 : 0, $
  - 파일 첫 라인, 마지막 라인 : **gg, G**

- Big step

  - 페이지 절반 앞, 뒤 이동 : ctrl+d, ctrl+u
  - 페이지 전체 앞, 뒤 이동 : **ctrl+f, ctrl+b**
  - 페이지 맨 위, 중간, 맨 아래 : H, M, L

- 삭제

  - 문장 삭제, 단어 삭제 후 입력 모드 : cc, cw
  - 문장 삭제, 단어 삭제, 한 글자 삭제 후 커맨드 모드 : **dd, dw, x**

- 검색

  - 검색, 이전찾기, 다음찾기 : **/ , n, N**
  - 파일 전체에 대해 특정 단어 치환 : %s/*string*/*replace*

- 복사 및 붙여 넣기

  - 한 줄 복사 : **yy**
  - 한 줄 붙여넣기 : **p**

- 종료

  - 저장 후 종료 : **wq**
  - 강제 종료 : **q!**

- 그 외

  - 블록 지정 : **v**
  - undo : **u**
  - redo : **ctrl+r**
  - 가장 마지막 명령어 재실행 : .
  - 실시간으로 write되는 로그 확인 : **e!**
  - 수직 창 분할 : sp
  - 수평 창 분할 : vs

- customizing

  - :set 명령으로 현재 설정 된 mode 확인

  - 'no' 옵션으로 설정 된 mode 해지 

    - ex) :set nonu

  - 내 customizing

    ```
    # ~/.vimrc file
    
    set nu
    set hlsearch
    set encoding=utf-8
    set autoindent
    set smartindent
    set cindent
    set paste
    set showmatch
    set ts=4
    set sts=4
    set sw=4
    syntax on
    ```



#### 교육 내용 출처: NHN 기술교육 - 1/17 vi tutorial (조영일 이사)