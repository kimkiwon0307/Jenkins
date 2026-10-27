# Jenkins 프로젝트 학습

## 1장 1교시 — CI/CD 개념과 Jenkins의 역할

### 1. CI/CD가 등장한 이유

기존 수동 배포는 담당자가 소스 코드를 가져와 직접 빌드·테스트·배포해야 했다.

**수동 배포의 문제점**
- 반복 작업으로 인한 시간 소모
- 잘못된 버전 배포 등 사람의 실수 발생
- 테스트 누락 가능성
- 배포 과정의 일관성과 이력 관리 부족

**CI/CD의 목적:** 빌드·테스트·배포 과정을 표준화하고 자동화하여 작업 효율성과 안정성을 높이는 것이다.

### 2. CI/CD 핵심 개념

| 구분 | 의미 | 핵심 역할 |
|---|---|---|
| CI | Continuous Integration (지속적 통합) | 코드 통합 및 자동 빌드·테스트 |
| CD | Continuous Delivery (지속적 제공) | 언제든 배포 가능한 상태 유지 |
| CD | Continuous Deployment (지속적 배포) | 검증 통과 후 운영 환경까지 자동 배포 |

**CI와 CD의 차이**
- CI는 변경된 코드를 통합하고 검증하는 과정이다.
- CD는 검증된 애플리케이션을 배포 가능한 상태로 준비하거나 실제 배포하는 과정이다.
- Continuous Delivery는 운영 배포에 수동 승인을 둘 수 있다.
- Continuous Deployment는 검증 통과 후 자동으로 운영 배포한다.

### 3. Jenkins란?

Jenkins는 빌드, 테스트, 배포 등의 작업을 자동으로 실행하고 관리하는 **오픈소스 자동화 서버**이다.

**Jenkins의 주요 역할**
1. GitHub에서 소스 코드 가져오기
2. Gradle/Maven을 이용한 빌드
3. 자동 테스트 실행
4. 빌드 결과물(JAR 등) 생성 및 관리
5. WAS 서버 자동 배포
6. 배포 후 서비스 상태 검증
7. 실패 시 배포 차단 및 설정된 복구 절차 실행

Jenkins가 직접 모든 작업을 처리하는 것은 아니다. Git, Gradle, SSH 등 외부 도구를 정해진 순서와 조건에 따라 실행하도록 관리한다.

### 4. Jenkins CI/CD 동작 흐름

```text
개발자 코드 수정
      ↓
GitHub Push
      ↓
Webhook (Jenkins 실행 요청)
      ↓
Jenkins Pipeline
      │
      ├── Checkout : 소스 코드 가져오기
      ├── Build    : 프로그램 빌드
      ├── Test     : 자동 테스트
      ├── Deploy   : WAS 서버 배포
      └── Verify   : 서비스 정상 확인
      ↓
Spring Boot 애플리케이션 운영
```

※ Build와 Test의 실행 순서는 프로젝트 설정에 따라 달라질 수 있다.

### 5. Jenkins Pipeline 기본 구조

**Pipeline:** 자동화 작업의 실행 순서와 조건을 정의한 것

**Stage:** Pipeline 안에서 작업을 구분하는 단계

**Step:** 각 Stage에서 실제로 실행하는 명령 또는 작업

**Jenkinsfile:** Pipeline을 코드로 정의하고 버전 관리할 수 있는 파일

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Build 실행'
            }
        }

        stage('Test') {
            steps {
                echo 'Test 실행'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy 실행'
            }
        }
    }
}
```

| 구성 요소 | 의미 |
|---|---|
| `pipeline` | Pipeline 전체 정의 |
| `agent any` | 사용 가능한 실행 노드 선택 |
| `stages` | 여러 Stage를 정의 |
| `stage` | 작업 단계 구분 |
| `steps` | 실제 실행할 작업 정의 |
| `echo` | Jenkins 콘솔에 메시지 출력 |

※ 위 코드는 실제 빌드·테스트·배포가 아닌 실행 흐름을 확인하는 예제이다.

### 6. 장애 대응 핵심

**상황 ① 신규 버전 테스트 실패 (배포 전)**

```text
v1.0 정상 운영 중
       ↓
v1.1 빌드 및 테스트
       ↓
테스트 실패
       ↓
신규 배포 차단
       ↓
기존 v1.0 유지
```

- 신규 버전 배포 중단
- 기존 정상 서비스 유지
- 오류 원인 분석 및 수정
- 테스트 재실행 후 배포

**상황 ② 신규 버전 배포 후 장애 발생**

```text
v1.0 정상 운영
       ↓
v1.1 신규 배포
       ↓
서비스 장애 발생
       ↓
장애 영향 확인
       ↓
필요 시 v1.0으로 롤백
```

**핵심 차이**

| 장애 상황 | 대응 |
|---|---|
| 배포 전 테스트 실패 | 배포 차단, 기존 버전 유지 |
| 배포 후 서비스 장애 | 필요시 이전 정상 버전으로 롤백 |

### 7. 자동화의 한계

**자동 배포라고 해서 모든 실수가 사라지는 것은 아니다.**

- Jenkinsfile에 잘못된 서버 IP를 작성하면 연결에 실패하거나 엉뚱한 서버에 배포될 수 있다.
- 배포 경로가 잘못되면 정상적인 서비스 반영에 실패할 수 있다.
- 테스트가 부족하면 오류가 있는 버전이 배포될 수도 있다.

따라서 자동화와 함께 설정 검증, 테스트, 헬스체크, 장애 복구 절차가 필요하다.

### 8. Jenkins와 GitHub Actions 비교

| Jenkins | GitHub Actions |
|---|---|
| Jenkins 서버 직접 운영 | GitHub 호스팅 환경 사용 가능 |
| Jenkinsfile 기반 | YAML 기반 |
| Controller/Agent 구조 | Runner 기반 |
| 유연한 확장 및 내부 인프라 통합 | GitHub와 간편한 연동 |
| 서버 운영·보안·업데이트 관리 필요 | 호스팅 방식은 관리 부담이 상대적으로 적음 |

Jenkins는 CI/CD 시스템 자체를 구축·운영하는 경험을 쌓기에 적합하며, 환경에 따라 GitHub Actions가 더 효율적일 수도 있다.

### 9. Jenkins 기본 점검 명령어

```bash
# Jenkins 실행 상태
systemctl is-active jenkins

# Jenkins 상세 상태
systemctl status jenkins --no-pager

# Jenkins 포트 확인
sudo ss -lntp | grep ':8080'

# Jenkins 최근 서비스 로그
sudo journalctl -u jenkins -n 50 --no-pager
```

※ systemd 서비스로 설치한 Jenkins 기준이다. Docker 설치형은 확인 방법이 다르다.

### 10. 반드시 기억할 핵심

1. **CI = 코드 통합 + 자동 검증**
2. **CD = 배포 준비 또는 자동 배포**
3. **Jenkins = 빌드·테스트·배포를 자동화하는 서버**
4. **Pipeline = 작업의 실행 순서와 조건**
5. **Jenkinsfile = Pipeline을 코드로 정의한 파일**
6. **테스트 실패 = 신규 배포 차단**
7. **배포 후 장애 = 필요시 롤백**
8. **자동화는 실수를 줄이지만 모든 장애를 방지하지는 못한다.**

---

**다음 학습:** 1장 2교시 — Jenkins 내부 구조 (Controller, Agent, Node, Executor, Job, Workspace, Plugin)

<img width="1312" height="1199" alt="image" src="https://github.com/user-attachments/assets/e6dda7d4-0503-411c-9e92-95311d81a97e" />
