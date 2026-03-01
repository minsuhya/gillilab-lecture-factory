# CLAUDE.md

Claude Code가 이 저장소에서 작업할 때 참고하는 가이드입니다.

## 프로젝트 개요

**gillilab-lecture-factory**는 30년차 풀스택 개발자이자 정보관리기술사가 운영하는 유튜브 기술 강의 채널(Gillilab)의 강의 자료 관리 저장소입니다. 두 개의 Claude Code 스킬로 강의 문서 생성과 YouTube 메타데이터 생성을 자동화합니다.

- **GitHub**: `https://github.com/minsuhya/gillilab-lecture-factory`
- **채널**: Tech Log(techlog.gillilab.com), Tistory(rupijun.tistory.com)

---

## 디렉토리 구조

```
gillilab-lecture-factory/
│
├── lectures/                              # 모든 강의 자료 루트
│   └── 0001-터미널-생산성-설정/           # 강의 폴더 (4자리번호-슬러그)
│       ├── lecture-script.md              # 강의자용 낭독 스크립트 (격식체, 큐 마커)
│       ├── README.md                      # 시청자용 실용 가이드 (GitHub 공개)
│       ├── youtube-metadata.md            # YouTube 업로드용 SEO 메타데이터
│       ├── assets/                        # 이미지, 다이어그램 (gitignore)
│       └── examples/                      # 실행 가능한 코드 샘플
│
├── shared/                                # 강의 간 공통 자료
│   ├── assets/                            # 공통 이미지/로고
│   └── snippets/                          # 공통 코드 스니펫
│
├── .claude/skills/
│   ├── lecture-doc-generator/             # 강의 문서 생성 스킬
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── template.md                # 강의 콘텐츠 구조 템플릿
│   │       ├── lecture-script-template.md # lecture-script.md 전용 형식
│   │       └── section-guidelines.md      # 섹션별 상세 작성 가이드
│   └── youtube-metadata-generator/        # YouTube 메타데이터 생성 스킬
│       ├── SKILL.md
│       └── references/
│           └── youtube-metadata-template.md
│
├── CLAUDE.md
└── README.md
```

---

## 강의 폴더 명명 규칙

```
lectures/[4자리번호]-[슬러그]/
```

- **번호**: `lectures/` 하위 폴더 수를 확인하여 자동 부여 (`0001`, `0002`, ...)
- **슬러그**: 주제를 한글 또는 영문 소문자 + 하이픈으로 변환
- **예시**: `lectures/0001-터미널-생산성-설정/`, `lectures/0002-fastapi/`
- 폴더 생성 시 `assets/`, `examples/` 하위 디렉토리도 함께 생성

---

## 스킬 사용법

### 1. 강의 문서 생성 (`lecture-doc-generator`)

주제와 URL을 제공하면 두 파일을 생성합니다:

```
"Docker Compose 완벽 가이드" 강의 문서를 작성해줘.
URL: https://docs.docker.com/compose/
```

| 생성 파일 | 대상 | 내용 |
|---|---|---|
| `lecture-script.md` | 강의자 | 격식체 낭독 스크립트 + 큐 마커 + 섹션별 시간 |
| `README.md` | 시청자 | 명령어/참조표 중심 실용 가이드 |

### 2. YouTube 메타데이터 생성 (`youtube-metadata-generator`)

강의 완성 후 YouTube 업로드용 메타데이터를 생성합니다:

```
lectures/0001-터미널-생산성-설정 강의의 YouTube 메타데이터를 생성해줘.
```

| 생성 항목 | 내용 |
|---|---|
| 제목 후보 3개 | 60자 이내, 각도별 SEO 최적화 |
| 설명문 | 훅 → 타임스탬프 → 학습 내용 → 채널 링크 → 해시태그 |
| 태그 | 500자 이내, 한국어+영어 혼합 |
| 썸네일 문구/컨셉 | 메인 15자·서브 10자 이내 + 디자인 명세 |
| 재생목록·CTA | 카테고리 제안 + 카드/최종 화면 |

---

## 문서 구조 기준

### `lecture-script.md` 작성 원칙

- **문체**: ~합니다 격식체 엄수 (구어체, 청중 참여 유도 문구 금지)
- **큐 마커**: `[인트로]`, `[📌 섹션 전환]`, `[💻 데모]`, `[⏸️ 잠깐]`, `[🎯 핵심 포인트]`, `[📝 코드 설명]`
- **시간 표시**: 섹션마다 `[약 N분]`, 총 60~90분 분량
- **순서**: "왜 → 어떻게" — 명령어/개념 전 필요성 먼저 서술
- **데모 서술**: 실행 전후 화면 동작을 텍스트로 서술

### `README.md` 작성 원칙 (표준 9섹션)

1. **개요** — 기술 소개, 핵심 특징, 사용 사례
2. **시작하기** — 사전 요구사항, 설치, 초기 설정
3. **핵심 개념** — 동작 원리, 아키텍처, 핵심 용어
4. **필수 도구 설치 및 설정** — 단계별 설치 가이드
5. **생산성 향상 플러그인** — 확장 기능, 추천 설정
6. **현대적 CLI 도구** — 최신 도구 비교, 벤치마크
7. **실무 예제** — 3개 이상의 완전한 실전 예제
8. **베스트 프랙티스** — 최적화, 보안, 테스트, 배포
9. **트러블슈팅** — 자주 발생하는 오류와 해결법

형식: 빠른 시작 체크리스트 → 명령어 모음 → 설정 파일 전문 → 참조 표 → 트러블슈팅 표 → 링크 표

---

## 콘텐츠 원칙

### 30년차 풀스택 개발자 + 정보관리기술사 관점
- 이론보다 **현장에서 실제로 쓰는 방법** 우선
- 기술 선택의 배경과 설계 원칙 함께 설명
- 스타트업부터 엔터프라이즈까지 규모별 접근법 차별화
- 자격증 시험 관련 개념을 실무 코드와 연결

### 코드 예제 기준
- 복사-붙여넣기로 즉시 실행 가능 (import/의존성 완전 포함)
- 한글 주석으로 핵심 동작 설명
- 예상 출력 결과 명시
- Apple Silicon / Intel 등 환경 분기 명시

### 한국어 스타일
- 존댓말 ("~합니다", "~입니다")
- 기술 용어는 영어 원어 유지, 첫 등장 시 한국어 병기: "컨테이너(Container)"
- 명확하고 직접적인 표현

---

## 핵심 작업 원칙

1. **순번 확인 필수** — 새 강의 폴더 생성 전 반드시 `lectures/` 하위 폴더 수 확인
2. **두 파일 동시 생성** — `lecture-doc-generator` 호출 시 `lecture-script.md`와 `README.md` 함께 생성
3. **명명 규칙 준수** — `lectures/[4자리번호]-[슬러그]/`
4. **표준 섹션 구조 유지** — README.md는 9섹션 구조
5. **실행 가능한 예제** — 모든 코드는 복사-붙여넣기로 즉시 동작해야 함
