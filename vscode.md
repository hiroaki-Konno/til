# VSCode.md
  
## vscodeで仮想環境を読み込みたい

[ctrl] [shift] [p]でコマンドパレット、python:select interpreterから仮想環境を選択
> If you are using VSCode, Ctrl + Shift + P -> Type and select 'Python: Select Interpreter' and enter into your projects virtual environment. This is what worked for me.   

[参考サイト](https://stackoverflow.com/questions/65369567/import-rest-framework-could-not-be-resolved-but-i-have-installed-djangorestfr)    

<!-- snippet で変換可能 -->
## vscode ユーザースニペットの利用(主に.md)
### 設定手順
VsCodeでmarkdownのスニペットを有効にする  
ファイル → 基本設定 → 設定 → 拡張機能 → settings.json で編集 を選択し、以下を追記  

```json
/* settings.json */
 "[markdown]": {
    "editor.wordWrap": "on",
    "editor.quickSuggestions": true,
    "editor.snippetSuggestions": "top"
 },
```

エラー(黄色い波線)が出るが動くっぽい  
object返せみたいなこと言ってるけどよくわからん  
### マークダウン用のスニペット設定を作成する
ファイル → 基本設定 → ユーザースニペット → Markdown.json を選択。

```json
/* Markdown.json */
"スニペット名": { 
  "prefix": "キーになる文字列", 
  "body":[ "呼び出される文字列" ], 
  "description": "このスニペットの説明文" }
```
上記参考にスニペットを追加していく、例としては

```json
"snippet":{
		"prefix": "snippet",
		"body": [
			"<!-- snippet で変換可能 -->",
			"## title_snippet",
			"<!-- heading で各サイズのhタグ変換可能 -->",
			"",
			"txt_txt_txt",
			"> 適当な引用   ",
			"",
			"```python",
			"# fenced_code_block",
			"```",
			"",
			"<!-- link で変換可能 -->",
			"[参考サイト](https://)    ",
		],
		"description": "基本単位"
	},
```
いっちゃん基本要素のやつ

<!-- link で変換可能 -->
[参考サイト](https://maasaablog.com/tools/visual-studio-code/1762/#toc12)  
  
[参考サイト？](https://qiita.com/12345/items/97ba616d530b4f692c97)

<!-- snippet で変換可能 -->
## title_snippet
<!-- heading で各サイズのhタグ変換可能 -->

txt_txt_txt
> 適当な引用   

```python
# fenced_code_block
```

<!-- link で変換可能 -->
[参考サイト](https://)     
