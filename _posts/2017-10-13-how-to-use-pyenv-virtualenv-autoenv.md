---
layout: post
title: '[Python] Python 개발환경 구축하기'
subtitle: 'pyenv, pyenv-virtualenv, autoenv를 이용한 Python 개발환경 구축'
date: 2017-10-13
author: heejeong Kwon
cover: '/images/pyenv-virtualenv-autoenv/pyenv-main.png'
tags: python 개발환경 pyenv virtualenv autoenv
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---


## Goal
> - pyenv, virtualenv, autoenv의 개념 이해하기
> - pyenv, virtualenv, autoenv 설치 및 사용
> - pip freeze를 이용하여 새로운 개발환경에서도 쉽게 같은 환경으로 동기화하기


## pyenv, virtualenv, autoenv를 사용해야 하는 이유
1. Various versions of Python
  - 각 Python Version 마다 문법과 기능이 다르다.
  - 프로젝트 마다 사용하는 Python Version이 다르다.
  - 프로젝트 별로 알맞은 Python Version을 깔고 설정해야 되는 번거로움이 있다.
  - ***로컬에 다양한 Python 버전을 설치하고 사용할 수 있도록 하여 Python 버전에 대한 의존성을 해결할 수 있다.***
2. Python package's Dependency
  - 프로젝트 마다 사용하는 환경이 다르다.
  - ***각 프로젝트에 맞게 환경을 설정할 수 있는 가상환경을 생성하여 사용하면 Python Version과 Python Package들에 대한 의존성을 해결할 수 있다.***
  - 가상환경에 설치한 package들은 requirements.txt라는 파일로 추출할 수 있다. 이 파일을 이용하여 다른 사람과의 협업에서 혹은 새로운 개발환경에서도 쉽게 같은 환경으로 동기화할 수 있다. (아래의 TIP 내용 참고)

## pyenv, virtualenv, autoenv 란?
- pyenv
  - "Simple Python Version Management"
  - 로컬에 다양한 Python 버전을 설치하고 사용할 수 있도록 한다. 
  - **Python 버전에 대한 의존성을 해결** 할 수 있다.    
- virtualenv
  - "Virtual Python Environment builder"
  - 로컬에 다양한 Python 환경을 구축하고 사용할 수 있도록 한다.
  - 시스템에 설치된 Python에 영향을 주지 않고 Python의 가상 환경을 유지한다.
  - 일반적으로 Python Packages라고 부르는 pip install을 통해서 설치하는 **Package들에 대한 의존성을 해결** 할 수 있다.
  - 프로젝트마다 독립된 개발환경을 제공하기 때문에 프로젝트들의 환경변수들이 꼬이는 일이 발생하지 않는다.  
- autoenv
  - 프로젝트 별로 가상환경으로의 접속을 자동화한다. 즉 특정 프로젝트 **디렉터리로 들어가면 자동으로 개발환경이 설정** 된다.
  - 터미널에서 디렉터리에 접근할 시에 .env파일을 찾아서 그 내용을 자동으로 실행시켜준다.
  - pyenv-virtualenv 사용 시 activate하는 것을 하지 않고도 해당 디렉터리에 들어가면 자동으로 가상환경이 실행된다.

## pyenv, virtualenv, autoenv 설치하기
~~~javascript
$ brew install pyenv
$ brew install pyenv-virtualenv
$ brew install virtualenv (독립적인 사용 시)
$ brew install autoenv
~~~

## pyenv, virtualenv, autoenv 환경설정
1. ~/.bash_profile(bash 사용자) 혹은 ~/.zshrc(zsh 사용자)에 아래의 내용을 추가한다.
  - ~~~javascript
  $ vi ~/.bash_profile
  혹은
  $ vi ~/.zshrc
  ~~~  
  - ~~~javascript
  # pyenv setting
  $ export PATH=$HOME/bin:/usr/local/bin:$PATHs
  $ eval "$(pyenv init -)"
  # pyenv-virtualenv setting
  $ eval "$(pyenv virtualenv-init -)"
  # autoenv setting: 터미널에서 새로운 세션이 실행될 때 마다 activate.sh 파일을 실행
  $ source /usr/local/opt/autoenv/activate.sh
  ~~~

2. 아래의 내용을 실행하여 환경설정을 적용한다.
  - ~~~javascript
  $ source ~/.bash_profile
  혹은
  $ source ~/.zshrc
  ~~~



## pyenv, virtualenv, autoenv 사용하기
### pyenv 사용하기
~~~javascript
# 설치 가능한 Python 버전 확인
$ pyenv install --list
$ pyenv install -l


