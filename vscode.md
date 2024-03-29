# VSCode 目次
<details>
<summary>目次</summary>

- [VSCode 目次](#vscode-目次)
- [vscode ショートカットキーの編集](#vscode-ショートカットキーの編集)
	- [開いているファイルタブの移動](#開いているファイルタブの移動)
- [vscodeで仮想環境を読み込みたい](#vscodeで仮想環境を読み込みたい)
- [vscode ユーザースニペットの利用(主に.md)](#vscode-ユーザースニペットの利用主にmd)
	- [設定手順](#設定手順)
	- [マークダウン用のスニペット設定を作成する](#マークダウン用のスニペット設定を作成する)
- [Markdown 予測変換](#markdown-予測変換)
			- [headingN (Nは任意の数字)](#headingn-nは任意の数字)

</details>

# vscode ショートカットキーの編集
## 開いているファイルタブの移動
```json
[
  {
    "key": "ctrl+shift+tab",
    "command": "workbench.action.previousEditor"
  },
  {
    "key": "ctrl+tab",
    "command": "workbench.action.nextEditor"
  },
]
```
を追加  
> Ctrl+PgUp/PgDownに割り当てられてるショートカットをCtrl+Tab/Ctrl+Shift+TabにすることでChromeライクにタブを切り替えられるようになってオススメです。


<!-- link で変換可能 -->
[参考qiita](https://qiita.com/TakahiRoyte/items/cdab6fca64da386a690b#%E6%A4%9C%E7%B4%A2%E3%81%A8%E7%BD%AE%E6%8F%9B-search-and-replace)  

# vscodeで仮想環境を読み込みたい

[ctrl] [shift] [p]でコマンドパレット、python:select interpreterから仮想環境を選択
> If you are using VSCode, Ctrl + Shift + P -> Type and select 'Python: Select Interpreter' and enter into your projects virtual environment. This is what worked for me.   

[参考サイト](https://stackoverflow.com/questions/65369567/import-rest-framework-could-not-be-resolved-but-i-have-installed-djangorestfr)    

<!-- snippet で変換可能 -->
# vscode ユーザースニペットの利用(主に.md)
## 設定手順
VsCodeでmarkdownのスニペットを有効にする  
ファイル → 基本設定 → 設定 → 拡張機能 → settings.json で編集 を選択し、以下を追記  

```json: settings.json
 "[markdown]": {
    "editor.wordWrap": "on",
    "editor.quickSuggestions": true,
    "editor.snippetSuggestions": "top"
 },
```
エラー(黄色い波線)が出るが動くっぽい  
object返せみたいなこと言ってるけどよくわからん  
たぶんこのエラー `incorrect type.Expected "odject"`

## マークダウン用のスニペット設定を作成する
ファイル → 基本設定 → ユーザースニペット → Markdown.json を選択。

```json:Markdown.json
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

# Markdown 予測変換
markdownの構文みたいなのまとめ

**bold**  
`code`

```python:test.py
fenced_codeblock
```

$$
fenced math
$$

#### headingN (Nは任意の数字)
image ![image](https://)  

$inline_math$  

*italic*  

link [link](https://)  

1. ordered_list
2. _
3. _

- unorderd_list
- _
- _

> quote  
> _

----------
horizontal rule

strikethrough ~~strikethrough~~  

| table | TH_xxxx | TH_xxxx |
| --- | --- | --- |
| TD | TD | TD |
| TD | TD | TD |