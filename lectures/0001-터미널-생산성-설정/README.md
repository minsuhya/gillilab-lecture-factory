# 터미널 생산성 극대화 - 실용 가이드

> 강의 시청 중 따라하거나, 나중에 참고하는 압축 레퍼런스

---

## 빠른 시작 체크리스트

- [ ] Homebrew 설치
- [ ] iTerm2 설치
- [ ] Oh My Zsh 설치
- [ ] Powerlevel10k 설치
- [ ] 개발용 폰트 설치 (JetBrains Mono / Fira Code)
- [ ] zsh-autosuggestions 플러그인 설치
- [ ] zsh-syntax-highlighting 플러그인 설치
- [ ] fzf 설치 + 셸 통합
- [ ] 현대적 CLI 도구 설치 (exa, bat, fd, ripgrep, git-delta)
- [ ] `~/.zshrc` 설정 완료 (alias, plugins, functions)
- [ ] Git delta 설정 완료
- [ ] dotfiles 저장소 생성 + 백업

---

## 1. 설치 명령어 모음

### 환경 확인 (먼저 실행)

```bash
uname -m          # arm64 = Apple Silicon / x86_64 = Intel
sw_vers           # macOS 버전 확인
echo $SHELL       # /bin/zsh 여야 함
```

### Homebrew

```bash
# 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Apple Silicon 전용 — PATH 설정
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# 설치 확인
brew --version
```

### iTerm2

```bash
brew install --cask iterm2
```

> 첫 실행 시 "확인되지 않은 개발자" 경고 → 시스템 설정 → 개인정보 보호 및 보안 → "여전히 열기"

### 개발용 폰트

```bash
brew install --cask font-jetbrains-mono font-fira-code
```

### 필수 패키지 일괄 설치

```bash
brew install \
  git \
  exa \
  bat \
  fd \
  ripgrep \
  fzf \
  git-delta \
  htop \
  tldr \
  direnv
```

### Oh My Zsh

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Powerlevel10k 테마

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
  ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

### zsh-autosuggestions

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### zsh-syntax-highlighting

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
  ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### fzf 셸 통합

```bash
# brew install fzf 이후 반드시 실행
$(brew --prefix)/opt/fzf/install
# 3가지 질문 모두 y 입력
```

---

## 2. 완성된 ~/.zshrc 설정

> 아래 내용을 그대로 복사해서 `~/.zshrc`에 붙여넣기 (기존 내용 대체)

```bash
# Powerlevel10k Instant Prompt (반드시 최상단)
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# Oh My Zsh 경로
export ZSH="$HOME/.oh-my-zsh"

# 테마
ZSH_THEME="powerlevel10k/powerlevel10k"

# 플러그인 (zsh-syntax-highlighting은 반드시 마지막에)
plugins=(
  git
  z
  zsh-autosuggestions
  zsh-syntax-highlighting
)

source $ZSH/oh-my-zsh.sh

# ─── 현대적 CLI 별칭 ──────────────────────────────────────────
alias ls='exa'
alias ll='exa -l --git'
alias la='exa -la --git'
alias lt='exa --tree --level=2'
alias cat='bat --paging=never'
alias find='fd'
alias grep='rg'

# ─── Git 별칭 ─────────────────────────────────────────────────
alias gs='git status'
alias ga='git add'
alias gaa='git add --all'
alias gc='git commit -m'
alias gca='git commit --amend'
alias gp='git push'
alias gpl='git pull'
alias gf='git fetch'
alias gb='git branch'
alias gco='git checkout'
alias gcb='git checkout -b'
alias gl='git log --oneline --graph --decorate'
alias gla='git log --oneline --graph --decorate --all'
alias gd='git diff'
alias gds='git diff --staged'
alias gst='git stash'
alias gsp='git stash pop'

# ─── 유틸리티 함수 ────────────────────────────────────────────
# 디렉토리 생성 후 바로 이동
mkcd() { mkdir -p "$1" && cd "$1" }

# 새 브랜치 생성 및 전환
gnb() {
  [ -z "$1" ] && echo "Usage: gnb <branch-name>" && return 1
  git checkout -b "$1"
}

# 전체 add + commit + push 한 번에
gcp() {
  [ -z "$1" ] && echo "Usage: gcp <commit-message>" && return 1
  git add --all && git commit -m "$1" && git push
}

# fzf로 브랜치 검색 후 전환
gcof() {
  local branch
  branch=$(git branch --all | fzf | tr -d ' ')
  [ -n "$branch" ] && git checkout "${branch#remotes/origin/}"
}

# ─── fzf ──────────────────────────────────────────────────────
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
export FZF_DEFAULT_COMMAND='fd --type f'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"

# ─── direnv (선택사항) ─────────────────────────────────────────
# direnv를 설치한 경우 활성화
# eval "$(direnv hook zsh)"

# ─── Powerlevel10k ────────────────────────────────────────────
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```

