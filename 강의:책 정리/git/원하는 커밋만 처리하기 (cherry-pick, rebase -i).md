cherry-pick
- 원하는 커밋만 나열해서 체크아웃 중인 HEAD 위에 복사할 수 있다

```git
git cherry-pick C3 C4 C5
```

rebase -i
인터렉티브 리베이스란?
- rebase 명령어를 사용할 때 -i를 같이 사용하는 것
- 원하지 않는 커밋을 뺄 수 있다. (pick을 통해 지정)
- 커밋을 스쿼시 할 수 있다.
- 커밋의 순서를 변경할 수 있다.
- 