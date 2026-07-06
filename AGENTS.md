# AGENTS.md

이것은 **Tauri v2 + Rust** 데스크탑 앱(원격 웹뷰)입니다. 코딩 전 필수 가이드. 행동 가이드라인은 `CLAUDE.md`.

## Tauri v2 / Rust 주의

- **Tauri v2는 v1과 API가 다름.** 코딩 전 공식 문서 확인 — https://tauri.app/develop/ . 헤드리스 추측 금지.
- Rust 코드는 `src-tauri/src/` (`lib.rs`에 `run()`, 명령은 `#[tauri::command]` + `invoke_handler`).
- 설정은 `src-tauri/tauri.conf.json` (schema v2). 웹뷰 URL·창·CSP·번들.
- 권한은 `src-tauri/capabilities/` (JSON). 외부 도메인·플러그인 권한 여기서.

## 빌드 / 실행

- 빌드 검증: `cargo build` (`src-tauri/`에서).
- 실행: `pnpm tauri dev`. 이 서버는 Zorin OS 그래픽 세션(`:1`) — SSH/tty 세션에선 `DISPLAY=:1 pnpm tauri dev`.
- 번들: `pnpm tauri build`.

## 원격 웹뷰

- 웹뷰가 `https://resm.approid.team` 로드 (`tauri.conf.json` `app.windows[0].url`).
- **UI·기능은 웹 프로젝트(`/opt/real-es`)에서.** 데스크탑은 로더·네이티브 셸.
- 오프라인 불가(서버 의존). 웹이 바뀌면 데스크탑도 자동 반영.

## 시스템 의존 (Linux)

`libwebkit2gtk-4.1-dev build-essential curl wget file libxdo-dev libssl-dev libayatana-appindicator3-dev librsvg2-dev` 필요. 누락 시 `cargo build` 링크 에러. 설치: https://tauri.app/start/prerequisites/#linux

## 응답 언어

사용자 응답은 **한국어**. 기술 토큰·코드·식별자는 원문 유지.
