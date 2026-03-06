# tmux + tmuxinator — 화면 데모 시트

> 강의 흐름 순서대로 정리 | 멘트 없이 명령어·단축키·예제만

---

## § 1. 계층 구조 (개념 설명)

```
tmux Server
└── Session  ← 프로젝트 단위 (my-app, monitoring ...)
    └── Window   ← 탭 (editor, server, logs ...)
        └── Pane     ← 분할창 (실제 터미널)
```

> **Prefix** = `Ctrl + B`  (이 시트에서 `C-b` 로 표기)

---

## § 2. 설치 및 첫 실행

```bash
# macOS
brew install tmux

# Ubuntu / Debian
sudo apt-get update && sudo apt-get install -y tmux

# 버전 확인
tmux -V

# 첫 실행
tmux
```

---

## § 3. 세션 관리

```bash
# 이름 있는 세션 생성
tmux new -s myproject

# 세션 목록
tmux ls

# Detach (세션 유지, 터미널 복귀)
# → 단축키: C-b d

# 세션 재접속 (Attach)
tmux a -t myproject
tmux a                  # 마지막 세션
```

| 단축키 | 동작 |
|---|---|
| `C-b d` | Detach — 세션 유지하고 나가기 |
| `C-b s` | 세션 트리 보기 (방향키 + Enter) |
| `C-b $` | 세션 이름 변경 |
| `C-b (` / `)` | 이전 / 다음 세션 |

---

## § 4. 윈도우 관리

| 단축키 | 동작 |
|---|---|
| `C-b c` | 새 윈도우 |
| `C-b ,` | 윈도우 이름 변경 |
| `C-b n` / `p` | 다음 / 이전 윈도우 |
| `C-b 0~9` | 번호로 직접 이동 |
| `C-b w` | 윈도우 목록 |
| `C-b &` | 윈도우 닫기 |

---

## § 5. 패인 관리

| 단축키 | 동작 |
|---|---|
| `C-b %` | **수직 분할** (좌우) |
| `C-b "` | **수평 분할** (상하) |
| `C-b 방향키` | 패인 이동 |
| `C-b z` | **줌 토글** (전체화면 ↔ 원래) |
| `C-b x` | 패인 닫기 |
| `C-b {` / `}` | 패인 위치 교환 |
| `C-b Space` | 레이아웃 자동 변경 |
| `C-b q` | 패인 번호 표시 |
| `C-b [` | 스크롤 모드 (`q` / ESC 종료) |

---

## § 6. synchronize-panes — 다중 패인 동시 입력

```bash
# C-b : 로 명령 모드 진입 후 입력
:setw synchronize-panes on

# 모든 패인에 동시에 명령 전송됨
# 예: sudo systemctl restart nginx

# 완료 후 해제
:setw synchronize-panes off
```

> **활용**: SSH 패인 여러 개 열고 동기화 → 다중 서버 동시 명령 전송

---

## § 7. ~/.tmux.conf

```bash
vim ~/.tmux.conf
```

```bash
# ~/.tmux.conf

# Prefix 변경: Ctrl+B → Ctrl+A
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# 기본 설정
set -g default-terminal "screen-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"
set -g history-limit 50000       # 스크롤 히스토리 5만 줄
set -g mouse on                  # 마우스 스크롤·클릭 활성화
set -g base-index 1
setw -g pane-base-index 1
set -sg escape-time 0            # ESC 지연 제거 (Vim 필수)
set -g renumber-windows on

# 설정 리로드 단축키: C-a r
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"

# 패인 분할 (현재 경로 유지)
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
unbind '"'
unbind %

# Vim 스타일 패인 이동 (hjkl)
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# 복사 모드 — vi 스타일
setw -g mode-keys vi
bind-key -T copy-mode-vi v send-keys -X begin-selection
bind-key -T copy-mode-vi y send-keys -X copy-selection-and-cancel

# 상태 바
set -g status-bg colour235
set -g status-fg colour136
set -g status-left '#[fg=green,bold][#S] '
set -g status-right '#[fg=yellow]%Y-%m-%d %H:%M'
set -g status-interval 60
```

