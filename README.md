# Docker ?
- [My Velog](https://velog.io/@kitree/Docker-%EC%A0%95%EB%A6%AC)

Docker의 기본 개념을 정리한 글입니다.

---
# Docker-Tutorial
- [Docker Tutorial for Beginners](https://www.youtube.com/watch?v=GFgJkfScVNU)

해당 영상을 통해 Docker를 실습한 내용입니다.

## Hello Docker Demo
#### 1. hello-docker 폴더 생성

#### 2. hello.js 파일 생성
```jsx
console.log('hello docker')
```
#### 3. Dockerfile 생성
```bash
FROM node:20-alpine

WORKDIR /app

COPY . .

CMD node hello.js
```
#### 4. Docker 이미지 빌드 하기
```bash
docker build -t hello-docker .
```
#### 5. Docker 이미지 목록 확인
```bash
docker images 
```
#### 6. Docker 컨테이너 실행
```bash
docker run hello-docker
```
#### 7. Docker 컨테이너 생성 후 대화형 셸 실행
```bash
docker run -it hello-docker sh

입력 후 진행

node hello.js
```

---
## React Docker Demo

#### 1. 리액트 프로젝트 생성하기
```bash
npm create vite@latest react-docker
```
#### 2. Dockerfile 생성
- [코드](https://github.com/skagn4929/Docker-Tutorial/blob/main/react-docker/Dockerfile)

#### 3. .dockerignore 파일 생성
```bash
node_modules/
```
#### 4. Docker 이미지 빌드하기 
```bash
docker build -t react-docker .
```
#### 5. Docker 컨테이너 실행
```bash
docker run react-docker
```
#### 6. 포트 매핑
```bash
docker run -p 5173:5173 react-docker
```
#### 7. package.json 수정
```json
"dev": "vite --host"
```

#### 8. App.tsx 내용 변경 후 업데이트하고 확인하기
```bash
docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules react-docker

# 명령어 해석 

-v "$(pwd):/app": 현재 작업 디렉토리를 컨테이너 내의 /app 경로로 마운트합니다. 이를 통해 소스 코드를 컨테이너 내부로 복사하거나, 컨테이너에서 생성된 파일을 호스트로 가져올 수 있습니다.

-v /app/node_modules: 컨테이너 내의 /app/node_modules 디렉토리를 호스트의 /app/node_modules로 마운트합니다. 이렇게 함으로써 로컬에서 이미 설치한 Node.js 패키지를 재사용하고, 컨테이너에서 패키지를 새로 설치하지 않도록 합니다.

이 명령어를 통해 React 애플리케이션을 실행하는 동시에 소스 코드를 수정하면 실시간으로 반영되며, Node.js 패키지는 로컬에서 관리됩니다.
```

#### 9. dockerhub에 게시하기
 1) `docker login`
 2) `docker tag react-docker kinamhoo/react-docker`
 3) `docker push kinamhoo/react-docker`

---
## Docker Compose (React Demo)
Docker Compose는 여러 컨테이너로 구성된 애플리케이션을 정의하고 실행하기 위한 도구입니다. 주로 개발 및 테스트 환경에서 여러 서비스를 효율적으로 관리하고 구성하는 데 사용됩니다. Docker Compose를 사용하면 각 서비스 간의 의존성을 관리하고 애플리케이션을 쉽게 배포할 수 있습니다.

#### 1. 프로젝트 생성
```bash
npm create vite@latest vite-project
```
#### 2. 컨테이너에서 프로젝트를 실행하는 데 필요한 파일로 프로젝트를 초기화
```bash
docker init

입력 후

해당 순서로 진행

Node -> npm -> No -> npm run dev
```
#### 3. Dockerfile 수정
- [코드](https://github.com/skagn4929/Docker-Tutorial/blob/main/vite-project/Dockerfile)

#### 4. compose.yaml 파일 수정
- [코드](https://github.com/skagn4929/Docker-Tutorial/blob/main/vite-project/compose.yaml)

#### 5. 컨테이너를 생성하고 시작하기
```bash
docker compose up
```
---
## Docker Compose Watch(Mern Demo)
서비스를 위한 빌드 컨텍스트를 보고 파일이 업데이트될 때 컨테이너를 재구성 하거나 새로 고칩니다.

#### 1. [starter_mern-docker](https://drive.google.com/file/d/15Yqkb6rNPv6DEfT6zuIHDKchzGkOUblZ/view?pli=1) - 다운로드 후 내 프로젝트로 복사

#### 2. frontend 폴더로 이동
- Dockerfile 생성 (이전 내용 복붙)
- .dockerignore 파일 생성 (내용 : `node_modules`)

#### 3. backend 폴더로 이동
- Dockerfile 생성 (이전 내용 복붙 후 `EXPOSE 8000`, `CMD npm start`로 수정)
- .dockerignore 파일 생성 (내용 : `node_modules`)

#### 4. compose.yaml 파일 생성 
- [코드](https://github.com/skagn4929/Docker-Tutorial/blob/main/mern-docker/compose.yaml)

#### 5. 컨테이너를 생성하고 시작하기
```bash
docker compose up
```
#### 6. 실시간 업데이트 확인
```bash
docker compose watch
```
> - docker 이미지 (web, api, mongo) 3개 생성 됨.
> - 컨테이너 안에 (db, api, web) 3개 돌아가는 중

---
## Full Stack Next.js 14 Demo
#### 1. [starter_next-docker](https://drive.google.com/file/d/1edSiwP0AwtKblE5y-ZlzIGfq2lcDJBPF/view) - 다운로드 후 내 프로젝트로 복사

#### 2. 컨테이너에서 프로젝트를 실행하는 데 필요한 파일로 프로젝트를 초기화
```bash
docker init

입력 후

해당 순서로 진행

Node -> npm -> No -> npm run dev -> 3000
```
#### 3. Dockfile 수정
```bash
FROM node

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD npm run dev
```
#### 4. compose.yaml 파일 수정
```
version: "3.8"

services:
  frontend:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 3000:3000
    develop:
      watch:
        - path: ./package.json
          action: rebuild
        - path: ./next.config.js
          action: rebuild
        - path: ./package-lock.json
          action: rebuild
        - path: .
          target: /app
          action: sync
    environment:
      - DB_URL=mongodb+srv://sujata:rnZzJjIDr3bIDymV@cluster0.hnn88vs.mongodb.net/

volumes:
  tasked:
```

#### 5. 컨테이너를 생성하고 시작하기
```bash
docker compose up
```

#### 6. 실시간 업데이트 확인
```bash
docker compose watch
```
--- 
### * 기타 Docker 명령어
- 현재 컨테이너 목록 : `docker ps` 
- 전체 컨테이너 목록 : `docker ps -a`
- 해당 컨테이너 중지 : `docker stop 해당id`
- 해당 컨테이너 중지하고 제거하기 : `docker rm 해당id --force`
- 중지된 컨테이너 제거하기 : `docker container prune`
