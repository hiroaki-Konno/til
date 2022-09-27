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