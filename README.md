# Private Diary
専用の日記アプリをDjangoで作成した。  
[参照の本](https://www.amazon.co.jp/%E5%8B%95%E3%81%8B%E3%81%97%E3%81%A6%E5%AD%A6%E3%81%B6-Python-Django%E9%96%8B%E7%99%BA%E5%85%A5%E9%96%80-NEXT-ONE/dp/4798162507/ref=asc_df_4798162507/?tag=jpgo-22&linkCode=df0&hvadid=343222257571&hvpos=&hvnetw=g&hvrand=15851572609106809943&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1009718&hvtargid=pla-848852189750&psc=1&th=1&psc=1)

## 環境
* Python 3.7
* Django 2.2.2
* PostgreSQL 10.14
* ChromeDriver 85.0.4183.87 (google chromのバージョンに合わせる)

## 仮想環境構築
Pythonで仮想環境を構築する。
Djangoのモジュールは仮想環境ごとで独立しているため、仮想環境同士の影響を受けることなく開発できる。

    $ python -m venv venv_private_diary
次に、仮想環境に入る

    $ source venv_private_diary/bin/activate
    (venv_private_diary)$
となればOK!

## requirements install
必要なパッケージをインストールする。

    $ pip install -r requirements.txt

## DataBaseの設定
[こちらを参照](https://qiita.com/kanzaki0507/items/12a2ef0b778250d699bd)  
今回はpostgresql@10を使用したのでインストールするときは@10をつける。

    $ pip install postgresql@10
インストールできたら、PATHを通す。

    $ vi .bash_profile
vimで.bash_profileを開きDataBaseのPATHを通す。

    export PATH=$PATH:/usr/local/Cellar/postgresql@10/10.14/bin/
PATHを通したら次はDataBaseを作成する

    $ brew services start postgresql@10
    $ create private_diary
「psql -l」コマンドでデータベース一覧を確認する。「private_diary」が存在していれば問題ない。

データベースを作成したら次はMigrationでデータベースとアプリケーションを関連付ける。

    $ python manage.py makemigrations
    $ python manage.py migrate
「makemigrations」でMigrateするためのファイルを作成  
「migrate」でマイグレーションを行う。
### Errorが出たら...
ここでエラーが出た場合、accounts/migrations/ & diary/migrations/ この二つの中の__init__.py以外(0001_initial.py or __pycach__.pyなど)を全て削除して
再度migrationする。  
それでもダメなら、上記のファイルを消した上でDataBaseを削除し、再度DBを構築してMigarationを行う。  
対処方法は[こちら](https://qiita.com/kanzaki0507/items/df056f20f3c42bae1abf)

## Web Serverを動かす
    $ python manage.py runserver --settings private_diary.settings_dev