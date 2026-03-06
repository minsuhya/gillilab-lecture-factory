# tmux + tmuxinator 실무 활용 가이드 — 강의 스크립트

> 예상 총 강의 시간: 약 70분

---

## [인트로] 터미널 멀티플렉서, tmux를 시작하며 [약 5분]

[인트로]

개발자는 하루에 터미널을 수십 번 열고 닫습니다. 서버에 SSH로 접속해 작업하다가 네트워크가 끊기면 그 순간까지 진행한 모든 작업이 사라집니다. 로컬에서 프론트엔드 개발 서버, 백엔드 API 서버, 데이터베이스 클라이언트를 동시에 띄우려면 터미널 탭을 서너 개 열고 왔다 갔다 해야 합니다.

tmux는 이 문제를 해결합니다. tmux는 터미널 멀티플렉서(Terminal Multiplexer)로, 하나의 터미널 창에서 여러 세션과 창, 패인을 동시에 관리합니다. 세션은 tmux 서버에서 독립적으로 실행되기 때문에 SSH 연결이 끊어져도, 터미널 앱을 닫아도 작업이 그대로 유지됩니다.

오늘 강의에서는 tmux의 핵심 개념과 실무에서 바로 쓸 수 있는 단축키 20개, 그리고 tmuxinator를 이용해 개발 환경을 한 명령어로 자동화하는 방법까지 다룹니다.

---

## [섹션 1] tmux 계층 구조와 설치 [약 10분]

[📌 섹션 전환]

tmux를 효과적으로 사용하려면 먼저 구조를 이해해야 합니다. tmux는 4계층으로 구성됩니다.

```
tmux Server
└── Session (세션)
    └── Window (윈도우/탭)
        └── Pane (패인/분할창)
```

**서버(Server)**는 백그라운드에서 실행되며 모든 세션을 관리합니다. tmux를 처음 실행하면 자동으로 시작됩니다.

**세션(Session)**은 하나의 작업 맥락입니다. 프로젝트별로 세션을 만들어 관리합니다. `my-app`, `monitoring`, `server-1` 같은 이름으로 구분합니다.

**윈도우(Window)**는 세션 내의 탭입니다. 브라우저 탭처럼 동일 세션 안에서 여러 작업 화면을 전환합니다.

**패인(Pane)**은 윈도우를 수직 또는 수평으로 분할한 실제 터미널 영역입니다. 하나의 화면에서 여러 터미널을 동시에 볼 수 있습니다.

[💻 데모]

macOS에서 Homebrew로 설치합니다. 터미널에 다음을 입력합니다.

```bash
# macOS
brew install tmux

# Ubuntu / Debian
sudo apt-get update && sudo apt-get install -y tmux

# CentOS / RHEL
sudo yum install -y tmux
```

설치 완료 후 버전을 확인합니다.

```bash
tmux -V
```

`tmux 3.x` 형태의 출력이 나오면 설치가 정상적으로 완료된 것입니다.

tmux를 처음 실행합니다.

```bash
tmux
```

화면 하단에 초록색 상태 바가 나타나면 tmux 세션에 진입한 것입니다. 상태 바 왼쪽에 `[0]`은 세션 번호, `0:zsh`는 첫 번째 윈도우를 의미합니다.

[🎯 핵심 포인트]

tmux의 모든 명령어는 **Prefix 키**로 시작합니다. 기본 Prefix는 `Ctrl + B`입니다. Prefix를 누른 후 손을 떼고 다음 키를 입력하는 방식입니다. 이 강의에서 Prefix는 `C-b`로 표기합니다.

---

## [섹션 2] 실무 필수 단축키 20개 [약 15분]

[📌 섹션 전환]

tmux 단축키는 크게 세션, 윈도우, 패인 관리로 나뉩니다. 실무에서 가장 자주 쓰는 20개를 익히겠습니다.

### 세션 관리

[🎯 핵심 포인트]

가장 중요한 단축키는 **Detach**입니다. `C-b d`를 누르면 현재 세션에서 빠져나오되, 세션은 백그라운드에서 계속 실행됩니다. SSH가 끊겨도 서버의 tmux 세션은 살아있습니다.

[💻 데모]

세션 관련 명령어를 실행해 보겠습니다. 먼저 이름 있는 세션을 새로 만듭니다.

