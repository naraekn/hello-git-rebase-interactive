Git Rebase Interactive
===

## 개념
### rebase 란?

> Reapply commits on top of another base tip

[git-rebase 공식문서](https://git-scm.com/docs/git-rebase#_name)

### rebase --interactive 옵션이란

> Make a list of the commits which are about to be rebased. Let the user edit that list before rebasing. This mode can also be used to split commits

[git-rebase 공식문서 - interactive](https://git-scm.com/docs/git-rebase#Documentation/git-rebase.txt--i)


## 사용방법
터미널 기준으로 설명합니다!

### STEP1. rebase interactive option 실행시키기

base를 삼고싶은 커밋을 기준으로 rebase 를 해주세요
   
이 때, unstaged changes가 있는지 확인해주세요! unstaged changes가 있으면 rebase를 진행할 수 없습니다

```
git rebase -i [base가 되는 커밋해시]
```

### STEP2. 수정해야할 커밋과 커맨드를 선택하기

STEP1을 실행시키면 텍스트 편집기가 열리고 아래와 같이 "base 커밋 이후의 커밋들"이 나열된 텍스트 편집기가 열릴 것입니다. 그리고 각 커밋 앞에는 `pick` 과 같이 rebase 커맨드가 있을 것입니다.

```
pick 168accfb 첫번째 커밋
pick 61fc134c 두번째 커밋
pick c8303295 세번째 커밋
pick 483f1217 네번째 커밋
pick 6010fea8 다섯번째 커밋
```

이 커밋들 앞에 있는 명령어를 수정하여 커밋의 히스토리를 수정할 수 있습니다

주로 사용을 많이 하는 커맨드는 아래와 같습니다.

* pick (기본옵션): 해당 커밋을 사용한다
* edit: 해당 커밋을 사용할 것이지만 수정을 일시정지한다.
* reword: 커밋은 사용하지만 커밋메시지를 수정한다
* squash: 전 커밋과 합치고 합친 커밋의 커밋메시지를 수정한다
* fixup: 전 커밋과 합치고 이전 커밋 메시지를 따라가겠다
* drop: 해당 커밋을 삭제하겠다

### STEP3-1. edit 옵션 활용하기

만약, 두번째 PR의 내용을 수정하고 싶다고 가정해보겠습니다.
그럴 때는 두번째 커밋의 명령어를 `edit`으로 바꾸고 텍스트 편집기를 닫습니다.

```
pick 168accfb 첫번째 커밋
edit 61fc134c 두번째 커밋
pick c8303295 세번째 커밋
pick 483f1217 네번째 커밋
pick 6010fea8 다섯번째 커밋
```

텍스트 편집기를 닫고나면 우리는 두번째 커밋이 만들어진 시점으로 "시간여행"을 하게 됩니다.
로그를 확인해보면 첫번째, 두번째 커밋만 남아있다는 것을 확인할 수 있습니다. 

이제 "두번째 커밋"을 수정할 준비가 끝났습니다.
두번째 커밋을 리셋한 다음 수정사항을 반영하여 다시 두번째 커밋을 만들어줍니다.

```
git reset HEAD^
(수정하기)
git add .
git commit -m "두번째 커밋"
```

모든 수정이 끝났다면 아래 명령어를 통해 rebase 내용을 반영해주세요!

```
git rebase --continue
```

### STEP3-2. squash 옵션 활용하기

첫번째, 두번째, 세번째 커밋을 합치고 합친 커밋의 커밋메시지를 "첫번째~세번째 커밋"으로 변경하고 싶다면 squash 옵션을 사용할 수 있습니다

```
pick 168accfb 첫번째 커밋
squash 61fc134c 두번째 커밋
squash c8303295 세번째 커밋
pick 483f1217 네번째 커밋
pick 6010fea8 다섯번째 커밋
```

편집기를 닫고나면 첫번째, 두번째, 세번째 커밋의 모든 커밋메시지가 나열된 새로운 텍스트 편집기가 다시 열리는데 여기서 합친 커밋의 커밋메시지를 작성해주면 됩니다!

### STEP3-3. 커밋순서바꾸기

커밋의 순서를 바꾸고 싶다면 커밋의 순서를 바꿔주면 됩니다!
다만, 순서를 변경할때 변경할 커밋이 이전 커밋에 "의존적인" 커밋인지는 확인해주세요! 만약, 의존적인 커밋이라면 충돌이 발생할 거예요

```
pick 6010fea8 다섯번째 커밋
pick 168accfb 첫번째 커밋
pick c8303295 세번째 커밋
pick 61fc134c 두번째 커밋
pick 483f1217 네번째 커밋
```

### rebase 를 하다가 망했어요

edit을 했는데 원래대로 돌아가고싶거나 rebase중 충돌이 발생하면 abort를 해주세요!

```
git rebase --abort
```

하지만, 처음 연습할때는 브랜치를 백업해놓고 연습하기를 추천합니다! 원본을 못찾아서 당황스러운 일이 생기지 않도록!


## 실습

### mission-01

두번째 커밋과 세번째 커밋을 합치고 합친 커밋의 메시지를 "두번째와 세번째 커밋"으로 만들어주세요
(힌트: `s`)

### mission-02

네번째 커밋의 커밋메시지를 `네번째 커밋 (으악 커밋메시지를 잘못썼다)`에서 `네번째 커밋`으로 변경해주세요

### mission-03

`edit`을 사용하여 첫번째 커밋의 변경내용을 `첫번째 커뮈이이잇 (으아아아 오타를 만들어버렸다)`에서 `첫번째 커밋`이 되도록 변경해주세요

### mission-03 (난이도 up 버전)

mission-03을 다른 방법으로도 해봅시다!
`fixup`을 사용해서 mission-03을 달성하려면 어떻게 해야할까요?

### mission-04

세번째 커밋과 네번째 커밋의 순서를 변경해주세요

### mission-05

여섯번째 커밋의 변경내용을 `여섯번째 커뮝 (으악 오타)`에서 `여섯번째 커밋`으로 변경하려고 합니다. 변경을 하려고 하면 "충돌"이 일어날텐데, 변경하려고 하는 내용을 버리고 원상복귀 시켜보세요!
(힌트: abort)

### mission-06

여섯번째 커밋의 변경내용을 `여섯번째 커뮝 (으악 오타)`에서 `여섯번째 커밋`으로 변경해주세요. 변경을 하려고 하면 "충돌"이 일어날텐데, 충돌을 해결하여 14번째 줄이 `여섯번째 커밋 일곱번째 커밋`이 되도록 수정해주세요
