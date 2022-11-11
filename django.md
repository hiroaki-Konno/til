<!-- snippet で変換可能 -->
# drfのAPIで返ってくるフィールド名を変更する
<!-- heading で各サイズのhタグ変換可能 -->

```python
# models.py
class City(models.Model):
    id = models.BigAutoField(primaly_key=True)
    city_name = models.CharField(max_length=20, unique=True)
    # pref instance(pk=1) = 未設定用
    prefecture = models.ForeignKey(Prefecture, 
                        on_delete=models.SET_DEFAULT, default=1)
```
```python
# serializers.py
class CitySerializer(serializers.ModelSerializer):
    prefecture = serializers.someField(略)

    class Meta:
        model = models.City
        fields = ['id', 'prefecture', 'city_name']
```
```json
/* APIが返すjson形式の出力 */
{    
    "id": 1,
    "prefecture": "未指定",
    "city_name": "未設定"
}
```
コードはスマクラナビ(2022時の実習)より
### 前提
Meta.field名はmodels.pyのクラスに定義したfieldたちの変数名が対応するように出来ている。  
例としては`CitySerializer.Meta.fields`は`City`のfieldの変数名のものが指定できる

### 問題
APIが返す要素を任意の名称でMeta.fieldsに追加したい。  
今回は例としてidをcity_idに変更する。

### 解決
```python
# serializers.py
class CitySerializer(serializers.ModelSerializer):
    prefecture = serializers.someField(略)

    # BigAutoFieldに指定されているが IntegerFieldの拡張なのでこれでよい
    city_id = serializers.IntegerField(source='id')

    class Meta:
        model = models.City
        # fields = ['id', 'prefecture', 'city_name']
        fields = ['city_id','prefecture','city_name']
```
`<変更したい名前>` に `serializers.対応するfield(source='<変更前の名称>')`を代入し、その代入した変数名を`Meta.fields`に追記する
### 結果
```json
/* 出力 */
{    
    "city_id": 1,
    "prefecture": "未指定",
    "city_name": "未設定"
}
```
表示する名称を変更をすることが出来た。

