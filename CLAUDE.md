# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

**응답 언어:** 사용자와의 모든 응답·커뮤니케이션은 **한국어**로 한다. 코드·식별자·로그·명령어 등 기술 토큰과 인용 원문은 그대로 둔다.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

## 5. End-of-Task Wrap-up

**코드 변경 작업 단위가 끝나면 문서 업데이트 → commit 제안까지 한 번에 마무리한다. push는 사용자가 명시적으로 요청한 경우에만 실행한다.**

다음 조건 중 하나가 만족되면 작업 단위가 끝난 것으로 본다:
- 한 묶음의 기능/수정이 빌드·테스트 검증까지 끝난 시점
- 사용자가 "끝", "다음", "정리해줘" 같은 전환 신호를 준 시점
- Stop hook이 uncommitted 변경을 system message로 알린 시점

수행 절차:
1. 변경 내용을 한두 줄로 요약해 [README.md](README.md) §"현재 반영 상태"와 [docs/PROJECT_GUIDE.md](docs/PROJECT_GUIDE.md) §"현재 참고 상태"에 추가한다. 중간 시행착오나 디버깅 잡일은 git history에 맡기고 문서엔 사용자 가시 효과만 적는다.
2. `git status`로 변경 파일을 확인한다. 의도치 않은 파일이 섞여 있으면 사용자에게 알린다.
3. commit 메시지 초안을 보여주고 **"이대로 commit 할까요?"** 확인을 받는다.
4. 사용자 승인 후에만 commit. **push는 절대 자동으로 하지 않는다** — 사용자가 명시적으로 "push해"라고 지시한 경우에만 실행한다.
5. **commit 제안 메시지 끝에 반드시 `문서 갱신: README §현재 반영 상태 + PROJECT_GUIDE §현재 참고 상태` 한 줄을 붙인다.** 이 줄을 적을 수 없으면(=아직 안 갱신함) commit하지 말고 1번으로 돌아가 먼저 갱신한다. 문서 변경 없이 코드만 staged면 누락 신호로 보고 멈춘다.

이 룰의 목적: 진척 문서와 코드 변경의 정합성 유지, 그리고 검토되지 않은 변경이 GitHub에 올라가는 것 방지.

## 6. UI·기능은 웹 프로젝트에서

**이 데스크탑 앱은 real-es 웹(`https://resm.approid.team`)을 웹뷰로 로드한다.** UI·페이지·컴포넌트·기능 변경은 **웹 프로젝트(`/opt/real-es`)에서** 한다(웹 프로젝트의 CLAUDE.md §6 템플릿 우선 규칙이 거기에 적용).

이 데스크탑 프로젝트에서 다루는 것:
- Tauri 셸·설정(`src-tauri/tauri.conf.json` — 웹뷰 URL·창·CSP·번들 아이콘)
- Rust 코드(`src-tauri/src/` — Tauri 명령·플러그인·네이티브 통합)
- 런타임 통합(메뉴·트레이·전역 단축키·파일 시스템·알림 등 Tauri 기능)

위젯·페이지 자체 수정은 웹 프로젝트로 — 데스크탑은 로더·네이티브 셸일 뿐이다. 웹 화면이 바뀌면 데스크탑도(원격 로드라) 자동 반영된다.

목적: 책임 분리 명확 — UI는 웹, 네이티브는 데스크탑. 중복 구현 방지.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.
