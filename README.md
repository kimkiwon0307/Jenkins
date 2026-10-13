# Jenkins

<details>
  


<summary><h2>1장 Jenkins 기본개념과 환경설정
</h2></summary> 

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
<details>
  


<summary><h2>Jenkins Job: Freestyle vs Pipeline</h2></summary> 

## Jenkins Job이란?
1. Jenkins에게 어떤 작업을 수행할지 정의해야 한다.
2. Jenkins에 등록하는 작업 단위를 Job이라고 한다.

## Freestyle Job 이란?
1. Freestryle은 Jenkins 웹 화면에서 설정을 클릭하면서 작업을 만드는 방식이다.
2. Jenkins Dashboard → New Item → Freestyle Project → Build Steps → Execute Shell → 명령어 입력
3. Job을 실행하면 Jenkins가 해당 명령어를 수행한다.

## Pipeline Job이란?
1. Pipeline은 자동화 작업을 코드로 정의하는 방식이다.
2. 대표적으로 Jenkinsfile이라는 파일에 Groovy 기반의 Pipeline 문법을 작성한다.

## Freestyle VS Pipeline
1. 실무에서는 Pipeline 많이 쓴다.
2. Pipeline을 사용하면 이러한 흐름을 코드로 관리할 수 있다.
3. Jenkinsfile을 GitHub에 저장하면 누가 언제 배포 절차를 수정했는지 추적할 수 있다.

## 실습 Freestyle 프로젝트 생성하기

# Jenkins Chapter 02 — 확인문제 및 정답

## 2-1. Freestyle vs Pipeline

**Q1. Jenkins Job이란 무엇인가요?**

**정답:** Jenkins가 실행할 작업을 정의한 단위입니다. 빌드, 테스트, 배포, 서버 점검 등을 수행할 수 있습니다.

**Q2. Freestyle Job이란 무엇인가요?**

**정답:** 주로 Jenkins 웹 UI에서 빌드 단계와 실행 명령어 등을 설정하는 Job입니다.

**Q3. Pipeline Job이란 무엇인가요?**

**정답:** 빌드, 테스트, 배포 등의 자동화 작업 흐름을 코드로 정의하는 Job입니다.

**Q4. Freestyle과 Pipeline의 가장 큰 차이는 무엇인가요?**

**정답:** Freestyle은 주로 UI 설정 기반이고, Pipeline은 코드 기반으로 작업 흐름을 정의합니다.

**Q5. Freestyle에서는 CI/CD를 구현할 수 없나요?**

**정답:** 구현할 수 있습니다. 다만 복잡한 작업 흐름과 변경 이력 관리는 Pipeline이 더 편리합니다.

---

## 2-2. Freestyle 프로젝트 실습

**Q6. Freestyle Job에서 Linux 명령어를 실행하는 빌드 단계는 무엇인가요?**

**정답:** `Build Steps → Execute shell`입니다.

**Q7. Jenkins의 Workspace란 무엇인가요?**

**정답:** Jenkins가 소스코드를 가져오거나 빌드 작업을 수행하는 작업 디렉터리입니다.

예: `/var/lib/jenkins/workspace/freestyle-practice2`

**Q8. Console Output의 역할은 무엇인가요?**

**정답:** Job의 실행 명령어, 출력 결과, 오류 및 최종 빌드 상태를 확인하는 것입니다.

**Q9. Running as SYSTEM과 `whoami` 결과가 `jenkins`인 것은 어떤 차이가 있나요?**

**정답:** SYSTEM은 Jenkins 내부 권한이고, `jenkins`는 실제 Linux 명령어 실행 계정입니다. SYSTEM이 Linux root라는 뜻은 아닙니다.

**Q10. `Finished: SUCCESS`가 의미하는 것은 무엇인가요?**

**정답:** Jenkins가 설정한 작업을 성공 상태로 종료했다는 뜻입니다. 실제 서비스 전체의 정상 동작까지 보장하지는 않습니다.

---

