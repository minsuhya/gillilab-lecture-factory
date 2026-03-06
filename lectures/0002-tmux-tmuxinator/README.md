# tmux + tmuxinator 실무 활용 가이드

> 강의 시청 중 따라하거나, 나중에 참고하는 압축 레퍼런스

---

## 빠른 시작 체크리스트

- [ ] tmux 설치 확인 (`tmux -V` → 3.x 이상)
- [ ] tmux 세션 생성 (`tmux new -s myproject`)
- [ ] 기본 단축키 5개 익히기 (Prefix, 분할, 이동, Detach, Attach)
- [ ] `~/.tmux.conf` 설정 적용 (마우스, Prefix 변경, vi 모드)
- [ ] tmuxinator 설치 (`brew install tmuxinator` 또는 `gem install tmuxinator`)
- [ ] 첫 번째 tmuxinator 프로젝트 생성 (`tmuxinator new myproject`)
- [ ] YAML 파일 편집 후 세션 실행 (`tmuxinator start myproject`)
- [ ] 팀 프로젝트 YAML 파일 Git에 커밋

---

## 1. 설치 명령어 모음

### macOS (Homebrew)

```bash
# tmux 설치
brew install tmux

# tmuxinator 설치 (Homebrew 권장)
brew install tmuxinator

# 설치 확인
tmux -V          # tmux 3.x
tmuxinator version
```

### Ubuntu / Debian

```bash
# tmux 설치
sudo apt-get update && sudo apt-get install -y tmux

# Ruby gem으로 tmuxinator 설치
sudo apt-get install -y ruby ruby-dev
gem install tmuxinator

# 설치 확인
tmux -V
tmuxinator version
```

### CentOS / RHEL

```bash
# tmux 설치
sudo yum install -y tmux

# tmuxinator
sudo yum install -y ruby ruby-devel
gem install tmuxinator
```

### 쉘 자동완성 설정 (~/.zshrc)

```bash
# alias 설정
alias mux="tmuxinator"

# Zsh 자동완성 (oh-my-zsh 사용 시)
plugins=(... tmuxinator)

# 적용
source ~/.zshrc
```

---

## 2. tmux 핵심 단축키 빠른 참조

> **Prefix**: 기본값 `Ctrl+B` / 아래 설정 파일 적용 시 `Ctrl+A`

### 세션 관리

| 단축키 / 명령어 | 기능 |
|---|---|
| `C-b d` | 세션 Detach (세션 유지, 터미널 복귀) |
| `C-b s` | 세션 트리 보기 (방향키 탐색 + Enter 이동) |
| `C-b $` | 현재 세션 이름 변경 |
| `C-b (` | 이전 세션으로 이동 |
| `C-b )` | 다음 세션으로 이동 |
| `tmux new -s <이름>` | 새 세션 생성 (이름 지정) |
| `tmux a -t <이름>` | 세션에 Attach |
| `tmux ls` | 세션 목록 |
| `tmux kill-session -t <이름>` | 세션 종료 |

### 윈도우 관리

| 단축키 | 기능 |
|---|---|
| `C-b c` | 새 윈도우 생성 |
| `C-b ,` | 현재 윈도우 이름 변경 |
| `C-b n` | 다음 윈도우 |
| `C-b p` | 이전 윈도우 |
| `C-b 0~9` | 번호로 윈도우 이동 |
| `C-b w` | 윈도우 목록 보기 |
| `C-b &` | 현재 윈도우 닫기 |

### 패인 관리

| 단축키 | 기능 |
|---|---|
| `C-b %` | 수직 분할 (좌우) |
| `C-b "` | 수평 분할 (상하) |
| `C-b 방향키` | 패인 간 이동 |
| `C-b z` | 현재 패인 전체 화면 토글 (Zoom) |
| `C-b x` | 현재 패인 닫기 |
| `C-b {` | 패인 왼쪽으로 이동 |
| `C-b }` | 패인 오른쪽으로 이동 |
| `C-b Space` | 레이아웃 자동 변경 |
| `C-b q` | 패인 번호 표시 (번호 키로 이동) |
| `C-b [` | 스크롤 모드 진입 (q 또는 ESC로 종료) |

### 패인 동기화 (다중 서버 동시 명령)

