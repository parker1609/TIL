# GIT 명령어

## Github Pull Requests 충돌
- `git remote add upstream` -> `git fetch upstream 자신의 github_id` -> `git reset --hard upstream/자신의 github_id`
- `git push --force [remote repository]`
- [참고 링크](https://planbs.tistory.com/entry/Git-Pull-request%EC%97%90%EC%84%9C-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94-%EC%B6%A9%EB%8F%8C-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)


## git add 특정 파일 취소하기
- 참고 링크: <https://gmlwjd9405.github.io/2018/05/25/git-add-cancle.html>


## PR 번호 코드 불러오기
- `~/.gitconfig`에 alias 내용을 복붙하고 `git pr [PR 번호] [remote 저장소]` 이런식으로 명령어를 입력하면 브랜치 checkout까지 해서 가져올 수 있다.
- 참고 링크: <https://gist.github.com/gnarf/5406589>
