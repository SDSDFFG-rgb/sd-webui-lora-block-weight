# LoRA Block Weight
- custom script for [AUTOMATIC1111's stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 
- When applying Lora, strength can be set block by block.

- [AUTOMATIC1111's stable-diffusion-webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) 用のスクリプトです
- Loraを適用する際、強さを階層ごとに設定できます

### Updates/更新情報
2023.03.20.2030(JST)
- Comment lines can now be added to presets
- プリセットにコメント行を追加できるようになりました
- support XYZ plot hires.fix
- XYZプロットがhires.fixに対応しました

2023.03.16.2030(JST)
- [LyCORIS](https://github.com/KohakuBlueleaf/LyCORIS)に対応しました
- Support [LyCORIS](https://github.com/KohakuBlueleaf/LyCORIS)

別途[LyCORIS Extention](https://github.com/KohakuBlueleaf/a1111-sd-webui-locon)が必要です。
For use LyCORIS, [Extension](https://github.com/KohakuBlueleaf/a1111-sd-webui-locon) for LyCORIS needed.

日本語説明は[後半](#概要)後半にあります。

# Overview
Lora is a powerful tool, but it is sometimes difficult to use and can affect areas that you do not want it to affect. This script allows you to set the weights block-by-block. Using this script, you may be able to get the image you want.

## Usage
Place lora_block_weight.py in the script folder.  
Or you can install from Extentions tab in web-ui. When installing, please restart web-ui.bat.

### Active  
Check this box to activate it.

### Prompt
In the prompt box, enter the Lora you wish to use as usual. Enter the weight or identifier by typing ":" after the strength value. The identifier can be edited in the Weights setting.  
\<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  
\<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  (LyCORIS, etc.)
\<lora:"lora name":1:IN02>  
Lora strength is in effect and applies to the entire Blocks.  
It is case-sensitive.
For LyCORIS, full-model blobks used,so you need to input 26 weights.
You can use weight for LoRA, in this case, the weight of blocks not in LoRA is set to 1.　　
If the above format is not used, the preset will treat it as a comment line.

### Weights Setting
Enter the identifier and weights.
Unlike the full model, Lora is divided into 17 blocks, including the encoder. Therefore, enter 17 values.
BASE, IN, OUT, etc. are the blocks equivalent to the full model.

|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|  
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|  
|BASE|IN01|IN02|IN04|IN05|IN07|IN08|MID|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

LyCORIS, etc.  
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|23|24|25|26|  
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN00|IN01|IN02|IN03|IN04|IN05|IN06|IN07|IN08|IN09|IN10|IN11|MID|OUT00|OUT01|OUT02|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

### Special Values (Random)
Basically, a numerical value must be entered to work correctly, but by entering `R` and `U`, a random value will be entered.  
R : Numerical value with 3 decimal places from 0~1
U : 3 decimal places from -1.5 to 1.5

For example, if ROUT:1,1,1,1,1,1,1,1,R,R,R,R,R,R,R,R,R  
Only the OUT blocks is randomized.
The randomized values will be displayed on the command prompt screen when the image is generated.

### Special Values (Dynamic)
The special value `X` may also be included to use a dynamic weight specified in the LoRA syntax. This is activated by including an additional weight value after the specified `Original Weight`.

For example, if ROUT:X,1,1,1,1,1,1,1,1,1,1,1,X,X,X,X,X and you had a prompt containing \<lora:my_lore:0.5:ROUT:0.7\>. The `X` weights in ROUT would be replaced with `0.7` at runtime.

> NOTE: If you select an `Original Weight` tag that has a dynamic weight (`X`) and you do not specify a value in the LoRA syntax, it will default to `1`.

### Save Presets

The "Save Presets" button saves the text in the current text box. It is better to use a text editor, so use the "Open TextEditor" button to open a text editor, edit the text, and reload it.  
The text box above the Weights setting is a list of currently available identifiers, useful for copying and pasting into an XY plot. 17 identifiers are required to appear in the list.

### Fun Usage
Used in conjunction with the XY plot, it is possible to examine the impact of each level of the hierarchy.  
![xy_grid-0017-4285963917](https://user-images.githubusercontent.com/122196982/215341315-493ce5f9-1d6e-4990-a38c-6937e78c6b46.jpg)

The setting values are as follows.  
NOT:0,0,0,0,0,0,0,0,0,0,0,0,0,0,0  
ALL:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1  
INS:1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0  
IND:1,0,0,0,1,1,1,1,0,0,0,0,0,0,0,0,0,0  
INALL:1,1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0  
MIDD:1,0,0,0,1,1,1,1,1,1,1,1,1,0,0,0,0,0  
OUTD:1,0,0,0,0,0,0,0,0,1,1,1,1,1,0,0,0,0  
OUTS:1,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1  
OUTALL:1,0,0,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1,1 

## XYZ Plotting Function
The optimal value can be searched by changing the value of each layer individually.
### Usage 
Check "Active" to activate the function. If Script (such as XYZ plot in Automatic1111) is enabled, it will take precedence.
Hires. fix is not supported. batch size is fixed to 1. batch count should be set to 1.  
Enter XYZ as the identifier of the LoRA that you want to change. It will work even if you do not enter a value corresponding to XYZ in the preset. If a value corresponding to XYZ is entered, that value will be used as the initial value.  
Inputting ZYX, inverted value will be automatically inputted.
This feature enables to match weights of two LoRAs.  
Inputing XYZ for LoRA1 and ZYX for LoRA2, you get,  
LoRA1 1,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0,0  
LoRA2 0,1,1,1,0,0,0,0,0,0,0,0,1,1,1,1,1    
### Axis type
#### values
Sets the weight of the hierarchy to be changed. Enter the values separated by commas. 0,0.25,0.5,0.75,1", etc.

#### Block ID
If a block ID is entered, only that block will change to the value specified by value. As with the other types, use commas to separate them. Multiple blocks can be changed at the same time by separating them with a space or hyphen. The initial NOT will invert the change, so NOT IN09-OUT02 will change all blocks except IN09-OUT02.

#### seed
Seed changes, and is intended to be specified on the Z-axis.

#### Original Weights
Specify the initial value to change the weight of each block. If Original Weight is enabled, the value entered for XYZ is ignored.

### Input example
X : value, value : 1,0.25,0.5,0.75,1  
Y : Block ID, value : BASE,IN01-IN08,IN05-OUT05,OUT03-OUT11,NOT OUT03-OUT11  
Z : Original Weights, Value : NONE,ALL0.5,ALL  

In this case, an XY plot is created corresponding to the initial values NONE,ALL0.5,ALL.
If you select Seed for Z and enter -1,-1,-1, the XY plot will be created 3 times with different seeds.

### Effective Block Analyzer
This function check which layers are working well. The effect of the block is visualized and quantified by setting the intensity of the other bocks to 1, decreasing the intensity of the block you want to examine, and taking the difference.  
#### Range
If you enter 0.5, 1, all initial values are set to 1, and only the target block is calculated as 0.5. Normally, 0.5 will make a difference, but some LoRAs may have difficulty making a difference, in which case, set 0.5 to 0 or a negative value.

#### settings
##### diff color
Specify the background color of the diff file.

##### chnage X-Y
Swaps the X and Y axes. By default, Block is assigned to the Y axis.

##### Threshold
Sets the threshold at which a change is recognized when calculating the difference. Basically, the default value is fine, but if you want to detect subtle differences in color, etc., lower the value.

#### Blocks
Enter the blocks to be examined, using the same format as for XYZ plots.

For more information on block-by-block merging, see

https://github.com/bbc-mc/sdweb-merge-block-weighted-gui

# 概要
Loraは強力なツールですが、時に扱いが難しく、影響してほしくないところにまで影響がでたりします。このスクリプトではLoraを適用する際、適用度合いをU-Netの階層ごとに設定することができます。これを使用することで求める画像に近づけることができるかもしれません。

## 使い方
scriptフォルダにlora_block_weightを置いてください。  インストール時はWeb-ui.batを再起動をしてください。

### Active  
ここにチェックを入れることで動作します。

### プロンプト
プロンプト画面では通常通り使用したいLoraを記入してください。その際、強さの値の次に「:」を入力しウェイトか識別子を入力します。識別子はWeights setting で編集します。  
\<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  
\<lora:"lora name":1:0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0>.  (LyCORISなどの場合)
\<lora:"lora名":1:IN04>
Loraの強さは有効で、階層全体にかかります。大文字と小文字は区別されます。
LyCORISに対してLoRAのプリセットも使用できますが、その場合LoRAで使われていない階層のウェイトは1に設定されます。  
上記の形式になっていない場合プリセットではコメント行として扱われます。

### Weights setting
識別子とウェイトを入力します。
フルモデルと異なり、Loraではエンコーダーを含め17のブロックに分かれています。よって、17個の数値を入力してください。
BASE,IN,OUTなどはフルモデル相当の階層です。


|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|  
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN01|IN02|IN04|IN05|IN07|IN08|MID|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

LyCORISなどの場合
|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|23|24|25|26|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|BASE|IN00|IN01|IN02|IN03|IN04|IN05|IN06|IN07|IN08|IN09|IN10|IN11|MID|OUT00|OUT01|OUT02|OUT03|OUT04|OUT05|OUT06|OUT07|OUT08|OUT09|OUT10|OUT11|

### 特別な値
基本的には数値を入れないと正しく動きませんが R および U を入力することでランダムな数値が入力されます。  
R : 0~1までの小数点3桁の数値
U : -1.5～1.5までの小数点3桁の数値

例えば　ROUT:1,1,1,1,1,1,1,1,R,R,R,R,R,R,R,R,R  とすると  
OUT層のみダンダム化されます  
ランダム化された数値は画像生成時にコマンドプロンプト画面に表示されます

saveボタンで現在のテキストボックスのテキストを保存できます。テキストエディタを使った方がいいので、open Texteditorボタンでテキストエディタ開き、編集後reloadしてください。  
Weights settingの上にあるテキストボックスは現在使用できる識別子の一覧です。XYプロットにコピペするのに便利です。17個ないと一覧に表示されません。

### 楽しい使い方
XY plotと併用することで各階層の影響を調べることが可能になります。  
![xy_grid-0017-4285963917](https://user-images.githubusercontent.com/122196982/215341315-493ce5f9-1d6e-4990-a38c-6937e78c6b46.jpg)

設定値は以下の通りです。  
NOT:0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0  
ALL:1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1  
INS:1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0  
IND:1,0,0,0,1,1,1,0,0,0,0,0,0,0,0,0,0  
INALL:1,1,1,1,1,1,1,0,0,0,0,0,0,0,0,0,0  
MIDD:1,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0,0  
OUTD:1,0,0,0,0,0,0,0,1,1,1,1,0,0,0,0,0  
OUTS:1,0,0,0,0,0,0,0,0,0,0,0,1,1,1,1,1  
OUTALL:1,0,0,0,0,0,0,0,1,1,1,1,1,1,1,1,1 

## XYZ プロット機能
各層の値を個別に変化させることで最適値を総当たりに探せます。
### 使い方 
Activeをチェックすることで動作します。 Script(Automatic1111本体のXYZプロットなど)が有効になっている場合そちらが優先されます。noneを選択してください。
Hires. fixには対応していません。Batch sizeは1に固定されます。Batch countは1に設定してください。  
変化させたいLoRAの識別子にXYZと入力します\<lora:"lora名":1:XYZ>。 プリセットにXYZに対応する値を入力していなくても動作します。その場合すべてのウェイトが0の状態からスタートします。XYZに対応する値が入力されている場合はその値が初期値になります。  
ZYXと入力するとXYZとは反対の値が入力されます。これはふたつのLoRAのウェイトを合わせる際に有効です。
例えばLoRA1にXYZ,LoRA2にZYXと入力すると、  
LoRA1 1,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0,0  
LoRA2 0,1,1,1,0,0,0,0,0,0,0,0,1,1,1,1,1    
となります。
### 軸タイプ
#### values
変化させる階層のウェイトを設定します。カンマ区切りで入力してください。「0,0.25,0.5,0.75,1」など。

#### Block ID
ブロックIDを入力すると、そのブロックのみvalueで指定した値に変わります。他のタイプと同様にカンマで区切ります。スペースまたはハイフンで区切ることで複数のブロックを同時に変化させることもできます。最初にNOTをつけることで変化対象が反転します。NOT IN09-OUT02とすると、IN09-OUT02以外が変化します。NOTは最初に入力しないと効果がありません。IN08-M00-OUT03は繋がっています。

#### Seed
シードが変わります。Z軸に指定することを想定しています。

#### Original Weights
各ブロックのウェイトを変化させる初期値を指定します。プリセットに登録されている識別子を入力してください。Original Weightが有効になっている場合XYZに入力された値は無視されます。

### 入力例
X : value, 値 : 1,0.25,0.5,0.75,1  
Y : Block ID, 値 : BASE,IN01-IN08,IN05-OUT05,OUT03-OUT11,NOT OUT03-OUT11  
Z : Original Weights, 値 : NONE,ALL0.5,ALL  

この場合、初期値NONE,ALL0.5,ALLに対応したXY plotが作製されます。
ZにSeedを選び、-1,-1,-1を入力すると、異なるseedでXY plotを3回作製します。

### Effective Block Analyzer
どの階層が良く効いているかを判別する機能です。対象の階層以外の強度を1にして、調べたい階層の強度を下げ、差分を取ることで階層の効果を可視化・数値化します。  
#### Range
0.5, 1　と入力した場合、初期値がすべて1になり、対象のブロックのみ0.5として計算が行われます。普通は0.5で差がでますが、LoRAによっては差が出にくい場合があるので、その場合は0.5を0あるいはマイナスの値に設定してください。

#### 設定
##### diff color
差分ファイルの背景カラーを指定します。

##### chnage X-Y
X軸とY軸を入れ替えます。デフォルトではY軸にBlockが割り当てられています。

##### Threshold
差分を計算する際の変化したと認識される閾値を設定します。基本的にはデフォルト値で問題ありませんが、微妙な色の違いなどを検出したい場合は値を下げて下さい。

#### Blocks
調べたい階層を入力します。XYZプロットと同じ書式が使用可能です。

階層別マージについては下記を参照してください

https://github.com/bbc-mc/sdweb-merge-block-weighted-gui

### updates/更新情報
2023.02.07 1250(JST)
- Changed behavior when XYZ plot Active (Script of the main UI is prioritized).

2023.02.06 2000(JST)
- Feature added: XYZ plotting is added.

2023.01.31 0200(JST)
- Feature added: Random feature is added
- Fixed: Weighting now works for negative values.

2023.02.16 2040(JST)
- Original Weight をxやyに設定できない問題を解決しました
- Effective Weight Analyzer選択時にXYZのXやYがValuesとBlockIdになっていないとエラーになる問題を解決しました

2023.02.08 2120(JST)
- 階層適応した後通常使用する際、階層適応が残る問題を解決しました
- 効果のある階層をワンクリックで判別する機能を追加しました

2023.02.08 0050(JST)
- 一部環境でseedが固定されない問題を解決しました

2023.02.07 2015(JST)
- マイナスのウェイトが正常に働かない問題を修正しました

2023.02.07 1250(JST)
- XYZプロットActive時の動作を変更しました(本体のScriptが優先されるようになります)

2023.02.06 2000(JST)
- 機能追加：XYZプロット機能を追加しました

2023.01.31 0200(JST)
- 機能追加：ランダム機能を追加しました
- 機能修正：ウェイトがマイナスにも効くようになりました
