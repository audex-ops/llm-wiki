<!-- disclosure: public -->

# USAGE — 사용 가이드

이 템플릿을 직접 운영하는 사람이 보는 문서. LLM 용 지시는 `CLAUDE.md`, 콘텐츠 규약은 `_meta/library_conventions.md` 에 따로 있다.

## 두 개의 레이어

- `projects/<slug>/` — 라이브러리 콘텐츠. 사실의 출처. 한 번, 같은 형식으로 기록한다.
- `outputs/<slug>/` — `compose-output` 의 산출물. 라이브러리에서 다시 만들 수 있다. 직접 편집하지 말고, 고치고 싶으면 라이브러리를 수정한 뒤 다시 컴포즈한다.

라이브러리는 그대로 두고, 출력물은 대상·기회마다 다시 만든다.

## 새 프로젝트 추가

새 Claude Code 세션에서 첫 메시지(아래의 작성법 참조)를 작성하면, 스킬이 다음 순서로 실행된다:

1. `extract-from-repo` — 코드·git·첫 메시지에서 사실을 채운다. 대상 챕터: `00`, `02`, `03`, `13`, `14`.
2. `interview-user` — 첫 메시지로 답하지 못한 부분만 묻는다. 대상 챕터: `01`, `04`, `05`, `06` 과 필요시 옵션 `10`, `11`, `12`.
3. `validate-library` — 태그·평가어·교차 참조·민감 정보 위반을 검사한다.
4. (선택) `compose-output` — 포트폴리오·자기소개서·이력서 등을 `outputs/<slug>/` 에 저장한다.
5. 커밋·푸시.

## 첫 메시지 작성법

품질과 속도에 가장 큰 영향을 주는 단일 요인. 첫 메시지에 인터뷰 수준의 내용을 풍부하게 담을수록 이후 질문 라운드가 줄어든다 (보통 2~4개 질문으로 끝난다).

다음을 가능한 한 구체적으로 적는다:

- **리포지토리 URL** (또는 리포 없음을 명시).
- **프로젝트 slug** (예: `02-some-name`).
- **라이프사이클 날짜**: 킥오프, 출시, 수상, EOL, 운영 모드 전환. 범위가 아닌 특정 날짜.
- **핵심 기능과 주변 기능의 구분**: "X 가 필수" 같은 식으로 명시.
- **결정과 그 시점의 대안**: 챕터 04 의 options / choice / reasoning 구조에 그대로 들어간다.
- **commit SHA, 파일 경로, schema·table 이름, index 정의**: 검증 가능한 인용 위치.
- **수치**: MAU, 행 수, EXPLAIN 결과 등. 출처와 측정 방법까지 같이 적는다. 기억에만 있는 숫자는 그 사실도 명시한다.
- **회고**: 다르게 했을 부분. 스킬은 이것을 발명하지 않는다.
- **시각화 아이디어**: 어떤 다이어그램·GIF 가 어느 위치에 필요한지. 챕터 14 의 `V*` 인벤토리에 들어간다.

한 세션에서 전체 라이브러리를 채운 사례: `01-dashhub` 프로젝트의 git log (`4ca002b` 부터).

## 위치별 파일

| 경로 | 용도 |
|---|---|
| `projects/<slug>/` | 라이브러리 콘텐츠 |
| `outputs/<slug>/` | 컴포즈 산출물 (재생성 가능) |
| `_meta/library_conventions.md` | 콘텐츠 규약 |
| `_meta/sensitivity_rules.md` | 민감 정보 정책 |
| `_meta/chapter_catalog.md` | 허용 챕터 종류 |
| `skills/<skill>/SKILL.md` | 스킬별 Claude 지시 |
| `CLAUDE.md` | 프로젝트 레벨 Claude 지시 |
| `cross_project_index.md` | 프로젝트 roster + 기술 매트릭스 |
| `_log.md` | 구조 변경 / 스킬 실행 로그 (append-only) |

## 더 보기

- 태그·평가어 금지·disclosure 등급·빈 섹션 처리: `_meta/library_conventions.md`
- 허용 챕터 종류: `_meta/chapter_catalog.md`
- 민감 정보 디폴트와 프로젝트별 override: `_meta/sensitivity_rules.md`
- 스킬별 동작 상세: 각 `skills/<skill>/SKILL.md`
