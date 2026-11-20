<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/fbd323b8-4553-49c8-afd2-09556628ed2e" />


# Jenkins 프로젝트 학습

## 1장 2교시 — Jenkins 내부 구조

### 1. Jenkins 주요 구성 요소

| 구성 요소 | 역할 |
|---|---|
| Controller | Job 관리, 작업 할당, 실행 결과 관리 |
| Agent | 실제 빌드·테스트 작업 실행 |
| Node | Jenkins가 관리하는 작업 실행 환경 |
| Executor | Node에서 동시 실행 가능한 작업 슬롯 |
| Job | 자동화 작업의 정의 |
| Build | Job을 실제 실행한 1회의 기록 |
| Queue | 실행을 기다리는 작업 대기열 |
| Workspace | 소스 코드와 빌드 파일을 다루는 작업 공간 |
| JENKINS_HOME | Jenkins 설정 및 운영 데이터 저장 |
| Plugin | Jenkins 기능을 확장하는 프로그램 |

### 2. Jenkins 내부 동작 흐름

```text
사용자가 Job 실행 요청
        ↓
Controller (작업 관리)
        ↓
Queue (실행 대기)
        ↓
Node / Executor 할당
        ↓
Agent 또는 Built-In Node에서 실행
        ↓
Workspace에서 빌드·테스트 수행
        ↓
Build 결과 기록
(SUCCESS / FAILURE)
```

### 3. 핵심 개념

**Controller와 Agent**
- Controller: 작업을 관리하고 실행을 조정
- Agent: 실제 작업을 수행
- 분리 이유: 자원 분리, 안정성, 보안, 확장성 향상

**Node와 Executor**
- Node: 작업을 실행하는 서버 또는 환경
- Executor: 동시에 실행할 수 있는 작업 슬롯
- Executor 수는 CPU 코어 수와 다름

**Job과 Build**
- Job: 실행할 작업의 설정
- Build: Job을 한 번 실행한 기록
- 하나의 Job에 여러 Build 기록이 생성될 수 있음

**Workspace와 JENKINS_HOME**
- Workspace: 소스 코드, 임시 파일, 빌드 결과물 저장
- JENKINS_HOME: Jenkins 시스템 설정, Plugin, 빌드 기록 등 운영 데이터 저장

### 4. Jenkins Pipeline 기본 문법

```groovy
pipeline {
    agent any

    stages {
        stage('Check') {
            steps {
                sh 'whoami'
                sh 'pwd'
            }
        }
    }
}
```

| 코드 | 의미 |
|---|---|
| `pipeline` | 전체 작업 정의 |
| `agent any` | 사용 가능한 실행 Node 선택 |
| `stages` | 작업 단계 목록 |
| `stage` | 개별 작업 단계 |
| `steps` | 실제 실행할 작업 |
| `sh` | Linux 명령어 실행 |

### 5. 장애 대응

**Job이 Queue에서 대기할 경우**

1. Queue 대기 사유 확인
2. Node Online/Offline 확인
3. Executor 사용 상태 확인
4. Job의 Agent/Label 설정 확인

**Workspace에서 Permission denied 발생 시**

```bash
whoami     # 실행 사용자 확인
pwd        # 작업 위치 확인
ls -ld .   # 디렉터리 권한 확인
df -h      # 디스크 용량 확인
```

### 6. 실습 결과

- Jenkins 서비스 및 Node 상태 확인
- `jenkins-architecture-lab` Pipeline Job 생성
- Build 번호 및 Executor 확인
- Workspace 경로 확인
- `lesson02.txt` 파일 생성
- Pipeline 실행 성공
- 객관식 확인문제 8/8 정답

### 7. 핵심 요약

1. **Controller = 작업 관리**
2. **Agent = 작업 실행**
3. **Node = 실행 환경**
4. **Executor = 동시 실행 슬롯**
5. **Job = 작업 정의 / Build = 실행 기록**
6. **Workspace = 작업 공간**
7. **JENKINS_HOME = Jenkins 운영 데이터**
8. **Plugin = Jenkins 기능 확장**

---

**학습 상태:** 1장 2교시 완료 ✅

**다음 학습:** 2장 1교시 — Freestyle Job과 Pipeline 기본 자동화
