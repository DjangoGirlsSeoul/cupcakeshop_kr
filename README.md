# Django Cupcake shop 파이콘장고튜토리얼

![](cupcake.png)

####### cc. [Designed by Freepik](http://www.freepik.com)

> 이 튜토리얼은 Creative Commons Attribution-ShareAlike 4.0 International 저작권을 따르고 있습니다. 라이센스 전문은 http://creativecommons.org/licenses/by-sa/4.0 에서 확인하세요.

## 번역
이 튜토리얼은 열정적인 장고걸스서울 코치와 자원봉사자들의 수고로 번역되었습니다.

> 번역 : [이수진](http://sujinlee.me/), [이소은](https://github.com/mojosoeun)

## 도움주신 분들

장고걸스서울의 운영진과 코치들이 함께 튜토리얼을 준비했습니다

하산 아비드, 박진우, 이소은, 이수진, 박조은, 김민선 , 송다운, 최장호, 장지호 

## 튜토리얼에서 무엇을 배우게 되나요? 
This tutorial will walk you through all the steps in creating a cupcake menu site. By the end, you'll be confident enough to make your website!

## 소개
장고가 처음이신 분들이라면, [공식 장고 튜토리얼 문서](https://docs.djangoproject.com/en/1.9/intro/tutorial01/)를 보는 것이 가장 좋습니다. 장고 튜토리얼 문서는 웹 프로그래밍 경험이 있는 사람들을 위해 쓰여진 가이드입니다. 입문자를 대상으로 모든 설치 방법과 과정이 설명된 쉬운 튜토리얼이 있을까요? 네, 모두가 알고 계시는 바로 [장고걸스 튜토리얼](http://tutorial.djangogirls.org/)입니다. 아마 이 글을 읽으시는 많은 분들이 집에서 장고걸스 튜토리얼을 해보셨으라 생각되는데요, 하지만 튜토리얼을 끝낸 이후 어디서부터 프로젝트를 시작해야할지 막막 하시리라 생각됩니다. 이런 분들을 위해 이번 PYCON APAC동안 함께 컵케이크 가게 메뉴 사이트를 만드는 모든 과정을 실습해보며 고민을 하나씩 풀어보고자 합니다. 튜토리얼을 함께 실습하시면서 나도 웹사이트를 만들 수 있다는 자신감을 가지게 되실 거에요! 그럼 컵케이크와 하이파이브 해봅시다!

## 설명

### 들어가며
장고(쟁고: Django)는 전 세계에서 가장 인기 있는 언어인 파이썬으로 작성된 웹 프레임워크로 다양하고 복잡한 기능을 지원하고 있습니다. 여러분은 장고로 어플리케이션과 사이트 개발을 할 수 있어요. 장고튜토리얼을 한번 마친 입문자들의 경우, 그 다음 무엇을 만들어봐야할지 막막하기만 합니다. 나만의 웹사이트를 만들기 위해 어디서부터 시작해야할까요? 우리는 튜토리얼에서 이미 어느정도 만들어진 리퍼지토리(소스)를 깃헙에서 다운 받아 여러 기능을 추가하고 코드를 조금씩 수정해볼 거에요. 이를 통해 장고 웹프레임워크을 활용한 웹 개발 과정을 조금씩 맛보고자 합니다.

[장고걸스에서는](https://www.djangogirls.org) 모든 이메일 끝에 ‘컵케이크와 하이파이브!’(Cupcakes and high fives)라는 메세지를 붙인답니다. 이 튜토리얼에서 우리는 장고컵케이크가게 사이트를 만들어 볼 거에요. 가상환경과 `pip install django`, 그리고 다른 패키지들을 생성하는 방법을 살펴볼 거에요. 모델에서는 이미지 입력(ImageField)과 사용자가 데이터를 입력하는 모델 폼을 추가하는 방법도 알아 볼 거에요. 마지막으로 시간이 허락된다면, 만든 여러분의 사이트를 PythonAnywhere 또는 Azure를 배포해 전 세계 모든 사람들이 볼 수 있도록 만들어봐요!

### 준비사항

장고에 입문하시는 여러분들을 환영합니다. 좀더 나은 실습환경을 위해 미리 파이썬, 장고, 코드 에디터를 설치하고 오세요. 다음 링크 [[영어](http://tutorial.djangogirls.org/en/installation/), [한국어](https://djangogirlsseoul.gitbooks.io/tutorial/content/installation/)]을 참고하세요. 어느정도 하루정도 장고걸스튜토리얼를 해보신 분들을 진행되니 사전에 장고 공식 튜토리얼, 파이썬 기초 등을 학습하시고 오셔도 좋습니다.
튜토리얼 진행은 **영어로** 진행 합니다.

### 튜토리얼의 목적
- 장고프로젝트를 시작하기 전에 가상환경을 올바른 방법으로 다룰 수 있습니다.
- 모델에 이미지필드를 생성할 수 있습니다.
- 장고 관리자의 역할과 기능에 대해 이해할 수 있습니다.
- 기본 템플릿과 확장 템플릿을 다룰 수 있습니다.
- 장고 폼을 생성하고 이를 템플릿에 활용할 수 있습니다.
- Git/Github의 기본 명령어로 프로젝트를 Github에 배포할 수 있습니다.
- PythonAnywhere 서버에서 `DEBUG=False mode`와 함께 사이트를 배포할 수 있습니다.
- 장고 사이트를 위한 간단한 테스트를 작성할 수 있습니다.

### 따라하기
그럼 다음 [링크](https://www.gitbook.com/book/djangogirlsseoul/djangocupcakeshop_ko/details)에서 차근차근 하나씩 따라가 봅시다.


