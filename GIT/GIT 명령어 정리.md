# Git 명령어 정리
> Git 쓰면서 겪었던 상황과 이를 해결한 명령어 모음입니다.

### Remote Branch에 잘못 올라간 파일 삭제하기

```
# 원격 저장소와 로컬 저장소에 있는 파일을 삭제한다.
git rm <파일 이름>

# 원격 저장소에 있는 파일을 삭제한다. 로컬 저장소에 있는 파일은 삭제하지 않는다.
git rm --cached <파일 이름>
```

- [[Git] Github에 잘못 올라간 파일 삭제하기](https://gmlwjd9405.github.io/2018/05/17/git-delete-incorrect-files.html)

### git add 취소하기

```
# git add 전체 취소
git reset HEAD

# git add 특정 파일 취소
git reset HEAD <파일 이름>
```

- [[Git] git add 취소하기, git commit 취소하기, git push 취소하기](https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html)

### .gitignore가 동작하지 않을 때
이미 리모트 브랜치에 올라간 프로젝트에서 `.gitignore`을 적용하고 싶을 때도 가능하다.

`.gitignore` 파일에 git에 올리고 싶지 않은 파일을 추가한 후 아래 명령어를 수행한다.

```
git rm -r --cached .
git add .
git commit -m "fix: Untracked files"
```

- [.gitignore가 작동하지 않을때 대처법](https://jojoldu.tistory.com/307)

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
rebase를 사용하면 여러 commit의 정보를 변경할 수 있다.

- [이미 커밋된 내용에서 author(작성자) 수정하기](https://jojoldu.tistory.com/120)

### PR 번호로 코드 불러오기 설정
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

- <https://gist.github.com/gnarf/5406589>

### Github Pull Requests 충돌
- `git remote add upstream` -> `git fetch upstream 자신의 github_id` -> `git reset --hard upstream/자신의 github_id`
- `git push --force [remote repository]`
- [참고 링크](https://planbs.tistory.com/entry/Git-Pull-request%EC%97%90%EC%84%9C-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%B6%A9%EB%8F%8C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)

- 로컬브랜치와 다른 원격 브랜치에 푸시하기
- 원격 브랜치의 커밋 되돌리기
- <https://meetup.toast.com/posts/122>
- rebase
- cherry pick
- reset/revert
-