```bash
# 이름 있는 세션 생성
tmux new-session -s myproject

# 축약형
tmux new -s myproject
```

세션에서 빠져나옵니다(Detach).

```bash
# 단축키: C-b d
```

세션 목록을 확인합니다.

```bash
tmux list-sessions
# 또는
tmux ls
```

`myproject: 1 windows (created ...)` 형태로 세션이 살아있음을 확인할 수 있습니다.

세션으로 다시 접속(Attach)합니다.

```bash
# 이름으로 접속
tmux attach-session -t myproject

# 축약형
tmux a -t myproject

# 마지막 세션으로 접속
tmux a
```

tmux 내부에서 세션 트리를 보려면 `C-b s`를 사용합니다. 방향키로 탐색하고 Enter로 이동합니다.

```
단축키 정리:
C-b d        — 세션 Detach (세션은 유지)
C-b s        — 세션 트리 보기 (방향키 탐색 + Enter 이동)
C-b $        — 현재 세션 이름 변경
C-b (        — 이전 세션으로 이동
C-b )        — 다음 세션으로 이동
```

### 윈도우 관리

[💻 데모]

새 윈도우를 만듭니다.

```bash
# 단축키: C-b c
```

화면 하단 상태 바에 `0:zsh  1:zsh*`처럼 윈도우가 추가된 것을 볼 수 있습니다. `*`가 현재 윈도우를 표시합니다.

```
단축키 정리:
C-b c        — 새 윈도우 생성
C-b ,        — 현재 윈도우 이름 변경
C-b n        — 다음 윈도우로 이동
C-b p        — 이전 윈도우로 이동
C-b 0~9      — 번호로 윈도우 직접 이동
C-b w        — 윈도우 목록 보기
C-b &        — 현재 윈도우 닫기 (확인 필요)
```

[⏸️ 잠깐]

윈도우 이름을 지정하는 습관이 중요합니다. 기본적으로 실행 중인 명령어 이름이 표시되지만, `C-b ,`로 `frontend`, `backend`, `logs`처럼 의미 있는 이름을 붙이면 탐색이 훨씬 빠릅니다.

### 패인 관리

[📝 코드 설명]

패인은 tmux의 핵심입니다. 하나의 화면을 분할해 여러 터미널을 동시에 보여줍니다.

[💻 데모]

현재 윈도우를 수직으로 분할합니다.

```bash
# 수직 분할 (좌우로 나뉨): C-b %
```

수평으로 분할합니다.

```bash
# 수평 분할 (위아래로 나뉨): C-b "
```

패인 간 이동은 `C-b` + 방향키를 사용합니다.

```
단축키 정리:
C-b %        — 수직 분할 (좌우)
C-b "        — 수평 분할 (상하)
C-b 방향키   — 패인 간 이동
C-b z        — 현재 패인 전체 화면 토글 (Zoom)
C-b x        — 현재 패인 닫기
C-b {        — 패인 왼쪽으로 이동
C-b }        — 패인 오른쪽으로 이동
C-b Space    — 패인 레이아웃 자동 변경
C-b q        — 패인 번호 표시 (번호 키로 이동)
```

[🎯 핵심 포인트]

**`C-b z`** (Zoom)은 실무에서 매우 유용합니다. 현재 패인을 전체 화면으로 확대해 코드를 집중해서 볼 수 있고, 다시 `C-b z`를 누르면 원래 레이아웃으로 돌아옵니다. 특히 로그를 자세히 확인할 때 활용합니다.

### 다중 서버 명령 전송: synchronize-panes

[📌 섹션 전환]

실무에서 강력한 기능 중 하나가 **패인 동기화(synchronize-panes)**입니다. 여러 패인에 동시에 같은 명령어를 전송하는 기능으로, 다중 서버 배포나 점검 시 활용합니다.

[💻 데모]

패인을 3개 이상 만든 상태에서 다음 명령어를 tmux 명령 모드로 실행합니다.

```bash
# C-b : 로 명령 모드 진입 후 입력
:setw synchronize-panes on
```

이 상태에서 어느 패인에 타이핑해도 모든 패인에 동시에 입력됩니다. 서버 3대에 동시에 `sudo systemctl restart nginx`를 실행하는 상황을 생각해봅니다. 작업 완료 후 동기화를 해제합니다.

