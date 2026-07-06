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
- 실행: `pnpm tauri dev` — SSH/tty 세션에선 `DISPLAY=:1 pnpm tauri dev` (Zorin 그래픽 세션)
- 번들(리눅스 .deb): `pnpm tauri build --bundles deb`

### Windows 크로스컴파일 (.exe 설치본)

- 도구(1회): `sudo apt install nsis lld llvm` · `rustup target add x86_64-pc-windows-msvc` · `cargo install --locked cargo-xwin`
- 빌드: `pnpm tauri build --runner cargo-xwin --target x86_64-pc-windows-msvc --bundles nsis`
- 산출물: `src-tauri/target/x86_64-pc-windows-msvc/release/bundle/nsis/RESM_<버전>_x64-setup.exe`
- MSI는 Windows 호스트에서만 생성 가능(리눅스에선 NSIS만). 서명 없음 — 설치 시 SmartScreen 경고는 "추가 정보 → 실행"으로 통과.

## 현재 반영 상태

- 초기 셋업: Tauri v2 셸(create-tauri-app), 원격 웹뵤(`resm.approid.team`), `cargo build` 검증 통과. 식별자 `com.resm.desktop`, 타이틀 RESM, 1280×800.
- 실행 검증 완료: Zorin 그래픽 세션(`:1`)에서 `DISPLAY=:1 pnpm tauri dev`로 창 정상 런칭 확인(원격 웹뷰 로드).
- 앱 아이콘 RESM 교체: 기본 Tauri 로고 → 웹 `icon.svg`(하우스 심볼) 기반. `pnpm tauri icon`으로 전체 세트(png/ico/icns/Square + iOS/Android) 재생성.
- 개발 가이드라인·하네스 인입: 웹 프로젝트(`/opt/real-es`)에서 `CLAUDE.md` + `.claude/` 복사, `CLAUDE.md` §6 데스크탑 맥락으로 조정(UI·기능은 웹에서). `AGENTS.md`(Tauri/Rust 주의)·`docs/PROJECT_GUIDE.md` 신규.
- `.deb` 번들 빌드·시스템 설치 완료: `pnpm tauri build --bundles deb` → `RESM_0.1.0_amd64.deb` 설치(`/usr/bin/resm`), 앱 메뉴(Office 카테고리) 등록, 설치본 실행 검증(창 확인). create-tauri-app 잔재 메타데이터 정리(크레이트·바이너리 `tauri-app`→`resm`, Maintainer·Description 정비).
- 네이티브 통합: 시스템 트레이(열기/종료), 창 닫기=트레이로 숨김(완전 종료는 트레이 "종료"), 창 크기·위치 기억(window-state 플러그인), 새 창·팝업(`target=_blank`)은 기본 브라우저로. 기본 창 1664×1040. 창 생성을 config → Rust로 이전, 템플릿 잔재 `greet` 명령 제거. 우편번호 검색(웹 embed 방식)과 호환 검증 완료.
- Windows 크로스컴파일: 리눅스 서버에서 cargo-xwin + NSIS로 `RESM_0.1.0_x64-setup.exe` 생성 성공(§Windows 크로스컴파일 절차 참고). **Windows 실기 설치·실행 검증 완료.**
- (작업 단위가 끝날 때마다 사용자 가시 효과를 한두 줄로 누적 기록한다. 절차는 [CLAUDE.md](CLAUDE.md) §5 참고.)
