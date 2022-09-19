# django.md
## [django] no such table:django_sessionが出た場合
python manage.py runserver するディレクトリと同じディレクトリで  
python manege.py migrate [参考](https://kaerupyokopyoko.hatenablog.com/entry/2017/10/06/063855)    
  

## classのインスタンスをprintなりした時の表示を変更
```
class Prefecture(models.Model):
    pref_name = models.CharField(max_length=20)

    def __str__(self):
        return self.pref_name # これが表示される
```

```
(cs2019_web) C:\Users\iniad\smaclanavi_background>python manage.py shell

(InteractiveConsole)
>>> from administer_data.models import Prefecture as Pref
>>> pref = Pref(pref_name = "tokyo") # tokyoを表示名に指定
>>> pref
<Prefecture: tokyo>
>>> quit()
```

`def __str__()` にて表示する属性を指定できるらしい。詳しくは調べていない。  

---


## 以下コピペ用テスト
- 箇条書き
- なります
  - 段落下げ
  
[リンクづける文](https://www.google.com/)
  
`test code span`

```
# this is test code block
def test():
    return code
```