```bash
:setw synchronize-panes off
```

[🎯 핵심 포인트]

synchronize-panes를 활용하면 10대 서버에 동시에 패키지 업데이트나 서비스 재시작 명령을 보낼 수 있습니다. 단순 반복 작업을 획기적으로 줄여줍니다.

---

## [섹션 3] ~/.tmux.conf 기본 설정 [약 10분]

[📌 섹션 전환]

기본 tmux 설정에는 불편한 점이 있습니다. Prefix `Ctrl+B`는 손이 먼 위치에 있고, 마우스 스크롤이 안 되며, 클립보드 연동도 직관적이지 않습니다. `~/.tmux.conf` 파일로 이 모든 것을 개선합니다.

[💻 데모]

설정 파일을 만듭니다. 화면에서 다음을 실행합니다.

```bash
vim ~/.tmux.conf
```

다음 설정을 입력합니다.

```bash
# ~/.tmux.conf — 실무 기본 설정

# ─── Prefix 변경: Ctrl+B → Ctrl+A ───────────────────────────────────────────
# Ctrl+A는 손 위치상 더 편리합니다
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# ─── 기본 설정 ────────────────────────────────────────────────────────────────
set -g default-terminal "screen-256color"   # 256색 지원
set -g history-limit 50000                  # 스크롤 히스토리 5만 줄
set -g mouse on                             # 마우스 스크롤 / 패인 클릭 활성화
set -g base-index 1                         # 윈도우 번호 1부터 시작
setw -g pane-base-index 1                   # 패인 번호 1부터 시작
set -sg escape-time 0                       # ESC 키 지연 제거 (Vim 사용자 필수)

# ─── 키 바인딩 ────────────────────────────────────────────────────────────────
# r: 설정 파일 즉시 리로드
bind r source-file ~/.tmux.conf \; display-message "Config reloaded!"

# 패인 분할을 | 와 - 로 직관적으로 변경
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

# ─── 상태 바 ──────────────────────────────────────────────────────────────────
set -g status-bg colour235
set -g status-fg colour136
set -g status-left-length 40
set -g status-left '#[fg=green][#S] '
set -g status-right '#[fg=yellow]%Y-%m-%d %H:%M'
set -g status-interval 60
```

저장 후 설정을 적용합니다.

```bash
tmux source-file ~/.tmux.conf
```

또는 tmux 내부에서 `C-a r` (새 Prefix 적용 후)을 누르면 "Config reloaded!" 메시지가 나타납니다.

[🎯 핵심 포인트]

설정에서 가장 중요한 세 가지는 다음과 같습니다.

첫째, `set -g mouse on` — 마우스 스크롤과 패인 클릭이 가능해집니다.
둘째, `set -sg escape-time 0` — Vim을 사용한다면 반드시 설정합니다. ESC 키 지연이 없어집니다.
셋째, `set -g history-limit 50000` — 기본값은 2000줄이지만, 긴 로그를 볼 때 50000줄이 필요합니다.

---

## [섹션 4] tmuxinator 소개 및 설치 [약 5분]

[📌 섹션 전환]

매일 아침 개발을 시작할 때 터미널을 열고, tmux 세션을 만들고, 윈도우를 나누고, 각 패인에서 서버를 실행하는 작업을 반복합니다. tmuxinator는 이 전체 과정을 YAML 파일 하나로 자동화합니다.

[💻 데모]

tmuxinator를 설치합니다. Ruby gem 방식과 Homebrew 방식을 모두 안내합니다.

```bash
# macOS — Homebrew (권장)
brew install tmuxinator

# Ruby gem (macOS / Linux 공통)
gem install tmuxinator

# 설치 확인
tmuxinator version
```

설치 후 쉘 자동완성을 설정합니다. `~/.zshrc` 또는 `~/.bashrc`에 추가합니다.

```bash
# Zsh 자동완성
echo 'source $HOME/.oh-my-zsh/plugins/tmuxinator/tmuxinator.zsh' >> ~/.zshrc
# 또는 직접 alias 설정
echo 'alias mux="tmuxinator"' >> ~/.zshrc
source ~/.zshrc
```

[⏸️ 잠깐]

