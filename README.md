------------------------------------------------------------------------------
HSP : Hot Soup Processor  
ホットスーププロセッサ  
copyright 1997-2020 (c) onion software/onitama  
------------------------------------------------------------------------------

# はじめに

このフォルダには、OpenHSP/Hot Soup Processorビルド用のファイルが含まれています。
HSP3の機能やSDKを検証することが可能です。
βなどテスト用ブランチに含まれる内容は、未実装の機能や、不具合が含まれていることをご了承の上お使い下さい。


# 動作環境

LinuxのGUI環境(X Window System)で動作します。
一部の機能は、OpenGL及びSDLライブラリを使用して動作します。


# Linuxインストール

githubから最新のリポジトリを取得してご使用ください。

	git clone https://github.com/onitama/OpenHSP

アーカイブにはソースのみが含まれていますので、makeによってコンパイルする必要があります。
コンパイルには、gcc及びmakeを実行できる環境が必要です。
コンパイルの際には、以下のライブラリが必要になりますので、あらかじめ確認を行なって下さい。

	OpenGLES2.0以降 / EGL
	SDL2
	gtk+-2

Debian(Ubuntu)であれば、取得したリポジトリに移動して、

	./setup.sh

を実行することで、ライブラリのインストール及びHSPのコンパイルを行います。
手動でライブラリを準備する場合は、パッケージマネージャから以下のようにライブラリをインストールできます。

	sudo apt update
	sudo apt install -y libgtk2.0-dev
	sudo apt install -y libglew-dev
	sudo apt install -y libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev
	sudo apt install -y libgles2-mesa-dev libegl1-mesa-dev

(Linuxのバージョンやディストリビューションによって正しくコンパイルされない場合は、修正が必要になります。)

	make

アーカイブの内容が展開されたディレクトリでmakeコマンドを実行してください。
必要なツールのコンパイルが行なわれ、HSP3が使用できる状態になります。


# Raspberry Piインストール

Raspberry Pi上のRaspbian上で動作します。(推奨バージョンは、September 2017 Kernel version4.9以降です)
hsp3dish及び、hsed(スクリプトエディタ)は、GUI環境でのみ動作します。
(描画に関する機能は、OpenGL及びSDLライブラリを使用して動作しています。)

githubから最新版を取得してください。

	git clone https://github.com/onitama/OpenHSP

Raspberry Piインストール用にpisetup.shスクリプトを同梱しています。
home/pi/OpenHSPに最新版を取得されている状態で、

	cd OpenHSP
	./pisetup.sh

を実行することで、HSPのコンパイル及びメニューへの登録を行います。
makeによってコンパイルを行う場合は、以下のように指定してください。

	make -f makefile.raspbian
		
Raspberry Pi版では、フルスクリーンで実行を行ないます。
実行の中断は、[ctrl]+[C]か[esc]キーを押してください。
キーボードだ正しく認識されていない場合など、中断ができなくなることがありますので注意してください。
GUIエディタだけでなく、コマンドラインから「./hsp3dish ****.ax」の形で実行を行なうことも可能です。

	※ Raspberry Pi4の場合は、Linuxと同様の方法でmakeによるインストールを行ってください
	※ Raspberry Pi4ではフルスクリーンによる実行はサポートされません


# macOSインストール (※macOS CatalinaとXcode 12.3で確認しました)

githubから最新のリポジトリを取得してご使用ください。

	git clone https://github.com/sntulix/OpenHSP -b macos_openhsp-experimental_from_hsp3dish_emscripten

アーカイブにはソースのみが含まれていますので、makeによってコンパイルする必要があります。
コンパイルには、gcc及びmakeなどXcode コマンドライン ツールを実行できる環境が必要です。

コンパイルの際には、以下のライブラリが必要になりますので、あらかじめ確認を行なって下さい。

	brew install sdl sdl_image sdl_mixer sdl_ttf sdl2 sdl2_image sdl2_mixer sdl2_ttf

アーカイブの内容が展開されたディレクトリでmakeコマンドを実行してください。

	make

必要なツールのコンパイルが行なわれ、HSP3が使用できる状態になります。※今はhspcmpのみ動きます。


# 使用方法

