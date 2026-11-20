# Public CI Validation Workbench
# 공개 CI 검증 작업대

A minimal public control repository for running validation against a separately maintained private source repository.
별도의 비공개 소스 저장소를 공개하지 않고 검증하기 위한 최소한의 공개 제어 저장소입니다.

The firmware source, build scripts, and test sources remain in the private repository.
펌웨어 소스, 빌드 스크립트 및 테스트 소스는 비공개 저장소에 유지합니다.

This repository contains only the runner workflow and public operational guidance.
이 저장소에는 실행용 Workflow와 공개 가능한 운영 안내만 둡니다.

## What it does
## 기능

- Requests a short-lived GitHub App installation token with read-only access to the configured private repository.
  설정된 비공개 저장소에 읽기 전용으로 접근하는 단기 GitHub App 설치 토큰을 요청합니다.
- Resolves and checks out one exact private source commit.
  비공개 소스의 특정 커밋을 확인하고 해당 리비전만 Checkout합니다.
- Runs the private validation entry point on a GitHub-hosted runner.
  GitHub-hosted runner에서 비공개 검증 진입 스크립트를 실행합니다.
- Records the attempted source revision and workflow outcome on the `ci-state` branch.
  시도한 소스 리비전과 Workflow 결과를 `ci-state` 브랜치에 기록합니다.

## Required repository configuration
## 필수 저장소 설정

Configure the following repository variables:
다음 저장소 Variables를 설정합니다.

- `PRIVATE_REPO_OWNER`: private repository owner.
  비공개 저장소 소유자입니다.
- `PRIVATE_REPO_NAME`: private repository name.
  비공개 저장소 이름입니다.
- `PRIVATE_REPO_APP_CLIENT_ID`: GitHub App client ID.
  GitHub App의 Client ID입니다.

Configure this repository secret:
다음 저장소 Secret을 설정합니다.

- `PRIVATE_REPO_APP_PRIVATE_KEY`: GitHub App private key in PEM format.
  GitHub App의 PEM 개인 키입니다.

The GitHub App installation on the private repository should have Contents: Read-only permission only.
비공개 저장소에 설치한 GitHub App의 권한은 Contents: Read-only만 부여해야 합니다.

Never place private keys, installation tokens, firmware source code, or personal information in issues, comments, or public files.
개인 키, 설치 토큰, 펌웨어 소스 코드 및 개인정보를 Issue, 댓글 또는 공개 파일에 올리지 마십시오.

## Run validation
## 검증 실행

On issue #1, the repository owner can post the exact comment `/run-private-validation`.
Issue #1에서 저장소 소유자가 정확히 `/run-private-validation` 댓글을 작성하면 검증을 요청할 수 있습니다.

A manual `workflow_dispatch` run is also available to an authorized maintainer.
권한이 있는 관리자는 `workflow_dispatch`로 수동 실행할 수도 있습니다.

A successful workflow run means only that the configured validation script completed successfully for the recorded source revision.
Workflow 성공은 기록된 소스 리비전에 대해 설정된 검증 스크립트가 성공적으로 끝났다는 의미입니다.

It does not establish IAR compilation, hardware operation, or real-fan communication unless those checks are explicitly included and independently evidenced.
IAR 컴파일, 실제 하드웨어 동작 또는 실제 팬 통신은 해당 검증이 명시적으로 포함되고 별도로 증명되지 않는 한 성공으로 간주하지 않습니다.

## Workflow
## 실행 Workflow

- `.github/workflows/private-source-pilot.yml`
  Private source validation workflow.
  비공개 소스 검증 Workflow입니다.

- `ci-state` branch
  Machine-readable record of the latest attempted source revision and result.
  마지막 검증 시도 소스 리비전과 결과를 기록하는 브랜치입니다.
