# html, css 目次
<details>
<summary>目次</summary>

- [html, css 目次](#html-css-目次)
- [bootstrapのカラム割り 横スクロールバーが出てくる](#bootstrapのカラム割り-横スクロールバーが出てくる)
- [marginが相殺される(marginがうまく足されない)](#marginが相殺されるmarginがうまく足されない)
- [block要素を左寄せ、中央寄せ、右寄せする方法](#block要素を左寄せ中央寄せ右寄せする方法)
- [\<label\> (label要素)](#label-label要素)

</details>

# bootstrapのカラム割り 横スクロールバーが出てくる
カラム割りの時に使う row クラスにwidth 100%を指定することで対処  
【css】 
```css 
.row{  
    max-width: 100%;  
} 
``` 

# marginが相殺される(marginがうまく足されない)
headerなどをposition:fixed等で判定をなくしておりそれの回り込み回避のためのmarginを指定しているdivがあるはず  
そこにほんの少しでも良いのでpaddingを設定することでmargin同士が吸われなくなる

[参考リンク](https://stocker.jp/diary/margin-offset/)  

下記の例でいうところの.contentのpadding-topの設定のところ

【html】
```html
<!DOCTYPE html>
<html>
<body>
    <div class="header">
        ヘッダー
    </div>
    <div class="content">
    この中にコンテンツ
    </div>
</body>
</html>
``` 
【css】 
```css
.content{
    margin: 90px 10px 10px;  
    padding-top: 1px;  
    height: 100%;  
}

.header{
    top: 0;
    margin-top: 0px;
    z-index: 100;
    height: 90px;
}
```

# block要素を左寄せ、中央寄せ、右寄せする方法
[参考リンク](https://www.acky.info/tips/css/0002.html)  
div・p・ul・li・dlなどのブロック要素を左寄せ・中央寄せ・右寄せするときはwidthとmarginを使います。
中央寄せのmargin-right: auto; margin-right: auto;をしっかり覚えましょう。
ブロック要素を左寄せや右寄せしたり、左右に並べるときはfloatを使うときが多いのでmarginはあまり使いません。

まとめ！
- 左寄せは、ブロック要素にwidth(幅)を指定して margin-right: auto;
- 中央寄せは、ブロック要素にwidth(幅)を指定して margin-left: auto;margin-right: auto;
- 右寄せは、ブロック要素にwidth(幅)を指定してmargin-left: auto;

# \<label\> (label要素)
[参考リンク](http://www.htmq.com/html/label.shtml)  

\<LABEL\>タグはフォームの構成部品（一行テキストボックス・チェックボックス・ラジオボタン等）と、 その項目名（ラベル）を明確に関連付けるための要素です。 これによりチェックボックスやラジオボタンでは、 関連付けられたテキスト部分をクリックしてもチェックを付けることができるようになります。  

<LABEL>タグの使用方法は2通りあります。  
1つは<LABEL>タグのfor属性の値と、フォーム部品のid属性の値を同じものにすることで両者を関連付ける方法です。もう1つは<LABEL>～</LABEL>内にフォーム部品とテキストを含める方法です。  
後者の方法は、Internet Explorer5や6には対応していないようなので、できるだけ前者を用いた方が良いでしょう。