tmuxinator는 Ruby 2.4 이상과 tmux 1.8 이상이 필요합니다. macOS에는 시스템 Ruby가 설치되어 있지만 버전이 낮을 수 있습니다. Homebrew로 Ruby를 설치하거나 rbenv를 사용하는 것을 권장합니다.

```bash
# Ruby 버전 확인
ruby -v  # 2.4.0 이상이어야 함

# tmux 버전 확인
tmux -V  # 1.8 이상이어야 함
```

---

## [섹션 5] tmuxinator YAML 프로젝트 설정 [약 10분]

[📌 섹션 전환]

tmuxinator는 `~/.tmuxinator/` 디렉토리에 프로젝트별 YAML 파일을 저장합니다. 파일 하나가 하나의 개발 환경을 정의합니다.

[💻 데모]

새 프로젝트를 만듭니다.

```bash
tmuxinator new myproject
```

기본 에디터가 열리며 템플릿 YAML이 표시됩니다. YAML 구조를 분석합니다.

```yaml
# ~/.tmuxinator/myproject.yml

name: myproject           # 프로젝트 이름 (tmux 세션 이름)
root: ~/projects/myapp    # 작업 디렉토리 (모든 창의 기본 경로)

# 환경 변수 (선택)
# environment:
#   RAILS_ENV: development

# tmux 옵션 (선택)
# tmux_options: -f ~/.tmux.conf

windows:
  - editor:               # 윈도우 이름
      layout: main-vertical   # 레이아웃: main-vertical, even-horizontal, tiled 등
      panes:
        - vim               # 각 패인에서 실행할 명령어
        - git status

  - server:               # 두 번째 윈도우
      panes:
        - npm run dev

  - logs:                 # 세 번째 윈도우
      panes:
        - tail -f log/development.log
```

[📝 코드 설명]

YAML의 핵심 필드를 정리합니다.

- `name` — tmux 세션 이름입니다. `tmuxinator start myproject` 시 이 이름으로 세션이 생성됩니다.
- `root` — 모든 윈도우/패인의 기본 작업 디렉토리입니다. 개별 윈도우에서 `root:`로 오버라이드 가능합니다.
- `windows` — 윈도우 목록입니다. 각 항목이 tmux 윈도우 하나입니다.
- `layout` — 패인 배치 방식입니다. `main-vertical`, `main-horizontal`, `even-horizontal`, `even-vertical`, `tiled` 중 선택합니다.
- `panes` — 각 패인에서 시작 시 실행할 명령어입니다.

tmuxinator 주요 명령어를 정리합니다.

```bash
tmuxinator new <이름>      # 새 프로젝트 생성 (에디터 오픈)
tmuxinator start <이름>    # 프로젝트 시작 (세션 생성)
tmuxinator stop <이름>     # 세션 종료
tmuxinator list            # 프로젝트 목록
tmuxinator edit <이름>     # 설정 파일 편집
tmuxinator delete <이름>   # 프로젝트 삭제
tmuxinator copy <원본> <새이름>  # 프로젝트 복사

# alias 설정 시
mux start myproject
mux list
```

---

## [섹션 6] 실무 예제 3가지 [약 20분]

[📌 섹션 전환]

이제 실제로 사용하는 3가지 시나리오를 통해 tmux + tmuxinator의 실전 활용을 살펴보겠습니다.

### 예제 1: 원격 서버 SSH 세션 관리

[💻 데모]

원격 서버에서 tmux 세션을 유지하는 시나리오입니다. 긴 배치 작업, 빌드, 마이그레이션 등 SSH가 끊겨도 작업이 계속되어야 할 때 사용합니다.

서버에 SSH로 접속한 직후, 반드시 tmux 세션을 만들고 작업합니다.

```bash
# 서버 접속 후
ssh user@my-server.example.com

# tmux 세션 시작
tmux new -s deploy

# 긴 배치 작업 실행
python3 migrate_database.py --all-tables
```

네트워크가 끊겨도 서버의 tmux 세션은 살아있습니다. 재접속 후 세션으로 돌아옵니다.

```bash
# 재접속 후
ssh user@my-server.example.com

# 실행 중인 세션 확인
tmux ls

# 세션 재접속
tmux a -t deploy
```

다중 서버를 관리하는 tmuxinator 설정입니다.

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

