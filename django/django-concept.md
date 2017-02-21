
# MVC & MTV

 1. Model
   * 안전하게 데이터를 저장
   * integrity, consistency, queries & mutations
 2. View
   * 데이터를 적절하게 유저에게 
   * presentation, assets & code
 3. Controller
   * 사용자의 입력과 이벤트에 반응하여 model과 view를 업데이트
   * receive, interpret & validate input, create & update views, query & modify models
 
# Django 개념

  1.웹브라우저에서 웹서버(nginx,apache)로 요청
  
  2.wsgi.py(게이트웨이 역활)
  
  3.urls.py가 정규표현식에 맞게 view로 보내줌
  
  4.views.py에 실질적인 파이썬 코드를 작성
  
  5.view에서 데이터를 가져올건지 입력할건지 판단
  
  6.model에 신호를 보내고 데이터베이스와 연결 후 데이터를 받거나 입력
  
  7.데이터를 view에 전달 후 가공
  
  8.template에서 ui작업, control과 관련된 로직 삽입
  
  9.form.py를 이용하여 모델과 템플릿에 있는 사용자들이 사용하는 ui를 관리
  
  10.처리 결과를 웹서버로 전달

# settings.py

프로젝트 환경 설정 파일
  - DEBUG
    - 디버그 모드 설정(true)
  - INSTALLED_APPS
    - pip로 설치한 앱 또는 본인이 만든 app을 추가
  - MIDDLEWARE_CLASSES
    - request와 response 사이의 주요 기능 레이어
  - TEMPLATES
    - django template관련 설정, 실제 뷰(html, 변수)
  - DATABASES
    - 데이터베이스 엔진의 연결 설정
  - STATIC_URL
    - 정적 파일의 URL(css, javascript, image, etc)

# manage.py

  - 프로젝트 관리 명령어 모음
  - 주요 명령어
    - statapp - 앱 생성
    - runserver - 서버 실행
    - createsuperuser - 관리자 생성
    - makemigrations app - app의 모델 변경 사항 체크
    - migrate - 변경 사항을 DB에 반영
    - shell - 쉘을 통해 데이터를 확인
    - collectstatic - static파일을 한 곳에 모음


## node 
  - pip = npm
  - requirememts.txt = package.json
 
 
     
