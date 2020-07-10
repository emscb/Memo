# Django

## `views.py`

- `비어있지 않음`을 필터링할 때 `paid_at__isnull=False`
- 

## `forms.py`

- Form에서 보일 값들 필터링 : `__init__` 안에서 `self.fields[필드명].queryset = 모델.objects.filter(조건)`
- `model = Sponsor`는 `queryset = Sponsor.objects.all()`이다.
    - `Sponsor.objects.filter(조건)`로 필터링
- Form의 `class Meta`에서는 테이블 읽거나 그런거 하지 말자 (test/deploy에서 터질 수 있음)
- 

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