# RESM Desktop

부동산 매물 관리 웹앱 **RESM**의 데스크탑 앱 (Tauri 원격 웹뷰).

## 개요

real-es 웹(`https://resm.approid.team`)을 Tauri 웹뷰로 로드하는 데스크탑 셸. UI·기능은 웹 프로젝트(`/opt/real-es`)에서, 데스크탑은 Tauri/Rust 네이티브 셸.

## 디렉토리 구조

```
real-es-desktop/
├── CLAUDE.md             # 개발 행동 가이드라인 (웹 프로젝트에서 인입)
├── AGENTS.md             # Tauri/Rust 주의 규칙
├── src-tauri/            # Rust + Tauri 설정
│   ├── tauri.conf.json   # 웹뷰 URL(resm.approid.team)·창·CSP·번들
│   ├── src/              # Rust (lib.rs·main.rs)
│   ├── capabilities/     # 권한 (JSON)
│   └── icons/
├── src/                  # vanilla 프론트 (원격 웹뷰라 사용 안 함, 잔재)
└── docs/
    └── PROJECT_GUIDE.md  # 프로젝트 가이드
```

## 선정 스택

- **Tauri v2 + Rust** + pnpm
- 원격 웹뷰 (`https://resm.approid.team`)

## 개발

### 시스템 의존 (Linux)

`libwebkit2gtk-4.1-dev build-essential curl wget file libxdo-dev libssl-dev libayatana-appindicator3-dev librsvg2-dev` — https://tauri.app/start/prerequisites/#linux

### 빌드 / 실행

- 빌드 검증: `cd src-tauri && cargo build`
- 실행: `pnpm tauri dev` (디스플레이 필요)
- 번들: `pnpm tauri build`

## 현재 반영 상태

- 초기 셋업: Tauri v2 셸(create-tauri-app), 원격 웹뷰(`resm.approid.team`), `cargo build` 검증 통과. 식별자 `com.resm.desktop`, 타이틀 RESM, 1280×800.
- 개발 가이드라인·하네스 인입: 웹 프로젝트(`/opt/real-es`)에서 `CLAUDE.md` + `.claude/` 복사, `CLAUDE.md` §6 데스크탑 맥락으로 조정(UI·기능은 웹에서). `AGENTS.md`(Tauri/Rust 주의)·`docs/PROJECT_GUIDE.md` 신규.
- (작업 단위가 끝날 때마다 사용자 가시 효과를 한두 줄로 누적 기록한다. 절차는 [CLAUDE.md](CLAUDE.md) §5 참고.)
