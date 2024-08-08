#  title : 깃허브 협업프로젝트 하는 법


- ##   로컬리포지토리에 있는 파일을 깃허브에 올리는 법

* 로컬디스크에 새로운 폴더를 만들고 안에 작업물을 넣는다.
* git clone '협업프로젝트를 진행하는 리포지토리 주소'를 넣는다.
로컬 디스크에서 폴더 간 이동 cd '폴더 주소'로 이동가능하다.

<img src="https://github.com/user-attachments/assets/4e86776a-178f-4687-ab07-458f10e2f48b" width="500" height="300">

* git add "해당 작업물 이름"의 형태로 github에 해당 작업물을 저장한다.
* git commit -m "Add text File [document.txt]"라고 commit 명령어를 통해 변경사항을 깃허브에 히스토리로 기록해준다.

<img src="https://github.com/user-attachments/assets/2494d5bb-b8ee-45ce-b316-f973e2b42793" width="500" height="300">

* git push를 통해 github에 작업물을 올린다.

<img src="https://github.com/user-attachments/assets/c94d2707-05e4-46d5-bf66-7953fe3809db" width="500" height="300">


- ## 깃허브에 올린 파일을 로컬리포지토리에 옮기는 법

* 현재까지 배운 내용은 나의 작업물을 깃허브 협업프로젝트 하는 리포지토리에 옮기는 일이었다.

<img src="https://github.com/user-attachments/assets/5cbbd7c8-fef2-4f32-9fbc-2b99241c3e4c" width="500" height="300">

* 이제 반대로 깃허브에 올려진 파일을 로컬리포지토리에 옮겨보자

* 위의 그림에서 git fetch와 git merge를 수행해서 로컬리포지토리에 옮기는데 git pull을 통해 두가지 기능을 한 번에 진행할 수 있다.

* 예시를 들기 위해 로컬디스크에 또다른 새로운 폴더 Education1이라는 폴더를 만든다. 이후에 git clone '깃허브_리포지토리 주소'를 통해 Education1폴더 안으로 복사한다.

<img src="https://github.com/user-attachments/assets/8236b4a3-b827-447b-b57e-b2643cb6d2ec" width="500" height="300">

이와 같은 과정을 거치고 나면 education1폴더 안에 interim_project 폴더가 들어간 것을 확인할 수가 있다.
interim_project안에 새로운 파일(예시.py)을 하나 추가하고 난 뒤에 git add 예시.py를 통해 staging area에 들어간 것을 확인할 수 있다.

<img src="https://github.com/user-attachments/assets/5f91623d-8593-43b6-ac44-be137857a51a" width="500" height="300">

* 여기서 add한 파일을 취소하고 싶다면 git reset 예시.py를 통해 취소 가능하다.

<img src="https://github.com/user-attachments/assets/6f422c9e-cab6-4af0-a82c-c225dacd3d24" width="500" height="300">

- ## 깃허브에 올린 파일 수정하는 법

* 예시를 들기 위해 예시.py를 깃허브에 올린다.

<img src="https://github.com/user-attachments/assets/391cd94e-b45b-4ebd-9e98-7cef85fc0b71" width="500" height="300">

* 이후에 예시.py를 임의로 수정해본다.

<img src="https://github.com/user-attachments/assets/fd007e87-11d2-448a-8c3d-d50c4553101c" width="500" height="300">

* 만약에 파일을 이전 버젼으로 되돌리고 싶다면 git checkout -- 예시.py 를 통해 이전 버젼으로 수정가능하다.

* 다시 돌아와 수정된 파일을 깃허브에 올려본다.

<img src="https://github.com/user-attachments/assets/2d30d87d-1a45-4f64-9b8f-456ff82ffdbf" width="500" height="300">

* 깃허브에 수정이 잘 되었나 확인해보면 다음과 같다.

<img src="https://github.com/user-attachments/assets/a88b4adf-1ae9-44e4-a4b8-908e2e3a4188" width="500" height="300">

- ## 깃허브에 예전 버전으로 수정하는 방법

* git log를 통해 log기록을 확인해보자. git log에서 빠져나오기 위해선 q를 입력하면 된다.

<img src="https://github.com/user-attachments/assets/32bab31b-c920-4016-89ee-6ebfc6aa5ae0" width="500" height="300">

* 만약에 log기록에서 특정 부분으로 돌아가고 싶다면 git reset hard -- "commit 번호"으로 특정 부분으로 돌아갈 수 있다.

<img src="https://github.com/user-attachments/assets/aeb9e68c-c11e-47d1-9e15-167c2cb50f7d" width="500" height="300">

* 그러면 로컬 리포지토리에 저장된 텍스트가 이전 버전으로 돌아간 것을 확인할 수 있다.

* 깃허브 저장소에는 반영이 안 되어있는 것을 확인할 수 있는데 로컬 리포지토리와 동기화 하고 싶다면 git push -f를 통해 가능하다.

<img src="https://github.com/user-attachments/assets/8e160e5f-b944-4f87-a109-c4e2ac4103c6" width="500" height="300">


- ## 깃허브 commit내역 수정하는 방법

* 다시 하나의 파일(예시.py)을 만들어  깃허브에 저장해보자

* commit 메세지를 수정하고싶다면 git --amend를 실행시킨 뒤에 a를 눌러 수정모드로 전환한다.

* 수정하고 싶은 부분을 수정한 뒤에 esc버튼으르 누른 뒤 :wq!를 누르면 자동으로 commit내역이 수정된다.

- ## 깃허브 branch 개요

<img src="https://github.com/user-attachments/assets/ef43e260-5bdb-4a1f-ada2-2c97ca9aa18a" width="500" height="300">

* 위의 그림처럼 Develop branch와 bug fix branch를 통해 master branch를 안정적으로 유지시킬 수 있다.

* git branch에 develop branch를 추가하고 싶다면 다음과 같다.

<img src="https://github.com/user-attachments/assets/351aa47d-1156-4678-b3dc-206e6bda198d" width="500" height="300">

* develop branch를 가리키고 싶다면 checkout을 이용한다.

<img src="https://github.com/user-attachments/assets/7a6b264f-cba7-43e4-aef9-a60ebb8f56a6" width="500" height="300">

* develop branch를 가리킨 상태에서 git add .를 통해 수정사항을 반영하면 main branch는 변화가 없고 develop branch만 변한 걸 알 수가 있다.

<img src="https://github.com/user-attachments/assets/d370b598-ec75-4d27-9de6-7974e030513d" width="500" height="300">

* 만약에 branch를 제거하고 싶다면 다음과 같다.

<img src="https://github.com/user-attachments/assets/24079f7f-711e-47e3-967e-6c9df3016a37" width="500" height="300">

- ## git branch가 서로 충돌할 경우 해결방법

* main branch와 develop branch에 각각 코드를 추가하여 서로 충돌할 수 있도록 만들자.

* merge를 통해 이를 합치려고 하면 오류가 발생한다.

<img src="https://github.com/user-attachments/assets/ffd80070-103d-4e8e-ac33-a487a22c4c48" width="500" height="300">

* 실제 예시.py에 들어가보면 충돌이 발생한 부분이 명시되어있다.

<img src="https://github.com/user-attachments/assets/909c52b7-2379-4ba1-bfef-acc4a6358a2e" width="500" height="300">

* 둘 중 하나를 지우고 하나를 선택하여 다시 저장하면 된다.
* 이후에 git merge develop을 통해 merge가 되었는지 확인하고 push를 통해 깃허브에 올리면 된다.

  



