이 설정을 시작하면 3개의 SSH 세션이 즉시 연결됩니다. 여기서 `synchronize-panes`를 켜면 3개 서버에 동시에 명령을 보낼 수 있습니다.

### 예제 2: 풀스택 개발 환경 자동화

[💻 데모]

React 프론트엔드 + FastAPI 백엔드 + PostgreSQL 개발 환경을 한 명령어로 시작하는 설정입니다.

```yaml
# ~/.tmuxinator/fullstack.yml
name: fullstack-dev
root: ~/projects/my-fullstack-app

windows:
  - editor:
      layout: main-vertical
      panes:
        - nvim .                           # 메인 에디터 (전체 화면의 70%)
        - git log --oneline -10            # 최근 커밋 확인

  - backend:
      root: ~/projects/my-fullstack-app/backend
      layout: main-horizontal
      panes:
        - uvicorn main:app --reload --port 8000    # FastAPI 개발 서버
        - |                                         # 가상환경 활성화 + 쉘
            source .venv/bin/activate
            bash

  - frontend:
      root: ~/projects/my-fullstack-app/frontend
      layout: main-horizontal
      panes:
        - npm run dev                      # Vite 개발 서버 (포트 5173)
        - bash                             # 빈 쉘 (테스트 실행용)

  - database:
      panes:
        - psql -U postgres -d myapp        # PostgreSQL 클라이언트
```

실행 방법입니다.

```bash
tmuxinator start fullstack-dev
# 또는
mux start fullstack-dev
```

실행하면 editor, backend, frontend, database 4개 윈도우가 만들어지고, 각 윈도우에 지정된 명령어가 자동으로 실행됩니다. 매일 아침 이 명령어 하나로 전체 개발 환경이 준비됩니다.

### 예제 3: 모니터링 대시보드

[💻 데모]

서버 상태, 애플리케이션 로그, 도커 컨테이너 현황을 한 화면에 배치하는 모니터링 대시보드입니다.

```yaml
# ~/.tmuxinator/monitoring.yml
name: monitoring
root: ~

windows:
  - dashboard:
      layout: tiled    # 4분할 격자 레이아웃
      panes:
        - watch -n 2 'df -h && echo "---" && free -h'   # 디스크/메모리 (2초 갱신)
        - htop                                            # CPU/프로세스 모니터
        - tail -f /var/log/nginx/access.log               # Nginx 접근 로그
        - tail -f /var/log/app/error.log                  # 앱 에러 로그

  - docker:
      layout: even-vertical
      panes:
        - watch -n 3 'docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"'
        - docker stats --no-trunc                          # 컨테이너 리소스 실시간

  - network:
      panes:
        - watch -n 2 'ss -tuln | grep LISTEN'             # 열린 포트 확인
```

실행합니다.

```bash
mux start monitoring
```

`tiled` 레이아웃은 패인을 자동으로 균등하게 배치합니다. dashboard 윈도우에서 4개 패인이 격자 형태로 배치되어 서버 전체 상태를 한눈에 볼 수 있습니다.

[🎯 핵심 포인트]

`watch -n 2 '명령어'` 는 tmux와 찰떡 조합입니다. 특정 명령어를 N초마다 자동으로 재실행하므로 별도 반복 스크립트 없이 실시간 모니터링이 가능합니다.

---

## [섹션 7] 베스트 프랙티스 [약 5분]

[📌 섹션 전환]

30년 개발 경험에서 정리한 tmux 실무 원칙입니다.

**세션 네이밍 규칙을 정합니다.**

```bash
# 좋은 예: 역할이 명확한 이름
tmux new -s api-v2
tmux new -s frontend-refactor
tmux new -s prod-monitoring

# 나쁜 예
tmux new -s test
tmux new -s asdf
```

프로젝트나 역할 기반으로 이름을 붙이면 `tmux ls`에서 즉시 맥락을 파악할 수 있습니다.

**세션을 너무 많이 만들지 않습니다.**

세션을 무한정 쌓으면 관리가 어렵습니다. 완료된 프로젝트의 세션은 정리합니다.

```bash
# 특정 세션 종료
tmux kill-session -t myproject

# 모든 세션 종료 (tmux 서버 종료)
tmux kill-server
```

