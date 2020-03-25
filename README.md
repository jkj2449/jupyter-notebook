jupyter-notebook
=

#### 1. jupyter-notebook 설치
```
sudo apt-get update
sudo apt-get install python3 (우분투에 기본적으로 설치되어 있음)
sudo apt-get install python3-pip
sudo pip3 install notebook

파이선 및 파이선 파이프 설치 후 주피터 노트북 설치
```

#### 2. jupyter-notebook 비밀번호 설정
```
python3
from notebook.auth import passwd
passwd()
비밀번호 입력 후 hash값 복사
exit()
```

#### 3. jupyter-notebook 환경설정 파일
```
jupyter notebook --gernerate-config
sudo vim 주피터 노트북 환경설정 파일 경로

아래 내용 작성

c = get_config();
c.NotebookApp.password = u'hash값'
c.NotebookApp.ip = '서버 아이피'
c.NotebookApp.notebook_dir = '/' (디렉토리 설정)
```

#### 4. jupyter-notebook 실행
```
sudo jupyter-notebook --allow-root (주피터 노트북 루트 권한으로 실행)
ctrl+c로 종료 후 bg
disown -h /ssh (연결을 끊어도 해당 프로세스가 종료되지 않기 위해 소유권 포기)

ssl 설정을 위해 종료

sudo netstat -nap | grep 8888
sudo kill -9 8888
```

#### 5. ssl 인증서 생성
```
cd ~
mkdir ssl
cd ssl
sudo openssl req -x509 -nodes -days 365 -newkey ras:1024 -keyout "cert.key" -out "cert.pem" -batch
```

#### 6. jupyter-notebook ssl 설정
```
sudo vim 주피터 노트북 환경설정 파일 경로

아래 내용 추가

c.NotebookApp.certfile = u'/home/ubuntu/ssl/cert.pem'
c.NotebookApp.keyfile = u'/home/ubuntu/ssl/cert.key'
```

#### 7. jupyter-notebook 서비스 등록
```
sudo jupyter-notebook --allow-root (주피터 노트북 루트 권한으로 실행)
ctrl+c 종료
which jupyter-notebook (경로 확인)

sudo vim /etc/systemd/system/jupyter.service

아래내용 추가

[Unit]
Description=Jupyter Notebook Server

[Service]
Type=simple
User=ubuntu
ExecStart=/usr/bin/sudo /usr/local/bin/jupyter-notebook --allow-root --cofing=/home/ubuntu/.jupyter/jupyter_notebook_config.py

[Install]
WantedBy=multi-user.target
```

#### 8. jupyter-notebook 서비스 실행
```
sudo systemctl daemon-reload
sudo systemctl enable jupyter
sudo systemctl start jupyter
sudo systemctl status jupyter
```
