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

</details>

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
# Jenkins 학습 기록

## Chapter 02. Jenkins Job — Freestyle & Pipeline

### 1. 학습 목표

- Jenkins Job의 개념 이해
- Freestyle과 Pipeline의 차이 이해
- Freestyle Job 생성 및 Shell 명령어 실행
- Jenkins 타임존 변경
- Pipeline Job 생성 및 Stage별 실행
- 테스트 실패 시 배포 중단 원리 이해

---

### 2. 핵심 개념

#### 2.1 Jenkins Job

Jenkins가 수행할 작업을 정의한 단위이다.

예시:
- 소스코드 다운로드
- 애플리케이션 빌드
- 자동 테스트
- Docker 이미지 생성
- 애플리케이션 배포

#### 2.2 Freestyle vs Pipeline

| 구분 | Freestyle | Pipeline |
|---|---|---|
| 작업 정의 | Jenkins 웹 UI 중심 | 코드 기반 |
| 난이도 | 쉬움 | 문법 학습 필요 |
| Git 버전 관리 | 상대적으로 불편 | Jenkinsfile로 관리 가능 |
| 복잡한 CI/CD | 관리가 어려워질 수 있음 | 적합 |
| 활용 | 단순 자동화 | 다단계 빌드·테스트·배포 |

**핵심:** Freestyle도 CI/CD가 가능하지만, Pipeline은 작업 흐름을 코드로 관리하고 단계별로 검증하기 편리하다.

#### 2.3 Pipeline 주요 문법

| 문법 | 설명 |
|---|---|
| `pipeline` | Pipeline 전체 정의 |
| `agent any` | 사용 가능한 실행 노드 선택 |
| `stages` | 여러 Stage의 묶음 |
| `stage` | 개별 실행 단계 |
| `steps` | 단계 안에서 실행할 작업 |
| `sh` | Linux Shell 명령어 실행 |
| `echo` | Jenkins 로그 출력 |
| `error` | 의도적인 실패 발생 |

---

### 3. 실습 기록

#### 3.1 Freestyle Job 생성

- Job 이름: `freestyle-practice2`
- 실행 방식: Execute shell
- 확인 항목: 사용자, 호스트명, 시간, 서버 가동 시간, 메모리

**실행 결과:** `Finished: SUCCESS`

**확인한 내용:**
- Jenkins는 `jenkins` Linux 계정으로 Shell 명령어를 실행했다.
- Workspace 경로는 `/var/lib/jenkins/workspace/freestyle-practice2`였다.
- Console Output에서 실행 결과를 확인했다.

#### 3.2 Jenkins 타임존 변경

Ubuntu 서버 시간대 변경:

`sudo timedatectl set-timezone Asia/Seoul`

Jenkins JVM 옵션:

`-Duser.timezone=Asia/Seoul`

Jenkins Script Console 확인:

`println java.util.TimeZone.getDefault().getID()`

**실행 결과:** `Asia/Seoul`

#### 3.3 Pipeline Job 생성

- Job 이름: `pipeline-practice`
- 실행 단계: Server Info → Resource Check → Finish

**실행 결과:** `Finished: SUCCESS`

#### 3.4 Pipeline 실패 시나리오

다음 흐름으로 테스트 실패 상황을 재현했다.

`Build 성공 → Test 실패 → Deploy 건너뜀`

Test 단계에서 `error '테스트 실패 발생!'`을 실행했다.

**실행 결과:**
- Build: 성공
- Test: 실패
- Deploy: 실행되지 않음
- Pipeline: `Finished: FAILURE`

**배운 점:** 처리되지 않은 테스트 실패가 발생하면 이후 배포 단계를 실행하지 않도록 구성할 수 있다.

---

### 4. 확인문제 및 정답

#### 2-1. Freestyle vs Pipeline

**Q1. Jenkins Job이란?**

A. Jenkins가 실행할 작업을 정의한 단위이다.

**Q2. Freestyle Job이란?**

A. 주로 Jenkins 웹 UI에서 실행할


</details>

