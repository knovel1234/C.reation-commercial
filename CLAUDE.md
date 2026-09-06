# C.reation 스토어 게시 표지판 — 운영 규칙

`store-latest.json` 은 스토어판 앱이 "새 버전이 스토어에 올라왔는지" 판단하는 표지판이다.
릴리스 `store` 의 자산으로 올라가며, 갱신은 시간당 도는 클라우드 루틴이 한다.

## 게시 확인 기준 (2026-09-06 갱신)

Partner Center 의 **"Your submission is processed"** 메일(발신자 `msftpc@microsoft.com`)이
`store-pending.json` 의 `submittedAt` 이후에 도착하면 **받은 즉시 게시 확인으로 인정**한다.

- 이전에는 메일 본문의 "can take up to two hours ... visible to customers" 문구 때문에
  수신 후 2시간을 기다렸다. **이 대기 규칙은 폐기됐다** — 사용자 지시로 즉시 반영한다.
- "published" / "is now available in the Microsoft Store" / "게시되었습니다" 메일도 즉시 인정.
- 인증 통과(certification passed)만 있는 메일, 접수 확인 메일은 게시가 **아니다**.
- 제출 ID 가 본문에 있으면 `pending.submissionId` 와 대조하고, 없으면 `submittedAt` 이후
  가장 최근 게시 메일을 쓴다.

## 파일 형식

`store-latest.json` 은 들여쓰기 2, 끝에 줄바꿈. 필드는 `pending` 에서 그대로 옮기고
`publishedAt` 만 게시 메일의 수신 시각(ISO 8601 UTC)으로 채운다.
`store-pending.json` 은 건드리지 않는다 — 다음 제출 때 덮어쓴다.

## 자산 반영 경로 (주의)

`.github/workflows/store-manifest.yml` 은 **`main` 브랜치의 push 만** 감시한다.
작업 브랜치에 커밋하면 워크플로가 돌지 않아 릴리스 자산은 그대로다.
브랜치에 push 했다면 `main` 병합이 끝나야 사용자에게 새 버전이 보인다.
