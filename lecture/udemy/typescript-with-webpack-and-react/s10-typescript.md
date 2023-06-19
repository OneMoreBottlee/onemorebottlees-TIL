---
description: https://github.com/OneMoreBottlee/TypeScript-Master/tree/main/S10
---

# \[S10] TypeScript 컴파일러

tsc —init

tsc \[filename]



**감시모드**

tsc —watch

tsc -w \[filename]



**여러 파일 다루기**

tsc 경로 내 모든 파일



**파일 컴파일러 옵션**

files - 컴파일할 파일만 리스트

include - 컴파일할 파일만 선택, files 있으면 필요 없음

exclude - 컴파일에 제외할 파일 선택, 경로에 nodeModules가 있다면 무조건 넣어야함



**Outdir**

컴파일이 끝난 파일을 내보낼 위치 지정. 보통 dist 폴더



**Target**

JS 버전 선택



**Strict**

strict 모드

원하는 모드만 설정할 수 있다. 기본은 전체
