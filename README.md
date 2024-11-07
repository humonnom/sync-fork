# Auto Sync Forked Repository

## 개요

이 GitHub Action은 포크된 저장소를 원본(upstream) 저장소와 자동으로 동기화하는 간편한 도구입니다. 직접 [Github 이벤트](https://docs.github.com/ko/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows)를 사용하거나 [humonnom/dispatch-event](https://github.com/marketplace/actions/trigger-event-to-sync)를 함께 사용하면 원본 레포지토리의 변경사항이 액션을 트리거 하도록 설정할 수 있습니다.

## 기능

- 원본 저장소의 최신 변경사항 자동 병합
- 포크된 저장소의 `main` 브랜치 실시간 업데이트
- 간단하고 직관적인 동기화 프로세스

## 입력 파라미터

| 파라미터 | 설명 | 필수 여부 |
|----------|------|-----------|
| `upstream_repository_owner` | 원본 저장소의 소유자 | **필수** |
| `upstream_repository_name` | 원본 저장소의 이름 | **필수** |

## 사용 예시

```yaml
- name: Sync Forked Repository
  uses: humonnom/sync-fork@v0.1.0
  with:
    upstream_repository_owner: 'original-owner'
    upstream_repository_name: 'original-repo'
```

## 워크플로우 예제
* 매일 자정에 동기화
```yaml
name: Sync Forked Repository

on:
  schedule:
    - cron: '0 0 * * *'  # 매일 자정에 동기화

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Auto Sync
        uses: humonnom/sync-fork@v0.1.0
        with:
          upstream_repository_owner: 'original-owner'
          upstream_repository_name: 'original-repo'
```
* 수동 트리거 옵션 - 특정 이벤트 발생 감지
```yaml
name: Sync Forked Repository

on:
  repository_dispatch:
    types: [main_updated]  # 수동 트리거 옵션

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Auto Sync
        uses: humonnom/sync-fork@v0.1.0
        with:
          upstream_repository_owner: 'original-owner'
          upstream_repository_name: 'original-repo'
```

## 주의사항

- 저장소의 `main` 브랜치에서 작동
- 충돌 발생 시 수동 해결 필요
- 정기적인 동기화를 위해 스케줄러 사용 권장

## 고급 사용법

### 수동 트리거
워크플로우 디스패치를 통해 언제든 동기화 가능

### 특정 브랜치 동기화
필요에 따라 `main` 외 다른 브랜치로 확장 가능

## 문제 해결

- Git 충돌 발생 시 수동 병합 필요
- 권한 문제는 저장소 설정 확인

## 라이선스

MIT 라이선스를 따릅니다.

## 기여

이슈 또는 풀 리퀘스트를 통해 개선 제안 가능합니다.