## 2-3. Jenkins 타임존 변경

**Q11. UTC와 한국 표준시 KST의 차이는 무엇인가요?**

**정답:** KST는 UTC보다 9시간 빠릅니다.

**Q12. Ubuntu 서버의 현재 시간대를 확인하는 명령어는 무엇인가요?**

**정답:** `timedatectl`

**Q13. Ubuntu 서버 시간대를 한국으로 변경하는 명령어는 무엇인가요?**

**정답:** `sudo timedatectl set-timezone Asia/Seoul`

**Q14. Jenkins JVM의 기본 시간대를 확인하는 Groovy 코드는 무엇인가요?**

**정답:** `println java.util.TimeZone.getDefault().getID()`

**Q15. Jenkins JVM의 시간대를 한국으로 설정하는 옵션은 무엇인가요?**

**정답:** `-Duser.timezone=Asia/Seoul`입니다. Ubuntu 운영체제 시간대와 Jenkins JVM 시간대는 별도로 관리될 수 있습니다.

---

## 2-4. Pipeline 프로젝트 실습

**Q16. `pipeline {}`의 의미는 무엇인가요?**

**정답:** Declarative Pipeline의 전체 구조를 정의합니다.

**Q17. `agent any`의 의미는 무엇인가요?**

**정답:** 사용 가능한 Jenkins 실행 노드에서 Pipeline을 실행하도록 지정합니다.

**Q18. `stages`, `stage`, `steps`의 차이는 무엇인가요?**

**정답:**
- `stages`: 여러 작업 단계의 묶음
- `stage`: 개별 작업 단계
- `steps`: 해당 단계에서 실행할 작업

**Q19. `sh`와 `echo`의 차이는 무엇인가요?**

**정답:**
- `sh`: 실행 노드에서 Linux Shell 명령어 수행
- `echo`: Jenkins 로그에 메시지 출력

**Q20. Jenkinsfile이란 무엇인가요?**

**정답:** Pipeline 정의를 코드로 작성한 파일이며, Git 저장소에 보관해 버전 관리할 수 있습니다.

---

## 2-5. Pipeline 실무 활용 및 장애 대응

**Q21. 실무에서 Pipeline을 선호하는 이유 세 가지는 무엇인가요?**

**정답:**
1. Jenkinsfile을 Git으로 버전 관리할 수 있습니다.
2. Stage별로 작업을 구분하고 실패 지점을 파악하기 쉽습니다.
3. 복잡한 CI/CD 작업 흐름을 코드로 관리하고 자동화하기 편리합니다.

**Q22. Build 성공, Test 실패 시 일반적인 Declarative Pipeline에서 Deploy는 어떻게 되나요?**

**정답:** 앞 단계에서 처리되지 않은 실패가 발생하면 Deploy 단계가 건너뛰어집니다.

**Q23. `error '테스트 실패 발생!'` 명령어의 역할은 무엇인가요?**

**정답:** Pipeline에서 의도적으로 오류를 발생시켜 해당 빌드를 실패 처리합니다.

**Q24. Jenkins 테스트가 성공했는데도 배포 후 502 Bad Gateway가 발생할 수 있는 이유는 무엇인가요?**

**정답:** Jenkins 테스트가 실제 운영 환경의 네트워크, 프록시 설정, WAS 실행 상태 등을 모두 검증하지 않기 때문입니다.

점검 항목:
1. WAS 서비스 실행 상태
2. WAS 리스닝 IP 및 포트
3. WEB에서 WAS로 연결 가능 여부
4. 방화벽 및 Nginx 설정
5. Nginx와 애플리케이션 오류 로그

**Q25. 배포 후 장애를 줄이기 위해 Pipeline에 추가할 수 있는 단계는 무엇인가요?**

**정답:** Health Check 단계입니다. 배포 후 HTTP 응답과 서비스 상태를 검증하고, 실패 시 알림이나 롤백 절차를 실행하도록 설계할 수 있습니다.
</details>

