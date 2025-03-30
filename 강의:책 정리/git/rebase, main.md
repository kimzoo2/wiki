
### rebase
1. 현재 체크아웃된 브랜치에서 다른 브랜치의 최종 커밋으로 이동

```
git checkout [브랜치명]
```


ex)
현재 브랜치 main
수정한 branch feat/47

```
git rebase feat/47
```

-> feat/47의 최종 branch로 이동

2. 특정 브랜치의 최종 커밋으로 이동하기

```
git rebase [특정 브랜치] [현브랜치]
```


ex)
현재 브랜치 main
수정한 branch feat/47

```
git rebase feat/47 main
```

-> feat/47의 최종 branch로 이동