설정 적용:

```bash
source ~/.zshrc

# Powerlevel10k 설정 마법사 (최초 1회)
p10k configure
```

---

## 3. 핵심 단축키 & 별칭 빠른 참조

### 셸 기본 단축키

| 단축키 | 기능 |
|---|---|
| `Ctrl+R` | fzf 히스토리 검색 |
| `→` (오른쪽 화살표) | autosuggestions 제안 수락 |
| `Ctrl+U` | 현재 줄 전체 삭제 |
| `Ctrl+W` | 커서 앞 단어 삭제 |
| `Ctrl+A` | 줄 맨 앞으로 이동 |
| `Ctrl+E` | 줄 맨 끝으로 이동 |

### iTerm2 키 매핑 (Preferences → Profiles → Keys)

| 키 | Escape Sequence | 기능 |
|---|---|---|
| `⌘+←` | `OH` | 줄 시작으로 이동 |
| `⌘+→` | `OF` | 줄 끝으로 이동 |
| `⌥+←` | `b` | 단어 단위 왼쪽 이동 |
| `⌥+→` | `f` | 단어 단위 오른쪽 이동 |
| `⌥+⌫` | Hex `0x17` | 단어 단위 삭제 |

### fzf 사용법

| 명령어 | 기능 |
|---|---|
| `Ctrl+R` | 명령어 히스토리 퍼지 검색 |
| `vim **<Tab>` | 파일 퍼지 검색 후 열기 |
| `cd **<Tab>` | 디렉토리 퍼지 검색 후 이동 |
| `kill -9 <Tab>` | 프로세스 퍼지 검색 후 종료 |

### z 디렉토리 점프

| 명령어 | 기능 |
|---|---|
| `z proj` | 과거에 방문한 "proj" 포함 디렉토리로 점프 |
| `z down` | ~/Downloads로 점프 |
| `z my-app` | 방문 기록 기반 최적 매칭 디렉토리로 이동 |

> z는 방문 기록이 쌓여야 동작함. 먼저 `cd`로 디렉토리들을 방문할 것.

### Git 별칭

| 별칭 | 원래 명령어 |
|---|---|
| `gs` | `git status` |
| `ga` | `git add` |
| `gaa` | `git add --all` |
| `gc "메시지"` | `git commit -m "메시지"` |
| `gp` | `git push` |
| `gpl` | `git pull` |
| `gb` | `git branch` |
| `gco` | `git checkout` |
| `gcb` | `git checkout -b` |
| `gl` | `git log --oneline --graph --decorate` |
| `gd` | `git diff` |
| `gds` | `git diff --staged` |
| `gst` | `git stash` |
| `gsp` | `git stash pop` |
| `gcp "메시지"` | add all + commit + push (커스텀 함수) |
| `gcof` | fzf로 브랜치 검색 후 전환 (커스텀 함수) |
| `gnb "브랜치명"` | 새 브랜치 생성 + 전환 (커스텀 함수) |

### 현대 도구 별칭

