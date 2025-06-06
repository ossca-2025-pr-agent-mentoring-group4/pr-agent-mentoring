## 과제 목표

### 문제 배경

PR을 올린 이후 Merge 전까지는 리뷰와 테스트가 반복되는 중요한 검토 구간입니다. 이 과정에서 코드 내 `TODO`나 `FIXME`와 같은 주석이 PR 설명에 명시되지 않으면, 리뷰어가 이를 인지하지 못하고 그대로 머지되는 문제가 발생할 수 있습니다.

특히 TODO는 작업 중이거나 향후 보완이 필요한 중요한 단서지만, 코드 주석에만 남아 있으면 리뷰 중 또는 머지 이후까지 방치되는 경우가 자주 발생합니다.

이에 따라, PR 작성 시점에 TODO 정보를 자동으로 요약하여 보여준다면, 리뷰어와 작성자 모두에게 도움이 될 수 있다는 공감대를 팀 내에서 형성하게 되었습니다.

### 과제 목표

PR-Agent의 Pull Request 설명 자동 생성 기능을 확장하여, 코드 내 TODO 주석을 감지하고 이를 Description에 명시해주는 기능을 구현합니다.

### 기능 상세 설명

현재 PR-Agent는 Pull Request의 diff 내용을 기반으로 PR Description에 코드 변경 내용을 파악해 자동 생성하지만, 코드 내 `TODO` 또는 `FIXME` 주석은 감지하거나 반영하지 않기 때문에, 리뷰어가 작업 보완 지시를 인식하지 못하고 머지되는 경우가 있습니다.

저희 팀은 이 한계를 보완하여, diff 내용 중 주석 형태로 남겨진 미완성 작업(TODO, FIXME 등)을 자동 감지하고 다음과 같은 정보를 PR 설명에 추가하는 기능을 제안합니다.

1. PR에 포함된 변경 파일 (diff) 내에서 `TODO` 또는 `FIXME`로 시작하는 주석을 탐색합니다.
2. 해당 주석이 발견될 경우 다음과 같은 정보를 함께 수집합니다:
    - 주석이 위치한 **파일 경로**
    - 해당 주석의 **라인 번호**
    - 주석 내 작성된 **내용**
3. 수집한 정보를 PR Description 하단에 다음과 같은 형식으로 자동 추가합니다:
    
    ```markdown
    ## 📌 TODO Summary (Auto-generated)
    - [ ] main.py:42 — 리팩토링 필요
    - [ ] utils.js:10 — 오류 처리 보강
    ```
    
4. 이 기능은 설정 파일(`configuration.toml`)에서 다음과 같이 사용자가 직접 활성화 여부와 키워드를 지정할 수 있도록 합니다:
    
    ```toml
    [pr_description]
    scan_patterns = ["TODO", "FIXME"]
    ```
    

### 기대 효과

- 리뷰어가 PR을 검토할 때 **미완성 작업이나 보완 항목을 명확히 인지**할 수 있습니다.
- 개발자 본인도 머지 전에 자신의 TODO 항목을 다시 확인하여 **실수나 누락을 방지**할 수 있습니다.
- 결과적으로 PR의 **품질이 향상**되며, 팀원 간 **커뮤니케이션 효율**도 함께 높아집니다.

### 팀 기여 목적

저희 팀은 PR-Agent에 새로운 기능을 직접 제안하고 구현하는 과정을 통해, 실제로 사용할 서비스를 더 나은 방향으로 개선하는 실질적인 오픈소스 기여 경험을 쌓고자 합니다.

또한 팀 단위의 협업을 통해 서로의 아이디어와 기술을 공유하며, 혼자서는 얻기 어려운 시너지와 성과를 함께 만들어가는 기여 경험을 목표로 하고 있습니다.

## 과제 범위

### 기본 과제 범위

PR-Agent의 `CONTRIBUTING.md`와 멘토링 내 가이드라인을 준수하여 기여를 진행합니다.

- pr-agent (오픈소스) : https://github.com/qodo-ai/pr-agent/blob/main/CONTRIBUTING.md
- 멘토링 : https://github.com/ossca-2025/pr-agent-mentoring/blob/main/weekly/week-5/CONTRIBUTING_GUIDE.md

### 세부 과제 범위

1. TODO/FIXME 주석 파싱
    - PR의 diff 내 변경 라인을 대상으로 `TODO`, `FIXME` 키워드가 포함된 주석을 정규표현식으로 탐지한다.
    - ‘파일 경로’, ‘라인 번호’, ‘주석 내용’의 내용을 포함해서 추출한다.
2. PR Description 하단 자동 추가
    - PR description의 body 하단에 다음과 같은 표준 포맷으로 자동 추가한다.
        
        ```markdown
        ## 📌 TODO Summary (Auto-generated)
        - [ ] main.py:42 — 리팩토링 필요
        - [ ] utils.js:10 — 오류 처리 보강
        ```
        
3. 사용자 설정 지원
    - PR-Agent의 `configuration.toml` 설정 파일에서 다음과 같은 항목처럼 인식할 패턴을 설정할 수 있도록 한다.
        
        ```toml
        [pr_description]
        scan_patterns = ["TODO", "FIXME"]
        ```
        
4. 기능 문서화
    - 새로 추가된 기능에 대한 간단한 사용 가이드 및 설정 방법을 문서에 추가한다.

### 향후 논의가 필요한 부분

- 멀티라인 TODO 지원: 여러 줄에 걸친 TODO 주석을 어떻게 인식하고 처리할지 방식 정의

## 참여자

- 5조 PullPull이들 팀원 전원 (주소미 / 채윤희 / 이재민 / 최민주 / 김민지)

## 계획

### 주요 마일스톤

- [x]  기능 제안 및 이슈 등록
    - 관련 링크: https://github.com/qodo-ai/pr-agent/issues/1763
    - 소미님께서 Issue를 생성했으며, 컨트리뷰터들의 논의 및 운영진 피드백 기다리는 중
- [ ]  코드 구조 분석 및 주요 로직 리딩
- [ ]  TODO 파싱 기능 프로토타입 구현
- [ ]  PR Description 생성 로직 확장
- [ ]  설정 옵션 추가 및 연동
- [ ]  테스트 및 리팩토링
- [ ]  기능 문서화 및 최종 PR 작성

### 1주차 계획 : 기획 및 분석

- 기간 : 05.18 (일) 마감
- 주요 목표
    - PR-Agent 코드 베이스에 대한 이해 및 구조 분석
    - 팀 레포지토리(PullPuller/pr-agent)에서 각자 브랜치를 생성하여 **기능별 프로토타입 구현**
    - 구현한 코드에 대해 PR을 생성하고, 정기회의가 끝난 뒤 코드 리뷰 및 의견 취합을 위한 회의 진행

### 2주차 계획: 통합 및 테스트

- 기간 : 05.25(일) 마감
- 주요 목표
    - 1주차의 구현 결과를 팀 회의를 통해 **기능 취합 및 코드 통합**
    - 취합한 코드 및 기능 구현에 대해서 멘토님께 리뷰 요청 → 피드백 반영
    - pr-agent 오픈소스 내 최종 PR 게시 (사용자 가이드 포함 문서 작성)
