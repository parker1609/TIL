# GIT 명령어

## .gitignore가 작동하지 않을때 대처법
- 이미 Github에 올라간 파일을 gitignore 적용할 때도 같은 방법으로 가능하다.
  - `.gitingnore` 파일에 git에 올리지 않을 파일을 추가한 후 아래의 명령어를 수행한 후, push하면 된다.

```
git rm -r --cached .
git add .
git commit -m "fix: Untracked files"
```

## Github Pull Requests 충돌
- `git remote add upstream` -> `git fetch upstream 자신의 github_id` -> `git reset --hard upstream/자신의 github_id`
- `git push --force [remote repository]`
- [참고 링크](https://planbs.tistory.com/entry/Git-Pull-request%EC%97%90%EC%84%9C-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%B6%A9%EB%8F%8C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)


## git add 특정 파일 취소하기
- 참고 링크: <https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html>


## PR 번호 코드 불러오기
1. `~/.gitconfig`에 아래의 내용(alias)을 복사 붙여넣기한다.

```
# for github remotes
[alias]
  pr  = "!f() { git fetch -fu ${2:-$(git remote |grep ^upstream || echo origin)} refs/pull/$1/head:pr/$1 && git checkout pr/$1; }; f"
  pr-clean = "!git for-each-ref refs/heads/pr/* --format='%(refname)' | while read ref ; do branch=${ref#refs/heads/} ; git branch -D $branch ; done"
# for bitbucket/stash remotes
  spr  = "!f() { git fetch -fu ${2:-$(git remote |grep ^upstream || echo origin)} refs/pull-requests/$1/from:pr/$1 && git checkout pr/$1; }; f"
```

2.  `git pr [PR 번호] [remote 저장소]` 와 같이 명령어를 입력하면 해당 브랜치를 checkout까지 해서 가져올 수 있다.

- 참고 링크: <https://gist.github.com/gnarf/5406589>
