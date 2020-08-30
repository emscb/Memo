# Django

## `views.py`

- `비어있지 않음`을 필터링할 때 `paid_at__isnull=False`
- 쿼리 결과 정렬 : `filter().order_by('컬럼명')`
- 

## `forms.py`

- Form에서 보일 값들 필터링 : `__init__` 안에서 `self.fields[필드명].queryset = 모델.objects.filter(조건)`
- `model = Sponsor`는 `queryset = Sponsor.objects.all()`이다.
    - `Sponsor.objects.filter(조건)`로 필터링
- Form의 `class Meta`에서는 테이블 읽거나 그런거 하지 말자 (test/deploy에서 터질 수 있음)

## `models.py`

- 한 테이블에서 같은 테이블에 2개의 포린키를 걸면 에러가 뜬다.
    - 장고에서는 포린키를 걸면 반대 방향의 reverse relation을 만들기 때문
    - 각 필드에 `related_name`을 명시해주면 해결

## Template

- Model에 있는 변수 값을 가져오고 싶을 때 `get_변수_display`
    - ForiegnKey 걸려있으면 그냥 `변수`
- `{% %}` 안에서는 변수를 쓸 때 `{{ }}`를 안 해도 된다.
- `base.html`에 들어가는 변수, context들은 `context_processor.py`에서 넣어준다.
- `date:"날짜 형식"` : 날짜 객체를 마음대로 표시해보자 (i18n 번역 가능) ([참조]( https://docs.djangoproject.com/en/3.0/ref/templates/builtins/#date ))
- 

## Admin Page

- Admin에서 import/export 버튼 만들기
    - `from import_export.admin import ImportExportModelAdmin`
    - `class ProgramCategoryAdmin(ImportExportModelAdmin)`
- Admin에서 `ordering` desc로 하려면 필드명 앞에 `-` 붙이기
- 인스턴스 만들 때 foreign key를 자동으로 채우려면 `autocomplete_field` 명시하기
- Action 만들기
    - `def 함수명(self.request, queryset): queryset.update(내용)`
    - `action = (함수명,)`
    - `action.short_description = "액션 설명"`
- Foriegn key 필드 만들기
    - `def 함수명(self, obj): return obj.필드명`
    - 필드 헤더 바꾸기 `함수명.short_description = "헤더명"`
    - 이 필드를 `search_fields`에 넣으면 가끔 에러가 뜨는데, 이건 FK가 걸린 필드도 `search_fields`에 들어있어서 그렇다.
    - `search_fields`에 넣을 때는 `함수명` 대신 `__`를 이용해서 그냥 쓰자
- 

## `tests.py`

- `django.test.TestCase`를 상속받아서 테스트 케이스 클래스를 만들자
    - 안에다가 테스트들을 함수로 만든다.
    - `self.assertIs`, `self.assertEqual` 등의 함수로 값이 맞는지 확인
    - 뷰 테스트는 `self.client.get('주소')`를 이용해서 내용을 본다.
- `python manage.py test [앱 이름]` : 테스트 실행
- 

## 기타

- Form이나 model에서는 `ugettext_lazy()`를 사용해야 번역이 제대로 된다.
- 