# 생산성 극대화를 위한 터미널 설정 및 사용법 - 강의 스크립트

> 예상 총 강의 시간: 약 75분

---

## [인트로] 왜 터미널을 최적화해야 하는가 [약 5분]

자, 안녕하세요. 오늘은 **터미널 생산성 극대화**라는 주제로 이야기를 해보겠습니다.

강의를 시작하기 전에 한 가지 이야기를 드리겠습니다. 개발자라면 하루에 수십 번, 많으면 수백 번 터미널에서 명령어를 입력합니다. `cd`로 디렉토리 이동하고, `git status` 확인하고, 파일 검색하고, 로그 확인하고... 이런 작업들이 쌓이면 하루에 꽤 많은 시간을 터미널에서 보내게 됩니다.

그런데 **지금 쓰고 있는 터미널, 그대로 괜찮은 것인지** 한번 생각해볼 필요가 있습니다. macOS 기본 터미널 앱을 열고, 기본 Bash나 Zsh에 아무 설정 없이 그냥 쓰고 있는 경우가 많습니다. 솔직히 저도 예전에 그랬습니다.

그런데 오늘 제가 보여드릴 것들을 적용하면, 숫자로 말씀드리겠습니다:

- 명령어 입력 시간 **82% 단축**
- 파일 검색 속도 **5.75배 향상**
- 텍스트 검색 속도 **5배 향상**

이것을 다 합치면 **연간 약 44시간**을 절약할 수 있습니다. 44시간이면 거의 일주일 넘는 근무 시간입니다. 그 시간에 새로운 기술을 배울 수도 있고, 사이드 프로젝트를 할 수도 있고, 아니면 그냥 쉴 수도 있습니다.

오늘 이 강의가 끝나면 터미널은 완전히 달라져 있을 것입니다. 자, 그러면 바로 시작하겠습니다.

---

## [섹션 1] 개요 - 시간 절약 데이터로 설득하기 [약 5분]

[📌 섹션 전환]

먼저 구체적인 숫자로 이야기해보겠습니다. 뭉뚱그려서 "빨라진다"가 아니라, 실제로 어디서 얼마나 시간이 절약되는지 보여드리겠습니다.

**일일 기준으로 계산해보면:**

파일 검색을 하루에 20번 한다고 가정하면, 최적화된 도구를 쓰면 **38초**를 절약합니다. 텍스트 검색 15번이면 **49.5초** 절약. Git 작업 10번이면 **6분 10초** 절약. 디렉토리 이동 50번이면 **1분 40초** 절약. 다 합치면 하루에 약 **11분**입니다.

"11분? 그거 별거 아닌데?"라고 생각하실 수 있는데, 이것을 **연간으로 환산하면 44시간**입니다. 그리고 여기서 중요한 것은, 이것이 보수적으로 잡은 수치라는 점입니다. 실제로는 자동완성이나 별칭 같은 것까지 포함하면 훨씬 더 많이 절약됩니다.

[🎯 핵심 포인트]

오늘 다룰 최적화는 크게 세 가지 영역입니다.

첫 번째, **시각적 개선**입니다. 문법 하이라이팅으로 명령어 오류를 즉시 감지하고, Git 상태를 프롬프트에 실시간으로 표시합니다.

두 번째, **기능적 개선**입니다. 자동완성으로 타이핑을 최소화하고, 퍼지 검색으로 파일을 빠르게 찾습니다.

세 번째, **성능 개선**입니다. 기존 Unix 명령어를 현대적인 도구로 교체해서 5배 이상 속도를 올립니다.

그리고 이것은 웹 개발자든, 백엔드 개발자든, DevOps 엔지니어든 상관없이 모든 개발자에게 해당되는 이야기입니다. 터미널은 모두의 공통 도구이기 때문입니다.

자, 그러면 이제 실제로 설치를 시작하겠습니다.

---

## [섹션 2] 시작하기 - Homebrew, iTerm2, Zsh/Oh My Zsh [약 10분]

[📌 섹션 전환]

오늘 강의는 **macOS** 기준으로 진행합니다. 특히 **Apple Silicon**, 즉 M1, M2, M3, M4 칩 기준으로 설명드리는데, Intel Mac을 쓰시는 분도 거의 동일하게 따라하실 수 있습니다. 다른 부분이 있으면 그때그때 말씀드리겠습니다.

### Homebrew 설치

[💻 데모]

자, 가장 먼저 설치할 것은 **Homebrew**입니다. Homebrew는 macOS의 **패키지 관리자**입니다. 리눅스에서 `apt`나 `yum`을 쓰는 것처럼, macOS에서는 Homebrew로 거의 모든 개발 도구를 설치합니다.

화면에서 터미널을 열고 다음 명령어를 입력합니다:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

[⏸️ 잠깐]

여기서 잠깐, Apple Silicon Mac을 쓰시는 분은 설치 후에 **PATH 설정**을 해줘야 합니다. Apple Silicon에서는 Homebrew가 `/opt/homebrew`에 설치되기 때문입니다. Intel Mac은 `/usr/local`에 설치되는데 이것은 자동으로 PATH에 잡혀 있어서 상관없지만, Apple Silicon은 직접 추가해줘야 합니다.

지금 터미널에 다음 명령어를 입력합니다:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

실행하면 PATH 환경 변수에 Homebrew 경로가 추가된 것을 확인할 수 있습니다.

본인 Mac이 어떤 칩인지 모르시겠으면 `uname -m`을 입력하면 됩니다. **arm64**라고 나오면 Apple Silicon이고, **x86_64**라고 나오면 Intel입니다.

