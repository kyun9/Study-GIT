# git 기본 명령어

```bash
$ git init
$ git add .
$ git commit -m 'message'
$ git status
```



# git 되돌리기

1. commit 메세지 수정

   ``` 
   $ git commit --amend
   ```

   * git bash에서 vim이 실행

   * 편집모드(i) 상태에서 수정후 esc+:wq

   * push, 즉, 원격저장소에 올린 이후에는 진행하면 충분히 무조건 발생한다

   * 커밋을 다시하게 되면 해시값이 변경되기 때문!

     

2. Stagging area(INDEX)에서 취소하기

   ```  bash
   $ git reset HEAD '폴더명 or 파일명'
   ```

3. git 이력에서 특정 파일 삭제 커밋

   ```  bash
   $ git rm --cached '폴더명 or 파일명'
   ```

   * 한번도 커밋된 이력이 없을 때에는 stagging area에서 취소와 동일함.
   * 다만, 커밋에 포함된 적 있는 경우에는 삭제 커밋이 됨.(실제 파일은 삭제되지 않음!)\

4. 특정 파일 포함해서 다시 커밋

   ``` bash
   $ git add a.py
   $ git commit -m 'a, b 추가'
   $ git add b.py
   $ git commit --amend
   ```

   * commit 메시지를 수정하기전에 stagging area에 변경을 해주면,

     해당 파일까지 포함하여 다시 커밋을 진행함

5. 현재 작업 내역 커밋 시점으로 되돌리기

   예를 들어, 특정 파일을 삭제하였거나 혹은 코드 수정과정에서 오류가 많이 발생하여 직전 커밋시점 상태로 돌아가고 싶을 때 사용이 가능함.

   ```bash
   $ git checkout -- 파일명 or 폴더명
   ```

   # 원격저장소(github) 활용하기

   1. 원격 저장소 설정

      ``` bash
      $ git remote add origin {url}
      ```

   2. 원격 저장소 확인

      ``` bash
      $ git remote -v
      ```

   3. 원격 저장소에 push

      ```bash
      $ git push origin master
      ```

   4. 원격 저장소 삭제

      ```bash
      $ git remote rm origin
      ```

   5.  원격 저장소 복제

      ````bash
      $ git clone {url}
      ````

      * 로컬에 원격 저장소를 받아오고 싶다면, clone 을 통해 가져온다.
      * 이후에는 push - pull 을 통해서 업데이트하면 된다.

   

   #  충돌 해결하기(Merge conflict)

   > 기본적으로 push - pull 하는 과정에서 동일한 파일의 다른 이력이 기록될 경우 충돌이 발생한다!
   >
   > 다른 파일이 수정되는 경우 fast-fowarding을 통해 자동으로 merge가 되기도 함!
   >
   > 이러한 오류를 발생시키지 않으려면, 항상 작업 하기전에 pull을 확인하고, 작업 및 커밋을 한 이후에 push를 하는 것을 습관화 하자!

   

   1. Local A에서 a.txt 작성 후 커밋

   2. Local A에서 원격저장소(Origin)로 push

   3. Local B에서 pull하지 않은 상태에서 a.txt 동일한 라인 작성 후 커밋

   4. Local B에서 원격저장소(Origin)로 push -> push 되지 않음!

   5. 해결 방안 (원격 저장송 변경사항 Local B에서 반영)

      ```bash
      $ git pull origin master
      ```

   6. 충돌 발생(동일 파일 수정시)

      * 어떤 파일에서 충돌되었는지 확인하는 명령어

      ```bash
      $ git log --online --left-right --p  // 충돌 어디서 났는지 
      ```

      * Git 에서 직접 충돌 파일에 기록을 남겨줌

      ``` bash
      <<<<<<<<  HEAD
      집 수정 내용
      ================
      멀캠 수정 내용
      >>>>>>>>>>>>>> 12dfa12asv24asdf
      ```

      * HEAD : 현재 작업하고 있는 곳(Local B)
      * 해쉬값 : pull 을 통해 받아온 변경사항(origin)
      * 해당 하는 위치를 찾아서 직접 수정을 진행함!

   7. merge conflict 해결 commit 남기기

   ```bash
   $ git status
   $ git add .
   $ git commit -m 'Merge conflict 해결'
   ```

   8. 원격 저장소로 push

   ```bash
   $ git push origin master
   ```

   