HSP3は、オープンソースとして公開されているOpenHSP技術をベースに、
Linux上で手軽にプログラミングを楽しむことができるよう構成されています。
インストールを行なうと、以下のコマンドが生成されます。

	hsed		スクリプトエディタ(簡易版)
	hspcmp		HSP3コードコンパイラ
	hsp3cl		HSP3コマンドラインランタイム
	hsp3dish	HSP3Dishランタイム
	hsp3gp		HGIMG4ランタイム

スクリプトエディタ(簡易版)は、HSP3のスクリプトを記述して、実行することのできる
GUIアプリケーションです。

	./hsed

上記のプログラムを起動することで、スクリプトエディタ(簡易版)がGUIで動作します。
HSP3のスクリプトを記述して、実行することのできる簡易的なエディタです。
基本的なスクリプトの編集、及びロード・セーブ機能を持っています。
[F5]キー、または「HSP」メニューから「Run」を選択することで編集している
スクリプトを実行できます。
現在のバージョンでは、ランタイムとしてhsp3dish,hsp3cl,hgimg4(hsp3gp)が用意されています。
サンプルコードがsampleフォルダに含まれていますので、お試しください。
スクリプトの文字コードはUTF-8として扱われます。Windowsが使用する文字コード(SJIS)とは異なりますので注意してください。

コマンドラインからスクリプトの実行を行なう場合は、hspcmpにより
HSPオブジェクトファイルを作成する必要があります。

	./hspcmp -d -i -u test.hsp

上の例では、「test.hsp」ファイルからオブジェクトファイル「test.ax」を生成します。
生成されたオブジェクトファイルを、ランタイムに渡して実行を行ないます。

	./hsp3cl test.ax

上の例では、「test.ax」をHSP3コマンドラインランタイムで実行します。
同様に、「hsp3dish」「hsp3gp」などのランタイムに合わせたスクリプトを
実行させることができます。
(「hsp3dish」「hsp3gp」の実行は、GUI環境が必要になります。)


# exec、devprm命令について

Linux版、Raspberry Pi版ともにexec命令により、シェルのコマンドを呼び出すことができます。
また、devprm命令によりファイルシステム上のデバイスに文字列を出力することが可能です。

	devprm "/sys/class/gpio/export", "2"

のように記述した場合は、「/sys/class/gpio/export」に「2」が出力されます。
シェルから「echo 2 > /sys/class/gpio/export」を実行するのと同じ動作になります。


# Raspberry PiのGPIO入出力

Raspberry Pi版では、devcontrol(gpio)命令によりGPIO入出力を行なう拡張が行われています。
GPIO出力を制御する場合は、以下のように記述します。

	devcontrol "gpio", ポート番号, 出力値
	( 「gpio ポート番号, 出力値」と記述することも可能 )

ポート番号は、GPIOのポートを数値で指定します。
出力値は、1(ON)か0(OFF)を数値で指定することで、デジタルポートの出力を制御します。
入力を行う場合は、以下のように記述します。

	devcontrol "gpioin", ポート番号
	( 関数として「gpioin(ポート番号)」と記述することも可能 )

命令の実行後にシステム変数statに0か1が代入されます。
(エラーが発生した場合は、マイナス値が代入されます)
GPIO入出力は、hsp3dishだけでなくhsp3clからも使用することが可能です。


# Raspberry PiのI2C入出力

Raspberry Pi版では、devcontrol(i2c～)命令によりI2Cデバイスとの入出力を行なう拡張が行われています。
I2C制御を行う場合は、RaspberryPiのオプション設定でI2CをEnableにする必要があります。(I2C制御を行なわない場合は、設定の必要ありません)
I2C制御を行う場合は、以下のように記述します。

	devcontrol "i2copen", スレーブアドレス, チャンネル
	( 「i2copen スレーブアドレス, チャンネル」と記述することも可能 )

	「チャンネル」は複数のデバイスを同時に指定する場合に識別を行う番号です。(0～31の整数値)
	単一のデバイスを制御する場合は省略(0)して構いません。
	i2copenにより指定されたアドレスのデバイスを制御できる状態になります。
	結果がシステム変数statに返ります。0の場合は成功、それ以外はエラーになります。

