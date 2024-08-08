#  title : 깃허브 협업프로젝트 하는 법


## 1. 로컬리포지토리에 있는 파일을 깃허브에 올리는 법

* 로컬디스크에 새로운 폴더를 만들고 안에 작업물을 넣는다.
* git clone '협업프로젝트를 진행하는 리포지토리 주소'를 넣는다.
로컬 디스크에서 폴더 간 이동 cd '폴더 주소'로 이동가능하다.

<img src="https://github.com/user-attachments/assets/4e86776a-178f-4687-ab07-458f10e2f48b" width="500" height="300">

* git add "해당 작업물 이름"의 형태로 github에 해당 작업물을 저장한다.
* git commit -m "Add text File [document.txt]"라고 commit 명령어를 통해 변경사항을 깃허브에 히스토리로 기록해준다.

<img src="https://github.com/user-attachments/assets/2494d5bb-b8ee-45ce-b316-f973e2b42793" width="500" height="300">

* git push를 통해 github에 작업물을 올린다.

<img src="https://github.com/user-attachments/assets/c94d2707-05e4-46d5-bf66-7953fe3809db" width="500" height="300">


## 2. 깃허브에 올린 파일을 로컬리포지토리에 옮기는 법

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

## 3. 깃허브에 올린 파일 수정하는 법

* 예시를 들기 위해 예시.py를 깃허브에 올린다.

<img src="https://github.com/user-attachments/assets/391cd94e-b45b-4ebd-9e98-7cef85fc0b71" width="500" height="300">

* 이후에 예시.py를 임의로 수정해본다.

<img src="https://github.com/user-attachments/assets/fd007e87-11d2-448a-8c3d-d50c4553101c" width="500" height="300">

* 만약에 파일을 이전 버젼으로 되돌리고 싶다면 git checkout -- 예시.py 를 통해 이전 버젼으로 수정가능하다.

* 다시 돌아와 수정된 파일을 깃허브에 올려본다.

<img src="https://github.com/user-attachments/assets/2d30d87d-1a45-4f64-9b8f-456ff82ffdbf" width="500" height="300">

* 깃허브에 수정이 잘 되었나 확인해보면 다음과 같다.

<img src="https://github.com/user-attachments/assets/a88b4adf-1ae9-44e4-a4b8-908e2e3a4188" width="500" height="300">



















