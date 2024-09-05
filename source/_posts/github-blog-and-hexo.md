---
title: GitHub Page와 Hexo 로 기술 블로그 만들기
date: 2023-10-15 03:55:43
tags:
---

# 블로그 시작하기

구글링을 하다보면 종종 깃헙 블로그를 보게되는데 뭔가 만들기 어렵고 복잡할 것 같고 이상하게 개발자스러운 느낌이 들었어요. 나도 만들어보고 싶다는 생각’만’ 했었는데  ‘주1회 개발 블로그 작성하기!’ 다짐을 실행하기 위해 깃헙 블로그를 개설합니다.

# 깃헙 블로그를 사용하려는 이유

- 도전 정신 - 편의성이 정말 전혀 없어보이지만 그래서 오히려 한번 써보고 싶었어요. 티스토리나 벨로그같은 블로그 시스템은 게시물 등록이라는 행위로 블로그 작성이 끝이나지만, 깃헙 블로그는 하나부터 열까지 다 코드로 작성해야 한다는 얘기를 들어서 경험해보고 싶었어요.
- 깃헙 잔디 관리 - 기존에는 비쥬얼 스튜디오 코드를 이용해서 md 파일로 TIL를 작성하고 있었어요. 그러다가 팀원들과 공유해야 할 필요성 + 간편함 + 깔끔함을 느끼고 노션으로 갈아탔어요. 이왕 작성하는 TIL와 정보 공유글, 깃헙 블로그로 작성한다면 깃헙에 잔디도 심을 수 있으니 일석이조 아닌가! 라는 생각이 들었어요.
- 심미성 - 깃헙 + 프레임워크를 사용하면 다양한 테마를 고를 수 있다고 하여 찾아봤더니 마음에 드는 테마를 발견했습니다! 티스토리보다 훨씬 더 다양한 테마가 있어서 블로그를 꾸며보고 싶다는 생각이 들었어요. 꾸준히 작성해보려고 마음 먹은 거 이왕이면 디자인까지 내 마음에 드는 게 있으면 좋잖아요.

# 블로그 프레임워크

정적 사이트를 만들 수 있는 블로그 프레임워크를 찾아보았고 Jekyll 와 Hexo 중에 어떤 걸 사용할지 고민하다가 Hexo 로 결정을 했어요. 두가지 프레임워크는 다음과 같은 특징을 가지고 있습니다.

## Jekyll

- 루비 기반
- 사용자가 가장 많음
- 테마가 다양함

## Hexo

- 자바스크립트(Node js) 기반
- 문서화가 잘 되어 있음
- 테마가 다양함

깃헙 블로그로 구글링을 하면 깃헙 블로그 + Jekyll 조합이 많은데 저는 자바스크립트 기반의 Hexo 로 선택을 했어요. 루비에 대해서 전혀 모르기도 하고 회사에서 Node js 를 배울 기회가 있을 것 같아서 블로그 작성하면서 미리 찍먹해보려고요. 

Hexo 에서 제공해주는 테마를 구경하다가 마음에 드는 테마를 발견했어요. 테마 이름은 Matery 입니다. Matery 테마를 어떻게 내 블로그에 적용하고 배포하는지 알아보도록 합시다.

# Hexo 로 깃헙에 배포까지!

## Hexo 가 뭔가요?

