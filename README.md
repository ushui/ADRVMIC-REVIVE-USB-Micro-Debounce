#### 最新バージョン
ファームウェア: 001d、001id  
設定ツール: 1.1.0(iOS版は2.0.0)  
最終更新日: 2020/11/15  
最終更新内容: 初版リリース  

#  REVIVE USB Micro チャタリング対策版
本プログラムは、株式会社ビット・トレード・ワンが提供するREVIVE USB MicroのファームウェアおよびWindowsアプリケーション「REVIVE USB Micro Configuration Tool」を、[Assembly Desk License](https://raw.githubusercontent.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/master/LICENSE.md)ライセンスに基づいて改変したプログラムです。  
ここでは追加・変更した機能について記載します。基本的な使用方法、プログラムの仕様は改変元プログラムと変わりませんので、概要については[公式リポジトリ](https://github.com/bit-trade-one/ADRVMIC-REVIVE-USB-Micro)をご覧ください。  

## 特徴
REVIVE USB Micro チャタリング対策版のファームウェア(以下、対策版ファームウェア)は、REVIVE USB Microのファームウェアを公式マニュアルに沿って書き換えることで使用できるマイコン用プログラムです。  
REVIVE USB Micro Debounce, Configuration Tool(以下、対策版設定ツール)は、対策版ファームウェアで使用できるWindowsアプリケーションです。  
公式ファームウェア/公式設定ツールに対し、以下の機能を実装しています。  

### チャタリング防止機能
公式ファームウェアでは対応していない、チャタリング防止やノイズ除去を実装した機能です。  
一度しかボタンを押していないのにダブルクリックになる、キーが複数回入力されるなどのケースを防ぐことができます。  
また、遅延を抑えたい場合やそれでもチャタリングが起きる場合に、対策版設定ツールにはチャタリング防止機能のしきい値を変更できる機能を備えています。  

### 遅延秒数の計算機能
チャタリング防止機能の設定により、入力遅延が発生する秒数が変わります。  
そこで、現在の設定では平均でどの程度遅延が発生するか、自動で計算する機能を備えています。  

### ロータリーエンコーダ設定機能
公式ファームウェアでは使用できない、ロータリーエンコーダが使用できる機能です。  
1P～12PのピンにロータリーエンコーダのA相/B相を接続して、対策版設定ツールで接続したピンに設定をすると、ロータリーエンコーダの回転による入力が行えるようになります。  
最大6つまで接続可能です。  

## 対策版ファームウェアの適用手順
1. ファームウェア書き込みソフトのダウンロード  
[ファームウェア書き込みソフト「HIDBootLoader.exe」](https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/raw/master/Firmware/HIDBootLoader.exe)をダウンロードしてください。  

2. 対策版ファームウェア(.hex)、対策版設定ツール(.exe)のダウンロード  
このリポジトリから以下のいずれかをダウンロードし、展開してください。  
 - [REVIVE_USB_Micro_Debounce-latest.zip(通常版)](https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/raw/master/REVIVE_USB_Micro_Debounce-latest.zip)
 - ~~REVIVE_USB_Micro_Debounce_Matrix-latest.zip(マトリックス版)~~
 - iOS
   - [REVIVE_USB_Micro_Debounce_for_iOS-latest.zip(iOS対応・通常版)](https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/raw/master/REVIVE_USB_Micro_Debounce_for_iOS-latest.zip)
   - ~~REVIVE_USB_Micro_Debounce_Matrix_for_iOS-latest.zip(iOS対応・マトリックス版)~~

3. ファームウェアの書き換え
Windows PCにREVIVE USB Microを接続し、以下の手順に沿ってファームウェアを書き換えてください（公式ドキュメントです）。 hexファイルは展開したものを指定します。  
https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/blob/master/Firmware/Readme.md  

4. 対策版設定ツールの起動  
展開したexeファイルを起動してください（_enが付いたものは英語版の設定ツールです）。  
右下の「FW Version: 」のバージョン文字列の末尾がdもしくはidならば対策版ファームウェアが正しく適用されています。

## 対策版設定ツールの追加変更箇所
![REVIVE USB Micro Debounce, Configuration Tool](https://raw.githubusercontent.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/master/ADRVMIC-REVIVE-USB-Micro-Debounce_ct.png "REVIVE USB Micro Debounce, Configuration Toolのデジタル設定画面")
#### チャタリング防止設定 - サンプリング周期
指定した時間の一定間隔をおいて入力値の受信・送信を行います。  
例えば4msに設定した場合は、REVIVE USB Microが4msごとに受信した入力値を反映し、USBで接続した機器に送信します。  
ON/OFFが確定するまでの不安定な信号の揺れを安定化する効果があります。  
1～174msまで設定可能で、デフォルト値は4ms、推奨値は1～10msです。  

#### チャタリング防止設定 - 一致検出回数
ON/OFFのどちらかを採る入力判定が、指定した回数分、同じだったときに入力値を反映します。  
例えば2回に設定した場合は、REVIVE USB Microが2回連続でONとして判定したとき、入力値をUSBで接続した機器に送信します。  
ON/OFFが確定している間の不安定な信号の揺れを無効化する効果があります。  
1～15回まで設定可能で、デフォルト値は2回、推奨値は1～3回です。  

#### ロータリーエンコーダ設定
「1P/2P」から「11P/12P」までがペアになったチェックボックスのうち、チェックを入れたピンをロータリーエンコーダのA相/B相として扱います。  
チェックを入れなければ通常のデジタル入力として扱います。  

#### チャタリング防止設定 - 一致検出回数(ロータリーエンコーダ)
「ロータリーエンコーダ設定」に設定したピンは、この設定値を使用します。  
1～255回まで設定可能で、デフォルト値は1回、推奨値は1～2回です。  

### 平均遅延秒数
チャタリング防止設定で設定したサンプリング周期、一致検出回数から自動で遅延する秒数を計算します。  
ゲーム用途などで操作デバイスの遅延秒数を知りたい場合は、この平均遅延秒数を遅延する秒数として扱ってください。  
設定ツールで使用している計算式は`サンプリング周期 * 一致検出回数 - (サンプリング周期 / 2)`です。  

　*※1 ロータリーエンコーダは、一致検出回数が一致検出回数(ロータリーエンコーダ)になります。*  
　*※2 以下に示すツール上で算出できない遅延は計算に含めていません。*  
　　　*・ディスプレイの遅延*  
　　　*・一致検出回数を基準とした処理における一致しなかった場合の遅延*  
　*※3 誤差は`±サンプリング周期 / 2`です。*  

## その他ドキュメントについて
 - [FAQ.md](https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/blob/master/FAQ.md)
   - 公式FAQに対策版のFAQを追加しました。
 - [公式の設定ツールのReadme.md](https://github.com/ushui/ADRVMIC-REVIVE-USB-Micro-Debounce/blob/master/App/Readme.md)
   - 公式ドキュメントです。対策版とUIが異なりますが、基本的な使用方法は変わりません。 

#### 開発者向け備考
オープンソースですので念のため。ファームウェアの改変について、注意事項があります。  
対策版ファームウェアはREVIVE USB Micro(PIC18F14K50)のROM（プログラムメモリおよびデータメモリ）の限界近くまで実装しているため、ソースコードを改変した上でコンパイルすると「Error - section 'xxxx' can not fit the section. 」のエラーが発生し、コンパイルできない事象が発生することがあります。  
何らかの機能追加等の改変を行うならば、不必要な機能を削ることをお勧めします。  
それが許容できなければ、以下の工夫を行ってください。  
 - バイトコードを減らすためにコードを最適化する
 - #pragmaディレクティブによってプログラムメモリ、データメモリの格納場所が変わるため、変数やコードの位置を変更する
 - メモリアドレスが記載されたmapファイルを書き換える
 - 有償版のコンパイラ（個人が買える金額ではなかったと思います）を使用して最適化を行った上でコンパイルする  

対策版ファームウェアは1つ目と2つ目を実施して今のコードに落ち着きました。もしかすると3つ目を実施すれば良かったのかもしれませんが、PICやコンパイラ仕様の熟知が必要であり、正しい理解がなくては取り扱うことができません。  
いずれの場合も[MPLAB® C18C コンパイラーユーザーズガイド](http://ww1.microchip.com/downloads/jp/devicedoc/51288c_jp.pdf)に一度目を通しておくことをおすすめします。  

----

開発環境(ファームウェア)： MPLAB IDE v8.92、MPLAB C for PIC18 v3.47 Standard-Eval Version  
開発環境(設定ツール): Microsoft Visual C# 2010 Express  
作成者： ushui（ゆーしゅい）  
Twitter: [@kaede_hrc](https://twitter.com/kaede_hrc)  