설치가 끝나면 화면에서 `brew --version`을 실행해 보겠습니다. 실행하면 버전 번호가 나오고, 이것이 확인되면 설치가 성공한 것입니다.

### iTerm2 설치

[💻 데모]

다음은 **iTerm2** 설치입니다. macOS 기본 Terminal.app도 나쁘지 않은데, iTerm2는 한 차원 다릅니다.

간단하게 비교해드리면, 10,000줄짜리 로그를 출력할 때 기본 Terminal.app은 약 2.3초 걸리는데, iTerm2는 약 1.8초입니다. 약 22% 빠릅니다. 그리고 성능보다 더 중요한 것은, 분할 화면이나 색상 테마, 키 매핑 같은 기능이 훨씬 풍부하다는 점입니다.

화면에서 다음 명령어를 실행해 보겠습니다. 설치는 Homebrew로 한 줄이면 끝납니다:

```bash
brew install --cask iterm2
```

실행하면 iTerm2가 자동으로 다운로드되고 Applications 폴더에 설치되는 것을 확인할 수 있습니다.

설치 후 처음 실행하면 macOS에서 "확인되지 않은 개발자"라는 경고가 나올 수 있습니다. 당황하지 마시고, **시스템 설정 - 개인정보 보호 및 보안**에서 "여전히 열기"를 클릭하시면 됩니다.

실행이 되면 바로 간단한 설정 하나만 해보겠습니다. `Command + 쉼표(,)`를 눌러서 Preferences를 열고, **Appearance - Theme - Minimal**을 선택합니다. 이것만 해도 외관이 훨씬 깔끔해집니다.

### Oh My Zsh 설치

[💻 데모]

자, 이제 가장 중요한 **Oh My Zsh**를 설치하겠습니다.

먼저 배경 설명을 조금 드리면, macOS는 Catalina 버전부터 **Zsh**가 기본 셸입니다. 그래서 별도 설치는 필요 없습니다. `zsh --version`을 입력하면 이미 5.9 이상이 설치되어 있을 것입니다.

**Oh My Zsh**는 이 Zsh를 더 편하게 쓰게 해주는 **프레임워크**입니다. 테마 관리, 플러그인 관리를 아주 쉽게 할 수 있게 해줍니다.

화면에서 설치 명령어를 실행해 보겠습니다:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

실행하면 터미널에 Oh My Zsh 로고가 ASCII 아트로 나타나고, 프롬프트가 바뀌는 것을 확인할 수 있습니다. 그리고 `~/.zshrc` 파일이 자동으로 생성됩니다. 이 파일이 앞으로 계속 편집할 설정 파일입니다.

[🎯 핵심 포인트]

여기까지 따라오셨으면 기본 3종 세트가 갖춰진 것입니다. **Homebrew**(패키지 관리), **iTerm2**(터미널), **Oh My Zsh**(셸 프레임워크). 이 세 가지가 앞으로 모든 설정의 기반이 됩니다.

---

## [섹션 3] 핵심 개념 - 터미널/셸/CLI, Zsh vs Bash [약 5분]

[📌 섹션 전환]

설치를 더 진행하기 전에, 잠깐 개념 정리를 하고 가겠습니다. 이 개념이 헷갈리면 나중에 문제가 생겼을 때 어디를 고쳐야 하는지 모르기 때문입니다.

### 터미널, 셸, CLI의 차이

이 세 가지를 많은 분들이 혼동하시는데, 완전히 다른 레이어입니다.

**터미널**은 화면에 보이는 그 창, 즉 **그래픽 인터페이스**입니다. iTerm2가 바로 터미널입니다. Terminal.app도 터미널입니다.

**셸**은 그 안에서 명령어를 해석하고 실행하는 **프로그램**입니다. Zsh, Bash, Fish 같은 것들이 셸입니다.

**CLI**는 명령줄로 실행하는 **개별 도구**입니다. `git`, `npm`, `docker` 같은 것들입니다.

비유하자면, 터미널은 **자동차의 계기판**, 셸은 **엔진**, CLI 도구들은 엔진 위에서 돌아가는 **각종 부품**이라고 생각하시면 됩니다.

그래서 "터미널이 느리다"고 하면 iTerm2 설정을 봐야 하고, "명령어 자동완성이 안 된다"고 하면 Zsh 설정을 봐야 하고, "git이 안 된다"고 하면 git 자체 설정을 봐야 합니다.

### Zsh vs Bash, 왜 Zsh인가

잠깐 Zsh와 Bash를 비교해보겠습니다. 가장 크게 체감되는 차이는 **자동완성**입니다.

Bash에서 디렉토리 이동하려면 경로를 다 입력해야 합니다. `cd ~/Documents/projects/my-app/src/components`처럼 전체 경로를 입력해야 합니다. 그런데 Zsh에서는 이렇게 할 수 있습니다:

```bash
cd ~/D/p/m/s/c
```

이 상태에서 **Tab**을 누르면 자동으로 전체 경로가 완성됩니다. 각 디렉토리의 **첫 글자만** 입력해도 되는 것입니다. 이것만으로도 Zsh를 쓸 이유가 충분합니다.

그 외에도 Zsh는 **플러그인 생태계**가 거대해서 1,000개 이상의 플러그인이 있고, 히스토리 검색도 공유나 중복 제거 같은 고급 기능이 있습니다.

### Oh My Zsh 플러그인 시스템

[🎯 핵심 포인트]