```bash
# C-b : 로 명령 모드 진입 후 입력
:setw synchronize-panes on    # 동기화 활성화 (모든 패인에 동시 입력)
:setw synchronize-panes off   # 동기화 비활성화
```

---

## 3. 완성된 ~/.tmux.conf 설정

```bash
# ~/.tmux.conf — 실무 기본 설정
# 적용: tmux source-file ~/.tmux.conf
# 또는 tmux 내부에서 C-a r (설정 적용 후)

# ─── Prefix 변경: Ctrl+B → Ctrl+A ───────────────────────────────────────────
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# ─── 기본 설정 ────────────────────────────────────────────────────────────────
set -g default-terminal "screen-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"   # True Color (Neovim용)
set -g history-limit 50000                               # 스크롤 히스토리 5만 줄
set -g mouse on                                          # 마우스 활성화
set -g base-index 1                                      # 윈도우 번호 1부터
setw -g pane-base-index 1                                # 패인 번호 1부터
set -sg escape-time 0                                    # ESC 지연 제거 (Vim 필수)
set -g renumber-windows on                               # 윈도우 닫히면 번호 재정렬

# ─── 키 바인딩 ────────────────────────────────────────────────────────────────
# r: 설정 즉시 리로드
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"

# 패인 분할 — 현재 경로 유지
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
unbind '"'
unbind %

# Vim 스타일 패인 이동 (hjkl)
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# ─── 복사 모드 (vi 스타일) ────────────────────────────────────────────────────
setw -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel

# macOS 클립보드 연동 (pbcopy)
# bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "pbcopy"

# ─── 상태 바 ──────────────────────────────────────────────────────────────────
set -g status-bg colour235
set -g status-fg colour136
set -g status-left-length 40
set -g status-right-length 60
set -g status-left '#[fg=green,bold][#S] '
set -g status-right '#[fg=colour245]#{pane_current_path} #[fg=yellow]%Y-%m-%d %H:%M'
set -g status-interval 60
setw -g window-status-current-style fg=colour166,bold
```

---

## 4. tmuxinator YAML 예제 모음

### 예제 1: SSH 다중 서버 관리

```yaml
# ~/.tmuxinator/servers.yml
name: servers
root: ~

windows:
  - web-servers:
      layout: even-horizontal
      panes:
        - ssh user@web-01.example.com
        - ssh user@web-02.example.com
        - ssh user@web-03.example.com

  - db-servers:
      layout: even-vertical
      panes:
        - ssh user@db-primary.example.com
        - ssh user@db-replica.example.com
```

### 예제 2: 풀스택 개발 환경 (React + FastAPI + PostgreSQL)

```yaml
# ~/.tmuxinator/fullstack.yml
name: fullstack-dev
root: ~/projects/my-fullstack-app

windows:
  - editor:
      layout: main-vertical
      panes:
        - nvim .
        - git log --oneline -10

  - backend:
      root: ~/projects/my-fullstack-app/backend
      layout: main-horizontal
      panes:
        - uvicorn main:app --reload --port 8000
        - |
            source .venv/bin/activate
            bash

  - frontend:
      root: ~/projects/my-fullstack-app/frontend
      layout: main-horizontal
      panes:
        - npm run dev
        - bash

  - database:
      panes:
        - psql -U postgres -d myapp
```

### 예제 3: 모니터링 대시보드

```yaml
# ~/.tmuxinator/monitoring.yml
name: monitoring
root: ~

windows:
  - dashboard:
      layout: tiled
      panes:
        - watch -n 2 'df -h && echo "---" && free -h'
        - htop
        - tail -f /var/log/nginx/access.log
        - tail -f /var/log/app/error.log

  - docker:
      layout: even-vertical
      panes:
        - watch -n 3 'docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"'
        - docker stats --no-trunc

  - network:
      panes:
        - watch -n 2 'ss -tuln | grep LISTEN'
```

---

## 5. tmuxinator 명령어 빠른 참조

