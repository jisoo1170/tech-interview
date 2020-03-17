# Django

1. 구조  ✔️
2. 동작 방식  ✔️
3. DRF 구조  ✔️
4. DRF 동작 방식  ✔️





## 구조

##### MTV 구조

- Model (데이터 관리)

  데이터베이스를 사용할 때는 SQL언어를 사용하지만 장고에서는 이에 대응되는 파이썬 문법을 통해서 데이터베이스를 정의한다. 이렇게 연결시키는 것을 Object-Relational-Mapping 이라고 하고 줄여서 ORM이라고 한다.

- Template (사용자가 보는 화면)

- View (중간 관리자)

  로직을 담당. URL을 통한 요청이 들어오면 해당 URL에 대한 view가 실행



##### config 폴더

프로젝트 설정 파일과 웹 서비스 실행을 위한 파일이 들어 있다.

- _init__.py

  파이선 2 버전과의 호환을 위해 만들어진 파일

  이 폴더가 하나의 묶음이라는 의미를 가지고 있다

- urls.py

  특정 기능을 수행하기 위해 접속하는 주소를 설정하는 파일

- wsig.py

  웹 서비스를 실행하기 위한 wsgi 관련 내용이 들어있다.

- settings.py

  프로젝트 설정에 관한 다양한 내용이 들어있다.

  - BASE_DIR

    프로젝트 루트 폴더를 지정한다

  - DEBUG

    디버그 모드를 설정한다. True일 경우 다양한 오류 메시지를 즉시 확인할 수 있다. 배포 할 때는 False로 바꿔준다.

  - ALLOWED_HOSTS

    서비스의 호스트를 정한다. 개발시에는 비어두고, 배포 시에는 실제 도메인을 적어준다.

  - INSTALLED_APPS

    현재 프로젝트에서 사용하는 앱의 목록을 기록하고 관리

  - MIDDLEWARE

    모든 요청/응답 메시지 사이에 실행되는 특수한 프레임워크

    주로 보안에 관한 내용

  - ROOT_URLCONF

    기준이 되는 urls.py의 경로를 설정한다.

  - WSGI_APPLICATION

    wsgi 어플리케이션을 설정



장고는 **웹 프레임워크**이며, 하나의 **어플리케이션**이다. 여러 사용자들이 동시에 사이트에 접근 할 경우, 이것을 처리하는 것은 **웹 서버**의 역할이다.

실제 배포 할 경우에는 nigix나 apache 같은 웹 서버가 필요하며, 웹 서버와 파이썬 어플리케이션인 django가 통신 할 수 있게 하는 wsgi(gunicorn, uwsgi 등)가 필요하다.

파이썬 프로젝트를 생성하면 wsgi.py라는 파일이 있다.

```python
from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "project.settings")

application = get_wsgi_application()
```

여기서 `application`이 바로 django 어플리케이션을 외부에서 사용 할 수 있도록  드러내는 것이다.

이 wsgi.py 파일 덕분에 우리는 어떤 wsgi를 사용하더라도, `application = get_wsgi_application()` 을 통해 외부에서 django 어플리케이션을 가져와 사용할 수 있는 것이다.





## 동작 방식

1. 장고 서버로 request가 들어온다.
2. 이벤트에 대한 URL Dispatcher가 URL을 분석해서 적합한 View로 요청을 보낸다.
3. View는 사용자 요청을 받아 Database 어디에 접근해서 어떤 data를 가공할 것인지 Model에게 알려준다.
4. Model은 DB와 연결하여 필요한 DB연산을 처리한다.
5. DB가 다시 Model로 결과 값을 보내주면 Model이 이것을 View로 전달한다.
6. View는 우리에게 보내줄 데이터를 Template에게 전달한다.
7. Template은 페이지를 만들어서 웹 브라우저에게 넘겨준다.

[출처](https://velog.io/@inyong_pang/Django-Intro)

![image](https://user-images.githubusercontent.com/26567962/76837361-c68fda00-6875-11ea-8505-adbd2ed7a365.png)





## DRF 구조

serializer는 직렬화하는클래스로서 사용자 모델 인스턴스를 JSON형태 혹은 Dictionary 형태로 직렬화 할 수 있다.

> 직렬화 : 나중에 재구성 할 수 있는 포맷으로 변환하는 과정



## DRF 동작 방식

1. 장고 서버로 request가 들어온다.
2. 이벤트에 대한 URL Dispatcher가 URL을 분석해서 적합한 View로 요청을 보낸다.
3. View는 사용자 요청을 받아 Database 어디에 접근해서 어떤 data를 가공할 것인지 Model에게 알려준다.
4. Model은 DB와 연결하여 필요한 DB연산을 처리한다.
5. DB가 다시 Model로 결과 값을 보내주면 Model이 이것을 Serializer에 전달한다.
6. Serializer에서 python 데이터 타입을 json 형태로 직렬화 해준다.
7. 이 후 view에 전달하고 결과를 response 해줍니다.



---

### Q. 장고의 구조를 설명해 주세요

장고는 MTV 형식을 가집니다. Model, Template, View로 모델은 데이터 베이스와 연결해 주는 역할입니다. 템플릿은 사용자가 보는 화면이고, 뷰는 로직을 담당합니다.



### Q. 장고의 동작 원리를 설명해 주세요

장고 서버로 request가 들어오면 URL Dispatcher가 URL을 분석해서 적합한 View로 요청을 보냅니다. 그러면 View는 DB의 어디에 접근해서 어떤 data를 가공할 것인지 Model에게 알려줍니다. Model은 DB와 연결하여 필요한 DB연산을 처리하고, 그 결과를 Model로 보내줍니다. 그러면 모델은 뷰로 전달하고, 뷰는 템플릿에게 전달합니다. 템플릿은 페이지를 만들어서 웹 브라우저에게 넘겨줍니다.



### Q. DRF 동작 원리를 설명해 주세요

장고 서버로 request가 들어오면 URL Dispatcher가 URL을 분석해서 적합한 View로 요청을 보냅니다. 그러면 View는 DB의 어디에 접근해서 어떤 data를 가공할 것인지 Model에게 알려줍니다. Model은 DB와 연결하여 필요한 DB연산을 처리하고, 그 결과를 Model로 보내줍니다. 그러면 모델은 시리얼라이저에 전달하고 파이썬 타입을 json 형태로 직렬화 해줍니다. 그 다음 뷰로 전달하고 그 결과를 response 해줍니다.