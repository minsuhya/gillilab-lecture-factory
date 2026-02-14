# 🎓 Gillilab Lecture Factory

> AI 기반 자동화된 기술 강의 문서 생성 시스템

실무 중심의 한국어 기술 강의 자료를 자동으로 생성하는 Claude Code 스킬 기반 시스템입니다. 유튜브 롱폼 동영상 강의에 최적화된 10,000-20,000자 분량의 완전한 학습 자료를 15분 내에 생성합니다.

[![Claude Code](https://img.shields.io/badge/Claude-Code-orange)](https://claude.ai/code)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## ✨ 주요 기능

- 🤖 **자동 문서 생성**: 주제와 URL만 입력하면 완전한 강의 문서 자동 생성
- 🔍 **공식 문서 기반**: WebSearch/WebFetch를 활용한 정확한 최신 정보 수집
- 📚 **표준화된 구조**: 11개 섹션으로 구성된 체계적인 학습 자료
- 💻 **실행 가능한 코드**: 모든 예제는 복사-붙여넣기로 즉시 실행 가능
- 🎯 **실무 중심**: 30년 풀스택 개발자 관점의 실전 위주 내용
- 🇰🇷 **한국어 최적화**: 기술 용어는 영어, 설명은 명확한 한국어

## 📦 설치

이 프로젝트는 [Claude Code](https://claude.ai/code)를 사용합니다.

```bash
# 저장소 클론
git clone https://github.com/yourusername/gillilab-lecture-factory.git
cd gillilab-lecture-factory

# Claude Code에서 프로젝트 열기
code .
```

## 🚀 사용 방법

### 기본 사용법

Claude Code에서 다음과 같이 요청하세요:

```
"FastAPI 완벽 가이드" 강의 문서를 작성해줘.
```

### URL 제공하기

공식 문서나 참고 자료 URL을 함께 제공하면 더 정확한 문서가 생성됩니다:

```
https://fastapi.tiangolo.com/
https://fastapi.tiangolo.com/tutorial/
"FastAPI" 강의 문서를 작성해줘.
```

### 생성되는 파일

문서는 루트 디렉토리에 자동으로 저장됩니다:

```
0001.FastAPI.md
0002.Docker.md
0003.React Hooks.md
```

## 📄 문서 구조

생성되는 모든 강의 문서는 다음 구조를 따릅니다:

1. **개요** - 기술 소개, 주요 특징, 사용 사례
2. **시작하기** - 설치 및 초기 설정
3. **핵심 개념** - 기본 개념과 작동 원리
4. **필수 도구 설치 및 설정** - 단계별 설치 가이드
5. **생산성 향상 플러그인** - 확장 기능
6. **현대적 CLI 도구** - 최신 도구 및 성능 비교
7. **실무 예제** - 3개 이상의 완전한 실전 예제
8. **베스트 프랙티스** - 최적화, 보안, 테스트
9. **트러블슈팅** - 자주 발생하는 오류 및 해결법
10. **추가 학습 자료** - 공식 문서, 튜토리얼, 커뮤니티
11. **결론** - 요약 및 다음 단계

## 📚 생성된 문서 예시

- [생산성 극대화를 위한 터미널 설정 및 사용법](./0001.생산성%20극대화를%20위한%20터미널%20설정%20및%20사용법.md)

## 🏗️ 프로젝트 구조

```
gillilab-lecture-factory/
├── .claude/
│   └── skills/
│       └── lecture-doc-generator/
│           ├── SKILL.md              # 스킬 정의
│           └── references/
│               ├── template.md        # 표준 템플릿
│               └── section-guidelines.md
├── CLAUDE.md                          # Claude Code 가이드
├── README.md                          # 프로젝트 소개
└── 0001.[주제].md                     # 생성된 강의 문서
```

## 🎯 작성 원칙

### 한국어 스타일
- 존댓말 사용 ("~합니다", "~입니다")
- 기술 용어는 영어, 첫 등장 시 한국어 병기
- 명확하고 직접적인 표현

### 코드 예제
- 모든 예제는 실행 가능해야 함
- 필요한 import/의존성 포함
- 한글 주석으로 핵심 설명
- 예상 출력 포함

### 실무 중심
- 이론보다 실전 위주
- 흔한 실수와 해결법
- 성능 및 보안 고려사항
- 프로덕션 환경 팁

## 🔧 커스터마이징

### 템플릿 수정

표준 템플릿을 수정하려면:

```
.claude/skills/lecture-doc-generator/references/template.md
```

### 섹션별 가이드라인

각 섹션의 작성 방법은:

```
.claude/skills/lecture-doc-generator/references/section-guidelines.md
```

## 💡 사용 사례

### 프로그래밍 강의
- Python, JavaScript, Go 등 언어 가이드
- FastAPI, React, Django 등 프레임워크 튜토리얼

### DevOps 교육
- Docker, Kubernetes 학습 자료
- CI/CD 파이프라인 가이드

### 개발 도구
- Git, VS Code, iTerm2 등 도구 매뉴얼
- 생산성 향상 도구 가이드

## 🤝 기여

기여는 언제나 환영합니다!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 라이선스

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 감사의 말

- [Claude Code](https://claude.ai/code) - AI 기반 개발 환경
- [Anthropic](https://www.anthropic.com/) - Claude AI 제공

## 📧 연락처

프로젝트 관련 문의: [GitHub Issues](https://github.com/yourusername/gillilab-lecture-factory/issues)

---

**Made with ❤️ using Claude Code**