# 새로운 Python 버전 설치하기
$ pyenv install 3.5.2


# 이미 설치된 Python 버전 삭제하기
$ pyenv uninstall 3.5.2


# 설치된 Python 버전 확인하기
$ pyenv versions


# 설치된 다른 Python 버전 사용하기
$ pyenv shell 3.5.2


# 현재 사용 중인 Python 버전 확인하기
$ python —version
$ python -V
~~~

### pyenv-virtualenv 사용하기
위에서 pyenv를 이용하여 Python의 버전을 설치해 보았다. 이제 설치한 Python 버전 위에 작동하는 가상환경을 설치해보자.
단독으로 virtualenv를 사용하여 가상환경을 설치할 수도 있지만 여기서는 pyenv-virtualenv를 사용하여 pyenv로 virtualenv까지 관리한다.

~~~javascript
# 설치한 Python 버전을 사용할 가상환경(virtualenv) 생성하기
$ pyenv virtualenv 3.5.2 testVirenv
=> pyenv virtualenv [VERSION_NAME] [VIRTUALENV_NAME] 형식으로 입력한다.


# 설치된 Python과 가상환경(virtualenv) 확인하기
$ pyenv versions
* system (set by PYENV_VERSION environment variable)
  3.5.2
  3.5.2/env/testVirenv
=> Python 3.5.2 버전을 사용하는 가상환경 testVirenv가 만들어진 것을 확인할 수 있다.


# 가상환경(virtualenv) 목록 확인하기
$ pyenv virtualenvs


# 가상환경(virtualenv) 활성화하기
$ pyenv shell testVirenv
혹은
$ pyenv activate testVirenv
=> 두 명령어의 차이점은 아래의 TIP 내용 참고
=> 가상환경 사용 시는 아래의 명령어를 사용하는 것을 추천한다.


# 가상환경(virtualenv) 비활성화하기
$ source deactivate
혹은
$ pyenv deactivate


# 가상환경(virtualenv) 삭제하기
$ pyenv uninstall testVirenv
~~~


---
<mark>TIP</mark>  
<span style="color:#4d0000">*가상환경(virtualenv) 활성화 시 shell과 activate의 차이점*</span>  
=> <span style="background-color: #e1e1e1">shell</span>명령어는 Python 버전을 사용할 때나 가상환경(virtualenv)에 모두 적용할 수 있는 명령어이다.
- 기본 버전으로 돌아오기 위해서는 pyenv shell system  

=> <span style="background-color: #e1e1e1">activate</span>명령어는 가상환경(virtualenv)에만 적용할 수 있는 명령어이다.
- 가상환경에서 나오기 위해서는 pyenv deactivate
- Python 버전을 직접 사용하는 것보다 가상환경을 만들어 작업하는 것이 좋다.  

=> 가상환경을 활성화하면 완전히 독립된 개발환경이므로 새롭게 pip install django나 pip install flask 등과 같이 Python Package를 독립적으로 설치할 수 있다.

---

### 독립적인 virtualenv 사용하기
~~~javascript
# 가상환경(virtualenv) 프로젝트 생성하기
$ virtualenv myVirtualenv
=> myVirtualenv 폴더가 생긴다.
=> 설치파일 및 라이브러리는 myVirtualenv 디렉터리 안의 ./bin, ./lib에 설치된다.


# 가상환경(virtualenv) 활성화하기
$ source myVirtualenv/bin/activate
혹은
$ . myVirtualenv/bin/activate


# 가상환경(virtualenv)의 Python 버전 확인하기
$ python -V


# 가상환경(virtualenv)의 Python 버전 변경하기
# [방법1]
$ cd /usr/local/bin/
=> 자신이 시스템 레벨에서 설치한 Python Version의 종류를 확인한다.
$ virtualenv myVirtualenv --python=/usr/local/bin/python3.6
=> virtualenv [VIRTUALENV_NAME] --python=/usr/local/bin/[VERSION_NAME] 형식으로 입력한다.

# [방법2]
$ cd /Users/heejeong/.pyenv/shims
=> /Users/[SYSTEM_USER_NAME]/.pyenv/shims
=> 자신이 pyenv를 통해 설치한 Python Version의 종류를 확인한다.
$ virtualenv -p python2.7 myVirtualenv
=> virtualenv -p [VERSION_NAME] [VIRTUALENV_NAME] 형식으로 입력한다.


# 가상환경(virtualenv) 비활성화하기
$ deactivate


