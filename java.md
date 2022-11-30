# Java 目次
<details>
<summary>目次</summary>

- [Java 目次](#java-目次)
- [データ型](#データ型)
    - [整数](#整数)
    - [浮動小数](#浮動小数)
    - [真偽値](#真偽値)
    - [文字 文字列](#文字-文字列)
- [配列](#配列)
    - [初期化も同時に](#初期化も同時に)
    - [代入もすると](#代入もすると)
    - [for文で配列の要素を取り出す](#for文で配列の要素を取り出す)
- [標準入出力](#標準入出力)
    - [標準出力](#標準出力)
    - [標準入力](#標準入力)
- [エラー処理](#エラー処理)
    - [try catch finally](#try-catch-finally)
- [修飾子](#修飾子)
  - [アクセス修飾子](#アクセス修飾子)
  - [static修飾子](#static修飾子)
  - [final修飾子](#final修飾子)
  - [abstract修飾子](#abstract修飾子)

</details>

# データ型
### 整数
byte  
short  
int  
long  

### 浮動小数
float 数字語尾にFつける  
double

### 真偽値
boolean true falseいずれか

### 文字 文字列
char 文字  
String 文字列  
- 文字の配列とは区別される  

# 配列
`Type[] varialbe; //一般的`

ex) `int[] num_array;`


### 初期化も同時に
`int[] num_array = new int[3];`

- 型と配列の長さを指定する
- 初期値： 数値やcharなら0, booleanならfalse, それ以外はnull

### 代入もすると
`int[] num_array = new int[] {0,1,2,3};`


- array.lengthで配列の長さを参照

### for文で配列の要素を取り出す
```
for(int x : num_array){
    //xに要素が入る
}
```

# 標準入出力
### 標準出力
System.out.println  
System.out.printf(format,arg1,arg2...)

### 標準入力
Scannerクラスを使う
```
Scanner myScanner = new Scanner(System.in);
String str = myScanner.nextline(); //strに次の行が代入される
```
--------
# エラー処理
### try catch finally
```
try{

} catch(exception e) { //コンパイルエラーで出るクラスをキャッチ

} finally {

}
```

# 修飾子

## アクセス修飾子
public  
protected  
private  


## static修飾子
クラスに属するものにする(つけないとインスタンスに属するものになる)


## final修飾子
1. フィールドや変数 → 変更できない
>初期化後にほかの値を代入することができなくなる
>一般に定数の宣言に用いられる
1. クラス → 継承できない
2. メソッド → オーバーライドできない

## abstract修飾子