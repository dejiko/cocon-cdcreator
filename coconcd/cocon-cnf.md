# cocon.cnf

## 概要
cocon.cnf は、 opencocon において、カスタム設定を提供するファイルです。通常 opencocon は"Read-only アーキテクチャ"を採用しており、設定やデータの恒久的な保存をする手段を提供していないため、その代わりとなるものです。
あらかじめ cocon.cnf を用意し、opencocon の起動時に読み込ませることにより、一部の設定や接続の自動化を行うことができます。

基本的には、 opencocon の起動時に、おのおのの認識できるディスクのルートディレクトリにある cocon.cnf ファイルを読み込もうとします。読み込みに成功した場合、 opencocon はいくつかフィルタを通した後に cocon.cnf の内容をメモリにコピーし、 opencocon 内の各種コマンドで使用できるようになります。

## 設置方法
opencocon が認識できるUSBメモリ、ハードディスク、CD等~※~ (現時点ではFAT, NTFS, ext3, ext4, iso9660ファイルシステム) のルートディレクトリに **cocon.cnf** というテキストファイルを作成し、その中に Windows の INI ファイルのように設定項目を記述します。

※ Raspberry Pi版のSDカードでは、 FAT 側・ ext3 側のどちらのパーティションにも置くことができます。

ファイル内の文字コードはUTF-8としますが、改行コードはWindows式、UNIX式のどちらでも正しく認識します。

## 設定項目

### システム

#### COCON_KBD_CONSOLE
X以外のコンソール画面上でのキーマップを指定します。日本語キーボードの場合は以下のように指定します。
>COCON_KBD_CONSOLE=jp106

初期値：指定なし(=us)
備考：opencoconではXでないコンソールを通常使用しません。

#### COCON_KBD_X_MODEL
X.org上でのキーボードモデルを指定します。日本語キーボードの場合は以下のように指定します。
> COCON_KBD_X_MODEL=jp106

初期値：jp106

#### COCON_KBD_X_LAYOUT
X.org上でのキーボードレイアウトを指定します。日本語キーボードの場合は以下のように指定します。
> COCON_KBD_X_LAYOUT=jp

初期値：jp

#### COCON_KBD_X_VARIANT
X.org上でのキーボードバリアントを指定します。

初期値：なし

#### COCON_MUTE_MASTER_ON_BOOT
このパラメータに 1 を指定すると、 opencocon 起動時に、最初のサウンドカードのマスターボリュームを 0 (ミュート)します。

例えば、以下のように指定します。
> COCON_MUTE_MASTER_ON_BOOT=1

初期値：なし
注意： COCON_MUTE_MASTER_ON_BOOT と COCON_MASTER_VOLUME の両方が指定されている場合、ミュートの方が優先されます。

#### COCON_MASTER_VOLUME
このパラメータを指定すると、 opencocon 起動時、最初のサウンドカードのマスターボリュームを、0〜100(%)の範囲で指定することができます。

例えば、50%の場合は以下のように指定します。
> COCON_MASTER_VOLUME=50

※ 本来音量調整はコンピュータや周辺環境毎に指定されるべきですが、このパラメータは cocon.cnf を読み込むすべての環境で一律に音量を設定します。指定の際はこの点、十分注意していただきますようお願いします。

初期値：なし
注意： COCON_MUTE_MASTER_ON_BOOT と COCON_MASTER_VOLUME の両方が指定されている場合、ミュートの方が優先されます。



### 自動接続

#### COCON_AUTOCONNECT
自動接続を使用する場合、このパラメータにプロトコルを指定します。
使用できる値は以下の通りです。

- spice
- rdp
- vnc
- www (Webブラウザ)
- x (XDMCP)

例えば、RDPに自動接続する場合は、以下のように指定します。

> COCON_AUTOCONNECT=rdp

初期値：なし
備考：自動接続を有効にするには、おのおののプロトコルの設定項目を記述する必要があります。

#### COCON_AUTOVPN
起動時の自動VPN接続を使用する場合、このパラメータにプロトコルを指定します。
使用できる値は以下の通りです。

