---
description: 2024.10.21
---

# #2 SQLite

## #2.1 Installation

(Windows)

### scoop 설치

* powershell 터미널 실행

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

* SQLite 설치

```
scoop bucket add main
scoop install main/sqlite
```