**tmuxinator 파일을 버전 관리합니다.**

```bash
# 팀 공유를 위해 프로젝트 로컬에 tmuxinator 파일 포함
mkdir -p .tmuxinator
cp ~/.tmuxinator/myproject.yml .tmuxinator/

# .gitignore에는 제외하지 않고 커밋
git add .tmuxinator/myproject.yml
```

팀원이 같은 개발 환경을 빠르게 구성할 수 있습니다.

**tmux 내에서 Vim을 쓴다면 반드시 이 설정을 적용합니다.**

```bash
# ~/.tmux.conf에 추가
set -sg escape-time 0
set -g default-terminal "screen-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"  # True Color 지원
```

ESC 지연 없이 빠른 모드 전환이 가능합니다.

---

## [섹션 8] 트러블슈팅 [약 5분]

[📌 섹션 전환]

실무에서 자주 만나는 문제와 해결법입니다.

**문제 1: tmux 내에서 색상이 깨집니다.**

```bash
# ~/.tmux.conf에 추가
set -g default-terminal "screen-256color"

# ~/.zshrc 또는 ~/.bashrc에 추가
export TERM=xterm-256color
```

**문제 2: 마우스 스크롤이 동작하지 않습니다.**

```bash
# ~/.tmux.conf에 추가
set -g mouse on
```

설정 추가 후 `tmux source-file ~/.tmux.conf`로 반영합니다.

**문제 3: tmuxinator 시작 시 "tmux not found" 오류가 납니다.**

tmux가 설치되었는데도 PATH 문제로 발생합니다.

```bash
# tmux 경로 확인
which tmux

# tmuxinator 환경 변수로 경로 지정
export TMUX_BIN=$(which tmux)
```

**문제 4: SSH 접속 시 기존 세션에 자동 연결하려면**

`~/.zshrc`에 다음을 추가합니다. SSH 접속 시 기존 tmux 세션이 있으면 자동으로 attach합니다.

```bash
# SSH 접속 시 자동 tmux attach
if [[ -n "$SSH_CONNECTION" ]] && [[ -z "$TMUX" ]]; then
  tmux attach-session -t main 2>/dev/null || tmux new-session -s main
fi
```

**문제 5: 패인 내용이 잘려서 보입니다 (스크롤이 안 됩니다).**

```bash
# 스크롤 모드 진입: C-b [
# 스크롤: Page Up / Page Down 또는 방향키
# 스크롤 모드 종료: q 또는 ESC

# 히스토리를 늘려서 더 많이 스크롤
set -g history-limit 50000  # ~/.tmux.conf
```

---

## [아웃트로] 마무리 [약 5분]

[인트로]

오늘 강의에서 tmux의 4계층 구조부터 실무 단축키 20개, `~/.tmux.conf` 기본 설정, tmuxinator를 이용한 개발 환경 자동화까지 다루었습니다.

tmux를 처음 사용하면 단축키가 낯설게 느껴질 수 있습니다. 처음에는 세션 생성(`tmux new -s`), Detach(`C-b d`), Attach(`tmux a -t`), 패인 분할(`C-b %`, `C-b "`) 이 다섯 가지만 익히면 충분합니다. 익숙해질수록 나머지 단축키가 자연스럽게 손에 붙습니다.

tmuxinator는 개발 환경을 처음 구성할 때 시간이 걸리지만, 한 번 YAML을 작성해두면 이후에는 `mux start 프로젝트명` 하나로 모든 것이 준비됩니다. 팀원과 공유하면 신규 개발자의 환경 구성 시간도 크게 줄일 수 있습니다.

다음 강의에서는 tmux 플러그인 매니저(TPM)와 함께 테마, 세션 저장 플러그인 등 생산성을 더 높이는 방법을 다루겠습니다.

이번 강의에서 다룬 설정 파일과 tmuxinator YAML 예제는 GitHub 저장소에서 확인할 수 있습니다. 링크는 설명란에 있습니다.

---

> 총 분량: 약 70분 (인트로 5분 + 계층/설치 10분 + 단축키 15분 + 설정 10분 + tmuxinator 설치 5분 + YAML 10분 + 예제 3개 20분 + 베스트 프랙티스 5분 + 아웃트로 5분 — 섹션 간 전환 및 질의응답 제외)
