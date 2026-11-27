# Jenkins 프로젝트 학습

<img width="1122" height="1402" alt="image" src="https://github.com/user-attachments/assets/fad35f26-eb60-4e37-90ed-dc249d90388b" />



## 2장 1교시 — Freestyle과 Pipeline 기본 자동화

### 1. Freestyle과 Pipeline이란?

Jenkins에서 자동화 작업을 구성하는 대표적인 두 가지 방식이다.

| 구분 | Freestyle | Pipeline |
|---|---|---|
| 설정 방식 | Jenkins 웹 UI | Groovy 기반 코드 |
| 작업 실행 | Build Steps | Stage / Steps |
| 설정 관리 | Jenkins 내부 설정 | Jenkinsfile로 Git 관리 가능 |
| 장점 | 간단한 작업 구성에 편리 | 복잡한 CI/CD 자동화에 적합 |
| 단점 | 복잡한 작업 관리가 어려움 | 문법 학습 필요 |

**핵심:** 간단한 자동화는 Freestyle, 여러 단계의 CI/CD는 Pipeline이 유리하다.

### 2. Freestyle Job

Jenkins 웹 화면에서 자동화 작업을 설정하는 방식이다.

**실행 흐름**

```text
Freestyle Job 생성
       ↓
Build Steps 설정
       ↓
Execute shell
       ↓
Linux 명령어 실행
       ↓
Console Output 확인
```

**Linux 실행 예시**

```bash
set -eu

whoami
pwd

echo "Jenkins Freestyle Lab" > sample.txt
test -s sample.txt
cat sample.txt
```

- `set -e`: 명령어 실패 시 실행 중단
- `set -u`: 정의되지 않은 변수 사용 시 오류
- `whoami`: 실행 사용자 확인
- `pwd`: 현재 작업 경로 확인
- `echo`: 메시지 출력 및 파일 작성
- `test -s`: 파일이 존재하고 비어 있지 않은지 검사
- `cat`: 파일 내용 출력

### 3. Pipeline Job

자동화 작업을 코드로 정의하고 여러 Stage로 구분하는 방식이다.

**Pipeline 기본 구조**

```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'echo "Build" > sample.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'test -s sample.txt'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy simulation'
            }
        }
    }
}
```

※ 실제 애플리케이션 빌드·배포가 아닌 학습용 예제이다.

| 구성 요소 | 의미 |
|---|---|
| `pipeline` | 전체 자동화 작업 정의 |
| `agent any` | 사용 가능한 실행 Node 선택 |
| `stages` | 여러 작업 단계 정의 |
| `stage` | 개별 작업 단계 |
| `steps` | 실제 실행 작업 |
| `sh` | Linux 셸 명령어 실행 |
| `echo` | Jenkins 콘솔 메시지 출력 |

### 4. Pipeline 동작 흐름

```text
Jenkins Pipeline 실행
         ↓
Build Stage (파일 생성)
         ↓
Test Stage (파일 검증)
         ↓
Deploy Stage (배포 모의 실행)
         ↓
SUCCESS
```

**Pipeline의 장점**
- Stage별 실행 결과 확인
- 실패한 단계 파악 용이
- 조건부 실행 및 자동 배포 구성 가능
- Jenkinsfile을 통한 버전 관리

### 5. 장애 대응

**① Job이 Queue에서 대기하는 경우**
- Node Online/Offline 상태 확인
- Executor 사용 현황 확인
- Agent 및 Label 설정 확인

**② Test Stage가 실패하는 경우**

```bash
test -s missing-demo.txt
```

파일이 없으면 테스트가 실패한다.

```text
Build 성공
    ↓
Test 실패
    ↓
Deploy 실행 중단
    ↓
FAILURE
```

**핵심:** 테스트에 실패했다면 검증되지 않은 결과물을 배포하지 않도록 차단해야 한다.

### 6. 핵심 요약

1. **Freestyle = 웹 UI 기반 자동화**
2. **Pipeline = 코드 기반 자동화**
3. **Stage = 작업 단계**
4. **Steps = 실제 실행 작업**
5. **agent any = 사용 가능한 Node 선택**
6. **Jenkinsfile = Pipeline을 코드로 관리**
7. **Test 실패 = 이후 일반 배포 단계 차단**

---

**다음 학습:** 3장 1교시 — GitHub 연동 및 Webhook 자동 실행