# 가상환경(virtualenv) 디렉터리 삭제하기
$ rm -rf ~/dev/myVirtualenv
=> rm -rf [PATH]/[VIRTUALENV_NAME] 형식으로 입력한다.
~~~

---
<mark>TIP</mark>  
<span style="color:#4d0000">*독립적인 virtualenv 사용 보다 pyenv-virtualenv를 사용하는 것이 더 편하다.*</span>

---


### autoenv 사용하기
터미널에서 디렉터리에 접근할 때 디렉터리 안에 있는 .env 파일을 찾아서 그 내용을 자동으로 실행시켜준다.  
test 디렉터리를 만들어서, 디렉터리에 들어올 때 위에서 만든 testVirenv 가상환경을 사용하도록 설정해보자.

~~~javascript
$ mkdir test
$ cd test
$ vi .env
~~~

~~~javascript
# .env파일 안에 다음과 같은 내용을 입력하자.
echo “+++++++++++++++++++++++++++++++++++++++++++++++”  
echo “Python Virtual Env > Django with Python 3.5.2”  
echo “+++++++++++++++++++++++++++++++++++++++++++++++”
pyenv activate testVirenv
~~~

제일 먼저 디렉터리에 들어가게 되면, .env의 내용을 실행할 것인지 확인한다. 내용 실행을 위해서 ‘y’를 입력한다.
터미널에서 test 디렉터리에 들어갈 때마다 자동으로 testVirenv 가상 환경을 사용할 수 있다.

---
<mark>TIP</mark>  
<span style="color:#4d0000">*git 사용 시에는 .env 파일은 .gitignore에 추가하는 것이 좋다.*</span>  

---


## pip freeze 사용하기
가상환경에 설치한 package들은 requirements.txt라는 파일로 추출할 수 있다. 이 파일을 이용하여 **다른 사람과의 협업에서 혹은 새로운 개발환경에서도 쉽게 같은 환경으로 동기화할 수 있다.**

1. freeze 명령어로 package들 목록을 추출해보자.  
- 프로젝트 폴더[name: pythonProject]에 Python 3.6.2 버전을 사용하는 가상환경[name: pythonProject-3.6.2]를 생성하고 활성화하자.
  - ~~~javascript
  $ mkdir pythonProject
  $ pyenv virtualenv 3.6.2 pythonProject-3.6.2
  $ pyenv activate pythonProject-3.6.2
  ~~~
- 이 가상환경에 Python Web Framework인 django package를 설치 할 수 있다.
  - ~~~javascript
  $ pip install django
  ~~~
- 아래의 명령어를 이용하여 현재 가상환경에 설치된 Python Package들의 목록을 저장할 수 있다.
  - ~~~javascript
  $ pip freeze > requirements.txt
  ~~~
  - requirements.txt 에는 Django==1.10.1 를 포함하여 현재 설치된 package들의 목록이 저장된다.
  - 다른 가상환경으로 이동하거나 다른 버전의 Python을 사용하면 이 Package들은 사용할 수 없다.  

2. requirements.txt라는 파일을 이용하여 개발환경을 동일하게 사용해보자.
- 이 package 들을 다른 사람과의 협업에서 혹은 새로운 개발환경에서 동일하게 사용하기 위해서는
  - 마찬가지로 새로운 가상환경을 만들고 활성화 시킨 후에 아래의 명령어를 입력하면 requirements.txt 에 저장되어 있는 package 목록들을 모두 설치할 수 있다.
  - ~~~javascript
  $ pip install -r requirements.txt
  ~~~
  - (*requirements.txt라는 이름은 관습적인 사용*)


## References
> - [https://cjh5414.github.io/python-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95/](https://cjh5414.github.io/python-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95/)
> - [http://kwonnam.pe.kr/wiki/python/pyenv](http://kwonnam.pe.kr/wiki/python/pyenv)
> - [https://ansuchan.com/how-to-set-python-dev-env/](https://ansuchan.com/how-to-set-python-dev-env/)
> - [https://skyoo2003.github.io/post/2017/04/02/manage-python-using-pyenv](https://skyoo2003.github.io/post/2017/04/02/manage-python-using-pyenv)
> - [http://dgkim5360.tistory.com/entry/python-virtualenv-on-linux-ubuntu-and-windows](http://dgkim5360.tistory.com/entry/python-virtualenv-on-linux-ubuntu-and-windows)
> - [http://guswnsxodlf.github.io/pyenv-virtualenv-autoenv](http://guswnsxodlf.github.io/pyenv-virtualenv-autoenv)