[参考サイト - Django REST Frameworkでフィールド名を変更する方法](https://www.web-dev-qa-db-ja.com/ja/django/django-rest-framework%E3%81%A7%E3%83%95%E3%82%A3%E3%83%BC%E3%83%AB%E3%83%89%E5%90%8D%E3%82%92%E5%A4%89%E6%9B%B4%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/1046156034/) 




# serializerでfieldに関数を設定して動的な値を返す
### 目的

子要素の最新のupdatedを取得して親のupdatedも更新するようにしたい 

### 解決
1対多の参照をmodel内に定義するのは難しそうだったのでmodelに対応したserializerのクラスに関数を絡ませたfieldを作った。

### 詳細
serializer内に
```python
class SomeSerializer(serializers.ModelSerializer):    
    updated = serializers.SerializerMethodField()

    def get_updated(self, obj):
        """ 複数あるschedulesのupdatedを比較して最新のupdatedを
        UpcominglecInfoのupdatedに設定する """
        # 該当するインスタンスをget、modelで定義したrelatednameで逆参照をかける
        upcomeinfo = UpcomingLecInfos.objects.get(id=obj.id)
        related_schedules = upcomeinfo.schedules.all()

        latest_date = related_schedules[0].updated

        for s in related_schedules:
            if latest_date < s.updated:
                latest_date = s.updated

        return latest_date
```
を追加  
`get_<field_name>`がメソッド名になる(今回はget_updated)  

処理としては
1. メソッドの第二引数に自身の情報が色々渡されるっぽいのでそこからidを取得してobject.get()  
1. models.py内のrelated_name(今回はschedules)を用いた逆参照で子要素を取得  
1. datetime型は比較演算子で比較可能なのでfor文まわして最新の更新時間を取得

### 参考
[methodField](https://www.django-rest-framework.org/api-guide/fields/#serializermethodfield)  
逆参照については「django model 逆参照」を検索してください、いい感じのなかった
### 未解決
メソッド名をmethodFieldに渡して指定する方法もあるっぽいがattribute Error?とかが出てうまくいかなかった。


# manage.py shellでmodelのimportがうまくいかない
#### python
```python
from models import TestSaveData
```

#### error
```shell
File "C:\Users\iniad\smaclanavi_background\administer_data\scraping.py", line 2, in <module>
    from models import TestSaveData
ModuleNotFoundError: No module named 'models'
```
moduleが見つからないと言われてしまう
### 解決
#### python
```python
from administer_data.models import TestSaveData
```
`from [app名].models import [model名]` の形に変更これだと相対パスからしても通る。  
ファイル単体でも通る？のはよくわからない。installした類の配下に置かれてる判定になってるからだろうか？

参考サイトはない

# djangoのmodels.pyをUMLに自動で起こす
実行するコマンド
```
manage.py graph_models administer_data -g -o test_visualized.png
```
django-extensions で追加される`manage.py graph_models`を使用  

正直pylintで何故代用できたのかわからない、環境パス通すだけでどうにかなった類なのだろうか  

### 必要パッケージ
- django-extensions
- ~~pygraphviz~~ (status code 2とかでうまくいかず、なんなん？)
- pyparsing
- pydot
- pylint [参考](https://qiita.com/kenichi-hamaguchi/items/c0b947ed15725bfdfb5a)

```
pip install django-extensions
pip install pyparsing pydot
pip install pylint
```
1. Graphvizが必要なので，[ここ](https://graphviz.gitlab.io/download/)からインストーラを落としてインストール
2. Graphvizのbinフォルダ(「C:\Program Files\Graphviz\bin」)のPATHを通す
3. 出力可能な形式をdot -Txxxで確認する。ここでエラーが起きると，Graphvizのインストールができてないか，パスが通っていない。
```
$ dot -Txxx
Format: "xxx" not recognized. Use one of: bmp canon cmap cmapx cmapx_np dot emf emfplus eps fig gd gd2 gif gv imap imap_np ismap jpe jpeg jpg metafile pdf pic plain plain-ext png pov ps ps2 svg svgz tif tiff tk vml vmlz vrml wbmp xdot xdot1.2 xdot1.4
```
4. インストールできたかテストする。
```
pyreverse -o png -p Pyreverse pylint/pyreverse/
```
以下の2つのファイル(冒頭の図)がカレントディレクトリの下にできていれば，インストール完了。
- classes_Pyreverse.png
- packages_Pyreverse.png

### 参考
<!-- link で変換可能 -->
[[python] クラス図を自動生成する](https://qiita.com/kenichi-hamaguchi/items/c0b947ed15725bfdfb5a)  

[django-extensions 公式docs](https://django-extensions.readthedocs.io/en/latest/graph_models.html)  

[windowsのパスを通す(環境変数の追加)](https://qiita.com/sta/items/63e1048025d1830d12fd)


# runserver時 no such table:django_sessionが出た場合
データベースのテーブルができていないっぽい？ので  
`python manage.py runserver` するディレクトリと同じディレクトリで `python manege.py migrate` 

[参考リンク](https://kaerupyokopyoko.hatenablog.com/entry/2017/10/06/063855)  

# リンクや画像のソースに変数を用いて動的に扱う方法
{% static 'css/パス名~~~' %}のように{% %}を使わず、'css/パス名'を変数に入れて扱うことでほかの変数も扱えるようになる
 
【成功例】
```html
{% load static %}
<body>
    {% static "cards/" as card_path %}
    {% for rank in card_rank %}
    <img src="{{ card_path }}c_{{ rank }}.png">
    {% endfor %}
</body>
```
【失敗例】
```html
{% load static %}
<body>
    {% for rank in card_rank %}
    <img src="{% static 'c_{{ rank }}.png' %}">
    {% endfor %}
</body>
```

# django の環境上でローカルの python ファイルをアナコンダプロンプトで実行する
python ファイルの文頭にこいつらを入力 ( パスは適宜変わる )  
modelをimport する前にこの設定が必要らしい  
ちなみにこいつらは  manage . py からある程度コピペしてきている

```python
import sys
import os
import django

sys.path.append('./')
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')
django.setup()
```
[参考リンク](https://qiita.com/jun6428/items/c14ca22d692295a5a391)  

# templateの継承、インクルード
htmlの共通した部分使いまわせるようにする機能たち。  
共通の部分をベースとして拡張していくものが継承、逆にパーツを作っておいてそれを差し込む形になるのがインクルード(include)  
どのファイルもアプリ内のtemplatesの内部に置く  

[参考リンク](https://django.kurodigi.com/template-parts/)  

## 継承：{% block 名前 %} と {% extends 'base.html '%}
継承はベースとなるhtml内に{% block 名前 %}と{% endblock %}を書く。これを仮に base.html とする
 
例) base.html
```html
{% load static %}<!--css file読み込み-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    {% block css %}{% endblock %}<!--css file紐づけ <link>用-->
    <title>
    {% block title %}{% endblock %}<!--ページタイトル-->
    </title>
</head>
<body>
    {% block body %}{% endblock %}<!--htmlの body記述用-->
</body>
</html>
```  
 
拡張する側にはファイルの先頭に  
{% extends 'ファイルへのパス' %} を記述  
(今回ならbase.html)
そして
{% block 名前 %} と {% endblock %} の内部に各々の部分を記述する
 
例)top.html
```html
{% extends 'base.html' %}  
{% block css %}
    <link rel="stylesheet" type="text/css" href="style.css" />
{% endblock %}
{% block title %}
top画面
{% endblock %}
{% block body %}
<h1>トップ画面</h1>
<p>あとは普通にhtmlを書くだけ</p>
{% endblock %}
``` 
以上

## インクルード ( include )
差し込みたい部品を別のファイルに保存しておき {% inclde 'ファイルへのパス' %} で差し込んだ場所に表示、使用できるようにする。
 
差し込む部品のファイルを parts.html 差し込み先を body.htmlとする

例) body.html  
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>body.html</title>
</head>
<body>
{% include 'parts.html' %}
<!--上に差し込んだ部品が表示される-->
</body>
</html>
```

例) parts.html
```html
<p>これはparts.htmlからのパーツです。</p>
```

# テンプレートでの文字列連結

(djangoのテンプレートで文字列を連結する方法/)  
組み込みフィルタ「add」を使用します。  

[参考リンク](https://intellectual-curiosity.tokyo/2019/12/17/django%E3%81%AE%E3%83%86%E3%83%B3%E3%83%97%E3%83%AC%E3%83%BC%E3%83%88%E3%81%A7%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E9%80%A3%E7%B5%90%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95/)  

```
{{ value|add:"2" }}
```
 
>このフィルタは、まず両方の値を強制的に整数とみなして加算しようとします。  
>失敗した場合は、とにかく値を足し合わせることを試みます。  
>これはいくつかのデータ型 (文字列、リストなど) では動作しますが、それ以外では>失敗します。失敗した場合、結果は空の文字列になります。  
>出典：Djangoドキュメント
 
文字列を連結する際は、以下のように使用することが出来ます。
 
```html
{% with template="string1/"|add:"string2" %}
    {{ template }}
{% endwith %}
```
 
上記は、「string1/」と「string2」を連結してtemplateという変数に代入しています。
templateを表示すると、「string1/string2」という値になります。
## 注意事項
addで連結するのもが文字列の場合、想定通りの結果となります。
しかし、連結しようとするものが文字列ではなく数値の場合、エラーが発生したり予期せぬ結果となります。
### 回避案
文字列連結用のカスタムタグを定義して使用します。

以下のようなファイルを作成します。  
\<appname>\\templatetags\\\<appname>_extras.py
#### ソース
```python
from django import template

register = template.Library()


@register.filter
def addstr(arg1, arg2):
    return str(arg1) + str(arg2)
```
テンプレートでは以下のように使用します。
```html
{% load <appname>_extras %}

{% with template="3"|addstr:"4" %}
        {{ template }}
{% endwith %}
```
#### 実行結果
```
34
```
3と4が文字列として連結された結果になります。
ちなみに「add」を使用した場合、3と4が加算されて7という結果になります。

# カスタムテンプレートフィルタ・テンプレートタグの作り方
[参考リンク](https://djangobrothers.com/blogs/custom_template_tags_filters/)

## テンプレートフィルタ / タグとは
Djangoのテンプレートには、テンプレート側で処理を行う様々なタグやフィルタが用意されており、これをビルトイン・テンプレートフィルタ / タグと呼ぶ。

よく利用するDjangoのビルトイン・テンプレートタグの例としては、{% if ... %}や{% for ... in ... %}などがあり、ビルトイン・テンプレートフィルタの例としては、lengthやlinebreaksなどがある。 {% %}で利用されるものがタグで、{{ }}で利用されるものがフィルタという認識でOK

これらを自分で作ることができる機能があり、それをカスタムテンプレートフィルタ / タグという
## カスタムテンプレートフィルタ / タグの作り方
カスタムテンプレートフィルタ/タグを作るには、3つのステップが必要
1. アプリ内に **stemplatetags** ディレクトリを作成する
2. **templatetags** ディレクトリ内に **\_\_init\_\_.py** ファイルを作成する
3. **templatetags** ディレクトリ内にカスタムフィルタ・タグのファイルを作成する

カスタムフィルタ/タグを作成するには、templatetagsというディレクトリをアプリケーション内に作成し、その中にフィルタやタグのロジックを記述する。

その際に、templatetagsがモジュールであることを示すために__init__.pyという名前の空のファイルを作成することを忘れないようにする。

ディレクトリの構成は、例えば以下のようになります。これはappというアプリケーション内にmath_tags.pyというカスタムフィルタ/タグのファイルを作成するパターンになります。
<br>
<br>

appアプリケーションにカスタムフィルタ / タグを追加する
```python
─app
  ├─__init.py
  ├─models.py
  ├─views.py
  ├─migrations
  │  └─__pycache__
  ├─templates
  │  └─video
  └─templatetags  ##ここのフォルダを作成する
     ├─__init__.py  
     └─math_tags.py
```

上記のようにカスタムフィルタ/タグを定義したら、テンプレート側でモジュールを呼び出し、実際にフィルタやタグを利用することができるようになる。
<br>
<br>
math_tagsモジュールをテンプレートで呼び出す
```
{% load math_tags %}
```
注意点として、{% load %}を利用するには、呼び出すモジュールを含むアプリケーションがINSTALLED_APPSに含まれていなければならない

カスタムテンプレートフィルタの作り方
カスタムフィルタの作成は、1つか2つの引数をとるPython関数を定義し、Djangoのテンプレートタグのライブラリに登録するというステップになります。

上記の例の、math_tags.pyに、二つの数字の掛け算を適用するフィルタを定義してみます。


from django import template

register = template.Library() ## Djangoのテンプレートタグライブラリ

## カスタムテンプレートフィルタの作り方
カスタムフィルタの作成は、1つか2つの引数をとるPython関数を定義し、Djangoのテンプレートタグのライブラリに登録するというステップになります。

上記の例の、math_tags.pyに、二つの数字の掛け算を適用するフィルタを定義してみます。
```python
from django import template

register = template.Library() ## Djangoのテンプレートタグライブラリ

## カスタムフィルタとして登録する

@register.filter
def multiply(value1, value2):
    return value1 * value2
```
上記のように定義するだけで、テンプレート側で以下のようにフィルタを利用することができるようになります。

```html
{% load math_tags %}

<!-- num1=3, num2=4のとき、HTML上には12が表示される -->
<span>掛け算:{{ num1 | multiply:num2 }}</span>
```
上記の処理の場合、multiply関数の第一引数（value1）にnum1が、第二引数（value2）にnum2が渡され、結果が返ってきます。

これで、View側で掛け算を行った結果をテンプレートに返すのではなく、テンプレート上の変数を掛け合わせて表示することができるようになりました。

## カスタムテンプレートタグの作り方
カスタムタグはフィルタよりも複雑な処理を扱うことができますが、カスタムフィルタで利用したmultiply関数の処理をカスタムタグで書いてみます。

※なお、カスタムテンプレートタグの作り方にはいくつか種類がありますが、ここでは最も一般的なsimple_tagを利用する方法を紹介します。
```python
from django import template

register = template.Library() ## Djangoのテンプレートタグライブラリ

## カスタムタグとして登録する
@register.simple_tag
def multiply(value1, value2):
    return value1 * value2
```
関数の定義は同じですが、simple_tagというデコレータ修飾しています。これでカスタムタグとして利用することができます。
```html
{% load math_tags %}

<!-- num1=3, num2=4のとき、HTML上には12が表示される -->
<span>掛け算:{% multiply num1 num2 %}</span>
```
## テンプレートフィルタとテンプレートタグの使い分け
テンプレートフィルタとテンプレートタグは似たような機能ですが、どのように使い分けるのが良いのでしょうか？

大きく、以下の二つのポイントがあると思います。

- 関数の引数が2つ以下か？
- ロジック内に、テンプレートコンテキスト（template context）を使うか？
 
カスタムフィルタは、その関数の引数を1つか2つにする必要があるため、それより多い引数が必要な場合には必然的にカスタムタグの方を利用せざるを得ません。

また、カスタムタグの方ではViewからTemplateに渡されるコンテクスト（テンプレート内で利用できる変数）を、そのロジック内で利用することができます。  
そのため、基本的にはフィルタよりもタグの方が複雑な処理に向いています。『テンプレートでの表示の前に、少しだけ内容を修正したい』というレベルであればカスタムフィルタを、それ以上の複雑なロジックを組み合わせたいという場合であればカスタムタグを使いましょう。
