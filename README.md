# djangoチュートリアル #11 メール送信

## メール送信を設定してサインアップメールを送ろう！

## 完成版プロジェクト

<https://github.com/shun-rec/django-website-11>

## 事前準備

前回から引き続き実装している方は事前準備は不要です。

### 前回サインアップを作ったプロジェクトをダウンロード

```sh
git clone https://github.com/shun-rec/django-website-10.git
cd django-website-10
```

### データベースを作成

```sh
python manage.py migrate
```

### 動かしてみよう

開発サーバーを立ち上げて、サインアップの機能が動いていたらOKです。

```sh
python manage.py runserver
```

## ステップ1：メールサービスMailgunに無料登録

メールサーバーは何でも良いですが今回は最も有名なメールサービスの１つである「Mailgun」の設定方法を紹介します。  
他のメールサービスでも手順は同様です。

テストメールの送信や、少量のメールを送信のする場合は無料で使えます。  

※今回は独自ドメインの設定は行いません。  
※その場合、登録したメールアドレスにしかメールを送ることが出来ません。

## ステップ2: メール設定用のライブラリをインストール

プロジェクト直下（pj_loginフォルダ）で以下を実行します。

```sh
pip install "django-anymail[mailgun]"
```

## ステップ3：全体設定でメールを設定

全体設定で１度設定するとdjangoのすべての機能でメールを送信する時に使用されます。

`pj_login/settings.py`の`INSTALLED_APPS`に`anymail`を追加。

```py
    'anymail',
```

続けて、末尾に以下を追加。

```py
ANYMAIL = {
    "MAILGUN_API_KEY": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
    "MAILGUN_SENDER_DOMAIN": 'sandboxYYYYYYYYYYYYYYYYYYYYYYYYYYY.mailgun.org',
}
EMAIL_BACKEND = "anymail.backends.mailgun.EmailBackend"
DEFAULT_FROM_EMAIL = "hondo@example.com"
SERVER_EMAIL = "server@example.com"
```

* ANYMAIL: メールサーバーの設定を書きます。
* MAILGUN_API_KEY: MailgunのダッシュボードからAPIキーを取得して貼り付けます。
* MAILGUN_SENDER_DOMAIN: Mailgunのダッシュボードにあるドメインを貼り付けます。
* DEFAULT_FROM_EMAIL: ユーザーに送信されるメールのデフォルトの送信者です。
* SERVER_EMAIL: サイト管理者に送られるエラーなどのお知らせメールの送信者です。

※Mailgunでは独自ドメインを設定しなくても、登録したメールアドレスにのみにはテストメールが送信出来ます。

## テスト送信してみよう

djangoシェルを開いてサンプルメールを送信してみましょう。

```sh
python manage.py shell
```

```py
from django.core.mail import send_mail

send_mail("テスト", "テストメールです。", "送信者 <from@example.com>", ["shunshun.dev@gmail.com"])
```

## サインアップしてみよう

サインアップの認証メールが届くことを確認しましょう。  
独自ドメインを設定していない場合は、Mailgunに登録したメール以外では受け取れないことに注意しましょう。
