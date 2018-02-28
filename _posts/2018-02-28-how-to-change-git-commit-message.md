---
title: Git Commit Message 바꾸는 방법
date : 2018-02-28 16:21:00
author : 강현정
---

Git을 사용하다 보면 커밋 메세지에 오타가 나거나 조금 더 나은 메세지 내용으로 바꿔야 할 때가 있습니다. 일반적으로 그대로 두는 것이 제일 좋지만 불가피하게 바꿔야 한다면 어떻게 해야 할까요?

**첫 번째 방법으로 `git commit --amend`을 이용하여 마지막 커밋 메세지를 바꿀 수 있습니다.**

마지막 커밋 메세지인 `3` 을 `33`으로 바꾸려면 아래 명령어를 통해 바꿀 수 있습니다.
 
![](https://github.com/yuaming/blog/raw/master/git/image/git-changing-message-1.png)

```bash
$ git commit --amend -m "33"

$ git push -f
```

마지막 메세지 내용과 Commit ID가 바뀌는 것을 아래 이미지를 통해 확인할 수 있습니다.

![](https://github.com/yuaming/blog/raw/master/git/image/git-changing-message-2.png)

**두 번째 방법으로 `git rebase -i`를 이용하는 방법이 있습니다.**

아래 이미지에서 `1`를 `11111`로 메세지 내용을 바꿔야 하는데 `git commit --amend` 으로 바로 바꿀 수 없습니다. 이때 사용할 수 있는 명령어가 `git rebase -i`가 있습니다. 이 명령어는 사용할 수 있는 용도가 많지만, 이 글에서 메세지 바꾸는 방법에 대해서만 다루도록 하겠습니다.

![](https://github.com/yuaming/blog/raw/master/git/image/git-changing-message-2.png)

`git rebase -i HEAD~3` 명령어를 통해 최근 현재 HEAD부터 최근 3개의 커밋을 rebase 합니다.

```bash
$ git rebase -i HEAD~3
```

rebase 하고 난 뒤, 아래 내용을 확인할 수 있습니다. 바꾸고자 하는 커밋 내역에 `pick -> edit`로 변경하고 `!wq` 명령어로 내용을 저장합니다. 

```bash
# edit으로 설정합니다.
edit 0a8829b 1
pick 8b2c3aa 2
pick f8483fc 33

# Rebase f18424f..f8483fc onto f18424f (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

아래 명령어를 실행하면 원래 있던 메세지 내용을 확인할 수 있고 그 자리에 새로운 메세지 내용으로 편집합니다.
그리고, `!wq`로 그 내용을 저장합니다. 

```bash
$ git commit --amend 
```

```bash
# 편집하고자 하는 내용을 작성합니다.
1 

# Please enter the commit message for your changes. Lines starting          
# with '#' will be ignored, and an empty message aborts the commit.                   
#                                                                           
# Date:      Sun Feb 25 17:18:30 2018 +0900                                 
#                                                                           
# interactive rebase in progress; onto ff1bccd                              
# Last command done (1 command done):                                       
#    edit 0a8829b 1
# No commands remaining.                                                    
# You are currently editing a commit while rebasing branch 'master' on 'ff1bccd'.     
#                                                                           
# Changes to be committed:  

# 커밋이 바뀌는 파일
```

`git rebase --continue`와 `git push -f`을 실행하면 메세지 내용이 바뀐 것을 확인할 수 있습니다.

```bash
$ git rebase --continue

Successfully rebased and updated refs/heads/master.

$ git push -f
```

아래 이미지에서 `11111`로 바뀐 것을 확인할 수 있습니다.

![](https://github.com/yuaming/blog/raw/master/git/image/git-changing-message-3.png)

`git rebase -i`를 사용하면서 주의할 점은 커밋 메세지 바꾸려는 시점부터 최근 시점까지 커밋한 파일들을 건드리고 Commit ID를 바꾸기 때문에 다른 사람과 충돌날 확률이 매우 높습니다. 그렇기 때문에 **여러 사람과 작업하는 공간에서 사용을 권하지 않습니다.**

다음 포스트에서 `git rebase -i`에 대해 알아보도록 하겠습니다.