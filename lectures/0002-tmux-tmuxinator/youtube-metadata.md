# YouTube 메타데이터 — tmux + tmuxinator 실무 활용 가이드

> 생성일: 2026-03-07
> 강의 폴더: `lectures/0002-tmux-tmuxinator/`

---

## 📌 제목 후보 (A/B 테스트용)

> YouTube 제목은 **60자 이내**를 권장합니다. 각 후보의 글자 수를 확인하세요.

### 제목 A — 수치/결과 중심

```
SSH 끊겨도 살아있는 개발 환경 | tmux + tmuxinator 실무 활용
```

글자 수: 44자

### 제목 B — 문제/해결 중심

```
한 명령어로 개발 환경 자동화 | tmux + tmuxinator 실전 가이드
```

글자 수: 43자

### 제목 C — 대상/도구 중심

```
tmux 완전정복 | 터미널 멀티플렉서 + tmuxinator YAML 자동화
```

글자 수: 43자

---

## 📝 설명 (Description)

> 아래 전체 텍스트를 YouTube 설명란에 붙여넣기 합니다.

```
SSH가 끊겨도 작업이 사라지지 않는 터미널 환경, 매일 아침 한 명령어로 전체 개발 환경이 열리는 자동화.
tmux + tmuxinator로 30분이면 구축할 수 있습니다.

⏱️ 타임스탬프
00:00 인트로 — tmux가 필요한 이유 (세션 유지, 멀티태스킹)
00:05 tmux 계층 구조와 설치 (Server → Session → Window → Pane)
00:15 실무 필수 단축키 20개 (세션 / 윈도우 / 패인 / synchronize-panes)
00:30 ~/.tmux.conf 기본 설정 (마우스, Prefix 변경, vi 모드)
00:40 tmuxinator 소개 및 설치
00:45 tmuxinator YAML 프로젝트 설정
00:55 실무 예제 3가지 — SSH 세션 관리 / 풀스택 개발 환경 / 모니터링 대시보드
01:15 베스트 프랙티스
01:20 트러블슈팅
01:25 마무리

✅ 이 강의에서 배우는 것
• tmux 4계층 구조(Server-Session-Window-Pane)와 핵심 단축키 20개
• ~/.tmux.conf 실무 설정 (마우스, Prefix, vi 복사모드, 상태 바)
• synchronize-panes로 다중 서버에 동시 명령 전송
• tmuxinator YAML로 개발 환경 한 명령어 자동화
• SSH 세션 유지 / 풀스택 개발 / 모니터링 대시보드 실무 예제 3가지

🔗 강의에서 사용한 도구 & 링크
• tmux 공식 GitHub: https://github.com/tmux/tmux
• tmuxinator 공식 GitHub: https://github.com/tmuxinator/tmuxinator
• Oh My Tmux (인기 설정 모음): https://github.com/gpakosz/.tmux
• tmux Cheat Sheet: https://tmuxcheatsheet.com

📁 강의 자료 (GitHub)
• 코드 & 설정 파일: https://github.com/minsuhya/gillilab-lecture-factory/tree/master/lectures/0002-tmux-tmuxinator/

📺 길리랩 채널
• Tech Log: https://techlog.gillilab.com
• Tistory: https://rupijun.tistory.com
• Blogger: https://gillilab.blogspot.com
• Instagram: https://www.instagram.com/gillilab/
• Threads: https://www.threads.com/@gillilab

---
길리랩(GilliLab)은 현업 개발자를 위한 실무 중심 기술 채널입니다.
구독하면 매주 새로운 개발 생산성 강의를 받아볼 수 있습니다.

#tmux #tmuxinator #터미널 #개발자생산성 #맥개발 #리눅스 #개발환경자동화 #터미널멀티플렉서
```

---

## 🏷️ 태그

> 쉼표로 구분하여 YouTube 태그 입력란에 붙여넣기 합니다. **500자 이내**를 유지하세요.

```
tmux, tmuxinator, 터미널, 터미널멀티플렉서, 개발자생산성, 개발환경, 맥개발, 리눅스, 터미널단축키, tmux설정, 개발도구, 터미널자동화, SSH, 서버관리, 다중서버, terminal multiplexer, developer productivity, macOS, Linux, CLI, dotfiles, devtools, tmux tutorial, tmuxinator yaml
```

태그 합산 글자 수: 약 195자 / 500자

---

## 🖼️ 썸네일

### 문구

| 구분                  | 문구              | 글자 수 |
| --------------------- | ----------------- | ------- |
| 메인 문구 (15자 이내) | **tmux 완전정복** | 9자     |
| 서브 문구 (10자 이내) | tmuxinator        | 10자    |

### 디자인 컨셉

| 요소           | 명세                                                             |
| -------------- | ---------------------------------------------------------------- |
| 배경           | 다크 그린 (#0D1117) — 터미널 분위기                              |
| 메인 문구 색상 | 밝은 초록 (#39FF14) — 터미널 텍스트 컬러                         |
| 서브 문구 색상 | 흰색 (#FFFFFF)                                                   |
| 레이아웃       | 좌측: 분할된 터미널 창 스크린샷 / 우측: 메인+서브 문구 세로 배치 |
| 아이콘/이미지  | tmux 로고, 터미널 분할창 UI 이미지                               |
| 분위기         | 다크 테마, 테크니컬, 미니멀                                      |

---

## 📂 재생목록 제안

| 우선순위 | 재생목록 이름          | 이유                                      |
| -------- | ---------------------- | ----------------------------------------- |
| 1순위    | 개발자 생산성 도구     | 터미널 생산성 설정 강의(0001)와 동일 계열 |
| 2순위    | 리눅스 & 터미널 마스터 | SSH / 서버 관리 관심층 타겟               |

---

## 🃏 카드 & 최종 화면 CTA

### 카드 (영상 중간 삽입)

| 삽입 시점 | 카드 유형     | 내용                                     |
| --------- | ------------- | ---------------------------------------- |
| 00:55 경  | 영상 카드     | 강의 0001 터미널 생산성 설정 (연관 강의) |
| 01:15 경  | 재생목록 카드 | 개발자 생산성 도구 재생목록              |

### 최종 화면 (영상 마지막 20초)

| 요소      | 내용                                            |
| --------- | ----------------------------------------------- |
| 영상 1    | 강의 0001 — 터미널 생산성 설정 (Oh My Zsh, fzf) |
| 영상 2    | 최신 업로드 (자동)                              |
| 구독 버튼 | 길리랩 채널 구독 CTA                            |

---

## ✅ 업로드 전 체크리스트

- [ ] 제목 3개 중 하나 선택 (60자 이내 확인)
- [ ] 설명 전체 복사-붙여넣기
- [ ] 태그 복사-붙여넣기 (500자 이내 확인)
- [ ] 썸네일 디자인 명세 디자이너/Canva에 전달
- [ ] 재생목록 지정 (개발자 생산성 도구)
- [ ] 카드 설정 (영상 공개 후 — 00:55, 01:15)
- [ ] 최종 화면 설정 (영상 마지막 20초)