| 별칭 | 원래 명령어 | 차이점 |
|---|---|---|
| `ls` | `exa` | 컬러 출력, 아이콘 |
| `ll` | `exa -l --git` | Git 상태 포함 긴 목록 |
| `la` | `exa -la --git` | 숨김 파일 포함 |
| `lt` | `exa --tree --level=2` | 트리 구조 |
| `cat` | `bat --paging=never` | 문법 하이라이팅 |
| `find` | `fd` | 5.7배 빠름, 직관적 문법 |
| `grep` | `rg` (ripgrep) | 5배 빠름, .gitignore 자동 적용 |

---

## 4. Git 설정 (~/.gitconfig)

```bash
# 한 줄씩 실행
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# delta 설정 (brew install git-delta 이후)
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global delta.side-by-side true
git config --global delta.line-numbers true
```

결과 확인:

```bash
cat ~/.gitconfig
```

---

## 5. Brewfile (패키지 백업 & 복원)

### 현재 환경 백업

```bash
# 현재 설치된 모든 패키지를 Brewfile로 저장
cd ~/dotfiles   # dotfiles 저장소 경로
brew bundle dump --force
```

### 완성형 Brewfile 예시

```ruby
# ~/dotfiles/Brewfile

tap "homebrew/cask-fonts"

# CLI 도구
brew "git"
brew "exa"
brew "bat"
brew "fd"
brew "ripgrep"
brew "fzf"
brew "git-delta"
brew "htop"
brew "tldr"
brew "direnv"

# GUI 앱
cask "iterm2"
cask "visual-studio-code"

# 폰트
cask "font-jetbrains-mono"
cask "font-fira-code"
```

### 새 Mac에서 일괄 설치

```bash
brew bundle install
# Brewfile에 있는 모든 패키지 자동 설치
```

---

## 6. dotfiles 백업

### 초기 구성

```bash
mkdir ~/dotfiles && cd ~/dotfiles

# 설정 파일 복사
cp ~/.zshrc zshrc
cp ~/.gitconfig gitconfig

# 심볼릭 링크 스크립트
cat > setup.sh << 'EOF'
#!/bin/bash
ln -sf ~/dotfiles/zshrc ~/.zshrc
ln -sf ~/dotfiles/gitconfig ~/.gitconfig
echo "Dotfiles linked."
EOF
chmod +x setup.sh

# Git 저장소로 관리
git init
git add .
git commit -m "Initial dotfiles"
git remote add origin https://github.com/yourusername/dotfiles.git
git push -u origin main
```

### 정기 백업 스크립트

```bash
cat > ~/dotfiles/backup.sh << 'EOF'
#!/bin/bash
brew bundle dump --force --file=~/dotfiles/Brewfile
cp ~/.zshrc ~/dotfiles/zshrc
cp ~/.gitconfig ~/dotfiles/gitconfig
cd ~/dotfiles
git add .
git commit -m "Backup: $(date '+%Y-%m-%d %H:%M:%S')"
git push
echo "Backup completed."
EOF
chmod +x ~/dotfiles/backup.sh
```

### 새 Mac에서 복원

```bash
git clone https://github.com/yourusername/dotfiles.git ~/dotfiles
cd ~/dotfiles && ./setup.sh
brew bundle install
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 7. 트러블슈팅 빠른 참조

| 문제 | 원인 | 해결 |
|---|---|---|
| `z: command not found` | plugins에 z 미추가 | `plugins=(git z ...)` 추가 후 `source ~/.zshrc` |
| z 명령어가 작동 안 함 | 방문 기록 없음 | `cd` 로 디렉토리 방문 먼저 (z는 학습 필요) |
| 터미널 시작이 5초 이상 | 플러그인 과다 | `plugins` 배열 최소화 (10개 이하) |
| Powerlevel10k 문자 깨짐 | 적합한 폰트 미설치 | `p10k configure` 실행 → 폰트 재설치 |
| `zsh-syntax-highlighting` 미작동 | 플러그인 순서 문제 | plugins 배열 **마지막에** 배치 |
| direnv 미작동 (로드 안 됨) | hook 미설정 | `echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc` 후 `source ~/.zshrc` |
| direnv `.envrc` 미로드 | 허용 안 됨 | `direnv allow` 실행 |
| fzf 단축키(`Ctrl+R`) 미작동 | 셸 통합 미설치 | `$(brew --prefix)/opt/fzf/install` 재실행 |
| Apple Silicon PATH 오류 | `/opt/homebrew/bin` 미포함 | `eval "$(/opt/homebrew/bin/brew shellenv)"` → `~/.zprofile`에 추가 |
| Oh My Zsh 권한 경고 | 디렉토리 권한 문제 | `chmod 755 ~/.oh-my-zsh && chmod 755 ~/.oh-my-zsh/custom` |

---

## 8. 환경 확인 명령어

```bash
# 칩셋 확인
uname -m
# arm64  → Apple Silicon (Homebrew: /opt/homebrew)
# x86_64 → Intel Mac   (Homebrew: /usr/local)

