# dJango 설치 및 실행방법



java기반의 웹 프로그래밍 : spring(스프링)

python 기반의 웹 프로그래밍 : django(장고)



python+django+vscode/pycham

anaconda+django+vscode/pycham



#vscode에서 Ctrl+`한 다음 Terminal에서 아래 내용을 입력한다.

-

#conda version

$ conda -V(대문자)

   conda 4.10.1



#가상환경(Virtual Enviroment)을 위한 프로젝트의 독립된 공간을 생성

File > open Folder... > ToDoList-with-Django



#가상환경생성

$ conda create -n ToDoList python=3.8.8 anaconda

-n:이름 / 해당 가상환경(폴더)에 아나콘다 설치



#생성된 가상환경 리스트 확인

$conda info --envs

#conda environments:
#
base                  *  C:\Users\jini8\anaconda3
ToDoList                 C:\Users\jini8\anaconda3\envs\ToDoList

#bash에서 콘다를 실행 하도록 해줌

$ conda init bash --all 

vscode의 bash에서 우클릭 > kill terminal 하고 재실행 시키기

#가상환경 활성화

$ conda activate ToDoList

#비활성화

$ conda deactivate

#가상환경 삭제

$ conda remove -n ToDoList --all

#dJango(장고)를 ToDoList 가상환경에 설치하기 위해서 활성화

#django(장고)설치

$ conda install django



#django project 생성

$ django-admin startproject myproject

setting : 환경설정

urls : url 연결

wsgi : 



#현재 위치가 d/mybiz/bigdata/ToDoList-with-Django이므로 myprojecet으로 이동을 해야한다.



#server 실행

$ python manage.py runserver

#chrome을 실행 한다음 다음 주소를 입력한다.

http://127.0.0.1:8000/

페이지에 장고 ``설치가 잘 되었다``고 나오면 성공!