- se (SoftEther)

例えば、SoftEtherに接続する場合は、以下のように指定します。

> COCON_AUTOVPN=se

初期値：なし
備考：自動接続を有効にするには、おのおののプロトコルの設定項目を記述する必要があります。
備考：今後、複数のプロトコルが可能になった場合は複数指定できるようにする予定です。ただし、1プロトコルあたり1セッションのみの制限を設ける予定です。

#### COCON_POWEROFF_AFTER_AUTOCONNECT
このパラメータに1を指定すると、自動接続セッションの後、自動的にシャットダウンします。それ以外の場合、自動接続セッションの後は通常のメニューが表示されます。

> COCON_POWEROFF_AFTER_AUTOCONNECT=1

初期値：なし

### RDP

#### COCON_RDP_HOST
RDPプロトコルによる自動接続時のホスト名/IPアドレスを指定します。例えば、以下のように指定します。
ポート番号を指定する場合は、ホスト名の後に「:3330」のように指定してください。

> COCON_RDP_HOST=hostname

初期値：なし

#### COCON_RDP_USER
RDPプロトコルによる自動接続時のユーザ名を指定します。以下のように指定します。

> COCON_RDP_USER=user

初期値：なし

#### COCON_RDP_DOMAIN
RDPプロトコルによる自動接続時のユーザ名を指定します。以下のように指定します。

> COCON_RDP_DOMAIN=domain

初期値：なし

#### COCON_RDP_RFX
このスイッチを1にすると、追加のリダイレクト(マイク、映像)とRemoteFXを有効にします。以下のように指定します。

> COCON_RDP_RFX=1

初期値：0 (無効)
備考：自動接続が有効でなくてもこのパラメータは機能します。

#### COCON_RDP_MODEM
このスイッチを1にすると、RDP接続は、Windows Vista以降のウィンドウデコレーションが省かれる等が行われ、低速接続用のプロファイルになります。以下のように指定します。

> COCON_RDP_MODEM=1

初期値：0 (無効)
備考：自動接続が有効でなくてもこのパラメータは機能します。

#### COCON_RDP_KBD
FreeRDPにおける、キーボード配列を指定します。例えば、以下のように指定します。

> COCON_RDP_KBD=jp

初期値：なし
備考：通常はX.orgのキー配列から指定するので指定する必要はありません。

### VNC

#### COCON_VNC_HOST
VNCプロトコルによる自動接続時のホスト名/IPアドレスを指定します。例えば、以下のように指定します。

> COCON_VNC_HOST=hostname

初期値：なし
備考： " : " の後にディスプレイ番号を指定することができます。

#### COCON_VNC_USER
VNCプロトコルによる自動接続時のユーザ名を指定します。例えば、以下のように指定します。

> COCON_VNC_USER=user

初期値：なし
備考：v9d以前のバージョンでは、libvncviewerを使用した接続においてこのオプションは無視されます。

### SPICE

#### COCON_SPICE_HOST
SPICEプロトコルによる自動接続時のホスト名/IPアドレスを指定します。例えば、以下のように指定します。

> COCON_SPICE_HOST=hostname

初期値：なし
備考：ここにポート番号を指定することはできません。 COCON_SPICE_(TLS)PORT のいずれかに指定してください。

#### COCON_SPICE_PORT
SPICEプロトコルによる自動接続時のポート番号(暗号化なしの場合)を指定します。例えば、以下のように指定します。

> COCON_SPICE_PORT=5900

初期値：なし

#### COCON_SPICE_TLSPORT
SPICEプロトコルによる自動接続時のポート番号(TLS暗号化ありの場合)を指定します。例えば、以下のように指定します。

> COCON_SPICE_TLSPORT=5901

初期値：なし

### XDMCP

#### COCON_X_HOST
XDMCPによる自動接続時のホスト名/IPアドレスを指定します。例えば、以下のように指定します。

> COCON_X_HOST=hostname

初期値：なし

### Webブラウザ

