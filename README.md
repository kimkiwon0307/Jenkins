# Jenkins

<details>
  


## 1장 Jenkins 기본개념과 환경설정

### CI/CD를 배우는 이유
1. 빌드, 테스트, 배포 과정을 자동화하는 CI/CD를 사용
2. CI : 코드를 자주 통합하고 빌드,테스트로 검증
3. CD : 검증된 결과물을 배포 가능한 상태로 만들거나 실제로 배포

### Jenkins란 무엇인가
1. Jenkins는 CI/CD 작업을 자동화하는 오픈소스 자동화 서버이다.

### Docker를 사용하는 이유
1. Docker를 사용한 이유는 Jenkins 실행 환경을 격리하고, 설치,관리,재생성을 편리하게 하기 위해서이다.

### Docker로 Jenkins 설치
1. Docker Volume 생성 : docker volume create jenkins_home
2. Jenkins 이미지 다운로드 : docker pull jenkins/jenkins:lts
3. Jenkins 컨테이너 실행 : docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
4. docker ps
5. docker logs jenkins

## Jenkins Plugin
1. Plugin은 Jenkins의 기능을 확장하거나 외부 도구와 연동하기 위해 설치하는 확장 프로그램이다.

## 확인 문제
[Jenkins 섹션 1 확인문제 및 정답]

**1. CI는 무엇의 약자이며, 어떤 목적으로 사용할까요?**
- **정답:** CI는 Continuous Integration(지속적 통합)의 약자이다. 개발자가 변경한 코드를 자주 통합하고 빌드와 테스트를 자동화하여 오류를 빠르게 발견하기 위해 사용한다.

**2. CD는 무엇의 약자이며, CI와 어떤 차이가 있을까요?**
- **정답:** CD는 Continuous Delivery(지속적 제공) 또는 Continuous Deployment(지속적 배포)의 약자이다. CI가 코드 통합과 빌드·테스트를 통한 검증에 중점을 둔다면 CD는 검증된 결과물을 배포 가능한 상태로 유지하거나 실제 환경에 자동 배포하는 데 중점을 둔다.

**3. Jenkins는 어떤 역할을 하는 도구인가요?**
- **정답:** Jenkins는 빌드, 테스트, 배포 등의 CI/CD 작업을 자동화하는 오픈소스 자동화 서버이다.

**4. Jenkins를 Ubuntu에 직접 설치하지 않고 Docker로 실행한 이유는 무엇인가요?**
- **정답:** Jenkins 실행 환경을 격리하고 설치, 관리, 업데이트 및 재생성을 편리하게 하기 위해서이다.

**5. jenkins_home Volume을 생성한 이유는 무엇인가요?**
- **정답:** Jenkins 설정, Job, Plugin 등의 데이터를 컨테이너와 분리하여 보존하기 위해서이다. 컨테이너를 삭제하고 재생성하더라도 동일한 Volume을 연결하면 기존 데이터를 사용할 수 있다.

**6. Docker 명령어에서 -p 8080:8080은 무엇을 의미하나요?**
- **정답:** 호스트의 8080 포트를 Jenkins 컨테이너의 8080 포트에 연결한다는 의미이다. 이를 통해 브라우저에서 Jenkins 웹 화면에 접속할 수 있다.

**7. Jenkins Plugin은 어떤 역할을 하나요? 예시도 하나 들어보세요.**
- **정답:** Jenkins의 기능을 확장하거나 외부 도구와 연동하기 위해 사용한다. 예를 들어 Git Plugin은 Git 저장소 연동 기능을 제공한다.

**8. Jenkins 컨테이너가 실행 중인지 확인하는 명령어는 무엇인가요?**
- **정답:** `docker ps`

**9. Jenkins 컨테이너에 문제가 발생했을 때 로그를 확인하는 명령어는 무엇인가요?**
- **정답:** `docker logs jenkins` 또는 `docker logs --tail 100 jenkins`

**10. 개발자가 GitHub에 코드를 Push했습니다. Jenkins가 자동으로 실행되도록 설정되어 있다면 어떤 순서로 빌드·테스트·배포가 진행될 수 있는지 설명해보세요.**
- **정답:** 개발자가 GitHub에 코드를 Push하면 Jenkins가 변경을 감지하거나 Webhook을 통해 실행된다. Jenkins는 코드를 가져와 Build → Test를 수행하고, 성공하면 설정된 Pipeline에 따라 배포를 진행한다.


</details>

