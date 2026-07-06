# Project Guide (RESM Desktop)

real-es 웹을 로드하는 Tauri 데스크탑 앱. 새 세션은 추측으로 구조를 바꾸지 말고 이 문서부터 읽는다.

## 1. 프로젝트 한 줄 요약

> real-es 웹(`https://resm.approid.team`)을 Tauri 웹뷰로 로드하는 데스크탑 셸.

## 2. 현재 확정된 큰 방향

- **원격 웹뷰**: 웹 서버(prod)를 웹뷰로 로드. 빠른 시작, 서버 의존, 오프라인 불가.
- UI·기능은 웹 프로젝트(`/opt/real-es`)에서. 데스크탑은 Tauri 셸·네이티브 통합만.
- 향후 로컬 동작(오프라인) 필요 시 사이드카(Next 서버 자식) 또는 static+로컬 재작성 검토.

## 3. 왜 이렇게 정했는가

- real-es는 Next.js SSR + 서버 액션 + PostgreSQL. Tauri 로컬 번들(static)이 불가(서버 기능 잃음).
- 원격 웹뷰가 가장 단순 + "동일한 기능"(서버 DB 재사용).
- 별도 프로젝트로 분리(웹과 셸 책임 분리).

## 4. 소스 구조 이해

- `src-tauri/` — Rust + Tauri 설정.
  - `tauri.conf.json` — 웹뷰 URL(`app.windows[0].url`)·창·CSP·번들.
  - `src/lib.rs`·`main.rs` — Tauri Builder·명령.
  - `capabilities/default.json` — 권한(core, opener).
  - `icons/` — 앱 아이콘.
- `src/` — create-tauri-app vanilla 프론트(**원격 웹뷰라 사용 안 함**, 잔재).

## 5. 각 영역의 책임

- 데스크탑: Tauri 셸·설정·Rust 명령·네이티브 통합(메뉴·트레이·단축키 등).
- 웹(`/opt/real-es`): UI·페이지·컴포넌트·비즈니스 로직·DB.

## 6. 새 세션이 시작되면 먼저 볼 문서

1. `README.md`
2. `docs/PROJECT_GUIDE.md`
3. `CLAUDE.md`
4. `AGENTS.md` (Tauri/Rust 주의)

## 7. 문서 갱신 규칙

- 작업 단위 종료 시 `README.md` §현재 반영 상태 + 이 문서 §현재 참고 상태에 한두 줄 누적. 절차는 `CLAUDE.md` §5.

## 8. 현재 참고 상태

- **초기 셋업**: `create-tauri-app`(vanilla)으로 Tauri v2 셸 생성 → `tauri.conf.json` 원격 웹뷰(`https://resm.approid.team`) 설정 → `cargo build` 검증 통과(1m 12s). 식별자 `com.resm.desktop`, 타이틀 RESM, 1280×800.
- **개발 가이드라인 인입**: 웹 프로젝트(`/opt/real-es`)에서 `CLAUDE.md`(행동 가이드) + `.claude/`(하네스) 복사. `CLAUDE.md` §6은 데스크탑 맥락으로 조정(UI·기능은 웹 프로젝트에서). `AGENTS.md`는 Tauri/Rust용으로 신규(real-es의 Next.js 것은 부적합).
- **환경**: Rust 1.96.1(rustup 설치). Linux 시스템 의존(webkit2gtk-4.1-dev 등) 설치 완료. Zorin OS 그래픽 세션(`:1`) — SSH/tty에선 `DISPLAY=:1 pnpm tauri dev`로 창 실행 가능(실행 검증 완료).
