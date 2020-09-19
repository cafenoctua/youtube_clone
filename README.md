# python package
```
pip install django==3.0.7
pip install djangorestframework==3.10
pipinstall djangorestframework-simplejwt==4.1.2
pip install djoser
pip install pillow
pip install django-cors-headers
```
- djangorestframework-simplejwt
  - jwt(json web token)というセキュリティトークンを生成する
- djsoer
  - jwtのトークン関係で必要
- django-cors-headers
  - フロントエンドと接続するために使う

# Django
新しいプロジェクトを作成
```
django-admin startproject youtube_api
```

新しいアプリケーション作成
```
django-admin startapp api
```

サーバーのローカル起動
```
python manage.py runserver
```

DBにモデルの登録
```
python mage.py makemigrations
python manage.py migrate
```

superuser作成
```
python manage.py createsuperuser
```

## settings
作成したプロジェクト名のフォルダ内にある```settings.py```というファイル開く

- 使用するアプリケーション追加
```python
INSTALLED_APPS = [ ...
    'rest-framework',
    'api.apps.ApiConfig',
    'corsheaders',
    'djoser',
]
```
'api.apps.ApiConfig': 作成したアプリケーションもここで定義する

- MIDDLEWARE
```python
MIDDLEWARE=[
    'corsheaders.middleware.CorsMiddleware',
    ...
]
```
これで外部接続を定義する

- 接続を許可する接続先の定義
```python
CORS_ORIGIN_WHITELIST = [
    "http://localhost:3000"
]
```

- ベースとなる認証を作る
```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```
```settings.py```で作ることで毎回views.pyのview毎に定義する必要なく基本的な認証を適用できる
また、view毎に適宜カスタマイズが可能
  - DEFAULT_PERMISSION_CLASSES: 認証のロール
    - rest_framework.permissions.IsAuthenticated: 認証を通ったユーザーのみ
  - DEFAULT_AUTHENTICATION_CLASSES: 認証方法
    - rest_framework_simplejwt.authentication.JWTAuthentication: simpleJWTを使った認証を行うと宣言

- simple jwtの設定
```python
SIMPLE_JWT = {
    'AUTH_HEADER_TYPES': ('JWT',),
    'ACCESS_TOKEN_LIFETIME': timedelta(minutes=30),
}
```
    - ACCESS_TOKEN_LIFETIME: トークンの生存時間の設定

- 画像、動画ファイル格納先定義
プロジェクト直下にmediaフォルダを作成後以下コードで参照先を定義する
```python
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```

- ユーザー認証方法を定義
```python
AUTH_USER_MODEL = 'api.User'
```
apiAppでUserモデルを定義することでdjangoに対して任意の認証方法を設定できる

## urls.py
- 認証
```python
urlpatters = [
    ...,
    path('authen/', include('djoser.urls.jwt')),
]
```
'authen/'に認証情報を送ることでdjoserでjwtを発行するように定義できる

## 構成要素
model + serializer + viewでアプリが構成される

## models.py
- uuidは複雑なidを生成することで推測されづらい実用的に使う
- class UserManager(BaseUserManager):で認証等のUser管理の設定モデルが作れる

# React
@material-ui/core
@material-ui/icons
react-icons
react-cookie
react-router-dom
axios
react-modal
react-player