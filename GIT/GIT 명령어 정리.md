# Git 명령어 정리
> Git 쓰면서 겪었던 상황과 이를 해결한 명령어 모음입니다.

- [Remote Branch 가져오기](#remote-branch-가져오기)
- [Remote Branch 참고하기](#remote-branch-참고하기)
- [Remote Branch에 올라간 커밋 취소하기](#remote-branch에-올라간-커밋-취소하기)
- [Local Branch와 Remote Branch의 이름이 다른 상태에서 Push 하기](#local-branch와-remote-branch의-이름이-다른-상태에서-push-하기)
- [Remote Branch에 잘못 올라간 파일 삭제하기](#remote-branch에-잘못-올라간-파일-삭제하기)
- [git add 취소하기](#git-add-취소하기)
- [.gitignore가 동작하지 않을 때](#gitignore가-동작하지-않을-때)
- [마지막 commit 정보 변경하기](#마지막-commit-정보-변경하기)
- [rebase 를 활용하여 commit 정보 변경하기](#rebase-를-활용하여-commit-정보-변경하기)
- [PR 번호로 코드 불러오기 설정](#pr-번호로-코드-불러오기-설정)
- [Github Pull Requests 충돌](#github-pull-requests-충돌)


### Remote Branch 가져오기
> 참고: [Git remote branch 가져오기](https://cjh5414.github.io/get-git-remote-branch/)

```
# git remote 최신화하기
git remote update
```

Local 환경에서 remote branch를 최신화하지 않으면 찾지 못한다는 오류가 발생할 수 있다.

```
# origin remote branch인 feature/Issue-1을 이름 그대로 가져온다.
git checkout -t origin/feature/Issue-1

# origin remote branch인 feature/Issue-1을 다른 이름으로 가져온다.
git checkout -b <생성할 branch 이름> origin/feature/Issue-1
```

### Remote Branch 참고하기
> 참고: [Git remote branch 가져오기](https://cjh5414.github.io/get-git-remote-branch/)

Remote Branch를 수정하지 않고 단지 참고만 하고 싶은 경우에 해당 명령어를 쓸 수 있다.

```
git checkout <원격 branch 이름>
```

위 명령어를 수행하면 `detached HEAD` 상태가 되며, 로컬 환경에서 소스는 변경할 수 있지만, **commit, push와 같은 명령어는 사용할 수 없다.** 그리고 다른 branch로 checkout하면 해당 branch는 사라진다.

### Remote Branch에 올라간 커밋 취소하기
> 참고: [원격 저장소에 올라간 커밋 되돌리기](https://jupiny.com/2019/03/19/revert-commits-in-remote-repository/)

### Local Branch와 Remote Branch의 이름이 다른 상태에서 Push 하기
> 참고: [[Git] Github에 잘못 올라간 파일 삭제하기](https://gmlwjd9405.github.io/2018/05/17/git-delete-incorrect-files.html)

```
git push origin <로컬 브랜치 이름>:<원격 브랜치 이름>
```

### Remote Branch에 잘못 올라간 파일 삭제하기

```
# 원격 저장소와 로컬 저장소에 있는 파일을 삭제한다.
git rm <파일 이름>

# 원격 저장소에 있는 파일을 삭제한다. 로컬 저장소에 있는 파일은 삭제하지 않는다.
git rm --cached <파일 이름>
```

### git add 취소하기
> 참고: [[Git] git add 취소하기, git commit 취소하기, git push 취소하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)

```
# git add 전체 취소
git reset HEAD

# git add 특정 파일 취소
git reset HEAD <파일 이름>
```

### .gitignore가 동작하지 않을 때
> 참고: [.gitignore가 작동하지 않을때 대처법](https://jojoldu.tistory.com/307)

이미 리모트 브랜치에 올라간 프로젝트에서 `.gitignore`을 적용하고 싶을 때도 가능하다.

`.gitignore` 파일에 git에 올리고 싶지 않은 파일을 추가한 후 아래 명령어를 수행한다.

```
git rm -r --cached .
git add .
git commit -m "fix: Untracked files"
```

### 마지막 commit 정보 변경하기
마지막 commit에 대한 정보를 변경할 때는 `--amend` 옵션을 사용한다.

```
git commit --amend
```

위 명령어를 사용하면 마지막 commit의 정보와 함께 자동으로 텍스트 편집기가 열린다. 여기서 수정할 수 있는 것은 **commit 메시지 뿐**이다. commit의 정보를 변경하여 저장하여도 반영되지 않는다.

- `--no-edit`: commit 메시지를 수정하지 않음

1. commit 날짜 변경
임의의 날짜로 변경

```
git commit --amend --no-edit --date="Mon 20 Aug 2020 19:19:19 KST"
```

- 요일: Sun, Mon, Tue, Wed, Thu, Fri, Sat

현재 날짜로 변경

```
git commit —amend —no-edit —date="$(date)"
```

2. commit 작성자 변경

```
git commit —amend —no-edit —author="codemcd"
```

### rebase 를 활용하여 commit 정보 변경하기
> 참고: [이미 커밋된 내용에서 author(작성자) 수정하기](https://jojoldu.tistory.com/120)

rebase를 사용하면 여러 commit의 정보를 변경할 수 있다.

### PR 번호로 코드 불러오기 설정
> 참고: <https://gist.github.com/gnarf/5406589>

1. `~/.gitconfig`에 아래의 내용(alias)을 복사 붙여넣기한다.

```
# for github remotes
[alias]
  pr  = "!f() { git fetch -fu ${2:-$(git remote |grep ^upstream || echo origin)} refs/pull/$1/head:pr/$1 && git checkout pr/$1; }; f"
  pr-clean = "!git for-each-ref refs/heads/pr/* --format='%(refname)' | while read ref ; do branch=${ref#refs/heads/} ; git branch -D $branch ; done"
# for bitbucket/stash remotes
  spr  = "!f() { git fetch -fu ${2:-$(git remote |grep ^upstream || echo origin)} refs/pull-requests/$1/from:pr/$1 && git checkout pr/$1; }; f"
```

2. `git pr [PR 번호] [remote 저장소]` 와 같이 명령어를 입력하면 해당 브랜치를 checkout까지 해서 가져올 수 있다.

### Github Pull Requests 충돌
- `git remote add upstream` -> `git fetch upstream 자신의 github_id` -> `git reset --hard upstream/자신의 github_id`
- `git push --force [remote repository]`
- [참고 링크](https://planbs.tistory.com/entry/Git-Pull-request%EC%97%90%EC%84%9C-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%B6%A9%EB%8F%8C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)