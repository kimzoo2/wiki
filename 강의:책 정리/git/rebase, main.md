
### rebase
브랜치의 기반을 다른 커밋으로 옮기는 행위 

> 현재 브랜치의 커밋들을 다른 브랜치의 최신 커밋 뒤로 옮긴다.
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
git rebase [기준될 브랜치] [추가할 브랜치]
```


ex)
현재 브랜치 main
수정한 branch feat/47

```
git rebase main feat/47
```

-> main 브랜치에 feat/47의 변경사항 커밋을 더한다


3. 