# python 目次
- [python 目次](#python-目次)
- [python 命名規則一覧](#python-命名規則一覧)
  - [classのインスタンスをprintなりした時の表示を変更](#classのインスタンスをprintなりした時の表示を変更)
- [関数アノテーション](#関数アノテーション)
- [辞書とformat文の組み合わせ](#辞書とformat文の組み合わせ)
- [モジュールのポップアップでよく見たkwargsについて](#モジュールのポップアップでよく見たkwargsについて)
    - [追記](#追記)
    - [参考](#参考)

# python 命名規則一覧

| 対象 | ルール | 例 |
| --- | --- | --- |
| パッケージ | 全小文字 なるべく短くアンダースコア非 | 推奨	tqdm, requests ...
| モジュール | 全小文字 なるべく短くアンダースコア可 | sys, os,...
| クラス | 最初大文字 + 大文字区切り | MyFavoriteClass
| 例外 | 最初大文字 + 大文字区切り | MyFuckingError
| 型変数 | 最初大文字 + 大文字区切り	MyFavoriteType
| メソッド | 全小文字 + アンダースコア区切り	| my_favorite_method
| 関数 | 全小文字 + アンダースコア区切り	| my_favorite_funcion
| 変数 | 全小文字 + アンダースコア区切り	| my_favorite_instance
| 定数 | 全大文字 + アンダースコア区切り


## classのインスタンスをprintなりした時の表示を変更
```
class Prefecture(models.Model):
    pref_name = models.CharField(max_length=20)

    def __str__(self):
        return self.pref_name # これが表示される
```

```
(cs2019_web) C:\Users\xxxx\django>python manage.py shell

(InteractiveConsole)
>>> from administer_data.models import Prefecture as Pref
>>> pref = Pref(pref_name = "tokyo") # tokyoを表示名に指定
>>> pref
<Prefecture: tokyo>
```

`def __str__()` にて表示する属性を指定できるらしい。詳しくは調べていない。  

# 関数アノテーション
引数名のあとの: 式がそれぞれの引数に対するアノテーション（注釈）、括弧と末尾のコロン:の間の-> 式が返り値に対するアノテーションとなる。アノテーションを記述するものと記述しないものが混在していても問題ない。


```python
def func_annotations(x: 'description-x', y: 'description-y') -> 'description-return':
    return x * y

print(func_annotations('abc', 3))
# abcabcabc

print(func_annotations(4, 3))
# 12
```
ただし関数アノテーションはあくまでも引数や返り値に対する注釈で、それをもとに特別な処理が行われることはない。  
実行時に型チェックが行われたりはしないのでアノテーションで指定した型以外の引数を渡しても何も起こらない（エラーにならない）。

[参考サイト](https://note.nkmk.me/python-function-annotations-typing/)  


# 辞書とformat文の組み合わせ

```python
>>> dict = { "name": "Mike", "age": 28 }
>>> "Name: {0[name]}, Age: {0[age]}".format(dict)
'Name: Mike, Age: 28'
```
`{引数の添え字[辞書のキー]}`で辞書をそのまま渡してもいい感じに適応してくれるらしい。

[Pythonのformatメソッドによる文字列フォーマットの方法](https://uxmilk.jp/40547#crayon-6360c02767bf8236995362)  


# モジュールのポップアップでよく見たkwargsについて
- *args: 複数の引数をタプルとして受け取る
- **kwargs: 複数のキーワード引数を辞書として受け取る

複数のやつをばらして渡したり、ポインタ関連のもの、たぶん。  

以下のコードはしっかりと動く。

```python
area_url_format = "https://www.softbank.jp/shop/search/list/?pref={0[pref]}&area={0[area]}&cid=tpsk_191119_mobile"

tmp_args = {
    "pref": 13,
    "area": 131172,
}

area_url = ScrapingBase.url_from_format(area_url_format, **tmp_args)
```

上記コードだとキーワード引数としてしか扱えなくなり、コードの最終行を`url_from_format(area_url_format, tmp_args)`にして実行する(位置引数として渡す)と

```python
TypeError: test() takes 0 positional arguments but 1 was given
```
とか言われる。

上記エラーはこのコードの最終行が`url_from_format(area_url_format, tmp_args)`だったために発生した。これは`tmp_args`に`**`を付けることによってキーワード引数の辞書として受け渡されるべきところを、位置引数として受け渡してしまっていたために起こった。

### 追記
実際に起こっているのはこういうことだろうたぶんな図解
```python
tmp_args = {
    "pref": 13,
    "area": 131172,
}

func(tmp_args)は第一引数として渡しているだけだが

func(**tmp_args)の場合は
func(pref=13, area=131172)と同義

```

### 参考
[note.nkmk.me](https://note.nkmk.me/python-args-kwargs-usage/)  
[Python 可変長引数（*args, **kwargs）とは](https://aiacademy.jp/media/?p=1496)
