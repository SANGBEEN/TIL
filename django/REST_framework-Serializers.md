#Serializers

##시리얼라이저?
 api를 만들기 위해서는 모델 클래스의 인스턴스를 json같은 형태로 직렬화 하거나 반직렬화 하여야 한다. rest framwork에서는 django의 form과 비슷한 시리얼라이저를 작성할 수 있다. 
 시리얼라이저 클래스에서 form클래스에서 사용하던 옵션을 지정할 수 있으며 인스턴스가 생성 혹은 수정되는 과정을 전부 명시할 수 있다. 
 ModelSerializer를 사용하면 세세한 기능을 일일이 구현하지 않아도 된다.

##사용
인스턴스를 생성하고
```
from snippets.models import Snippet  
from snippets.serializers import SnippetSerializer  
from rest_framework.renderers import JSONRenderer  
from rest_framework.parsers import JSONParse

snippet = Snippet(code='print "hello, world"\n')  
snippet.save()
```
직렬화  
```
serializer = SnippetSerializer(snippet)  
serializer.data
```

폼을 다루는 방식과 매우 비슷하며 쿼리셋도 직렬화할 수 있다.
```
serializer = SnippetSerializer(Snippet.objects.all(), many=True)  
serializer.data 
```

##ModelSerializers
 django에서 Form클래스와 ModelForm클래스를 제공하듯이, rest framework에서도 Serializer클래스와 ModelSerializer클래스를 제공한다.

```
class UserCreateSerializer(ModelSerializer):

    class Meta:
        model = MyUser
        fields = [
            'email',
            'password',
        ]
```
ModelSerializer클래스를 사용하면 피드를 자동으로 인식하고 create() 메서드와 update()메서드가 이미 구현되어 있기 때문에 보다 쉽게 시리얼라이저 클래스를 작성 할 수 있다.

##view 만들기
```
class UserCreateAPIView(CreateAPIView):
    """
    유저 회원가입 API
    """
    serializer_class = UserCreateSerializer
    permission_classes = [AllowAny]

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)

        if serializer.is_valid(raise_exception=True):
            self.perform_create(serializer)

            response_data = {
                "success": True,
                "message": "회원 가입이 완료되었습니다."
            }

            return Response(response_data, status=HTTP_201_CREATED)

```

url 연결
```
from django.conf.urls import url
from users.views import UserCreateAPIView,
urlpatterns = (
    url(r'^signup/$', UserCreateAPIView.as_view(), name='signup')
)
```
