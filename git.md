
# 필수 vim 명령어

git GUI 툴을 사용하지 않고 쉘(터미널)로 작업할 때는 다수의 git 명령어가 vim을 통해 문서를 편집하도록 요구하므로, 필수적인 vim 명령어를 알아야 git 명령어를 통해 파일 수정을 용이하게 할 수 있다. 

```$ vi <파일명>``` : vim으로 해당 파일을 수정할 수 있는 편집 모드로 진입한다.

먼저 내가 편집하기를 원하는 텍스트에 커서를 옮긴다. 

```i``` : 입력 모드 전환 

원하는 텍스트 수정을 완료하였으면 ```esc```를 눌러서 입력 모드를 종료한다

그리고 ```:```를 누르고 ```wq```를 누르면 현재 상태를 저장하고 종료한다.

추가적인 vim 명령어 참고 : http://gyuha.tistory.com/157



# git flow

대략적인 git의 flow는 다음과 같다. 이 구조를 잘 이해하고 있어야 git의 흐름을 이해할 수 있다.

<br>

![ê´ë ¨ ì´ë¯¸ì§](https://cdn-images-1.medium.com/max/1147/1*S1fNUzJCu7ftH5F-Gk6YOw.png)

<br>

임시 저장 공간인 stash까지 나타난 좀 더 구체적인 이미지이다.

<br>

![ê´ë ¨ ì´ë¯¸ì§](http://2.bp.blogspot.com/-cV5VnaWGWWw/U7KvbXcHvKI/AAAAAAAABjQ/Z1GPENKsTf8/w1200-h630-p-k-no-nu/git+commands.png)

<br>

# git status

git 명령어에 따라 변화하는 Lifecycle을 나타낸 그림이다. flow와 매치시켜 보는 연습이 필요하다.

<br>

![git statusì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼](https://git-scm.com/figures/18333fig0201-tn.png)

<br>

```$ git status``` 명령어를 통해 현재 파일들이 어떤 상태인지 조회할 수 있다.

# git 명령어 요약 정리

기본적인 git 사용법은 이미 알고 있기 때문에 간단한 명령어를 나열하는 것 보다는 주요 상황별로 정리하는 것이 실용적이라고 생각하여 상황 별로 명령어의 흐름을 기록하였다.

## 원격 저장소와 로컬 저장소 연결

1. 원격 저장소를 clone받을 때
   1. clone할 원격 저장소(폴더)가 생성되기를 원하는 경로로 이동
   2. ```$ git clone <url>```
   3. ```$ git remote -v```로 원격저장소가 제대로 연결 되었는 지 확인
      1. ```$ git remote show <원격 저장소 url의 단축 이름>``` 을 입력하면 단축 이름에 맞는 원격 저장소의 상세 정보를 확인할 수 있다.
2. 로컬 저장소를 원격 저장소에 연결할 때
   1. 로컬 저장소(폴더)가 생성되기를 원하는 경로로 이동
   2. ```$ mkdir <폴더명>``` 을 입력하면 폴더가 생성된다.
   3. ```$ cd <만들어진 폴더명>``` 을 입력하여 폴더 안으로 이동한다.
   4. ```$ git init``` 을 입력하여 현재 폴더 안에 .git 폴더를 생성한다.
   5. .git폴더가 만들어진 상태에서 아래 명령어를 이용하여 연결한다.
      ```$ git remote add origin <원격 저장소 url>``` 
      여기서 origin은 원격 저장소 url의 단축이름으로써, 별명이라고 생각하면 된다.

## 변경 사항 저장, 원격 저장소에 반영

1. 파일의 변경 사항을 저장한다.

   1. ``` $ git add -A``` : 모든 변경 사항을 stage에 반영한다. **( 따라서 항상 이 커멘드를 쓰기를 권장한다. )**

   2. ```$ git add .``` : 삭제 된 파일을 제외하고 나머지 파일의 새롭게 생기거나 변경된 사항을 stage에 반영한다.

   3. ``` $ git add -u``` : 새로 생긴 파일을 제외하고 나머지 파일의 변경되거나 삭제된 사항을 stage에 반영한다.

      (https://stackoverflow.com/questions/572549/difference-between-git-add-a-and-git-add) 
2. ```$ git commit -m "<커밋 메시지>"```
   나중에 변경 사유를 알 수 있게 메시지를 입력한다.

3. ``` $ git push <원격 저장소 url의 단축이름> <push할 branch>```
   ```$ git push origin master```
   원격 저장소에 master 브랜치의 변경사항을 반영한다.

## 저장한 커밋 히스토리 조회

1. ```git log <commit id>``` 를 입력하면 해당 commit 정보 상세 정보를 조회할 수 있다.
2. ```$ git log -all --oneline --decorate --graph -10``` 로 한 줄씩 간편하게 조회가 가능하다. 하지만 입력이 너무 길어서 불편하므로 아래 사이트를 참고하여 custom 하는 것이 쓰기 편하다.
   * git log 명령어 커스텀 : https://coderwall.com/p/euwpig/a-better-git-log
   * git graph 명령어 커스텀 : https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs

## 직전 커밋 수정

1. ```$ git commit --amend``` : 마지막 커밋을 수정할 수 있다.

## 인덱싱, 커밋하지 않은 상태의 변경된 작업 내역 삭제

1. ``` $ git checkout <path/filename>``` : 파일 수정 후 인덱스에 추가하지 않은 상태(modified)에서 특정 파일의 작업 내역을 모두 삭제한다.

## 파일, 폴더 관련 기본 명령어

1. 파일 생성 : ```$ echo >>> <파일명>```
2. 폴더 생성 : ```$ mkdir <폴더명>```
3. 파일 이름 변경: ```$ git mv <변경 전 file-name><변경 후 file-name>```
4. 파일 삭제
   1. untracked 상태 (add x, commit x)
      - 파일 제거 : ```$ rm <파일명>```
      - 폴더 제거 : ```$ rm -rf <폴더명>/```
   2. unmodified 상태 (add o, commit x)
      - 파일 제거 :
        - ```$ git rm <파일명> --cached``` : 인덱스에서만 제거
        - ```$ git rm <파일명> -f``` : 인덱스와 working directory에서 모두 제거
      - 폴더를 삭제하는 방법은 조금 다르다. 
        - https://stackoverflow.com/questions/6313126/how-to-remove-a-directory-from-git-repository
   3. staged(add o, commit o)
      - 파일 제거 : ```$ git rm <파일명>``` 

## Branch

branch는 아래와 같이 독립된 환경에서 다른 작업을 할 때 사용한다. 작업이 끝나면 나눠져있는 브랜치를 합쳐서 하나로 만들고, 버그나 운영 상의 문제점을 찾고 이상이 없으면 master에 push해서 새로운 버전이 만들어진다.

<br>

![git flowì ëí ì´ë¯¸ì§ ê²ìê²°ê³¼](https://cdn-images-1.medium.com/max/1400/1*9yJY7fyscWFUVRqnx0BM6A.png)

<br>

1. branch 생성 : ```$ git branch <branch 이름>```
   1. branch 생성 후 곧바로 생성된 branch로 활성 branch 변경 : ```$ git checkout -b <branch 이름>```
      이 때, commit이 모두 되지 않은 상태라면 변경이 불가능하다. 이 상황에서는 ```$ git stash``` 명령어를 통해 commit되지 않은 파일들을 임시 구역에 옮겨 놓거나, commit을 모두 하고 변경해야한다.
      1. stash 관련 명령어
         * stash는 작업 디렉토리와 인덱스의 현재 상태를 임시로 저장해두는 공간이다. 진행 중인 작업 내용을 커밋하기 애매할 때 잠시 저장하는 용도로 사용한다. 임시로 저장된 데이터들을 다른 브랜치로 옮기거나 특정 브랜치에서 이동하여 작업하고 기존의 브랜치로 돌아와 복구하여 작업할 수도 있다. 기본적으로 add 명령어를 통해 인덱싱이 되고 변경된 modified 상태, commit 이전인 staged 상태에만 적용된다.
         * commit되지 않은 변경된 파일들을 stash로 보내기 : ```$ git stash -u```
         * stash에 저장된 데이타 목록 확인 : ```$ git stash list```
         * 특정 stash 상세 정보 확인 : ```$ git show stash@{1}```
         * 가장 최근에 stash된 내역을 되돌리고, 해당 내역을 리스트에서 삭제 : ```$ git stash pop```
         * stash를 반영하지만 list에서는 삭제 하지 않는다. : ```$ git stash apply``` 이 명령어를 이용하면 여러 브랜치에 동일한 stash작업을 적용할 수 있다.
         * 특정 stash 삭제 : ```$ git stash drop stash@{1}```
         * 모든 stash 삭제 : ```$ git stash clear```
2. branch list 조회 : ```$ git branch```
3. 활성 branch 변경 : ```$ git checkout <branch 이름>```
4. branch 삭제 : ```$ git branch -d <branch 이름>```
   1. 병합되지 않은 branch 삭제 : ```$ git branch -D <branch 이름>``` (이 때, 브랜치는 삭제되도 커밋 로그는 삭제되지 않는다. 따라서 브랜치가 삭제되었더라고 커밋 해시명을 이용해서 제거된 브랜치의 작업을 다시 반영할 수 있다.)



## 다수의 커밋 수정

HEAD : 현재 브랜치를 가리키는 포인터이며, 현재 브랜치에 담긴 커밋 중 가장 마지막 커밋을 가리킨다. HEAD가 가리키는 커밋은 바로 다음 커밋의 부모가 된다.

1. reset : HEAD를 이전 상태로 되돌리고 과거부터 다시 시작.

   1. ```$ git reset --mixed <commit 해시명>``` (default) : 기본값이므로 --mixed를 생략할 수 있다. 작업 디렉토리는 유지하면서 인덱스를 HEAD와 함께 되돌린다. 즉 명시한 commit의 내용대로 파일 내용은 바뀌지만 add, commit되어 있던 것은 취소된다.
   2. ```$ git reset --soft <commit 해시명>``` : 현재의 인덱스 상태와 작업 디렉토리 내용을 그대로 보존한 채 커밋만 취소한다. 명시한 commit의 내용으로 파일 내용이 바뀌고 ```$ git add``` 명령어를 통해 인덱싱까지 되어있는 상태로 돌아간다.
   3. ```$ git reset --hard <commit 해시명>``` : 작업 내용까지 모두 삭제할 때 사용한다. 해당 커밋의 직전 커밋 상태로 돌아간다.

2. revert : reset과 달리 취소 이력을 남긴다. 즉, 명시한 커밋 직전의 상태로 돌아가지만 명시한 커밋 로그, 명시한 커밋을 취소한 로그 두 개가 남아있다.

   1. ```$ git revert <commit 해시명>```
   2. ```$ git revert <commit 해시명>^..<commit 해시명>``` : 여러 개 커밋을 반영할 경우 
   3. ```$ git revert < merge commit 해시명>``` : merge 커밋을 되돌릴 경우

3. cherry-pick : 브랜치를 통째로 병합하지 않고, 특정 커밋(현재 브랜치와 다른브랜치 모두 가능)을 반영하고 싶을 때 사용한다.

   1. ```$ git cherry-pick <commit 해시명>```
   2. ```$ git cherry-pick <commit 해시명>^..<commit 해시명>```

4. rebase : 대화형 스크립트를 제공하기 때문에 커밋에 관해 다양한 명령을 수행할 수 있다.

   1. ```$ git rebase -i HEAD~<최근 head부터 편집을 원하는 commit의 갯수>```

   2. 대화형 스크립트가 열리면 내가 지정한 커밋의 default 명령어는 pick으로 되어있다. vim 편집기를 통해 이 pick 명령어를 수정하고 저장 후 종료하면 원하는 명령이 수행된다.

      1. 명령어의 종류

         1. pick : 기본적으로 선택되는 옵션. 변화를 주지 않고 커밋을 그대로 사용
         2. reword : 커밋 내용은 바뀌지 않고 커밋 메시지만 변경
         3. edit : 커밋을 사용하되, 일시 정지한 후 commit --amend 명령 실행. 즉 해당 커밋 내용 수정
         4. squash : 두 개 이상의 커밋을 단일 커밋으로 병합. 적용된 커밋은 이전 커밋으로 합쳐지게 된다.
         5. fixup : squash와 비슷하지만 커밋 메시지는 생략(삭제)된다. 앞선 커밋의 메시지가 적용된다.
         6. exec : 커밋에 대해 원하는 쉘 명령어 실행
         7. drop : 커밋 삭제

         * 명령어 사용법 참조 
           * http://minsone.github.io/git/github-advanced-git-interactive-rebase
           * https://git-scm.com/book/ko/v1/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0

      2. 명령어들은 커밋 별로 적용할 수 있다. 만약 커밋 간의 순서를 바꾸고자 한다면 vim 편집기를 통해 커밋 번호 라인의 순서를 변경해주면 된다.

         1. 줄 전체 이동
            1. `:m` 명령은 원하는 줄을 이동시킨다.
               1. `:m +1`: 현재 커서의 줄을 1줄 아래로 이동
               2. `m -2`: 2 현재 커서의 줄을 1줄(2-1줄) 위로 이동
               3. `m 3`: 현재 커서의 줄을 3행 다음으로 이동. + 혹은 - 기호 없이 사용하면 문서내의 특정 줄로 이동한다.
            2. `v` 혹은 `<Shift>+v`(한 줄 선택)로 여러 줄을 선택한 후 `:m` 명령을 내리면 여러 줄을 한번에 이동 시킬 수 있다.




## 브랜치 병합 

1. merge
   1. 병합하고자 하는 branch로 이동 : ``` git checkout <branch 이름>```
   2. merge 명령어로 병합 : ```git merge <병합할 branch 이름>```
   3. 공통된 파일을 수정했을 경우 충돌 발생
   4. vim 편집기를 통해서 해당 내용 수정
   5. 저장 후 ```$git add .```, ```git commit -m "메시지 내용"```
   6. merge 과정 자체 취소 : ```$ git merge --abort```
2. rebase : 커밋을 나누고, 합치고, 지우고, 순서를 바꾸는 등의 기능이 가능하다. 
   1. ```git rebase -i HEAD~<최근 head부터 편집을 원하는 commit의 갯수>```
      대화형이므로 병합을 제외한 commit 수정, 삭제 순서 변경은 위의 명령어를 통해 모두 가능하다.

   2. vim 편집기를 통해서 pick 명령어를 원하는 명령어로 변경

   3. 병합의 경우에는 명령어가 다르다. 
      ```$ git rebase <병합할 branch 이름1> <병합할 branch 이름2>``` 명령어를 통해 branch를 병합할 수 있다.

      사실 rebase는 병합이라는 번역이 적절하지 않다. rebase는 말그대로 브랜치 분기의 기저가 되는 부분을 다시 지정한다는 뜻이다. 즉, 분기점의 커밋을 다른 커밋으로 변경한다고 생각하면 된다. merge처럼 branch와 branch의 병합을 위해 쓸 수는 있지만, 사용 목적이 전혀 다르다. 만약 merge와 같은 개념으로 이해한다면 rebase를 제대로 이해하지 못한 것과 같다. 글로는 설명이 어려우므로 아래 이미지와 명령어 순서를 통해 알아본다.

      ![](http://theeye.pe.kr/wp-content/uploads/2014/01/rebase_5.png)

      위의 그림을 보면 feature 브랜치의 base는 Old Base라고 표시된 commit이었다. 우리는 rebase 명령어를 통해 feature 브랜치의 base를 master 브랜치의 head commit으로 바꾸고 싶다.

      이 때는, ```$ git rebase master feature```  명령어로 master 브랜치의 head commit을 feature 브랜치의 base로 다시(re) 지정하면 된다.
      
      ```<:Projectname:>/dev $ git rebase upstream/master``` 라는 명령어를 입력한다면 현재 활성화 되어있는 브랜치인 dev를 upstream/master 브랜치 위에 올리겠다는 얘기다. 즉 upstream/master의 head에 현재 브랜치인 dev를 붙이겠다는 뜻.
      
      결과적으로는 feature 브랜치가 master 브랜치의 뒤에 연결되는 형식이 된다. 바꿔 말하면 master 브랜치가 feature branch의 base가 되는 것과 같은 모양이다. 이렇게 연결되는 과정이 간단해보이지만, rebase를 제대로 이해하기 위해서는 feature 브랜치의 base를 변경하는 과정에서 어떻게 충돌이 발생하는지를 이해해야 한다.

      1. 일단 rebase 명령어를 실행하면 기존의 base인 Old Base로 이동한다.

      2. Old Base로 부터 rebase 할 브랜치인 feature가 현재 가리키고 있는 head 커밋까지의 diff를 차례대로 만들어서 임시로 저장해놓는다.

      3. 그리고 feature 브랜치의 base를 master 브랜치의 head commit로 변경한다. 

      4. 그리고 저장되어 있던 diff를 차례대로 적용한다. 이 때, Old Base commit부터 master 브랜치의 head commit까지 달라진 내용과, 현재 적용하는 feature 브랜치의 diff 내용이 달라 충돌이 연속적으로 발생할 수 있다. 

      5. 충돌이 일어난 파일을 수정하고 ```$ git add``` 명령어를 통해 변경사항을 index에 추가한다.

      6. git rebase에서 충돌이 발생하게 되면 현재 local 상태는 (no branch) 상태로 존재하게 된다. 따라서 다시 git rebase 명령으로 conflict를 해제하고 계속 진행(--continue)하거나 취소(--abort)해야한다.

         ```$ git rebase --continue```

      7. 다시 충돌이 발생하면 5~6번을 반복한다.

      8. 위의 단계를 모두 수행하면 branch는 위의 그림과 같은 상태가 된다.

      rebase 관련 참조 링크

      * https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0
      * https://git-scm.com/docs/git-rebase
      * http://pinocc.tistory.com/89
      * 이미지 참조 : http://theeye.pe.kr/archives/1980


## 병합하지 않고 원격 저장소의 데이터를 로컬에 가져오기

이 기능이 언제 쓰일까 싶지만, fork한 저장소의 변경 내용을 가져올 때는 이 기능이 필수적이다. 내가 정보를 가져오고 싶은 fork한 레포지토리의 원본 url을 원격 저장소로 지정해놓고, 내가 쓸 일이 있을 때마다 fetch 명령어로 데이터를 가져와서 최신화 시키면 된다. 이렇게 되면 브랜치가 두 개로 나눠져서 원격저장소의 최신 커밋과 로컬 저장소의 최신 커밋으로 분기된 상태가 되어 있다. 따라서 브랜치를 변경하면서 내용을 확인한 후, 분기된 두 개의 브랜치에서 원하는 정보만 병합하면 된다.

1. ```$ git fetch <원격저장소 url 단축 이름>```
2. 참고 자료 
   1. https://blog.outsider.ne.kr/866
   2. https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98



## 분기를 위한 empty commit 남기기

보통 master branch에 아무런 작업 내역이 없는 상태에서 브랜치를 분기하고 싶을 때가 많은데, 커밋 내용이 아무것도 없으면 브랜치가 분기되어 생성되지 않는다. 따라서 아무런 내용이 없는 커밋을 남기고, 원하는 feature 별로 브랜치를 만들어서 분기하고 시작하는 것이 깔끔하다.

```$ git commit --allow -empty```

참고 : https://coderwall.com/p/vkdekq/git-commit-allow-empty



# + 추가 부분 리서치

##### 메타데이터(Meta-data)
* 자료를 설명하는 데이터(자료의 속성 등)
	* 데이터에 관한 정보의 기술, 데이터 구성의 정의, 데이터 분류를 위한 데이터 등	 
* 데이터에 대한 데이터로써 하위레벨의 데이터를 기술함
	* 상위레벨에서 하위레벨 데이터에 대한 각종 정보(자원의 속성)를 담고있는 데이터 (문서 유형, 문서 구조, 문서 길이 등) 		

##### 리비전(Revision)
SVN(Subversion)이라는 git 이전의 형상관리툴의 논리적 단위이다. git에는 논리적 단위가 커밋이지만, 특정 시점의 저장소 전체의 스냅샷을 나타내는 리비전과 커밋의 의미적 차이가 크지 않으므로 리비전이라고 표현하는 문서들이 많다.

### 참고 자료
* http://www.ktword.co.kr/abbr_view.php?m_temp1=1195
* https://academy.realm.io/kr/posts/360andev-savvas-dalkitsis-using-git-like-a-pro/
* https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%EA%B3%A0%EA%B8%89-Merge
* https://backlog.com/git-tutorial/kr/stepup/stepup3_2.html



### git은 빈 폴더를 tracking하지 않는다.

https://backlog.com/git-tutorial/kr/reference/config.html#sec5 

* 질문 내용 : git 사용시 조금 의문이 드는 점이 있어 질문 드립니다. 

  checkout을 통해 branch를 변경하면서 작업하고 있는데, A라는 branch에서 homework4라는 폴더를 만들고 파일을 생성하여 작업하였습니다. 그리고 B라는 branch로 checkout을 하였는데, 기존에 B에는 homework4라는 폴더가 존재하지 않음에도 불구하고 homework4 폴더가 남아있었습니다. 그런데 폴더 안을 살펴보니 A브랜치에서 작업했던 내용은 없었습니다.

  제가 생각하기에는 homework4폴더는 B branch에 기존에 존재하지 않았던 폴더이므로 branch를 변경하게되면 folder 자체가 사라져야 하는데, 왜 folder껍데기는 남아있고 폴더 안의 파일들만 없어지는 것인지 궁금합니다. 혹시 directory 구조까지 sync할 수 있는 방법은 없는지도 궁금합니다.

* 답변 : Git에서는 빈 폴더는 관리 대상이 되지 않습니다. 따라서 적당한 파일을 폴더에 넣어둠으로써 폴더 디렉토리를 유지하게 해야 합니다. 파일명은 무엇이든 상관없지만 관례적으로 .gitkeep 이라는 파일명이 사용됩니다.
* ```git clean -fd``` 라는 명령을 사용하면 undtracked folder를 제거해준다. 만약 변경한 브랜치에 빈 폴더가 있다면 이 명령어로 제거가 가능하다. 하지만 귀찮으므로 .gitkeep이라는 파일을 생성해서 디렉토리 구조를 계속 유지하는 것이 좋을 듯 하다.

