---
description: 230719 과제 디벨롭 중
---

# src refspec main does not match any

### 💣 상황

과제 디벨롭을 하면서 새로운 브런치에서 작업 후 git push 하니 이 에러가 발생했다..

```
src refspec main does not match any ...
```



### 🔍  원인

repository 정보가 변경되거나

다른 곳에서 클론한 repository를 새로 만들어 push 할 때

git을 초기화한 후 add나 commit 없이 push하려할 때 발생한다고 한다.



### 🤟 해결

git 정보를 최신화하거나

repository를 새로 클론해 작업을 다시 하거나

커밋 후 push하는 방법이 있다.



내 경우, 2번째 원인이 문제였던것 같다.

다행히 작업량이 적어 새롭게 clone해서 작업한 부분을 옮겨 다시 push 하는 방법을 사용했다.

항상 느끼지만 깃은 알듯말듯한 묘한 매력이 있는 툴이다.