# macOS 버전
sw_vers

# Zsh 버전 (5.8 이상이면 OK)
zsh --version

# 현재 셸 확인 (/bin/zsh 여야 함)
echo $SHELL

# Homebrew 경로 확인
which brew

# 설치된 도구 버전 확인
exa --version
bat --version
fd --version
rg --version
fzf --version
delta --version

# fzf 통합 확인
grep fzf ~/.zshrc
# [ -f ~/.fzf.zsh ] && source ~/.fzf.zsh 가 있어야 함

# Zsh 시작 시간 측정 (0.5초 이하면 양호)
time zsh -i -c exit
```

---

## 9. iTerm2 설정 요약

### 폰트 적용

```
Preferences (⌘+,) → Profiles → Text
→ Font: JetBrains Mono, 14pt
→ "Use ligatures" 체크
```

### 색상 테마 적용

```bash
# Snazzy 테마 다운로드
curl -o ~/Downloads/Snazzy.itermcolors \
  https://raw.githubusercontent.com/sindresorhus/iterm2-snazzy/main/Snazzy.itermcolors
```

```
Preferences → Profiles → Colors → Color Presets → Import
→ Snazzy.itermcolors 선택 → Apply
```

### 투명도 & 블러

```
Preferences → Profiles → Window
→ Transparency: 10
→ Blur: 체크, 값 10
```

### Status Bar

```
Preferences → Profiles → Session
→ Status bar enabled 체크
→ Configure Status Bar
→ 드래그: CPU Utilization, Memory Utilization, Current Directory, Clock, Battery Level
```

---

## 10. 유용한 링크

| 도구 | 링크 |
|---|---|
| iTerm2 | https://iterm2.com |
| Oh My Zsh | https://ohmyz.sh |
| Powerlevel10k | https://github.com/romkatv/powerlevel10k |
| Homebrew | https://brew.sh |
| exa | https://the.exa.website |
| bat | https://github.com/sharkdp/bat |
| fd | https://github.com/sharkdp/fd |
| ripgrep | https://github.com/BurntSushi/ripgrep |
| fzf | https://github.com/junegunn/fzf |
| git-delta | https://github.com/dandavison/delta |
| direnv | https://direnv.net |
| iTerm2 색상 테마 모음 | https://iterm2colorschemes.com |
| Awesome Zsh Plugins | https://github.com/unixorn/awesome-zsh-plugins |
| Modern Unix 도구 목록 | https://github.com/ibraheemdev/modern-unix |

---

## Apple Silicon vs Intel 차이점

| 항목 | Apple Silicon (arm64) | Intel Mac (x86_64) |
|---|---|---|
| Homebrew 경로 | `/opt/homebrew` | `/usr/local` |
| PATH 설정 필요 여부 | 필요 (`.zprofile`에 추가) | 불필요 (자동) |
| fzf 설치 경로 | `/opt/homebrew/opt/fzf` | `/usr/local/opt/fzf` |
| `brew shellenv` | `/opt/homebrew/bin/brew shellenv` | 보통 불필요 |
