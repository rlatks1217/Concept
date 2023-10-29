# Merge

### fast-forward merge
- master(종합본이 될 branch)이 가리키고 있던 commit(참조 개체 = 동그라미)에서 merge할 branch(dev1)가 가리키고 있던 commit(참조 개체)으로 가리키는 대상을 옮기는 것
- 새로운 commit이 생기는 것이 아님(단순히 master branch가 가리키고 있던 commit에서 옮긴 것이기 때문)
실행 전
![](Pasted%20image%2020231028022628.png)
실행 후
![](Pasted%20image%2020231028022654.png)

### 3-way-merge
- 그림처럼 master와 dev1이 각자 commit을 진행한 상태라고 했을 때 사용
- Base와 같은 위치의 commit 즉, 최적 공통조상을 찾고 base로부터 변경사항이 있는 파일들을 merge commit(새로 생기는 commit을 의미함)에 반영함
실행 전
![](Pasted%20image%2020231028023812.png)
실행 후
![](Pasted%20image%2020231028024425.png)

`fetch` : 원격 저장소의 정보를 로컬 저장소로 가져오는 명령(merge는 아직 안 된 상태)
-> pull은 fetch + merge가 한번에 이뤄지는 명령임

github의 pull request는 merge 방법 3가지를 제공
### Create merge commit
- fast-forward가 이뤄지는 상황과 같은 데에도 불구하고 새로운 merge commit을 생성하는 방법
- 이런 식으로 merge를 할 경우 merge라는 것도 commit에 남기 때문에 어떤 기능들을 만든 다음 merge했는지 확인이 용이해져 가독성이 좋아진다고 할 수 있음

실행 코드
```
git checkout master
git merge --no-ff dev1
```
### Squash and merge
- Create merge commit와 마찬가지로 새로운 merge commit을 생성하는 방식인데 이 방식의 경우 여러 commit을 하나의 commit으로 묶어서 merge하게 됨
- 여러 명이 작업하는 환경에서 하나의 브랜치에 각자가 구현한 여러 기능을 merge할 경우 commit log가 난잡해짐 -> 이것을 사용하면 단순화하여 merge할 수 있기 때문에 가독성이 좋아짐
![](Pasted%20image%2020231028033856.png)

실행 코드
```
git checkout master
git merge squash dev1
git commit -m "squash merge message"
```
### Rebase
- rebase : 말 그대로 base를 옮긴다
rebase 실행 전
![](Pasted%20image%2020231028033038.png)
rebase 실행 후
![](Pasted%20image%2020231028033200.png)
- 이렇게 issue2의 base를 issue1으로 옮겨 하나의 줄기로 만들었음
- rebase 실행 시 base를 바꿀 branch(issue2)에서 실행해야 함

실행 코드
```
git checkout issue2
git rebase issue1

// issue1의 commit(참조 개체)를 issue2의 commit을 가리키도록 옮김
git checkout issue1
git merge issue2
```

-------------------------
* * *
# Cherry-pick

다른 브랜치에 있는 commit을 선택적으로 내 브랜치에 적용시키는 것
- 완성되지 않은 기능과 완성된 기능이 하나의 브랜치에 있을 때 완성된 기능 부분만 쏙 가져와야 하는 상황에서 쓰임
```
git checkout -b cherry // 새로운 branch 생성
git cherry-pick 가져오고 싶은 commit의 ID //여러 개를 가져오고 싶은 경우 공백을 경계로 커밋 아이디를 작성해주면 됨
git push origin 반영할 원격저장소branch
```
중간에 conflict가 생겼을 경우 충돌된 부분을 해결한 후
```
git add .
git cherry-pick --continue
```
를 하면 이어서 cherry-pick을 이어서 cherry-pick을 진행할 수 있음


## PR을 한 번 작성했다면 닫지 말고 추가 커밋을 한다

PR을 이미 한 번 보냈다면, 새로운 PR을 생성할 필요가 없다. 수정이 필요하다면 추가 커밋을 하면 자동으로 반영된다. 단, 미션 제출 기간 이후에는 추가 커밋을 하지 않는다.