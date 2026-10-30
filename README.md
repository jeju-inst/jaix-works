# jaix-works — 웍스(NAVER WORKS) 스킬

제주연구원 JAIX 묶음의 웍스 도구다. 본인 웍스 계정 권한으로 드라이브·캘린더·메일을 다룬다.
편입안: [jeju-inst/jaix-kit `docs/works.md`](https://github.com/jeju-inst/jaix-kit/blob/main/docs/works.md) (2026-10-30 승인).

- 실행은 기성 CLI [`nworks`](https://github.com/yjcho9317/nworks)(Apache-2.0)가 한다. 이 저장소는 짧은 스킬과 런타임 선언만 둔다. **MCP 서버는 등록하지 않는다** — nworks MCP 도구 36개가 상시 토큰 예산을 넘기 때문이다.
- 인증은 기관 User OAuth 앱 하나를 모두가 쓴다. 토큰은 사람마다 따로 발급되고 **본인 계정 범위로만** 동작한다. 사내 관리용 `works` CLI(도메인 관리자급 키)는 쓰지 않는다.
- **비밀은 이 저장소에 없다.** Client ID/Secret은 `jaix setup` 이 원내망에서만 열리는 `runtime.json` 의 `client_source` 에서 받는다.

## 파일

| 파일 | 역할 |
|---|---|
| `skills/works/SKILL.md` | 스킬(호스트 중립: PATH의 `nworks` 를 셸로 부른다) |
| `runtime.json` | Node 최소 버전, nworks **정확한 버전과 npm 무결성 해시**, 로그인 스코프, 기관 앱 정보 위치. `jaix setup` 이 잠금 커밋의 이 파일을 읽는다 |
| `.github/workflows/calver.yml` | main 머지마다 날짜 태그 → jaix-kit 잠금 갱신 |

## nworks 버전 올리기

자동 갱신하지 않는다(외부 개인 개발자 패키지). 새 버전의 변경 내역을 읽은 뒤 `runtime.json` 의 `version` 과 `integrity`(`npm view nworks@<버전> dist.integrity`)를 함께 바꾸는 PR을 올린다.

## 스코프 넓히기

스코프는 기관 앱의 속성이라 넓히면 이후 동의하는 모두에게 적용된다. 먼저 jaix-kit `docs/works.md` 와 이 저장소 `runtime.json` 을 PR로 고치고, 그다음 웍스 Developer Console을 바꾼다. 앱에 없는 스코프를 하나라도 요청하면 웍스가 로그인 전체를 `invalid_scope` 로 거절한다.