| 명령어 | 설명 |
|---|---|
| `tmuxinator new <이름>` | 새 프로젝트 생성 (에디터 오픈) |
| `tmuxinator start <이름>` | 세션 시작 |
| `tmuxinator stop <이름>` | 세션 종료 |
| `tmuxinator list` | 프로젝트 목록 |
| `tmuxinator edit <이름>` | YAML 파일 편집 |
| `tmuxinator delete <이름>` | 프로젝트 삭제 |
| `tmuxinator copy <원본> <새이름>` | 프로젝트 복사 |
| `mux start <이름>` | alias 사용 시 |

---

## 6. tmuxinator YAML 레이아웃 옵션

| 레이아웃 | 설명 |
|---|---|
| `main-vertical` | 좌측 메인 패인 + 우측 나머지 (기본값처럼 쓰임) |
| `main-horizontal` | 상단 메인 패인 + 하단 나머지 |
| `even-horizontal` | 모든 패인을 수평으로 균등 분할 |
| `even-vertical` | 모든 패인을 수직으로 균등 분할 |
| `tiled` | 4분할 격자로 균등 배치 |

---

## 7. SSH 자동 tmux attach 설정

```bash
# ~/.zshrc 또는 ~/.bashrc에 추가
# SSH 접속 시 기존 tmux 세션이 있으면 자동 attach, 없으면 새 세션 생성
if [[ -n "$SSH_CONNECTION" ]] && [[ -z "$TMUX" ]]; then
  tmux attach-session -t main 2>/dev/null || tmux new-session -s main
fi
```

---

## 8. 트러블슈팅 빠른 참조

| 문제 | 원인 | 해결 |
|---|---|---|
| 색상 깨짐 (터미널 색이 이상함) | TERM 환경변수 불일치 | `set -g default-terminal "screen-256color"` 추가 |
| 마우스 스크롤 안 됨 | 마우스 모드 비활성화 | `set -g mouse on` 추가 후 config 재적용 |
| ESC 키 지연 (Vim에서 느림) | escape-time 기본값이 높음 | `set -sg escape-time 0` 추가 |
| tmuxinator "tmux not found" 오류 | PATH 문제 | `export TMUX_BIN=$(which tmux)` 설정 |
| tmuxinator Ruby 버전 오류 | 시스템 Ruby 버전 낮음 | `brew install ruby` 또는 rbenv 사용 |
| 패인 스크롤이 안 됨 | 히스토리 한계 | `set -g history-limit 50000` 추가 |
| 창 닫을 때 번호가 불연속 | 번호 재정렬 비활성화 | `set -g renumber-windows on` 추가 |
| tmux 내에서 pbcopy 안 됨 (macOS) | 클립보드 연동 미설정 | `reattach-to-user-namespace` 설치 또는 tmux 3.2+ 사용 |
| synchronize-panes 후 해제를 잊음 | 동기화 상태 표시 없음 | 상태 바에 `#{?pane_synchronized,[SYNC],}` 추가 |

---

## 9. 환경 확인 명령어

```bash
# 버전 확인
tmux -V                          # tmux 3.x 이상 권장
ruby -v                          # 2.4.0 이상 (tmuxinator 의존성)
tmuxinator version               # tmuxinator 버전

# tmux 서버 상태
tmux ls                          # 실행 중인 세션 목록
tmux info                        # tmux 서버 전체 정보

# tmuxinator 프로젝트 목록
tmuxinator list
ls ~/.tmuxinator/                # YAML 파일 직접 확인

# 설정 파일 문법 확인
tmux source-file ~/.tmux.conf    # 오류 없으면 적용됨

# 현재 tmux 세션 내 패인 목록
tmux list-panes -a

# 특정 세션 상세 정보
tmux display-message -p '#S:#W.#P'  # 현재 세션:윈도우.패인
```

---

## 10. 유용한 링크

| 항목 | URL |
|---|---|
| tmux 공식 GitHub | https://github.com/tmux/tmux |
| tmux 위키 (단축키 전체 목록) | https://github.com/tmux/tmux/wiki |
| tmuxinator 공식 GitHub | https://github.com/tmuxinator/tmuxinator |
| tmuxinator YAML 레퍼런스 | https://github.com/tmuxinator/tmuxinator#project-configuration |
| Oh My Tmux (인기 설정 모음) | https://github.com/gpakosz/.tmux |
| tmux Cheat Sheet | https://tmuxcheatsheet.com |
| Awesome tmux (플러그인 목록) | https://github.com/rothgar/awesome-tmux |
