# Git 目次
<details>
<summary>目次</summary>

- [Git 目次](#git-目次)
- [Git コミットメッセージ 書き方](#git-コミットメッセージ-書き方)
    - [コミット種別](#コミット種別)
      - [通常版](#通常版)
      - [ライト版](#ライト版)
- [gitのaliasを登録する(短縮形設定できるやつ)](#gitのaliasを登録する短縮形設定できるやつ)
  - [aliasを設定する前に](#aliasを設定する前に)
    - [gitコマンドを使ってエイリアス設定(こっち使った)](#gitコマンドを使ってエイリアス設定こっち使った)
    - [設定ファイルに直接記述](#設定ファイルに直接記述)
  - [実際に登録したalias](#実際に登録したalias)
      - [参考](#参考)
- [ワーキングツリー、インデックス、HEADを使いこなす](#ワーキングツリーインデックスheadを使いこなす)
- [Git コマンドを実行するとMore?と表示される](#git-コマンドを実行するとmoreと表示される)
    - [原因](#原因)
    - [回避方法](#回避方法)
- [Gitでブランチの派生元を間違えたときの解決方法（rebase --onto、cherry-pick）](#gitでブランチの派生元を間違えたときの解決方法rebase---ontocherry-pick)
  - [1. git rebase --onto](#1-git-rebase---onto)
  - [2. git cherry-pick](#2-git-cherry-pick)

</details>

# Git コミットメッセージ 書き方
[参考サイト](https://qiita.com/itosho/items/9565c6ad2ffc24c09364)  

コミット種別と要約を書きます。フォーマットは以下とします。  

 [コミット種別]要約  

例 `[fix]削除フラグが更新されない不具合の修正`

### コミット種別
以下の中から適切な種別を選びます。
（多すぎても悩むのでこれぐらいで）

#### 通常版
- fix：バグ修正  
- hotfix：クリティカルなバグ修正
- add：新規（ファイル）機能追加
- update：機能修正（バグではない）
- change：仕様変更
- clean：整理（リファクタリング等）
- disable：無効化（コメントアウト等）
- remove：削除（ファイル）
- upgrade：バージョンアップ
- revert：変更取り消し  
（2014年12月17日追記）  
コミットが種別が多すぎて、運用が大変そうというコメントがありましたので、ライト版を考えてみました。

#### ライト版
- fix：バグ修正
- add：新規（ファイル）機能追加
- update：機能修正（バグではない）
- remove：削除（ファイル）

# gitのaliasを登録する(短縮形設定できるやつ)
## aliasを設定する前に
gitコマンドのaliasの設定の仕方は
- gitコマンドを使う
- ~/.gitconfig等、設定ファイルに直接記述  

の2通り、記事の主旨と逸れるので簡単に。

### gitコマンドを使ってエイリアス設定(こっち使った) 
```
# コマンドにスペースが入らない場合はそのまま記載
$ git config --global alias.ch checkout

# コマンドでスペースを入れる場合は "" で囲む
$ git config --global alias.chb "checkout -b"
```
--globalの部分は--system・--local・--worktreeがあるのでお好みで。

### 設定ファイルに直接記述
vim ~/.gitconfig等で設定。  
設定ファイルいじるのに慣れている人はこちらが速いと思います。
```console
~/.gitconfig
[color]
      ui = auto
・
・
[alias]
    ch = checkout
    chb = checkout -b
```

## 実際に登録したalias
```console
<!--
git <短縮形> -> git <本来のやつ>
git config --global alias.<短縮系> <本来のやつ> で登録した
-->
git ch -> git checkout
git br -> git branch
git st -> git status
git sh -> git stash
git la -> git log --oneline --decorate --graph --branches --tags --remotes --all
```
#### 参考
[Git大好き人間のおすすめGit alias設定 | tech-broccoli.life](https://tech-broccoli.life/articles/engineer/recommend-git-aliases/)  

# ワーキングツリー、インデックス、HEADを使いこなす
[参考サイト](https://qiita.com/shuntaro_tamura/items/db1aef9cf9d78db50ffe)  

[git reset (--hard/--soft)]


# Git コマンドを実行するとMore?と表示される
[参考サイト](https://pasomaki.com/git-windows-commandprompt/)

```
c:¥gitwork>git diff HEAD HEAD^
More?
```
### 原因
- Windowsコマンドプロンプトで実行している
- コマンド文の中に「^」（キャレット）が含まれている

Windowsコマンドプロンプトでは「^」（キャレット）は エスケープ記号として使われる特別な意味を持った記号なので、このような変な結果になる

### 回避方法
キャレットを含む文字列をダブルクォーテーション（"）で括る
```
c:¥gitwork>git diff HEAD "HEAD^"
```


<!-- snippet で変換可能 -->
# Gitでブランチの派生元を間違えたときの解決方法（rebase --onto、cherry-pick）
<!-- heading で各サイズのhタグ変換可能 -->

##  1. git rebase --onto  
作業ブランチの派生元を修正する  
```
git rebase --onto {本来の親ブランチ} {間違えて親にしたブランチ} {親変更するブランチ名}
``` 

## 2. git cherry-pick
新しいブランチを作り直す,新しい作業ブランチを正しく作成、欲しいコミットだけを適用  
``` 
git cherry-pick {コミットID}
``` 

<!-- link で変換可能 -->
[参考サイト](https://www.granfairs.com/blog/staff/git-mistake-parent-branch)  