# GIT

## 명령어
### commit 정보 변경하기
#### 마지막 commit 정보 변경하기
마지막 commit에 대한 정보를 변경할 때는 `--amend` 옵션을 사용한다.

```
git commit --amend
```

위 명령어를 사용하면 마지막 commit의 정보와 함께 자동으로 텍스트 편집기가 열린다. 여기서 수정할 수 있는 것은 commit 메시지 뿐이다. commt의 정보를 변경하여 저장하여도 반영되지 않는다.

- `--no-edit`: commit 메시지를 수정하지 않음

1. commit 날짜 변경
임의의 날짜로 변경

```
git commit --amend --no-edit --date="Mon 20 Aug 2020 19:19:19 KST"
```

현재 날짜로 변경

```
git commit —amend —no-edit —date="$(date)"
```

2. commit 작성자 변경

```
git commit —amend —no-edit —author="codemcd"
```

#### reabse를 활용하여 commit 정보 변경하기

- 로컬브랜치와 다른 원격 브랜치에 푸시하기
- 원격 브랜치의 커밋 되돌리기
- <https://meetup.toast.com/posts/122>
- rebase
- cherry pick
- reset/revert
-