```bash
# 설정 적용
tmux source-file ~/.tmux.conf
# 또는 tmux 내부에서 C-a r
```

---

## § 8. tmuxinator 설치

```bash
# macOS (권장)
brew install tmuxinator

# Ruby gem
gem install tmuxinator

# 설치 확인
tmuxinator version

# alias 설정 (~/.zshrc)
alias mux="tmuxinator"
source ~/.zshrc
```

```bash
# 의존성 확인
ruby -v   # 2.4.0 이상
tmux -V   # 1.8 이상
```

---

## § 9. tmuxinator YAML 구조

```bash
# 새 프로젝트 생성 (에디터 오픈)
tmuxinator new myproject
```

```yaml
# ~/.tmuxinator/myproject.yml

name: myproject             # tmux 세션 이름
root: ~/projects/myapp      # 기본 작업 디렉토리

windows:
  - editor:                 # 윈도우 이름
      layout: main-vertical
      panes:
        - vim               # 패인별 실행 명령어
        - git status

  - server:
      panes:
        - npm run dev

  - logs:
      panes:
        - tail -f log/development.log
```

```bash
tmuxinator new <이름>              # 생성
tmuxinator start <이름>            # 시작
tmuxinator stop <이름>             # 종료
tmuxinator list                    # 목록
tmuxinator edit <이름>             # YAML 편집
tmuxinator copy <원본> <새이름>    # 복사
```

> 레이아웃: `main-vertical` · `main-horizontal` · `even-horizontal` · `even-vertical` · `tiled`

---

## § 10. 예제 1 — SSH 세션 관리

```bash
# 서버 접속 후 → tmux 세션 먼저 생성
ssh user@my-server.example.com
tmux new -s deploy

# 긴 작업 실행 (SSH 끊겨도 세션 유지)
python3 migrate_database.py --all-tables

# 네트워크 재접속 후
ssh user@my-server.example.com
tmux ls
tmux a -t deploy
```

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

```bash
mux start servers
# → synchronize-panes on 으로 3대 동시 명령 가능
```

---

## § 11. 예제 2 — 풀스택 개발 환경

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

```bash
mux start fullstack-dev
```

---

## § 12. 예제 3 — 모니터링 대시보드

```yaml
# ~/.tmuxinator/monitoring.yml
name: monitoring
root: ~

windows:
  - dashboard:
      layout: tiled           # 4분할 격자
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

```bash
mux start monitoring
```

---

## § 13. 베스트 프랙티스

```bash
# 세션 이름 — 역할 기반으로
tmux new -s api-v2
tmux new -s frontend-refactor
tmux new -s prod-monitoring

# 완료된 세션 정리
tmux kill-session -t myproject

# tmuxinator YAML 팀 공유 (Git 커밋)
mkdir -p .tmuxinator
cp ~/.tmuxinator/fullstack.yml .tmuxinator/
git add .tmuxinator/fullstack.yml
```

```bash
# SSH 자동 attach (~/.zshrc에 추가)
if [[ -n "$SSH_CONNECTION" ]] && [[ -z "$TMUX" ]]; then
  tmux attach-session -t main 2>/dev/null || tmux new-session -s main
fi
```

---

## § 14. 트러블슈팅

| 문제 | 해결 |
|---|---|
| 색상 깨짐 | `set -g default-terminal "screen-256color"` |
| 마우스 스크롤 안 됨 | `set -g mouse on` |
| ESC 지연 (Vim) | `set -sg escape-time 0` |
| tmuxinator "tmux not found" | `export TMUX_BIN=$(which tmux)` |
| 스크롤 히스토리 부족 | `set -g history-limit 50000` |
| 창 번호 불연속 | `set -g renumber-windows on` |