하나 중요한 점을 짚고 넘어가겠습니다. Oh My Zsh 플러그인에는 **두 종류**가 있습니다.

첫 번째, **기본 포함 플러그인**입니다. `git`, `z`, `npm`, `brew` 같은 것들입니다. 이것들은 이미 Oh My Zsh에 포함되어 있어서, `~/.zshrc`의 `plugins` 배열에 이름만 추가하면 바로 쓸 수 있습니다.

두 번째, **별도 설치 플러그인**입니다. `zsh-autosuggestions`, `zsh-syntax-highlighting` 같은 것들입니다. 이것들은 `git clone`으로 먼저 다운로드한 다음에 plugins에 추가해야 합니다.

이 차이를 모르면 나중에 "왜 안 되지?" 하고 당황하시게 되니까 꼭 기억해두시기 바랍니다.

---

## [섹션 4] 필수 도구 설치 - Powerlevel10k, 폰트, iTerm2 고급 설정 [약 10분]

[📌 섹션 전환]

자, 기본 개념 정리가 끝났으니 이제 본격적으로 터미널을 꾸며보겠습니다. 이 섹션이 끝나면 터미널이 눈에 띄게 달라질 것입니다.

### Powerlevel10k 테마

[💻 데모]

**Powerlevel10k**는 Zsh에서 가장 인기 있는 테마입니다. 단순히 예뻐지는 것이 아니라, 정말 **실용적인 정보**를 프롬프트에 보여줍니다.

어떤 정보를 보여주냐면, 현재 디렉토리, Git 브랜치, 변경/추가/삭제된 파일 수, Python이나 Node.js 버전, 마지막 명령어 실행 시간, 그리고 이전 명령어의 성공/실패 여부까지 표시합니다. 이 모든 것이 프롬프트 한 줄에 표시됩니다.

화면에서 설치 명령어를 실행해 보겠습니다:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

[📝 코드 설명]

이 명령어를 설명드리면, `--depth=1`은 최신 커밋만 가져오라는 뜻입니다. 전체 히스토리를 다운받을 필요가 없으니까 빠르게 설치하기 위한 옵션입니다. 그리고 `${ZSH_CUSTOM}`은 Oh My Zsh 커스텀 디렉토리 경로인데, 보통 `~/.oh-my-zsh/custom`입니다.

다운로드가 끝나면 `~/.zshrc` 파일을 열어서 테마를 변경합니다:

```bash
# 이 줄을 찾아서
ZSH_THEME="robbyrussell"

# 이렇게 변경
ZSH_THEME="powerlevel10k/powerlevel10k"
```

그리고 화면에서 `source ~/.zshrc`를 실행하겠습니다. 실행하면 **설정 마법사**가 자동으로 시작되는 것을 확인할 수 있습니다. 여러 가지 질문을 하는데, "이 문자가 보이나요?", "프롬프트 스타일은 뭘 원하세요?" 같은 것들입니다. 취향대로 선택하시면 됩니다. 저는 보통 **Rainbow** 스타일을 추천드립니다.

나중에 다시 설정하고 싶으시면 `p10k configure` 명령어로 언제든 마법사를 다시 실행할 수 있습니다.

### 개발용 폰트 설치

[⏸️ 잠깐]

여기서 잠깐, 폰트 이야기를 해야 합니다. Powerlevel10k가 표시하는 Git 아이콘이나 화살표 기호 같은 것들은 **일반 폰트로는 안 보일 수 있습니다**. 네모 깨진 문자로 표시될 수 있습니다.

그래서 코딩에 최적화된 폰트를 설치해줘야 합니다. 두 가지를 추천드립니다.

화면에서 다음 명령어를 실행합니다:

```bash
brew install --cask font-jetbrains-mono
brew install --cask font-fira-code
```

실행하면 두 폰트가 순서대로 설치되는 것을 확인할 수 있습니다.

**JetBrains Mono**는 Powerlevel10k에서 공식 권장하는 폰트이고, **Fira Code**는 **리게처(Ligature)** 지원이 뛰어납니다. 리게처란, 코드에서 `!=`, `>=`, `=>` 같은 기호들이 하나의 연결된 문자로 예쁘게 표시되는 기능입니다. 취향 차이인데, 저는 개인적으로 JetBrains Mono를 선호합니다.

폰트를 설치했으면 iTerm2에 적용해줘야 합니다. **Preferences - Profiles - Text - Font**에서 방금 설치한 폰트를 선택하고, 크기는 **14pt** 정도를 추천드립니다.

### iTerm2 고급 설정

[💻 데모]

이제 iTerm2를 좀 더 세밀하게 설정해보겠습니다.

**색상 테마**를 먼저 적용해보겠습니다. 저는 **Snazzy**라는 테마를 좋아하는데, 현대적이면서 눈이 편합니다.

화면에서 다음 명령어를 실행합니다:

```bash
curl -o ~/Downloads/Snazzy.itermcolors \
  https://raw.githubusercontent.com/sindresorhus/iterm2-snazzy/main/Snazzy.itermcolors
```