[Documentation](https://hexo.io/docs/)

- Hexo는 빠르고 간단하고 강력한 블로그 프레임워크예요. 마크다운이나 다른 마크업 언어로 게시글을 작성하면 Hexo 는 몇초만에 정적파일을 생성해줍니다.

## Hexo 설치하기

### Hexo를 설치하기 이전에 필요한 요구사항

- Hexo를 설치하는 방법은 아주 쉽고 간단합니다. 아래와 같은 2가지 사항만 준비하면 돼요.

```
﹒Node.js (최소한 Node.js 10.13 버전이 필요하고, 12.0 버전 이상을 추천합니다.)
﹒Git
```

- Node.js 와 Git 이 이미 설치가 되어 있다면 바로 Hexo 설치하기 단계로 가시면 됩니다.  (Git 설치는 생략하도록 하겠습니다.)

### Node.js 설치하기

- Homebrew를 이용해서 Node.js를 설치합니다. node 설치가 성공적으로 끝났다면 아래의 명령어를 이용해서 node 버전을 확인해봅니다.

```bash
$ brew install node
$ npm -version
```

### Hexo 설치하기

- 모든 요구사항을 설치했다면 이제 npm 으로 Hexo를 설치할 수가 있습니다.

```bash
$ npm install -g hexo-cli
```

### Hexo 초기화

Hexo 가 설치 됐다면, 다음 명령어를 실행해서 <folder>에서 Hexo를 초기화해봅시다!

```bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

- 저는 디렉토리명을 ‘blog’로 해서 생성했어요. blog 디렉토리로 이동을 해서 리스트를 살펴보겠습니다.
    
    ```bash
    $ ~/workspace ▶️ hexo init blog
    $ ~/workspace ▶️ cd blog
    $ ~/workspace/blog ▶️ ll
    $ ~/workspace/blog ▶️ npm install
    ```
    
    Hexo를 초기화 하고 나면, 프로젝트 폴더는 아래와 같은 구조로 되어있을 거예요. 제 blog 디렉토리 구조를 보시죠.
    
    ```bash
    _config.landscape.yml
    _config.yml
    node_modules
    package-lock.json
    package.json
    scaffolds
    source
    themes
    ```
    

### _config.yml 수정

_config.yml 은 사이트 환경설정 파일이에요. 우리는 여기에서 블로그에 대한 대부분의 세팅을 설정 할 수가 있어요. 몇가지 블로그 세팅을 한번 해볼게요!

- 환경설정 문서: https://hexo.io/docs/configuration
    
    참고로 문서를 편집하기 위한 명령어는 아래와 같아요. vim 을 이용해서 yml 파일을 열어볼게요.
    
    ```bash
    $ blog ▶️ vim _config.yml
    ```
    

### Site

내 블로그의 타이틀과 서브타이틀, 설명, 작성자를 수정해봅시다.

```bash
# Site
title: 지식 보관소
subtitle: '주요활동: 자료 수집 및 정돈'
description: '필요할 때 꺼내쓰기 위해 잘 정리된 자료를 보관하는 곳'
keywords:
author: myname
language: en
timezone: ''
```

### URL

URL 도 수정해보겠습니다. url 에는 내 깃헙 블로그 주소를 입력해주면 돼요.

```bash
# URL
url: https://my_github_username.github.io/
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```

- 먼저 url 부분을 보면 내 깃헙 블로그 주소를 적어야 하는데 우선 [username.github.io](http://username.github.io) 라는 이름으로 깃헙 레포지토리를 생성해야 해요. (usrname 은 GitHub 계정의 username을 의미합니다.) 레포지토리 생성 후 해당 레포지토리의 주소를 url 부분에 적어주세요.

## Deployment

### ****One-command deployment****

해당 가이드는 https://hexo.io/docs/one-command-deployment 에서 가져온 내용이에요. 

Hexo는 빠르고 쉬운 배포 전략을 제공하고 있는데요, 내 블로그를 서버에 배포하려면 하나의 명령어만 입력하면 끝입니다.

```bash
$ ~/workspace/blog ▶️ hexo deploy
```

- _config.yml 파일에 deploy 부분을 찾아서 실행시켜주는 명령어예요. 그러려면 deploy 부분이 먼저 정의되어 있어야겠죠.

_config.yml 파일을 통해서 배포 설정을 할 수가 있습니다. 배포 구성요소에는 type 필드가 반드시 있어야 해요.

여러개의 배포 플러그인을 사용할 수도 있는데요 아래와 같이 type을 작성해주면 Hexo는 작성된 순서대로 플러그인을 실행시켜 줄 거예요.

```bash
deploy:
- type: git
  repo:
- type: heroku
  repo:
```

### Git

Git 배포 플러그인을 사용하는 방법을 소개하겠습니다. 

1. hexo-deployer-git 을 설치해주세요.

```bash
$ ~/workspace/blog ▶️ npm install hexo-deployer-git --save
```

1. _config.yml 파일을 아래와 같이 수정해 봅시다.

```bash
deploy:
  type: git
  repo: <repository url> # https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
  branch: [branch]
  message: [message]
```

예시를 살펴볼까요? Hexo 에서 배포할 때 어떤 레포지토리와 브랜치에 배포를 할 것인지를 설정해주었어요. 

$ hexo deploy 명령어를 실행하면 해당 레포지토리의 브랜치로 푸쉬가 됩니다.

```bash
deploy:
  type: git
  repo: https://github.com/username/username.github.io
  branch: main
```

1. deploy 하기 위한 명령어를 입력해줍니다

```bash
$  ~/workspace/blog ▶️ hexo clean
$  ~/workspace/blog ▶️ hexo deploy
```

1. 설정한 깃헙 레포지토리의 ‘Settings’ → ‘Pages’ 탭을 확인해봅니다. Branch 부분이 gh-pages로 되어있는지 확인하고, 그렇지 않다면 변경해줍니다.
2. 블로그가 잘 만들어졌는지 웹페이지를 확인해봐야겠죠? [username.github.io](http://username.github.io) 로 접속해 확인해봅시다!

# 마치며

- 새로운 게시글을 등록하려면 매번 hexo clean → hexo deploy 명령어를 입력해야 합니다. GitHub Actions 를 사용해 배포 과정을 자동화할 수 있는 방법이 [공식문서](https://hexo.io/docs/github-pages)에 소개되어 있는데, 따라 만들어보면 좋을 것 같아요.
- main 브랜치와 gh-pages 브랜치를 나눠서 관리하는 방법으로 만들어보려다 처참히 실패하였습니다. gh-pages 브랜치에서 게시글 이력 관리를 할 수 있게끔 하고 main 브랜치에 수정 부분을 반영해서 관리하는 것 같은데, 이 부분은 추후에 다시 해보고 어떻게 고쳤는지 작성해보겠습니다.
- 튜토리얼을 보고 블로그를 만드는 데까지는 시간이 얼마 안걸렸지만 블로그 글로 정리 해보려고 하니 헷갈리는 부분이 많아 구글링 해보고 프로젝트 날렸다가 다시 만들어보고 하다보니 시간이….상당히 많이 걸리네요 ㅠㅠ….