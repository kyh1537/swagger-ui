# swagger-ui docs
### 1. 개요
- swagger-ui를 이용해 각 서비스별 openAPI를 작성하여, API 문서를 명세 할 수 있게 환경 구축

### 2. 환경 구축(ubuntu 기준)
1. docker 설치 
   1. 우분투 시스템 패키지 업데이트 
      - sudo apt-get update<br/><br/>
   2. 필요한 패키지 설치
      - sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common<br/><br/>
   3. Docker의 공식 GPG키를 추가
      - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -<br/><br/>
   4. Docker의 공식 apt 저장소를 추가
      - sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"<br/><br/>
   5. 시스템 패키지 업데이트
      - sudo apt-get update<br/><br/>
   6. Docker 설치
      - sudo apt-get install docker-ce docker-ce-cli containerd.io<br/><br/>
   7. 도커 실행상태 확인
      - sudo systemctl status docker <br/><br/>
   8. 도커 실행
      - sudo docker run hello-world<br/><br/>
2. swagger-ui를 띄울 서버에 git clone 후 해당 디렉토리에서 docker-compose up -d 실행
3. http://localhost:8080으로 swagger 확인(port는 docker-compose.yml에서 설정 가능)
4. 신규 service API 명세가 필요한 경우
   - .docs/ 하위에 openAPI yaml로 기술한 문서 저장
   -  docker-compose.yml 파일 수정
   - ex)       
     - before -> URLS=[{ url: '/docs/token-service.yaml', name: 'token-service' } ]
     - after -> URLS=[
       { url: '/docs/token-service.yaml', name: 'token-service' },
       { url: '/docs/{new-service}.yaml', name: '{new-service}' }]
     - volumes: 에 경로 추가  -> "- ./docs/{new-service}.yaml:/usr/share/nginx/html/docs/{new-service}.yaml"
     
### 3. 배포(Jenkins)
- github webhook & jenkins github hook trigger 사용
- main Branch push가 일어나면, code 최신화 후 기존 경로에 업데이트
- docker-compose down & docker-compose up -d
