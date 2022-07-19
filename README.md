## Docker란?

컨테이너 기반 오픈소스 가상 플랫폼. 가상환경을 연결해주는 매개체' 라고 생각하면 편하다


</br>

## 필요 개념
- Docker Image: MySQL, Pytorch, Tensorflow, jupyter notebook 등 컨테이너를 실행할 수 있는 실행 파일
- Container: Host OS 상에서 리소스를 구분해 마치 별도의 서버인것처럼 사용할 수 있는 기술, 물리적으로 구분한 것이 아니기에 쉽게 생성 및 제거 가능

</br>

## 전반적 과정 
서버 접속 - container 생성할 dir 만들기 - 필요한 Image pull - Container 생성 및 실행 - 하고싶은 일 하기

</br>

## 예시  "Pytorch Image로 Container 생성해 Jupyter notebook 연결하기"

<h4> 1.서버 접속, dir 만들기</br> </br> 
- 서버 접속은 각자 환경에 맞는 sever와 password 입력</br></br>


    cd '이동하고자하는 경로'
    mkdir '만들려는 dir'
    
    
</br>

<h4>2.필요한 Image pull</h4>
    
    
    docker pull repo/image_name:tag 이때 추가 다운로드하고싶다면 (https://hub.docker.com/) 접속
    
    
</br>
    
<h4>3. 컨테이너 생성 및 실행</h4>


    docker run -itd -v 마운트하고싶은 경로:컨테이너내부경로: --ipc=host --network=host --gpus='"사용하려는 gpus번호"' --name=설정할 컨테이너이름 pytorch/pytorch:latest 
    docker exec -it 컨테이너이름 bash 
  
  
  
</br>
  
<h4>4.하고싶은일- jupyter notebook 실행</br></br>
- 내컨테이너, 즉 workspace에 들어온 상태임. 이제 jupyter notebook 실행해볼 것</h4>


    pip install --upgrade pip
    pip install jupyter
    
    jupyter notebook --generate-config -y
    >> jupyternotebook.config파일 경로
    
    apt -get update
    apt -get install nano
    
    nano '위에서 찾은 jupyternotebook.config파일 경로 복붙'



</br>

<h4>5.목적에 맞게 jupyter notebook config 파일 수정</br> </br> 
- 아래처럼 직접 타이핑해도 되고 아래 주석으로된 걸 풀어서 수정해도 됨</h4>

    
    c.NotebookApp.ip='0.0.0.0'
    c.NotebookApp.open_browser=True
    c.NotebookApp.port=8888
    c.NotebookApp.allow_root=True
    c.NotebookApp.remote_access=True
    
    #config 파일 저장 후 닫기
    ctrl+x > y> enter 클릭
    
    
</br>
   
<h4>6.jupyter notebook 실행</h4>


    jupyter notebook --ip='서버 ip 주소'
    jupyter-lab --ip='210.125.84.154' --port=54325 --allow-root

    
chrome 실행 후, 검색창에 'ip주소:port번호' 클릭해 접속
    


</br>


### *왜 config 파일에서 설정하는 ip주소와 실제 jupyter notebook 실행시 입력하는 ip주소가 다르지?
'docker는 가상환경을 연결해주는 매개체'라고 생각해보자


    나 - docker - 가상환경(주피터놑북) 


docker에 접속할 때 필요한 내 ip주소가 있고 docker에서 가상환경 juptrer notebook에 접속하는 ip주소는 0.0.0.0이다(개발자가 만든거)


따라서 jupyter notebook접속을 위한 ip주소는 '0.0.0.0'으로 설정하되 내pc에서 docker에 접속할 때는 내 ip주소로 해야하므로 주소창에는 위에처럼 하게 된다