실행하면 Downloads 폴더에 테마 파일이 다운로드된 것을 확인할 수 있습니다. 다운받고 나서 **Preferences - Profiles - Colors - Color Presets - Import**로 가서 방금 다운받은 파일을 불러오면 됩니다. Dracula, One Dark, Nord 같은 테마도 인기가 많으니 [iterm2colorschemes.com](https://iterm2colorschemes.com)에서 둘러보시기 바랍니다.

[🎯 핵심 포인트]

다음, **키 매핑 설정**입니다. 이것을 안 하면 정말 불편합니다. macOS에서 터미널을 쓸 때 `Command + 왼쪽 화살표`로 줄 시작으로 이동하거나, `Option + 왼쪽 화살표`로 단어 단위로 이동하는 것이 기본적으로 안 되기 때문입니다.

**Preferences - Profiles - Keys - Key Mappings**에서 다음을 추가합니다:

- `Command + 왼쪽 화살표` : Send Escape Sequence `OH` (줄 시작으로 이동)
- `Command + 오른쪽 화살표` : Send Escape Sequence `OF` (줄 끝으로 이동)
- `Option + 왼쪽 화살표` : Send Escape Sequence `b` (단어 단위 왼쪽 이동)
- `Option + 오른쪽 화살표` : Send Escape Sequence `f` (단어 단위 오른쪽 이동)
- `Option + Delete` : Send Hex Code `0x17` (단어 단위 삭제)

이 다섯 개만 설정하면 터미널에서의 텍스트 편집이 **완전히** 달라집니다. 강력 추천합니다.

그리고 마지막으로 **Status Bar** 설정도 해보겠습니다. **Preferences - Profiles - Session**에서 "Status bar enabled"를 체크하고, Configure Status Bar를 클릭합니다. 여기서 CPU, 메모리, 현재 디렉토리, Git 상태, 시계 같은 컴포넌트를 드래그해서 추가할 수 있습니다. 개발할 때 시스템 상태를 한눈에 볼 수 있어서 편리합니다.

---

## [섹션 5] 생산성 플러그인 - autosuggestions, syntax-highlighting, z, fzf [약 10분]

[📌 섹션 전환]

자, 이제 진짜 생산성을 끌어올리는 **플러그인**들을 설치하겠습니다. 여기서부터가 체감이 확 달라지는 부분입니다.

### zsh-autosuggestions

[💻 데모]

첫 번째, **zsh-autosuggestions**입니다. 이것은 이전에 입력했던 명령어를 기억해서, 타이핑을 시작하면 **회색 글씨로 자동 제안**을 해줍니다.

예를 들어 `git sta`까지만 치면, 이전에 `git status`를 입력한 적이 있다면 나머지 `tus`가 회색으로 표시됩니다. 여기서 **오른쪽 화살표**를 누르면 바로 채택됩니다. 명령어 입력 시간이 **82% 단축**된다는 그 통계가 바로 이 플러그인 덕분입니다.

화면에서 설치 명령어를 실행합니다:

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

실행하면 플러그인 소스가 custom/plugins 디렉토리에 클론되는 것을 확인할 수 있습니다.

### zsh-syntax-highlighting

다음, **zsh-syntax-highlighting**입니다. 이것은 명령어를 입력할 때 **실시간으로 색상을 표시**해줍니다.

올바른 명령어는 **초록색**, 잘못된 명령어는 **빨간색**으로 표시됩니다. 예를 들어 `git`이라고 치면 초록색으로 나오고, 오타로 `gti`라고 치면 빨간색으로 나옵니다. Enter를 누르기 **전에** 오류를 알 수 있으니까, "command not found" 에러를 미리 방지할 수 있습니다.

화면에서 다음 명령어를 실행합니다:

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

실행하면 마찬가지로 custom/plugins 디렉토리에 클론되는 것을 확인할 수 있습니다.

[🎯 핵심 포인트]

여기서 중요한 점 하나를 말씀드리겠습니다. **syntax-highlighting은 plugins 배열에서 반드시 마지막에** 넣어야 합니다. 다른 플러그인보다 뒤에 와야 제대로 작동합니다. 이것을 모르면 나중에 "왜 색상이 안 나오지?" 하고 삽질하게 됩니다.

### z 플러그인

세 번째, **z**입니다. 이것은 자주 방문하는 디렉토리를 학습해서, **키워드 몇 글자만으로 점프**할 수 있게 해줍니다.

z는 Oh My Zsh에 기본 포함된 플러그인이라 별도 설치가 필요 없습니다. plugins 배열에 `z`만 추가하면 됩니다.

[⏸️ 잠깐]

여기서 주의하실 점이 있습니다. z는 **macOS에 기본 설치된 명령어가 아닙니다**. Oh My Zsh 플러그인입니다. 그래서 Oh My Zsh 없이 그냥 `z`를 치면 "command not found"가 나옵니다. 이것은 트러블슈팅에서도 다시 다루겠지만, 미리 알아두시면 좋습니다.

그리고 z는 처음부터 작동하는 것이 아니라, **학습 기간**이 필요합니다. `cd`로 디렉토리를 방문할수록 z가 기억을 해서, 나중에 `z proj`만 치면 `~/Documents/projects`로 바로 이동하는 방식입니다. 하루 이틀 쓰다 보면 굉장히 편해집니다.

### fzf (퍼지 파인더)

[💻 데모]

네 번째, **fzf**입니다. 이것은 **퍼지 검색** 도구인데, 설치하면 삶의 질이 올라갑니다.

화면에서 다음 명령어를 실행합니다:

```bash
brew install fzf
$(brew --prefix)/opt/fzf/install
```

실행하면 fzf가 설치되고, 셸 통합 설정이 시작되는 것을 확인할 수 있습니다. 설치할 때 세 가지 질문이 나오는데, **전부 y**를 입력합니다. fuzzy auto-completion 활성화, key bindings 활성화, shell configuration 업데이트, 다 예스입니다.

설치가 끝나면 바로 써볼 수 있습니다. **Ctrl + R**을 눌러보겠습니다. 실행하면 이전에 입력했던 명령어 전체를 **퍼지 검색**으로 찾을 수 있는 인터페이스가 나타납니다. 정확한 명령어가 기억 안 나도 일부 키워드만 입력하면 찾아줍니다. 이것은 한 번 쓰면 없이는 못 쓰게 됩니다.

그리고 `vim **<Tab>`을 치면 현재 디렉토리 아래의 모든 파일을 퍼지 검색할 수 있고, `cd **<Tab>`을 치면 디렉토리를 퍼지 검색할 수 있습니다.

### plugins 배열 최종 모습

자, 이제 `~/.zshrc`의 plugins를 최종 정리해보겠습니다:

```bash
plugins=(
  git
  z
  zsh-autosuggestions
  zsh-syntax-highlighting  # 반드시 마지막!
)
```

이 네 개가 기본 추천 플러그인 세트입니다. 화면에서 `source ~/.zshrc`를 실행하여 적용합니다. 실행하면 플러그인이 모두 로드되어 자동완성과 문법 하이라이팅이 활성화된 것을 확인할 수 있습니다.

---

## [섹션 6] 현대적 CLI 도구 - exa, bat, fd, ripgrep, delta, htop, tldr [약 12분]

[📌 섹션 전환]

자, 이번 섹션이 정말 재미있는 부분입니다. 매일 쓰는 `ls`, `cat`, `find`, `grep` 같은 명령어들은 수십 년 된 도구들입니다. 물론 잘 작동하지만, 현대에 만들어진 대안 도구들이 있고, 이것들은 **속도도 빠르고 출력도 훨씬 보기 좋습니다**.

화면에서 한 번에 다 설치하겠습니다:

```bash
brew install exa bat fd ripgrep git-delta htop tldr
```

실행하면 7개의 도구가 순서대로 설치되는 것을 확인할 수 있습니다.

[💻 데모]

하나씩 살펴보겠습니다.

### exa (ls 대체)

먼저 **exa**입니다. `ls` 명령어의 현대적 대체품입니다. 기본 `ls`는 흰색 텍스트만 쭉 나옵니다. exa는 **파일 타입별 컬러**, **아이콘**, **Git 상태**까지 표시해줍니다.

화면에서 다음 명령어를 실행해 보겠습니다:

```bash
exa -la --git --icons
```

실행하면 파일 목록이 색상과 아이콘과 함께 나오고, 각 파일의 Git 상태(수정됨, 추가됨 등)까지 표시되는 것을 확인할 수 있습니다. 그리고 `exa --tree`로 트리 구조도 볼 수 있습니다.

### bat (cat 대체)

다음, **bat**입니다. `cat` 명령어의 대체품인데, **문법 하이라이팅**이 자동으로 적용됩니다.

화면에서 다음 명령어를 실행합니다:

```bash
bat app.js
```

실행하면 JavaScript 문법에 맞춰서 색상이 입혀져서 나오는 것을 확인할 수 있습니다. 라인 번호도 기본으로 표시됩니다. `cat`으로 코드를 볼 때 흰색 텍스트 벽을 보는 것과는 차원이 다릅니다.

### fd (find 대체)

**fd**는 `find` 명령어의 대체품입니다.

여기서 숫자를 보시면, 같은 검색을 할 때 `find`는 약 1.2초, fd는 약 **0.21초**입니다. **5.7배** 빠릅니다. 물론 Mac 사양이나 SSD 속도에 따라 다를 수 있지만, 체감상으로도 확실히 빠릅니다.

사용법도 훨씬 직관적입니다:

```bash
# find로 하면: find . -name "*.js" -not -path "./node_modules/*"
# fd로 하면:
fd -e js
```

fd는 `.gitignore`를 자동으로 존중해서 `node_modules` 같은 것은 알아서 제외합니다. 이것만으로도 매우 편리합니다.

### ripgrep (grep 대체)

[🎯 핵심 포인트]

**ripgrep**, 명령어는 `rg`입니다. 이것은 제가 **가장 좋아하는 도구** 중 하나입니다.

텍스트 검색 속도를 비교하면, 100MB 코드베이스에서 grep은 약 2.5초, ripgrep은 약 **0.5초**입니다. **5배** 차이입니다.

화면에서 몇 가지 검색 예제를 실행해 보겠습니다:

```bash
# 대소문자 구분 없이 TODO 검색
rg -i "TODO"

# JavaScript 파일에서만 import 검색
rg -t js "import"

# 전후 2줄 컨텍스트와 함께 검색
rg -C 2 "error"
```

실행하면 각 검색 결과가 컬러풀하게 출력되고, 파일명과 라인 번호가 함께 표시되는 것을 확인할 수 있습니다.

ripgrep도 `.gitignore`를 존중하고, 바이너리 파일은 자동으로 건너뛰고, 출력이 컬러풀하게 나옵니다. `grep`을 쓰던 모든 곳에서 `rg`로 대체하시면 됩니다.

### delta (git diff 향상)

**delta**는 `git diff` 출력을 아름답게 만들어줍니다.

설치 후에 Git 설정만 해주면 됩니다. 화면에서 다음 명령어를 실행합니다:

```bash
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global delta.side-by-side true
git config --global delta.line-numbers true
```

실행하면 Git의 전역 설정에 delta가 등록됩니다. 이렇게 설정하면 `git diff`, `git log -p`, `git show` 같은 명령어를 실행했을 때 출력이 **사이드 바이 사이드**로 나오고, 라인 번호도 표시되고, 문법 하이라이팅도 적용됩니다. 코드 리뷰할 때 정말 좋습니다.

### htop과 tldr

빠르게 두 개 더 소개하겠습니다.

**htop**은 `top`의 업그레이드 버전입니다. CPU, 메모리 사용량을 컬러 바 그래프로 보여주고, F5로 트리 뷰, F9로 프로세스 종료가 가능합니다.

**tldr**은 `man` 페이지의 대안입니다. `man tar`를 치면 엄청나게 긴 문서가 나옵니다. `tldr tar`를 치면 핵심 사용 예제만 딱 보여줍니다. 명령어 옵션이 갑자기 기억 안 날 때 정말 유용합니다.

### 별칭으로 기본 명령어 대체

[💻 데모]

자, 이 도구들을 설치했으면 **별칭**을 등록해서 기존 명령어를 대체합니다. `~/.zshrc`에 다음을 추가합니다:

```bash
alias ls='exa'
alias ll='exa -l --git'
alias la='exa -la --git'
alias lt='exa --tree --level=2'
alias cat='bat --paging=never'
alias find='fd'
alias grep='rg'
```

이렇게 하면 평소처럼 `ls`, `cat`, `find`, `grep`을 쓰는데 실제로는 현대적 도구가 실행됩니다. 습관을 바꿀 필요 없이 자연스럽게 성능 향상을 누릴 수 있습니다.

---

## [섹션 7] 실무 예제 3가지 [약 12분]

[📌 섹션 전환]

자, 지금까지 개별 도구들을 설치하고 설정했는데, 이것을 **실무에서 어떻게 조합해서 쓰는지** 세 가지 예제로 보여드리겠습니다.

### 예제 1: 제로에서 시작하는 완벽한 개발 환경 구축

[💻 데모]

첫 번째 예제는 **새 Mac에서 1시간 안에 개발 환경 완성하기**입니다. 실제로 새 Mac을 샀거나, 회사에서 새 장비를 받았을 때 쓸 수 있는 시나리오입니다.

순서는 이렇습니다. 먼저 Homebrew 설치, 그 다음 필수 도구 일괄 설치입니다.

화면에서 다음 명령어를 실행합니다:

```bash
brew install git node python3
brew install --cask iterm2 visual-studio-code
brew install exa bat fd ripgrep fzf git-delta htop tldr
```

실행하면 모든 패키지가 순서대로 설치되는 것을 확인할 수 있습니다.

그 다음 Oh My Zsh, Powerlevel10k, 플러그인 설치하고, `.zshrc`에 별칭과 함수를 정리하고, Git 설정하고, 폰트 설치하면 끝입니다.

이것을 한 번 해두면, 그 다음부터는 `.zshrc` 파일 하나만 복사해오면 대부분의 설정이 복원됩니다. 나중에 베스트 프랙티스 섹션에서 **dotfiles 관리**를 다루겠지만, 이 과정 자체를 자동화할 수도 있습니다.

설정이 끝나면 이런 것들이 가능합니다:

```bash
# 자동완성: git sta → 오른쪽 화살표로 git status 완성
# fzf 검색: vim **<Tab> → 파일 퍼지 검색
# 명령어 색상: 유효한 명령어는 초록, 잘못된 건 빨강
# Git 프롬프트: ~/project main* ⇡1 ⇣2 ✗2 ✚1
```

### 예제 2: Git 워크플로우 최적화

[💻 데모]

두 번째 예제는 **Git 작업 자동화**입니다. 개발자라면 하루에 수십 번 Git 명령어를 사용합니다. 이것을 최적화하면 시간을 매우 많이 절약할 수 있습니다.

먼저 기본 별칭부터 설정합니다:

```bash
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline --graph --decorate'
```

그리고 더 강력한 **함수**도 만들어보겠습니다:

```bash
# 커밋과 푸시를 한 번에
gcp() {
  git add --all
  git commit -m "$1"
  git push
}

# fzf로 브랜치 검색해서 전환
gcof() {
  local branch
  branch=$(git branch --all | fzf | tr -d ' ')
  if [ -n "$branch" ]; then
    git checkout "${branch#remotes/origin/}"
  fi
}
```

이것을 쓰면 어떻게 되는지 설명드리겠습니다. 기존에 `git status`, `git add .`, `git commit -m "..."`, `git push`를 하나하나 치던 것을 **`gcp "Add feature"` 한 줄**로 끝낼 수 있습니다. 브랜치 전환도 `gcof`를 치면 fzf가 뜨면서 브랜치를 검색해서 바로 전환할 수 있습니다.

기존 워크플로우가 약 2분 걸리던 것을 10초로 줄일 수 있습니다. 하루 10번이면 **19분 절약**, 연간 **약 76시간**입니다.

### 예제 3: 프로젝트별 환경 자동 전환

[💻 데모]

세 번째 예제입니다. 여러 프로젝트를 동시에 작업할 때, 각 프로젝트가 다른 Node.js 버전이나 환경 변수를 요구하는 경우가 있습니다. 이것을 **direnv**와 **nvm**으로 자동화할 수 있습니다.

화면에서 다음 명령어를 실행합니다:

```bash
brew install direnv
```

실행하면 direnv가 설치되는 것을 확인할 수 있습니다.

`~/.zshrc`에 `eval "$(direnv hook zsh)"`를 추가하고, 프로젝트 디렉토리에 `.envrc` 파일을 만들어두면 됩니다.

예를 들어, `legacy-app` 프로젝트는 Node 18을 쓰고, `modern-app` 프로젝트는 Node 20을 쓴다고 가정하겠습니다.

```bash
# ~/projects/legacy-app/.envrc
nvm use
export NODE_ENV=development
export API_URL=http://localhost:3000

# ~/projects/modern-app/.envrc
nvm use
export NODE_ENV=production
export API_URL=https://api.example.com
```

이렇게 설정해두면, `cd ~/projects/legacy-app`만 치면 **자동으로** Node 18이 활성화되고 환경 변수가 세팅됩니다. `cd ~/projects/modern-app`으로 이동하면 자동으로 Node 20으로 전환되고 환경 변수도 바뀝니다.

[🎯 핵심 포인트]

기존에는 프로젝트 전환할 때마다 `nvm use 18`, `export NODE_ENV=development`, `export API_URL=...` 이런 것을 하나하나 입력해야 했는데, 이제는 `cd` **한 번이면 끝**입니다. 일일 20회 프로젝트 전환 시 약 **10분 절약**, 연간 **40시간**입니다.

---

## [섹션 8] 베스트 프랙티스 - 별칭 관리, dotfiles, Brewfile, 성능, 보안 [약 8분]

[📌 섹션 전환]

자, 이제 지금까지 만든 환경을 **잘 유지하고 관리하는 방법**을 이야기하겠습니다.

### 별칭 관리 원칙

별칭을 만들 때 원칙이 있습니다. **짧고 직관적**이어야 합니다.

```bash
# 좋은 별칭: 뭔지 바로 알 수 있음
alias ll='exa -l --git'
alias gs='git status'
alias dc='docker-compose'

# 나쁜 별칭: 뭔지 모름
alias x='exa -la --git --icons'
alias f='find'
```

그리고 별칭은 **단순 명령어 대체**에 쓰고, 인자가 필요하거나 로직이 있으면 **함수**를 쓰는 것이 좋습니다. 별칭이 많아지면 파일을 분리하는 것도 좋습니다. `~/.aliases.zsh`, `~/.git-aliases.zsh` 이런 식으로 분리합니다.

### dotfiles 관리

[🎯 핵심 포인트]

이것이 정말 중요한 습관입니다. **dotfiles를 GitHub에서 관리**하는 것을 권장합니다.

`~/dotfiles` 디렉토리를 만들고, `.zshrc`, `.gitconfig`, `.vimrc` 같은 설정 파일들을 여기에 복사합니다. 그리고 원래 위치에 **심볼릭 링크**를 만듭니다:

```bash
ln -sf ~/dotfiles/zshrc ~/.zshrc
ln -sf ~/dotfiles/gitconfig ~/.gitconfig
```

이것을 GitHub에 올려두면, 새 Mac을 사든 회사에서 새 장비를 받든, `git clone`하고 링크만 연결하면 내 환경이 그대로 복원됩니다.

### Brewfile로 패키지 관리

Homebrew에도 비슷한 기능이 있습니다. **Brewfile**이라는 것인데, 현재 설치된 모든 패키지를 파일로 저장할 수 있습니다.

화면에서 다음 명령어를 실행합니다:

```bash
brew bundle dump --force --file=~/dotfiles/Brewfile
```

실행하면 현재 설치된 모든 brew 패키지, cask 앱, 폰트가 Brewfile에 기록되는 것을 확인할 수 있습니다. 새 Mac에서 복원할 때는:

```bash
brew bundle install --file=~/dotfiles/Brewfile
```

이 한 줄이면 **모든 패키지가 자동 설치**됩니다.

### 성능 최적화

[⏸️ 잠깐]

여기서 잠깐, Oh My Zsh를 설정하고 나서 터미널이 느려지는 경우가 있습니다. 이것은 흔한 문제입니다.

먼저 시작 시간을 측정해보겠습니다. 화면에서 다음 명령어를 실행합니다:

```bash
time zsh -i -c exit
```

실행하면 Zsh 시작에 걸리는 시간이 출력됩니다. **0.5초 이하**면 양호하고, **1초 이상**이면 최적화가 필요합니다.

최적화 팁 세 가지를 말씀드리겠습니다.

첫째, **플러그인을 10개 이하로 유지**합니다. 많을수록 느려집니다.

둘째, **nvm 같은 느린 도구는 lazy loading** 합니다:

```bash
alias nvm='unalias nvm && source "$NVM_DIR/nvm.sh" && nvm'
```

이렇게 하면 nvm을 실제로 쓸 때만 로드됩니다.

셋째, Powerlevel10k의 **Instant Prompt** 기능이 자동으로 활성화되어 있는지 확인합니다. 이것은 Zsh가 완전히 로드되기 전에 프롬프트를 먼저 보여주는 기능입니다.

### 보안 고려사항

마지막으로 보안 이야기를 잠깐 하겠습니다.

`.envrc` 파일에 API 키, 시크릿 키 같은 민감한 정보를 넣었다면 **절대 Git에 커밋하면 안 됩니다**. `.gitignore`에 `.envrc`를 추가하고, 대신 `.envrc.template`이라는 템플릿 파일을 만들어서 이것을 커밋합니다. 팀원들은 템플릿을 복사해서 자기 값을 넣으면 됩니다.

---

## [섹션 9] 트러블슈팅 - 자주 발생하는 오류들 [약 5분]

[📌 섹션 전환]

자, 거의 다 왔습니다. 이 섹션에서는 설정하면서 **높은 확률로 만날 오류들**과 해결법을 정리해드리겠습니다. 미리 알아두시면 삽질 시간을 크게 줄일 수 있습니다.

### z 명령어 "command not found"

[🎯 핵심 포인트]

가장 자주 발생하는 오류입니다. `z` 명령어를 치면 "command not found: z"라고 나오는 경우입니다.

원인은 간단합니다. **z는 macOS에 기본 설치된 명령어가 아닙니다.** Oh My Zsh 플러그인입니다.

해결법은 `~/.zshrc`의 `plugins` 배열에 `z`를 추가하고, `source ~/.zshrc`를 실행하는 것입니다. 그리고 z는 디렉토리를 학습해야 하니까, 먼저 `cd`로 몇 군데 이동한 다음 사용해야 합니다.

### Oh My Zsh 설치 후 터미널이 느림

원인은 대부분 **플러그인이 너무 많은 경우**입니다.

```bash
# 최소한의 추천 플러그인 세트
plugins=(git zsh-autosuggestions zsh-syntax-highlighting z)
```

이 네 개만 쓰는 것을 권장합니다. 불필요한 플러그인은 과감하게 제거하시면 됩니다. 어떤 플러그인이 느린지 찾으려면 `~/.zshrc` 맨 위에 `zmodload zsh/zprof`를 추가하고 맨 아래에 `zprof`를 추가해서 프로파일링 하면 됩니다.

### Powerlevel10k 문자 깨짐

화살표나 Git 아이콘이 **네모**로 보이는 경우입니다. 폰트 문제입니다.

`p10k configure`를 실행하면 폰트 설치 옵션이 나옵니다. 또는 직접 JetBrains Mono나 MesloLGS NF 폰트를 설치하고, iTerm2 설정에서 해당 폰트를 선택합니다.

### zsh-syntax-highlighting이 작동 안 함

명령어에 색상이 안 입혀지는 경우입니다. 원인은 십중팔구 **플러그인 순서**입니다.

```bash
plugins=(
  git
  z
  zsh-autosuggestions
  zsh-syntax-highlighting  # 반드시 마지막!
)
```

syntax-highlighting은 **반드시 마지막**에 와야 합니다.

### fzf Ctrl+R이 안 됨

fzf를 brew로 설치했는데 `Ctrl+R`이 안 되는 경우입니다. **셸 통합이 안 된 것**입니다.

화면에서 다음 명령어를 실행합니다:

```bash
$(brew --prefix)/opt/fzf/install
```

실행하면 셸 통합 설정이 다시 진행되는 것을 확인할 수 있습니다. 모든 질문에 `y`를 입력합니다. 그리고 `~/.zshrc`에 `[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh` 라인이 있는지 확인합니다.

### PATH 문제

도구를 설치했는데 "command not found"가 나오면, PATH를 확인합니다.

화면에서 다음 명령어를 실행합니다:

```bash
echo $PATH
```

실행하면 현재 PATH에 등록된 경로 목록이 출력됩니다. Apple Silicon Mac이라면 `/opt/homebrew/bin`이 PATH에 포함되어 있어야 합니다. 없으면 `~/.zshrc`에 추가합니다:

```bash
export PATH="/opt/homebrew/bin:$PATH"
```

---

## [클로징] 핵심 요약과 다음 단계 [약 3분]

[📌 섹션 전환]

자, 오늘 정말 많은 내용을 다뤘습니다. 빠르게 정리하겠습니다.

**오늘 설치한 것들:**

1. **iTerm2** - macOS 기본 터미널보다 빠르고 기능이 풍부한 터미널
2. **Oh My Zsh + Powerlevel10k** - 셸 프레임워크와 정보가 풍부한 프롬프트
3. **핵심 플러그인 4개** - autosuggestions, syntax-highlighting, z, fzf
4. **현대적 CLI 도구 7개** - exa, bat, fd, ripgrep, delta, htop, tldr

**얻은 생산성 향상:**

- 파일 검색 **5.75배** 빨라짐
- 텍스트 검색 **5배** 빨라짐
- 명령어 입력 **82%** 단축
- 연간 약 **44시간** 절약

[🎯 핵심 포인트]

이것을 한 번에 다 익히려고 하지 않는 것을 추천드립니다. **점진적으로** 가시는 것이 좋습니다.

**1주차**에는 iTerm2, Oh My Zsh, Powerlevel10k를 쓰면서 기본 별칭과 자동완성에 익숙해집니다.

**2주차**에는 exa, bat, fd, ripgrep으로 기존 명령어를 대체하고, fzf로 파일과 히스토리를 검색해봅니다.

**3주차**에는 Git 워크플로우를 별칭과 함수로 자동화하고, direnv로 프로젝트별 환경 설정을 해봅니다.

**4주차**에는 dotfiles 저장소를 GitHub에 만들고, Brewfile로 패키지를 관리하고, 백업 시스템을 구축합니다.

마지막으로 한 말씀 드리겠습니다. 터미널 최적화는 "멋있어 보이기" 위한 것이 아닙니다. 매일 수십 번, 수백 번 사용하는 도구를 1% 개선하면, 그 효과는 시간이 지남에 따라 **엄청나게 축적**됩니다. 연간 44시간은 새로운 기술을 배우고, 사이드 프로젝트를 하고, 또는 그냥 쉴 수 있는 **소중한 시간**입니다.

오늘 강의가 개발 생산성을 높이는 데 도움이 되셨으면 좋겠습니다. 강의에서 다룬 모든 명령어와 설정은 아래 설명란에 정리해두겠습니다.

궁금한 점이 있으시면 댓글로 남겨주시고, 도움이 되셨다면 좋아요와 구독 부탁드립니다.

감사합니다. 행복한 코딩 되시기 바랍니다.