データを出力する場合は、以下のように記述します。

	devcontrol "i2cwrite", データ値, データサイズ, チャンネル
	( 「i2cwrite データ値, データサイズ, チャンネル」と記述することも可能 )

	データ値は、HSPが扱う32bitの整数値になります。
	データサイズが0または省略した場合は、8bit(1byte)のみ出力されます。
	データサイズは、1,2,3,4が指定できます。それぞれ、1～4byteのサイズが出力されます。

データを入力する場合は、以下のように記述します。

	devcontrol "i2cread", チャンネル
	( 関数として「i2cread(チャンネル)」と記述することも可能 )

	devcontrol "i2creadw", チャンネル
	( 関数として「i2creadw(チャンネル)」と記述することも可能 )

命令の実行後にシステム変数statに結果が代入されます。
i2creadは、8bit(1byte)の値、i2creadwは、16bit(2byte)の値を取得します。
(エラーが発生した場合は、マイナス値が代入されます)
I2C制御は、hsp3dishだけでなくhsp3clからも使用することが可能です。


# オンラインマニュアル

HSP3.6に関する情報はオンラインマニュアルでご覧いただけます。
https://www.onionsoft.net/hsp/v36/

HSPについての最新情報やコミュニティは、HSPTV!サイトにて提供されています。
https://hsp.tv/


# 将来の予定

HSP3標準ランタイム、及びヘルプリファレンスや周辺ツールなどHSP3で用意されている
機能なども、今後追加される予定です。
まだ不備も多く、必要なものが足りない状況ですが、今後整備したいと考えています。
パッケージの不備やアドバイスなどありましたら、お知らせ頂ければ幸いです。

	onion software (hsp@onionsoft.net)
	https://www.onionsoft.net/

頂いたメールには、すべて目を通しておりますが、返信や、要望の反映などについては
作者がすぐに対応できないこともありますので、予めご了承下さい。
HSPについての一般的な質問や、スクリプトの作り方に関するご質問は、
ネット上のFAQや、書籍などでも情報を提供していますので、まず調べてみることを
お勧め致します。


# 謝辞

HSPスクリプトエディタ(hsed)は、K-K(瓶詰堂)さんのご協力により作成されています。  TTFフォントデータとしてIPAフォントを同梱させて頂きました。
(ライセンスについては、IPAフォントライセンスv1.0を参照してください。)

HSP3及びOpenHSPに多大なご協力を頂いた以下の皆様に感謝致します。  
  
Senchaさん、ゆめゆめゆうかさん、Lonely Wolfさん、Shark++さん、
HyperPageProjectさん、ちょくとさん、S.Programsさん、zakkiさん、
山田 雄己(Yuki)さん、K-Kさん、USKさん、NGND001さん、yoshisさん、
nakaさん、JETさん、ellerさん、さくらさん、うすあじさん、悠黒喧史さん、tomさん、
ぷまさん、arueさん、mjhd_otsukaさん、tds12さん、fujidigさん、naznyarkさん  
  
その他、HSP周辺ツール開発者ML(HSPDev-ML)の皆様及び、β版のテスト報告や、
ご意見をお寄せいただいた多くの方々に感謝致します。  


# ライセンス

HSP3 Linuxは、OpenHSPの派生物として取り扱い、ライセンスもOpenHSP/HSP3に準拠した修正BSDライセンスとなります。  

-------------------------------------------------------------------------------
Hot Soup Processor (HSP) / OpenHSP
Copyright (c) 1997-2020, onion software/onitama
in collaboration with Sencha, Yume-Yume Yuuka, Y-JINN, chobin,
Usuaji, Kenji Yuukoku, puma, tom, sakura, fujidig, zakki, naznyark,
Lonely Wolf, Shark++, HyperPageProject, Chokuto, S.Programs, 
Yuki, K-K, USK, NGND001, yoshis, naka, JET, eller, arue, mjhd_otsuka, tds12
All rights reserved.

These softwares are provided by the copyright holders and contributors "as is" and
any express or implied warranties, including, but not limited to, the implied
warranties of merchantability and fitness for a particular purpose are disclaimed.
-------------------------------------------------------------------------------
                                                HSP users manual / end of file 
-------------------------------------------------------------------------------
