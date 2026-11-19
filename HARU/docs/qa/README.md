# HARU 소스 경계 시험 재실행

이 스크립트는 실제 배포 UI를 자동 조작하는 E2E가 아니다. 다운로드한 배포 소스의 데이터 저장 경계와 FAQ 순수 로직을 시험한다. A30은 복원 핸들러의 병합 동작을 재구성한 시험이다. 원래 배포 코드 전체를 저장소에 추가하지 않았다.

Node 내장 모듈만 필요하다. 빈 로컬 검증 폴더에서 아래처럼 파일을 받고 해시가 같을 때 실행한다. 소스가 바뀌면 runner가 중단한다. 해시를 임의로 갱신해 기존 결과와 같은 버전으로 보고하지 않는다.

```bash
mkdir -p audit/source audit/evidence
curl --fail -L https://dialysis-os-haru.jovial-comet-6952.chatgpt.site/core.js -o audit/source/core.js
curl --fail -L https://dialysis-os-haru.jovial-comet-6952.chatgpt.site/app.js -o audit/source/app.js
TZ=Asia/Seoul node HARU/docs/qa/run_checks.cjs audit/source audit/evidence
TZ=UTC node HARU/docs/qa/run_checks.cjs audit/source audit/evidence
```

현재 v16에서는 알려진 결함4건 때문에 종료코드1이 예상된다. 총35개 중31PASS4FAIL이다. 두 번 실행한 것을70개 고유 시험으로 세지 않는다. 기존47개 시험의 스크립트가 아니며 환자 평가나 임상 검증도 아니다.

결과 JSON의 runAt은 UTC ISO 시각이고 timezone은 시험 환경이다. 브라우저 UI 관찰은 자동 로그와 별개로 QA 보고서에 수동 기록했다. 근거 파일에 담긴 수치는 모두 가상 데이터다.
