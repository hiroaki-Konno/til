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