# D-2 Branch 활용하기

git init 을 하였을 때 (master)는 사실 master 브랜치에 있다라는 사실을 보여주고 있는 것이다.

1. branch 생성

```bash
(master) $ git branch {branch이름}
(master ) $ git branch
*master
{branch이름}
```

2. branch 이동

   ```bash
   $ git checkout {branch이름}
   ```

   * 위 두 명령어를 동시에 실행하려면 아래와 같이 한다.

     ```bash
     $ git checkout -b {branch이름}
     ```

3.  branch 삭제

   ```bash
   $ git branch -d {branch이름}
   ```

4. branch 병합

   ```bash
   (master) $ git merge feature/main
   ```

   master 브랜치에 feature/main 을 병합한다. 

   항상 병합하고 싶은 대상의 브랜치로 옮겨서 진행해야 한다.

# Git merge

## 1. Fast-forwarding

실제로 master 브랜치에 커밋이 발생하지 않았고, 단순히 커밋만 옮기면 되는 경우.

merge 커밋이 발생하지는 않음.

## 2. Auto Merge

브랜치를 나눈 이후에  master 브랜치에 커밋이 발생하였으나, 동일한 파일이 수정되지 않아서 자동으로 병합이 되는 경우, merge 커밋이 발생함.

``` bash
$ git log --graph --oneline  //show commit tree structure
```



## 3. Merge conflict 발생

브랜치를 나눈 이후에 master 브랜치에 커밋이 발생하였고, 동일한 파일이 각자 다른 브랜치에서 수정된 경우 자동으로 merge가 되지 않는다. 따라서 merge conflict가 발생하고, 직접 수정 후 커밋을 해야한다.

``` bash
$ git merge feature/main
```

Git은 충돌이 발생한 파일에 아래와 같이 표기를 해준다. 해당 부분을 찾아서 수동으로 해결 해야한다.

충돌 위치를 파악하기 위해서 "git status"를 통해 확인하자!!

이후

```bash
$ git add .
$ git commit
```

커밋을 하게 되면,  merge 커밋이 발생한다.

# Git stash

현재 변경 사항을 담아 둘 수 있는 임시 공간이 존재한다.

1. 현재 변경사항 담기

   ```bash
   $ git stash
   $ git stash list
   ```

2. 임시 저장사항 불러오기

   ```bash
   $ git stash pop
   ```

   위의 명령어는 apply + drop 과 동일하다.

# 참고 사이트

| TIL 예시         | https://github.com/edutak/algorithms                         |
| ---------------- | ------------------------------------------------------------ |
| git md 정리 내용 | https://gist.github.com/edutak/0b3ec40bdecbc9bad074e8df1e5a7998 |
| git 입문편       | https://backlog.com/git-tutorial/kr/intro/intro1_1.html      |
| 잘 정리된 예시   |                                                              |
| 기술 면접 가이드 | https://github.com/JaeYeopHan/Interview_Question_for_Beginner |
| 논산             | https://github.com/krta2/awesome-nonsan                      |
| 자율 출퇴근      | https://github.com/milooy/remote-or-flexible-work-company-in-korea |
|                  |                                                              |
| 커밋 메시지      |                                                              |
| 본문/명령문      | https://meetup.toast.com/posts/106                           |
| 커밋 낱말 사전   | https://blog.ull.im/engineering/2019/03/10/logs-on-git.html  |
|                  |                                                              |
| git 시각화       | http://git-school.github.io/visualizing-git/                 |
| git cheatsheet   | [ttp://ndpsoftware.com/git-cheatsheet.html](http://ndpsoftware.com/git-cheatsheet.html) |