#### COCON_X_HOST
Webブラウザの自動起動設定を有効にした場合、最初に開くにURLを指定します。例えば、以下のように指定します。

> COCON_WWW_START=[http://opencocon.org/](http://opencocon.org/)

初期値：なし

### SoftEther

#### COCON_SE_HOST
SoftEtherによる自動接続時のホスト名/IPアドレスとポート名を指定します。例えば、以下のように指定します。

> COCON_SE_HOST=hostname:443

初期値：なし
注意：現在のところ、ポート番号は自動補完されません。必ず指定する必要があります。

#### COCON_SE_USER
SoftEtherによる自動接続時のユーザ名を指定します。例えば、以下のように指定します。

> COCON_SE_USER=user

初期値：なし

#### COCON_SE_HUB
SoftEtherによる自動接続時の仮想名を指定します。例えば、以下のように指定します。

> COCON_SE_USER=VPN

初期値：なし

#### COCON_SE_AUTH_METHOD
SoftEtherによる自動接続時の認証手段を指定します。以下のパラメータを指定することができます。

- pass (パスワード認証)
- anon (匿名認証)

例えば、以下のように指定します。

> COCON_SE_AUTH_METHOD=pass

初期値：なし
備考：今後他の認証手段を追加する予定です。

#### COCON_SE_SET_GATEWAY_ONVPN
SoftEtherによる自動接続時が確立後、opencocon のデフォルトゲートウェイをVPN接続先のものにしたい場合、1を指定します。

例えば、以下のように指定します。

> COCON_SE_SET_GATEWAY_ONVPN=1

初期値：0 (デフォルトゲートウェイは変更されません)

## 制限

- 任意のコードを cocon.cnf にて実行することはできません。必要な要素などがあれば、ディストリビューション自体のカスタマイズを行うか、フォーラムにて機能の提案を行っていただくと有り難いです。こちらでも検討致します。

- 複数プロファイルの作成にはまだ対応していませんが、今後対応する予定です。

- 組み合わせによっては、不具合が生じることがあります。

## 付録

### cocon.cnf を読み込む順番
以下の順番に読み込みますが、同じ設定項目が複数ドライブにあった場合、反映される順番は上下逆となります。
(1) ここんイメージ内 /usr/share/cocon/default.cnf
(2) 起動ディスクのルートディレクトリにある cocon.cnf
(3) それ以外のディスクのトップにある cocon.cnf (探索順になるため指定はできません)

### NetworkManagerの設定
注意：これは暫定仕様です。今後変わることがあります。

opencocon 以外の、NetworkManager を用いるLinuxディストリビューションにおいて、 /etc/NetworkManager/system-connections/ 内にある接続設定ファイル(拡張子なし)を USB メモリ等のルートディレクトリに作成した **coconnm** ディレクトリ内にコピーすると、その設定をopencocon起動時に読み込みます。

ディストリビューションによっては、WPA等のパスワードは平文で格納されています。取り扱いには注意するようお願いします。

### ファームウェアの読み込み
ライセンスの関係上ファームウェアをバンドルできない、いくつかの無線LANチップ向けに、別のハードディスク・USBメモリからファームウェアを読み込むようになっています。

opencocon 以外の Linux ディストリビューションにて coconfrm-downloader スクリプト(opencoconのCDに同梱しています)をホームディレクトリ等にコピーして実行し、出来た **coconfrm** ディレクトリをハードディスク・USBメモリ等のルートディレクトリ直下にコピーしてください。

この方法でなければファームウェアが読み込めないチップは以下の通りです。いずれも、一部の旧型PCに内蔵されていたり、あるいはCardBusカードに搭載されています。

- Broadcom bcm43xx
- Prism54 (p54usb, p54pci)
- Intel ipw2100, ipw2200

## バージョン
このドキュメントは opencocon v9f 用に書かれています。他のバージョンでは挙動が異なることがあります。

## ライセンス
このドキュメントは opencocon の一部として、以下のライセンスが適用されます。

Creator : SHIMADA Hirofumi

This work is licensed under the Creative Commons Attribution 3.0 Unported License. To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.