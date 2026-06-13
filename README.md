# aws-nginx-practice

🇯🇵[JP]
# AWS EC2 + nginx サーバー構築練習

## 1. EC2サーバー接続（SSH）

まず Downloads フォルダへ移動して、pemキーの権限設定を行った後、SSHでEC2サーバーへ接続した。

使用したコマンド：

cd ~/Downloads

chmod 400 linux-practice-key.pem

ssh -i linux-practice-key.pem ubuntu@IPアドレス

### 学んだこと

・SSHを使うことで、自分のMacからAWS上のLinuxサーバーへ接続できる  
・pemキーの権限設定が正しくないと接続できない  
・EC2をStop→StartするとPublic IPv4アドレスが変わる場合がある

---

## 2. パッケージ一覧更新

使用したコマンド：

sudo apt update

### 学んだこと

Ubuntuでは apt を使ってソフトウェアを管理する。

apt update はインストール可能なパッケージ一覧を最新状態に更新するコマンドで、アプリストアを更新する感覚に近い。

---

## 3. nginx インストール

使用したコマンド：

sudo apt install nginx -y

### 学んだこと

・sudo → 管理者権限  
・apt → Ubuntuのパッケージ管理システム  
・install → ソフトウェアをインストール  
・nginx → Webサーバープログラム  
・-y → 確認メッセージを自動承認

nginxをインストールすることで、EC2サーバーをWebサーバーとして利用できるようになる。

---

## 4. nginx 状態確認

使用したコマンド：

systemctl status nginx

### 学んだこと

systemctl はLinuxでサービスを管理するツール。

status nginx を実行すると、nginxが正常に動作しているか確認できる。

正常な場合：

Active: active (running)

と表示される。

状態確認画面は q を押すことで閉じることができる。

---

## 5. HTTP 80ポート開放

最初はSSH(22番ポート)のみ許可されていたため、ターミナル接続しかできない状態だった。

ブラウザからWebサイトへ接続できるようにするため、HTTP 80ポートを開放した。

### 設定場所

EC2 → セキュリティグループ → インバウンドルール編集 → ルール追加

### 設定内容

Type：HTTP  
Protocol：TCP  
Port：80  
Source：0.0.0.0/0

### 学んだこと

・HTTP → Webサイト接続用  
・TCP 80 → ブラウザ通信に使われるポート  
・0.0.0.0/0 → 全ての外部アクセスを許可

---

## 6. nginx 動作確認

使用したコマンド：

curl localhost

### 結果

<title>Welcome to nginx!</title>

が表示された。

### 学んだこと

localhost はサーバー自身を意味する。

curl localhost を使うことで、サーバー内部からnginxが正常に応答しているか確認できる。

---

## 今回学んだ内容

・EC2  
・SSH  
・Public IP / Private IP  
・TCP / ポート  
・nginx  
・HTTP 80ポート  
・systemctl  
・apt パッケージ管理  
・Linux基本コマンド


# Node.js サーバー実行練習

## 7. Node.js インストール

使用したコマンド：

sudo apt install nodejs npm -y

### 内容

Node.js と npm をインストールした。

・Node.js  
→ JavaScriptでサーバープログラムを実行するための環境

・npm  
→ Node.js関連パッケージを管理するツール

・-y  
→ インストール確認を自動承認

---

## 8. インストール確認

使用したコマンド：

node -v

npm -v

### 結果

Node.js と npm のバージョンが正常に表示された。

### 学んだこと

・node -v  
→ Node.js のバージョン確認

・npm -v  
→ npm のバージョン確認

---

## 9. 作業フォルダ作成

使用したコマンド：

mkdir node-practice

cd node-practice

### 内容

Node.js サーバー練習用フォルダを作成した。

### 学んだこと

・mkdir  
→ 新しいフォルダを作成

・cd  
→ フォルダ移動

---

## 10. Node.js サーバーファイル作成

使用したコマンド：

nano server.js

### 内容

server.js ファイルを作成し、簡単なWebサーバーコードを記述した。

使用したコード：

const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Node Server');
});

server.listen(3000, '0.0.0.0', () => {
  console.log('Server is running on port 3000');
});

---

## 11. コード内容理解

### const http = require('http');

Node.js の http モジュールを読み込むコード。

http モジュールを使うことで、ブラウザとの通信が可能になる。

---

### http.createServer()

Webサーバーを作成する関数。

ブラウザからリクエストが来ると、その内容を受け取ってレスポンスを返す。

---

### req と res

・req  
→ ブラウザから送られてきたリクエスト情報

・res  
→ サーバーからブラウザへ返すレスポンス

---

### res.writeHead(200, {'Content-Type': 'text/plain'});

HTTPレスポンスヘッダー設定。

・200  
→ 通信成功を意味するHTTPステータスコード

・Content-Type  
→ 送信するデータ種類

・text/plain  
→ 普通のテキストデータ

---

### res.end('Hello Node Server');

ブラウザへ実際に送る内容。

レスポンス終了も同時に行う。

---

### server.listen(3000, '0.0.0.0')

3000番ポートでサーバー待機開始。

・3000番ポート  
→ 開発用でよく使われるポート番号

・0.0.0.0  
→ 全ての外部接続を許可

---

## 12. Node.js サーバー実行

使用したコマンド：

node server.js

### 結果

Server is running on port 3000

と表示された。

### 学んだこと

Node.js サーバーが3000番ポートで実行開始された状態。

この状態ではターミナルがサーバープログラム専用状態になる。

---

## 13. 3000番ポート開放

EC2 → セキュリティグループ → インバウンドルール編集 → ルール追加

### 設定内容

Type：Custom TCP  
Port：3000  
Source：0.0.0.0/0

### 学んだこと

Node.js サーバーが3000番ポートで待機しているため、外部アクセス許可が必要。

---

## 14. ブラウザ接続確認

ブラウザで：

http://PublicIPv4:3000

へ接続。

### 結果

Hello Node Server

表示成功。

---

## 今回学んだ内容

・Node.js  
・npm  
・Webサーバー  
・HTTP通信  
・req / res  
・HTTPステータスコード  
・3000番ポート  
・サーバープロセス  
・外部アクセス許可


# Linux 基本コマンド練習

## 15. 現在位置確認

使用したコマンド：

pwd

### 内容

現在自分がいるディレクトリ（フォルダ）の場所を表示するコマンド。

### 学んだこと

Linuxではディレクトリ構造の中を移動しながら作業する。

pwd は “Print Working Directory” の略。

---

## 16. ファイル一覧確認

使用したコマンド：

ls

追加：

ls -al

### 内容

現在のディレクトリ内に存在するファイルやフォルダを表示する。

### 学んだこと

・ls  
→ 普通の一覧表示

・ls -al  
→ 詳細情報 + 隠しファイル表示

Linuxでは .bashrc や .profile などの隠しファイルが多く使われる。

---

## 17. ディレクトリ作成

使用したコマンド：

mkdir practice-linux

### 内容

新しいディレクトリを作成した。

### 学んだこと

mkdir は “make directory” の略。

---

## 18. ディレクトリ移動

使用したコマンド：

cd practice-linux

### 内容

practice-linux ディレクトリへ移動。

### 学んだこと

cd は “change directory” の略。

Linuxではディレクトリ移動を頻繁に行う。

---

## 19. ファイル作成

使用したコマンド：

touch test.txt

### 内容

空のファイルを作成。

### 学んだこと

touch はファイル作成や更新日時変更に使われる。

---

## 20. ファイル編集

使用したコマンド：

nano test.txt

### 内容

nano エディタを使ってファイル編集を行った。

ファイル内に：

Hello Linux

を記述。

### 保存方法

Ctrl + O  
Enter  
Ctrl + X

### 学んだこと

nano は Linux の基本テキストエディタ。

サーバー設定ファイル編集などでよく使用される。

---

## 21. ファイル内容確認

使用したコマンド：

cat test.txt

### 内容

ファイル内容をターミナルへ表示。

### 学んだこと

cat はファイル内容確認によく使われる。

---

## 22. ファイルコピー

使用したコマンド：

cp test.txt copy.txt

### 内容

test.txt をコピーして copy.txt を作成。

### 学んだこと

cp の基本構造：

cp [コピー元] [コピー先]

例：

cp test.txt backup/

→ backup フォルダ内へ同じ名前でコピー

cp test.txt newname.txt

→ 内容は同じで名前だけ違う新しいファイル作成

---

## 23. ファイル移動 / 名前変更

使用したコマンド：

mv copy.txt moved.txt

### 内容

copy.txt の名前を moved.txt へ変更。

### 学んだこと

mv は移動と名前変更の両方に使われる。

基本構造：

mv [元] [移動先]

---

## 24. ファイル削除

使用したコマンド：

rm moved.txt

### 内容

moved.txt を削除。

### 学んだこと

rm は “remove” の略。

Linuxでは削除後にゴミ箱へ入らず、そのまま削除される場合が多い。

---

## 25. ファイル権限確認

使用したコマンド：

ls -al

### 内容

ファイル詳細情報確認。

例：

-rw-r--r--

### 学んだこと

Linuxではファイルごとに権限設定が存在する。

今後：

・SSH接続  
・nginx  
・実行ファイル

などで権限問題が重要になる。

---

## 今回学んだ内容

・Linux ディレクトリ構造  
・ファイル操作  
・ディレクトリ移動  
・nano エディタ  
・ファイルコピー  
・ファイル移動  
・ファイル削除  
・Linux 権限概念


# Process / Service / Log 管理練習

## 26. プロセスとは

Linuxでは、実行中のプログラムを「プロセス」と呼ぶ。

例：

node server.js

を実行すると、Node.jsサーバープログラムが実際にメモリ上で動作し続ける。

### 学んだこと

・ファイルそのものと実行中状態は違う  
・実行中の状態を「プロセス」と呼ぶ

---

## 27. Node.js サーバー実行

使用したコマンド：

cd ~/node-practice

node server.js

### 結果

Server is running on port 3000

表示成功。

### 学んだこと

Node.jsサーバーが3000番ポートで待機状態になった。

この状態ではターミナルがNode.jsサーバープロセス専用状態になる。

---

## 28. 新しいターミナル接続

Node.jsサーバー実行中は別コマンド入力が難しいため、新しいターミナルタブを開いて再度SSH接続を行った。

### 学んだこと

実際のサーバー運用では複数ターミナルを使いながら管理することが多い。

---

## 29. 実行中プロセス確認

使用したコマンド：

ps aux

### 内容

現在Linuxサーバー内で実行中のプロセス一覧表示。

### 学んだこと

ps は “process status” の略。

aux オプションによって：

・a → 他ユーザーのプロセス表示  
・u → ユーザー情報表示  
・x → バックグラウンドプロセス表示

を行う。

---

## 30. Node.js プロセス検索

使用したコマンド：

ps aux | grep node

### 内容

実行中プロセスの中から node を含むものだけ検索。

### 学んだこと

| は「パイプ」と呼ばれ、前のコマンド結果を次のコマンドへ渡す。

grep は文字列検索コマンド。

つまり：

ps aux の結果
↓
node を含む行だけ抽出

という流れ。

---

## 31. PID(Process ID)

プロセス一覧内に：

1234

のような数字が表示された。

### 学んだこと

PID はプロセスごとの管理番号。

LinuxではPIDを使ってプロセス管理を行う。

---

## 32. プロセス終了

使用したコマンド：

kill PID番号

例：

kill 1234

### 内容

指定PIDのプロセスへ終了信号送信。

### 学んだこと

Node.jsサーバープロセスを手動終了できた。

サーバー運用では不要プロセス停止や異常プロセス終了に使われる。

---

## 33. 強制終了

使用したコマンド：

kill -9 PID番号

### 内容

通常終了できないプロセスを強制終了。

### 学んだこと

-9 は強制終了シグナル。

ただし強制終了はデータ破損リスクもあるため注意が必要。

---

## 34. リアルタイムサーバー状態確認

使用したコマンド：

top

### 内容

CPU、メモリ、実行中プロセスなどをリアルタイム表示。

### 学んだこと

WindowsタスクマネージャーやMac Activity Monitor に近い役割。

終了は q。

---

## 35. systemctl と ps の違い

### ps

現在実行中のプロセス確認。

例：

ps aux | grep node

### systemctl

Linuxサービス管理ツール。

サービス開始・停止・再起動・状態確認を行う。

### 学んだこと

・ps  
→ 実行中プロセス確認

・systemctl  
→ Linuxサービス管理

全てのサービスは最終的にプロセスとして動作している。

---

## 36. nginx サービス状態確認

使用したコマンド：

systemctl status nginx

### 内容

nginxサービス状態確認。

### 学んだこと

nginx は Linux の systemd によって管理されているサービス。

正常状態：

Active: active (running)

---

## 37. nginx サービス停止 / 開始

使用したコマンド：

sudo systemctl stop nginx

sudo systemctl start nginx

sudo systemctl restart nginx

### 学んだこと

・stop → 停止  
・start → 開始  
・restart → 再起動

sudo が必要なのはシステムサービス管理に管理者権限が必要だから。

---

## 38. Node.js と nginx の違い

### Node.js

node server.js

で手動実行する一般プロセス。

### nginx

systemd が管理するLinuxサービス。

### 学んだこと

Node.js は現在手動管理状態。

nginx はLinuxシステム側で管理されるサービス。

---

## 39. ログ確認

使用したコマンド：

journalctl -u nginx

### 内容

nginxサービス関連ログ確認。

### 学んだこと

journalctl は systemd ログ確認ツール。

-u nginx は nginx ユニットログのみ表示。

Linuxサーバー問題調査ではログ確認が非常に重要。

---

## 40. 最近ログ確認

使用したコマンド：

journalctl -u nginx -n 20

### 内容

最近20行のnginxログのみ表示。

---

## 41. リアルタイムログ監視

使用したコマンド：

journalctl -u nginx -f

### 内容

新しいログをリアルタイム表示。

### 学んだこと

-f は follow の意味。

リアルタイム監視終了は Ctrl + C。

---

## 今回学んだ内容

・プロセス  
・PID  
・プロセス検索  
・プロセス終了  
・top  
・systemctl  
・Linuxサービス  
・systemd  
・ログ管理  
・journalctl

---

# Linux Permission 練習

## 42. Linux 権限とは

Linuxでは、ファイルやディレクトリごとに「誰が何をできるか」が決められている。

権限には主に3種類ある。

・r → read（読み取り）  
・w → write（書き込み）  
・x → execute（実行）

---

## 43. ls -al で権限確認

使用したコマンド：

ls -al

### 内容

ファイルやディレクトリの詳細情報を確認する。

例：

-rw-rw-r--

### 学んだこと

先頭の1文字はファイルの種類を表す。

・- → 通常ファイル  
・d → ディレクトリ

その後の9文字は権限を表す。

例：

rw-rw-r--

これは3つに分けて読む。

rw- / rw- / r--

・1つ目 → owner（所有者）  
・2つ目 → group（グループ）  
・3つ目 → other（その他ユーザー）

---

## 44. r / w / x の意味

### r

read の略。

ファイルの場合は内容を読む権限。

例：

cat file.txt

---

### w

write の略。

ファイルの場合は内容を編集する権限。

例：

nano file.txt

---

### x

execute の略。

ファイルの場合は実行する権限。

例：

./script.sh

---

## 45. ディレクトリでの権限

ディレクトリの場合、r / w / x の意味が少し変わる。

・r  
→ ディレクトリ内の一覧を見る権限

・w  
→ ディレクトリ内でファイル作成・削除する権限

・x  
→ ディレクトリの中に入る権限

---

## 46. chmod とは

chmod は change mode の略。

ファイルやディレクトリの権限を変更するコマンド。

基本形：

chmod 権限 ファイル名

例：

chmod 644 file.txt

---

## 47. 数字で権限を表す方法

Linuxでは権限を数字でも表す。

・r = 4  
・w = 2  
・x = 1

この数字を足して権限を決める。

例：

rwx = 4 + 2 + 1 = 7

rw- = 4 + 2 = 6

r-- = 4

--- = 0

---

## 48. chmod 777

使用したコマンド：

chmod 777 permission-test.txt

### 意味

permission-test.txt を全ユーザーが読み取り・書き込み・実行できる状態にする。

777 の意味：

・owner → 7 → rwx  
・group → 7 → rwx  
・other → 7 → rwx

結果：

rwxrwxrwx

### 学んだこと

777 は全員に強い権限を与えるため、実務では注意して使う必要がある。

---

## 49. chmod 644

使用したコマンド：

chmod 644 permission-test.txt

### 意味

所有者だけが編集でき、他のユーザーは読み取りのみ可能にする。

644 の意味：

・owner → 6 → rw-  
・group → 4 → r--  
・other → 4 → r--

結果：

rw-r--r--

### 学んだこと

644 は一般的なファイルでよく使われる権限。

---

## 50. chmod 400

使用したコマンド：

chmod 400 permission-test.txt

### 意味

所有者だけが読み取り可能で、他のユーザーは何もできない状態にする。

400 の意味：

・owner → 4 → r--  
・group → 0 → ---  
・other → 0 → ---

結果：

r--------

### 学んだこと

400 は秘密鍵のような重要ファイルで使われることがある。

---

## 51. SSH pemキーと chmod 400

EC2へSSH接続するときに使用したpemキーには、以下の権限設定が必要だった。

chmod 400 linux-practice-key.pem

### 理由

pemキーはサーバーへ接続するための秘密鍵。

他のユーザーが読める状態だと危険なので、SSHは接続を拒否する場合がある。

そのため、所有者だけが読み取り可能な 400 に設定する。

---

## 今回学んだ内容

・Linux権限  
・r / w / x  
・owner / group / other  
・chmod  
・数字権限  
・777  
・644  
・400  
・SSH秘密鍵の権限管理

---

# Linux Network Command 練習

## 52. ネットワーク確認コマンドを学ぶ理由

サーバー運用では、接続できない原因を調べることが多い。

例：

・SSH接続できない  
・Webサイトが開かない  
・80番ポートが開いていない  
・Node.jsの3000番ポートに接続できない

このような問題を調査するために、ネットワーク確認コマンドを使う。

---

## 53. ip addr

使用したコマンド：

ip addr

### 意味

ip は Internet Protocol の略。  
addr は address の略。

つまり、ip addr はサーバーのIPアドレス情報を確認するコマンド。

### 何をするか

サーバーが持っているネットワークインターフェースとIPアドレスを表示する。

### なぜ使うか

現在のサーバーのPrivate IPやネットワーク情報を確認するため。

### 学んだこと

EC2のサーバー内部ではPrivate IPが使われる。

例：

172.31.x.x

これはAWS内部ネットワークで使われるIPアドレス。

---

## 54. hostname -I

使用したコマンド：

hostname -I

### 意味

hostname は host name、つまりコンピューター名を意味する。  
-I はIPアドレスを表示するオプション。

### 何をするか

現在のサーバーが持つIPアドレスだけを簡単に表示する。

### なぜ使うか

ip addr の結果は長いため、IPだけを素早く確認したい時に使う。

---

## 55. ss -tulpn

使用したコマンド：

ss -tulpn

### 意味

ss は socket statistics の略。  
ネットワーク接続やポート状態を確認するコマンド。

オプションの意味：

・-t → TCPを表示  
・-u → UDPを表示  
・-l → LISTEN状態のポートを表示  
・-p → 使用中のプロセス情報を表示  
・-n → ポート名ではなく数字で表示

### 何をするか

現在サーバー内で、どのプログラムがどのポートを使っているか確認する。

### なぜ使うか

WebサーバーやNode.jsサーバーが本当にポートを開いて待機しているか確認するため。

例：

0.0.0.0:80

→ nginxが80番ポートで待機中

0.0.0.0:3000

→ Node.jsサーバーが3000番ポートで待機中

---

## 56. TCP と UDP

### TCP

TCP は Transmission Control Protocol の略。

接続を確認しながら、データを確実に送る通信方式。

### 特徴

・接続確認あり  
・データの順番を保証  
・信頼性が高い  
・少し遅くなる場合がある

### 主な用途

・HTTP  
・HTTPS  
・SSH  
・Webアプリケーション

---

### UDP

UDP は User Datagram Protocol の略。

接続確認をせず、データを素早く送る通信方式。

### 特徴

・接続確認なし  
・速い  
・一部データが失われる可能性あり  
・リアルタイム通信向き

### 主な用途

・オンラインゲーム  
・音声通話  
・動画通話  
・リアルタイム配信

---

## 57. LISTEN とは

ss -tulpn の結果で、TCPは LISTEN と表示されることがある。

### 意味

LISTEN は「接続を待っている状態」。

### 例

0.0.0.0:80 LISTEN

これは、80番ポートで接続を待っているという意味。

nginxやNode.jsサーバーが正常に起動している時に確認できる。

---

## 58. UNCONN とは

UDPでは UNCONN と表示されることがある。

### 意味

UNCONN は Unconnected の略。

UDPはTCPのように接続を作って通信する方式ではないため、UNCONN と表示される。

### 学んだこと

TCPは接続ベースなので LISTEN 状態がある。  
UDPは非接続型なので UNCONN と表示される。

---

## 59. curl

使用したコマンド：

curl http://localhost

### 意味

curl は Client URL の略として説明されることが多い。  
URLに対してリクエストを送るコマンド。

### 何をするか

ブラウザを使わず、ターミナルからWebサーバーへHTTPリクエストを送る。

### なぜ使うか

Webサーバーが正常に応答しているか確認するため。

### 例

curl http://localhost

→ サーバー自身に対してWebリクエストを送る

nginxが正常ならHTMLが返ってくる。

---

## 60. ping

使用したコマンド：

ping google.com

### 意味

ping はネットワーク上の相手が応答するか確認するためのコマンド。

### 何をするか

指定した相手に小さな信号を送り、応答が返ってくるか確認する。

### なぜ使うか

インターネット接続や相手サーバーへの到達性を確認するため。

### 終了方法

Ctrl + C

---

## 61. 0.0.0.0 の意味

0.0.0.0 は「全てのネットワークインターフェースで待ち受ける」という意味。

例：

0.0.0.0:3000

これは、サーバーが持つ全てのIPアドレスで3000番ポートへの接続を受け付けるという意味。

### 学んだこと

Node.jsサーバーを外部から接続可能にするには、localhostだけでなく0.0.0.0で待ち受ける必要がある。

---

## 62. localhost の意味

localhost は「自分自身」を意味する。

例：

curl http://localhost

これは、サーバーが自分自身のWebサーバーへアクセスしている状態。

### 学んだこと

localhostでは接続できても、外部ブラウザから接続できない場合は、セキュリティグループやルーティングなど外部ネットワーク側に問題がある可能性が高い。

---

## 今回学んだ内容

・ip addr  
・hostname -I  
・ss -tulpn  
・TCP  
・UDP  
・LISTEN  
・UNCONN  
・curl  
・ping  
・localhost  
・0.0.0.0  
・ポート確認  
・ネットワーク疎通確認

------

63. Dockerを学ぶ理由

Dockerはアプリケーションを実行するために必要な環境をまとめてパッケージ化し、どこでも同じように実行できるようにする技術である。

従来はサーバーごとにNode.jsやnginxなどを個別にインストールしていたため、環境の違いによるトラブルが発生しやすかった。

Dockerを利用することで、開発環境と本番環境を同じ状態で再現できる。

⸻

64. Dockerとは

Dockerはコンテナを作成・管理するためのプラットフォームである。

Docker自体がアプリケーションを実行するのではなく、コンテナを管理する役割を持つ。

イメージからコンテナを作成し、実行・停止・削除を行うことができる。

⸻

65. ImageとContainer

Dockerを理解する上で最も重要な概念。

Image

Imageはアプリケーションを実行するための設計図である。

必要なプログラムや設定が含まれているが、まだ実行されていない状態。

例：

nginx Image

⸻

Container

ContainerはImageを実際に実行した状態である。

Imageから複数のContainerを作成することができる。

例：

nginx Container

⸻

学んだこと

Image = 設計図

Container = 設計図から作られた実行中の実体

⸻

66. docker pull

使用したコマンド：

docker pull nginx

意味

pullは「取得する」という意味。

Docker Hubからnginx Imageをダウンロードする。

学んだこと

Containerを作成する前にImageが必要である。

⸻

67. docker images

使用したコマンド：

docker images

意味

ダウンロード済みのImage一覧を表示する。

なぜ使うか

現在どのImageを持っているか確認するため。

⸻

68. docker run

使用したコマンド：

docker run -d -p 8080:80 nginx

意味

nginx ImageからContainerを作成して実行する。

-d

detached の略。

バックグラウンド実行。

-p

port の略。

ポートを接続する。

8080:80

EC2の8080番ポートとContainer内の80番ポートを接続する。

学んだこと

Container内部と外部サーバーのポートは別である。

⸻

69. docker ps

使用したコマンド：

docker ps

意味

実行中のContainer一覧を表示する。

学んだこと

Linuxのpsコマンドと似ているが、Docker Container専用のコマンドである。

⸻

70. docker ps の結果を読む

例：

CONTAINER ID

IMAGE

STATUS

PORTS

NAMES

CONTAINER ID

Container固有の識別番号

IMAGE

Container作成時に使用したImage

STATUS

現在の状態

例：

Up

↓

実行中

PORTS

ポートマッピング情報

例：

0.0.0.0:8080->80/tcp

意味：

EC2の8080番ポート

↓

Containerの80番ポート

NAMES

Dockerが自動生成したContainer名

⸻

71. docker stop

使用したコマンド：

docker stop コンテナ名

意味

実行中のContainerを停止する。

学んだこと

Containerを停止してもImageは削除されない。

⸻

72. docker start

使用したコマンド：

docker start コンテナ名

意味

停止中のContainerを再度実行する。

⸻

73. docker ps -a

使用したコマンド：

docker ps -a

意味

実行中だけでなく停止済みContainerも表示する。

学んだこと

Containerは停止しても削除されるわけではない。

⸻

74. docker rm

使用したコマンド：

docker rm コンテナID

意味

Containerを削除する。

学んだこと

Containerを削除してもImageは残る。

⸻

75. Dockerで最も重要な理解

今回の学習で理解したこと。

ImageとContainerは別物である。

Imageは設計図。

Containerは実際に動作する実体。

Containerを削除してもImageは残るため、同じImageから何度でも新しいContainerを作成できる。

⸻

今回学んだ内容

・Dockerの目的

・Dockerの基本概念

・Image

・Container

・docker pull

・docker images

・docker run

・docker ps

・docker ps -a

・docker stop

・docker start

・docker rm

・ポートマッピング（8080:80）

・ImageとContainerの違い


---

🇰🇷[KOR]
# AWS EC2 + nginx 서버 구축 연습

## 1. EC2 서버 연결(SSH)

먼저 Downloads 폴더로 이동하여 pem 키의 권한 설정을 한 후 SSH로 EC2 서버에 연결했다.

사용한 명령:

cd ~/Downloads

chmod 400 linux-practice-key.pem

ssh -i linux-practice-key.pem ubuntu@IP 주소

### 배운 것

· SSH를 사용하면 Mac에서 AWS의 Linux 서버에 연결할 수 있습니다.
· pem 키의 권한 설정이 올바르지 않으면 연결할 수 없습니다.
· EC2를 Stop → Start하면 Public IPv4 주소가 바뀔 수 있음

---

## 2. 패키지 목록 업데이트

사용한 명령:

sudo apt update

### 배운 것

Ubuntu에서는 apt를 사용하여 소프트웨어를 관리합니다.

apt update는 설치 가능한 패키지 목록을 최신 상태로 업데이트하는 명령으로 앱 스토어를 업데이트하는 느낌에 가깝습니다.

---

## 3. nginx 설치

사용한 명령:

sudo apt install nginx -y

### 배운 것

・sudo → 관리자 권한
· apt → 우분투 패키지 관리 시스템
· install → 소프트웨어 설치
· nginx → 웹 서버 프로그램
· -y → 확인 메시지 자동 승인

nginx를 설치하면 EC2 서버를 웹 서버로 사용할 수 있습니다.

---

## 4. nginx 상태 확인

사용한 명령:

systemctl status nginx

### 배운 것

systemctl은 Linux에서 서비스를 관리하는 도구입니다.

status nginx를 실행하면 nginx가 제대로 작동하는지 확인할 수 있습니다.

정상적인 경우:

Active: active (running)

라고 표시된다.

상태 확인 화면은 q를 눌러 닫을 수 있습니다.

---

## 5. HTTP 80 포트 개방

처음에는 SSH(22번 포트)만 허가되어 있었기 때문에, 터미널 접속 밖에 할 수 없는 상태였다.

브라우저에서 웹 사이트에 연결할 수 있도록 HTTP 80 포트를 개방했다.

### 설정 위치

EC2 → 보안 그룹 → 인바운드 규칙 편집 → 규칙 추가

### 설정 내용

유형: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0

### 배운 것

・HTTP → Web 사이트 접속용
· TCP 80 → 브라우저 통신에 사용되는 포트
・0.0.0.0/0 → 모든 외부 액세스를 허가

---

## 6. nginx 동작 ​​확인

사용한 명령:

curl localhost

### 결과

<title>Welcome to nginx!</title>

가 표시되었습니다.

### 배운 것

localhost는 서버 자체를 의미합니다.

curl localhost를 사용하면 서버 내부에서 nginx가 성공적으로 응답하는지 확인할 수 있습니다.

---

## 이번에 배운 내용

・EC2
· SSH
・Public IP/Private IP
· TCP / 포트
· nginx
· HTTP 80 포트
· systemctl
· apt 패키지 관리
· Linux 기본 명령


# Node.js 서버 실행 연습

## 7. Node.js 설치

사용한 명령:

sudo apt install nodejs npm -y

### 내용

Node.js와 npm을 설치했습니다.

· Node.js
→ JavaScript로 서버 프로그램을 실행하기 위한 환경

・npm
→ Node.js 관련 패키지를 관리하는 도구

・-y
→ 설치 확인 자동 승인

---

## 8. 설치 확인

사용한 명령:

node -v

npm -v

### 결과

Node.js 및 npm 버전이 성공적으로 표시되었습니다.

### 배운 것

· 노드 -v
→ Node.js 버전 확인

· npm -v
→ npm 버전 확인

---

## 9. 작업 폴더 만들기

사용한 명령:

mkdir node-practice

cd node-practice

### 내용

Node.js 서버 연습 폴더를 만들었습니다.

### 배운 것

· mkdir
→ 새 폴더 만들기

・cd
→ 폴더 이동

---

## 10. Node.js 서버 파일 만들기

사용한 명령:

nano server.js

### 내용

server.js 파일을 만들고 간단한 웹 서버 코드를 작성했습니다.

사용한 코드:

const http = require('http');

const server = http.createServer((req, res) => { 
res.writeHead(200, {'Content-Type': 'text/plain'}); 
res.end('Hello Node Server');
});

server.listen(3000, '0.0.0.0', () => { 
console.log('Server is running on port 3000');
});

---

## 11. 코드 내용 이해

### const http = require('http');

Node.js의 http 모듈을 읽는 코드.

http 모듈을 사용하면 브라우저와의 통신이 가능해진다.

---

### http.createServer()

웹 서버를 만드는 함수.

브라우저에서 요청이 오면 해당 내용을 받고 응답을 반환합니다.

---

### req 및 res

・req
→ 브라우저에서 보낸 요청 정보

・res
→ 서버에서 브라우저로 반환하는 응답

---

### res.writeHead(200, {'Content-Type': 'text/plain'});

HTTP 응답 헤더 설정.

・200
→ 통신 성공을 의미하는 HTTP 상태 코드

· Content-Type
→ 전송할 데이터 유형

・text/plain
→ 일반 텍스트 데이터

---

### res.end('Hello Node Server');

브라우저에 실제로 보내는 내용.

응답 종료도 동시에 실시한다.

---

### server.listen(3000, '0.0.0.0')

3000번 포트에서 서버 대기 개시.

・3000번 포트
→ 개발용으로 자주 사용되는 포트 번호

・0.0.0.0
→ 모든 외부 연결 허용

---

## 12. Node.js 서버 실행

사용한 명령:

node server.js

### 결과

Server is running on port 3000

라고 표시되었다.

### 배운 것

Node.js 서버가 3000번 포트에서 실행 시작된 상태.

이 상태에서는 터미널이 서버 프로그램 전용 상태가 된다.

---

## 13. 3000번 포트 개방

EC2 → 보안 그룹 → 인바운드 규칙 편집 → 규칙 추가

### 설정 내용

Type:Custom TCP
Port: 3000
Source: 0.0.0.0/0

### 배운 것

Node.js 서버가 3000번 포트에서 대기하고 있으므로 외부 권한이 필요합니다.

---

## 14. 브라우저 연결 확인

브라우저에서:

http://PublicIPv4:3000

에 연결.

### 결과

Hello Node Server

디스플레이 성공.

---

## 이번에 배운 내용

· Node.js
・npm
· 웹 서버
· HTTP 통신
・req/res
· HTTP 상태 코드
・3000번 포트
· 서버 프로세스
· 외부 액세스 권한



# Linux 기본 명령 연습

## 15. 현재 위치 확인

사용한 명령:

pwd

### 내용

현재 자신이 있는 디렉토리(폴더)의 위치를 ​​표시하는 명령입니다.

### 배운 것

Linux에서는 디렉토리 구조 안을 이동하면서 작업한다.

pwd는 "Print Working Directory"의 약자입니다.

---

## 16. 파일 목록 확인

사용한 명령:

ls

추가:

ls -al

### 내용

현재 디렉토리에 존재하는 파일과 폴더를 표시합니다.

### 배운 것

・ls
→ 일반 목록 표시

· ls -al
→ 상세 정보 + 숨겨진 파일 표시

Linux에서는 .bashrc나 .profile 등의 숨겨진 파일이 많이 사용된다.

---

## 17. 디렉토리 만들기

사용한 명령:

mkdir practice-linux

### 내용

새 디렉토리를 만들었습니다.

### 배운 것

mkdir은 “make directory”의 약자입니다.

---

## 18. 디렉토리 이동

사용한 명령:

cd practice-linux

### 내용

practice-linux 디렉토리로 이동.

### 배운 것

cd 는 “change directory” 의 약자.

Linux에서는 디렉토리 이동을 자주 실시한다.

---

## 19. 파일 만들기

사용한 명령:

touch test.txt

### 내용

빈 파일을 만듭니다.

### 배운 것

touch 는 파일 작성이나 갱신 일시 변경에 사용된다.

---

## 20. 파일 편집

사용한 명령:

nano test.txt

### 내용

nano 편집기를 사용하여 파일 편집을 수행했습니다.

파일에:

Hello Linux

설명.

### 저장 방법

Ctrl+O
Enter
Ctrl+X

### 배운 것

nano는 Linux의 기본 텍스트 편집기입니다.

서버 설정 파일 편집 등에서 자주 사용된다.

---

## 21. 파일 내용 확인

사용한 명령:

cat test.txt

### 내용

파일 내용을 터미널에 표시.

### 배운 것

cat은 파일 내용 확인에 자주 사용됩니다.

---

## 22. 파일 복사

사용한 명령:

cp test.txt copy.txt

### 내용

test.txt를 복사하여 copy.txt를 만듭니다.

### 배운 것

cp의 기본 구조:

cp [복사 원본] [복사 대상]

예:

cp test.txt backup/

→ backup 폴더에 같은 이름으로 복사

cp test.txt newname.txt

→ 내용은 같고 이름만 다른 새 파일 작성

---

## 23. 파일 이동 / 이름 바꾸기

사용한 명령:

mv copy.txt moved.txt

### 내용

copy.txt의 이름을 moved.txt로 변경합니다.

### 배운 것

mv는 이동과 이름 변경 모두에 사용됩니다.

기본 구조:

mv [원본] [이동 대상]

---

## 24. 파일 삭제

사용한 명령:

rm moved.txt

### 내용

moved.txt를 삭제합니다.

### 배운 것

rm은 "remove"의 약자입니다.

Linux에서는 삭제 후에 휴지통에 들어가지 않고 그대로 삭제되는 경우가 많다.

---

## 25. 파일 권한 확인

사용한 명령:

ls -al

### 내용

파일 상세 정보 확인.

예:

-rw-r--r--

### 배운 것

Linux에서는 파일마다 권한 설정이 존재한다.

미래:

· SSH 연결
· nginx
· 실행 파일

등으로 권한 문제가 중요해진다.

---

## 이번에 배운 내용

· Linux 디렉토리 구조
· 파일 조작
· 디렉토리 이동
· nano 에디터
· 파일 복사
· 파일 이동
· 파일 삭제
· Linux 권한 개념


# Process / Service / Log 관리 연습

## 26. 프로세스란?

Linux에서는 실행중인 프로그램을 "프로세스"라고합니다.

예:

node server.js

실행하면 Node.js 서버 프로그램이 실제로 메모리에서 계속 작동합니다.

### 배운 것

· 파일 자체와 실행 중 상태는 다릅니다.
· 실행중인 상태를 "프로세스"라고합니다.

---

## 27. Node.js 서버 실행

사용한 명령:

cd ~/node-practice

node server.js

### 결과

Server is running on port 3000

디스플레이 성공.

### 배운 것

Node.js 서버가 3000번 포트에서 대기 상태가 되었다.

이 상태에서는 터미널이 Node.js 서버 프로세스 전용 상태가 된다.

---

## 28. 새로운 터미널 연결

Node.js 서버 실행 중에는 별도의 명령 입력이 어렵기 때문에 새로운 터미널 탭을 열고 다시 SSH 연결을 실시했다.

### 배운 것

실제 서버 운용에서는 복수 터미널을 사용하면서 관리하는 경우가 많다.

---

## 29. 실행 중 프로세스 확인

사용한 명령:

ps aux

### 내용

현재 Linux 서버에서 실행 중인 프로세스 목록 표시.

### 배운 것

ps는 “process status”의 약자입니다.

aux 옵션에 따라:

· a → 다른 사용자의 프로세스 표시
· u → 사용자 정보 표시
· x → 백그라운드 프로세스 표시

한다.

---

## 30. Node.js 프로세스 검색

사용한 명령:

ps aux | grep node

### 내용

실행중 프로세스중에서 node 를 포함한 것만 검색.

### 배운 것

|는 "파이프"라고 불리며 이전 명령 결과를 다음 명령으로 전달합니다.

grep은 문자열 검색 명령입니다.

즉:

ps aux 결과
↓
노드를 포함하는 행만 추출

라는 흐름.

---

## 31. PID(Process ID)

프로세스 목록 내에:

1234년

같은 숫자가 나타났습니다.

### 배운 것

PID는 프로세스 당 관리 번호입니다.

Linux에서는 PID를 사용하여 프로세스 관리를 실시한다.

---

## 32. 프로세스 종료

사용한 명령:

kill PID 번호

예:

킬 1234

### 내용

지정 PID의 프로세스에 종료 신호 송신.

### 배운 것

Node.js 서버 프로세스를 수동으로 종료할 수 있습니다.

서버 운용에서는 불필요 프로세스 정지나 이상 프로세스 종료에 사용된다.

---

## 33. 강제 종료

사용한 명령:

kill -9 PID 번호

### 내용

통상 종료할 수 없는 프로세스를 강제 종료.

### 배운 것

-9 는 강제 종료 시그널.

단, 강제 종료는 데이터 파손 위험도 있으므로 주의가 필요.

---

## 34. 실시간 서버 상태 확인

사용한 명령:

top

### 내용

CPU, 메모리, 실행중 프로세스 등을 실시간 표시.

### 배운 것

Windows 작업 관리자 및 Mac Activity Monitor에 가까운 역할.

종료는 q.

---

## 35. systemctl과 ps의 차이

### ps

현재 실행중인 프로세스 확인.

예:

ps aux | grep node

### systemctl

Linux 서비스 관리 도구.

서비스 시작, 정지, 재시작, 상태 확인.

### 배운 것

・ps
→ 실행 중 프로세스 확인

· systemctl
→ Linux 서비스 관리

모든 서비스는 결국 프로세스로 작동합니다.

---

## 36. nginx 서비스 상태 확인

사용한 명령:

systemctl status nginx

### 내용

nginx 서비스 상태 확인.

### 배운 것

nginx는 Linux systemd에 의해 관리되는 서비스입니다.

정상 상태:

Active: active (running)

---

## 37. nginx 서비스 중지 / 시작

사용한 명령:

sudo systemctl stop nginx

sudo systemctl start nginx

sudo systemctl restart nginx

### 배운 것

・stop → 정지
・start → 시작
· restart → 재부팅

sudo가 필요한 것은 시스템 서비스 관리에 관리자 권한이 필요하기 때문입니다.

---

## 38. Node.js와 nginx의 차이

### Node.js

node server.js

에서 수동으로 실행하는 일반 프로세스.

### nginx

systemd가 관리하는 Linux 서비스.

### 배운 것

Node.js는 현재 수동 관리 상태입니다.

nginx는 Linux 시스템 측에서 관리되는 서비스입니다.

---

## 39. 로그 확인

사용한 명령:

journalctl -u nginx

### 내용

nginx 서비스 관련 로그 확인.

### 배운 것

journalctl은 systemd 로그 확인 도구입니다.

-u nginx는 nginx 유닛 로그 만 표시합니다.

Linux 서버 문제 조사에서는 로그 확인이 매우 중요하다.

---

## 40. 최근 로그 확인

사용한 명령:

journalctl -u nginx -n 20

### 내용

최근 20행의 nginx 로그만 표시.

---

## 41. 실시간 로그 모니터링

사용한 명령:

journalctl -u nginx -f

### 내용

새로운 로그를 실시간으로 표시.

### 배운 것

-f 는 follow 의 의미.

실시간 감시 종료는 Ctrl+C.

---

## 이번에 배운 내용

· 프로세스
· PID
· 프로세스 검색
· 프로세스 종료
・ top
· systemctl
· Linux 서비스
· systemd
· 로그 관리
· journalctl

---

# Linux Permission 연습

## 42. Linux 권한이란?

리눅스에서는 파일이나 디렉토리마다 「누가 무엇을 할 수 있을까」가 정해져 있다.

권한에는 주로 3종류 있다.

・r → read(읽기)
・w → write(쓰기)
・x → execute(실행)

---

## 43. ls -al로 권한 확인

사용한 명령:

ls -al

### 내용

파일 및 디렉토리에 대한 자세한 정보를 확인합니다.

예:

-rw-rw-r--

### 배운 것

선두의 1 문자는 파일의 종류를 나타낸다.

· - → 일반 파일
· d → 디렉토리

이후 9자는 권한을 나타냅니다.

예:

rw-rw-r--

이것은 3개로 나누어 읽는다.

rw-/rw-/r--

・1번째 → owner(소유자)
・2번째 → group(그룹)
・3번째 → other(그 외 사용자)

---

## 44. r / w / x의 의미

### r

read의 약자.

파일의 경우 내용을 읽는 권한.

예:

cat file.txt

---

### w

write의 약어.

파일의 경우 내용을 편집할 수 있는 권한.

예:

nano file.txt

---

### x

execute의 약어.

파일의 경우 실행할 권한.

예:

./script.sh

---

## 45. 디렉토리의 권한

디렉토리의 경우, r / w / x 의 의미가 조금 바뀐다.

・r
→ 디렉토리의 목록을 볼 수있는 권한

・w
→ 디렉토리에서 파일을 만들고 삭제할 수있는 권한

・x
→ 디렉토리에 들어가는 권한

---

## 46. chmod 란 무엇입니까?

chmod는 change mode의 약자입니다.

파일 및 디렉토리의 권한을 변경하는 명령.

기본형:

chmod 권한 파일 이름

예:

chmod 644 file.txt

---

## 47. 숫자로 권한을 나타내는 방법

Linux에서는 권한을 숫자로 표현한다.

・r=4
・w=2
· x = 1

이 숫자를 더하여 권한을 결정한다.

예:

rwx=4+2+1=7

rw-=4+2=6

r--=4

---=0

---

## 48. chmod 777

사용한 명령:

chmod 777 permission-test.txt

### 의미

permission-test.txt를 모든 사용자가 읽기, 쓰기 및 실행할 수 있는 상태로 만듭니다.

777 의미:

・owner → 7 → rwx
· 그룹 → 7 → rwx
・other → 7 → rwx

결과:

rwxrwxrwx

### 배운 것

777은 모두에게 강한 권한을 부여하기 때문에 실무에서는 주의해서 사용할 필요가 있다.

---

## 49. chmod 644

사용한 명령:

chmod 644 permission-test.txt

### 의미

소유자만 편집할 수 있으며 다른 사용자는 읽기만 가능합니다.

644 의미:

・owner → 6 → rw-
· 그룹 → 4 → r--
・other → 4 → r--

결과:

rw-r--r--

### 배운 것

644는 일반적인 파일에서 자주 사용되는 권한입니다.

---

## 50. chmod 400

사용한 명령:

chmod 400 permission-test.txt

### 의미

소유자만 읽을 수 있고 다른 사용자는 아무것도 할 수 없는 상태로 한다.

400 의미:

・owner → 4 → r--
· 그룹 → 0 → ---
・other → 0 → ---

결과:

r--------

### 배운 것

400은 개인 키와 같은 중요한 파일에 사용될 수 있습니다.

---

## 51. SSH pem 키와 chmod 400

EC2에 SSH 연결할 때 사용한 pem 키에는 다음 권한 설정이 필요했습니다.

chmod 400 linux-practice-key.pem

### 이유

pem 키는 서버에 접속하기 위한 비밀키.

다른 사용자가 읽을 수 있는 상태라면 위험하기 때문에 SSH는 접속을 거부하는 경우가 있다.

따라서 소유자만 읽을 수 있는 400으로 설정합니다.

---

## 이번에 배운 내용

· 리눅스 권한
・r/w/x
・owner/group/other
· chmod
・숫자 권한
・777
・644
・400
· SSH 비밀 키의 권한 관리

---


# Linux Network Command 연습

## 52. 네트워크 확인 명령을 배우는 이유

서버 운용에서는 접속할 수 없는 원인을 조사하는 경우가 많다.

예:

· SSH 연결할 수 없음
· 웹 사이트가 열리지 않음
・80번 포트가 열려 있지 않다
· Node.js의 3000번 포트에 연결할 수 없음

이러한 문제를 조사하기 위해 네트워크 확인 명령을 사용합니다.

---

## 53. ip addr

사용한 명령:

ip addr

### 의미

ip는 Internet Protocol의 약자입니다.
addr은 address의 약자입니다.

즉, ip addr은 서버의 IP 주소 정보를 확인하는 명령입니다.

### 무엇을 할 것인가

서버가 가지고 있는 네트워크 인터페이스와 IP 주소를 표시합니다.

### 왜 사용합니까?

현재 서버의 Private IP 및 네트워크 정보를 확인하기 위해.

### 배운 것

EC2에서는 서버 내부에서는 Private IP가 사용된다.

예:

172.31.x.x

이것은 AWS 내부 네트워크에서 사용되는 IP 주소입니다.

---

## 54. hostname -I

사용한 명령:

hostname -I

### 의미

hostname은 host name, 즉 컴퓨터 이름을 의미합니다.
-I는 IP 주소를 표시하는 옵션입니다.

### 무엇을 할 것인가

현재 서버가 가지는 IP 주소만을 간단하게 표시한다.

### 왜 사용합니까?

ip addr의 결과는 길기 때문에, IP만을 신속하게 확인하고 싶을 때에 사용한다.

---

## 55. ss -tulpn

사용한 명령:

ss -tulpn

### 의미

ss는 socket statistics의 약자입니다.
네트워크 연결 및 포트 상태를 확인하는 명령.

선택적 의미:

· -t → TCP 표시
· -u → UDP 표시
· -l → LISTEN 상태의 포트 표시
· -p → 사용중인 프로세스 정보 표시
· -n → 포트 이름이 아닌 숫자로 표시

### 무엇을 할 것인가

현재 서버 내에서 어떤 프로그램이 어떤 포트를 사용하고 있는지 확인한다.

### 왜 사용합니까?

웹 서버와 Node.js 서버가 실제로 포트를 열고 기다리고 있는지 확인하기 위해.

예:

0.0.0.0:80

→ nginx가 80번 포트에서 대기 중

0.0.0.0:3000

→ Node.js 서버가 3000번 포트에서 대기 중

---

## 56. TCP 및 UDP

### TCP

TCP는 Transmission Control Protocol의 약자.

접속을 확인하면서 데이터를 확실하게 보내는 통신 방식.

### 특징

・연결 확인 있음
・데이터의 순서를 보증
· 신뢰성이 높음
・조금 늦어지는 경우가 있다

### 주요 용도

· HTTP
· HTTPS
· SSH
· 웹 애플리케이션

---

### UDP

UDP는 User Datagram Protocol의 약자입니다.

접속 확인을 하지 않고, 데이터를 신속하게 보내는 통신 방식.

### 특징

・연결 확인 없음
· 빠른
· 일부 데이터가 손실 될 수 있음
· 실시간 통신 방향

### 주요 용도

· 온라인 게임
· 음성 통화
・동영상 통화
· 실시간 배포

---

## 57. LISTEN이란?

ss -tulpn의 결과로 TCP는 LISTEN으로 표시 될 수 있습니다.

### 의미

LISTEN은 "연결을 기다리는 상태".

### 예제

0.0.0.0:80 LISTEN

이것은 80번 포트에서 접속을 기다리고 있다는 의미.

nginx나 Node.js 서버가 정상적으로 기동하고 있을 때 확인할 수 있다.

---

## 58. UNCONN이란?

UDP는 UNCONN으로 표시될 수 있다.

### 의미

UNCONN은 Unconnected의 약자입니다.

UDP는 TCP와 같이 연결을 만들어 통신하는 방식이 아니기 때문에 UNCONN으로 표시된다.

### 배운 것

TCP는 연결 기반이므로 LISTEN 상태가 있습니다.
UDP는 비 연결형이므로 UNCONN으로 표시됩니다.

---

## 59. curl

사용한 명령:

curl http://localhost

### 의미

curl은 종종 클라이언트 URL의 약자로 설명됩니다.
URL에 요청을 보내는 명령.

### 무엇을 할 것인가

브라우저를 사용하지 않고 터미널에서 웹 서버로 HTTP 요청을 보냅니다.

### 왜 사용합니까?

웹 서버가 정상적으로 응답하는지 확인하기 위해.

### 예제

curl http://localhost

→ 서버 자신에게 웹 요청 보내기

nginx가 정상이면 HTML이 반환됩니다.

---

## 60. 핑

사용한 명령:

ping google.com

### 의미

ping은 네트워크상의 상대가 응답하는지 확인하기 위한 명령입니다.

### 무엇을 할 것인가

지정한 상대에게 작은 신호를 보내, 응답이 돌아오는지 확인한다.

### 왜 사용합니까?

인터넷 접속이나 상대 서버에의 도달성을 확인하기 위해.

### 종료 방법

Ctrl+C

---

## 61. 0.0.0.0 의미

0.0.0.0은 "모든 네트워크 인터페이스에서 대기"라는 의미.

예:

0.0.0.0:3000

이것은, 서버가 가지는 모든 IP 주소로 3000번 포트에의 접속을 받아들인다고 하는 의미.

### 배운 것

Node.js 서버를 외부에서 접속 가능하게 하려면 , localhost 뿐만이 아니라 0.0. 0. 0 에서 기다려야 한다.

---

## 62. localhost의 의미

localhost는 "자신"을 의미합니다.

예:

curl http://localhost

이것은 서버가 자신의 웹 서버에 액세스하는 상태.

### 배운 것

localhost에서는 접속할 수 있어도, 외부 브라우저로부터 접속할 수 없는 경우는, 보안 그룹이나 라우팅등 외부 네트워크측에 문제가 있을 가능성이 높다.

---

## 이번에 배운 내용

· ip addr
· hostname -I
· ss -tulpn
· TCP
· UDP
・LISTEN
・UNCONN
・curl
・핑
· localhost
・0.0.0.0
・포트 확인
· 네트워크 소통 확인


---

63. Docker를 배우는 이유

Docker는 응용 프로그램을 실행하는 데 필요한 환경을 함께 패키징하고 어디서나 동일한 방식으로 실행할 수 있도록하는 기술입니다.

종래는 서버마다 Node.js나 nginx등을 개별적으로 인스톨하고 있었기 때문에, 환경의 차이에 의한 트러블이 발생하기 쉬웠다.

Docker를 이용함으로써 개발 환경과 프로덕션 환경을 같은 상태로 재현할 수 있다.

⸻

64. Docker란?

Docker는 컨테이너를 만들고 관리하기 위한 플랫폼이다.

Docker 자체가 어플리케이션을 실행하는 것이 아니라, 컨테이너를 관리하는 역할을 가진다.

이미지에서 컨테이너를 만들고 실행, 중지 및 삭제할 수 있습니다.

⸻

65. Image와 Container

Docker를 이해하는 가장 중요한 개념.

이미지

Image는 애플리케이션을 실행하기 위한 설계도이다.

필요한 프로그램과 설정이 포함되어 있지만 아직 실행되지 않은 상태.

예:

nginx 이미지

⸻

Container

Container는 Image를 실제로 실행한 상태이다.

Image에서 여러 Container를 만들 수 있습니다.

예:

nginx Container

⸻

배운 것

Image = 설계도

Container = 설계도로 만든 실행중인 엔티티

⸻

66. docker pull

사용한 명령:

docker pull nginx

의미

pull은 「취득한다」라고 하는 의미.

Docker Hub에서 nginx Image를 다운로드합니다.

배운 것

Container를 만들기 전에 Image가 필요합니다.

⸻

67. docker images

사용한 명령:

docker images

의미

다운로드된 이미지 목록을 표시합니다.

왜 사용하는가

현재 어느 Image가 있는지 확인하기 위하여.

⸻

68. docker run

사용한 명령:

docker run -d -p 8080:80 nginx

의미

nginx Image에서 Container를 만들고 실행합니다.

-d

detached의 약어.

백그라운드 실행.

-p

port의 약자.

포트를 연결합니다.

8080:80

EC2의 8080번 포트와 Container 내의 80번 포트를 연결한다.

배운 것

Container 내부와 외부 서버의 포트는 다르다.

⸻

69. docker ps

사용한 명령:

docker ps

의미

실행중인 컨테이너 목록을 표시합니다.

배운 것

Linux의 ps 명령과 비슷하지만 Docker Container 전용 명령입니다.

⸻

70. docker ps 결과 읽기

예:

CONTAINER ID

IMAGE

STATUS

PORTS

NAMES

CONTAINER ID

Container 특정 식별 번호

IMAGE

Container를 만들 때 사용한 이미지

STATUS

현재 상태

예:

Up

↓

실행 중

PORTS

포트 매핑 정보

예:

0.0.0.0:8080->80/tcp

의미:

EC2의 8080번 포트

↓

Container의 80번 포트

NAMES

Docker가 자동으로 생성한 Container 이름

⸻

71. docker stop

사용한 명령:

docker stop 컨테이너 이름

의미

실행 중인 Container를 중지합니다.

배운 것

Container를 중지해도 Image는 삭제되지 않는다.

⸻

72. docker start

사용한 명령:

docker start 컨테이너 이름

의미

중지된 Container를 다시 실행합니다.

⸻

73. docker ps -a

사용한 명령:

docker ps -a

의미

실행중뿐만 아니라 정지된 Container도 표시한다.

배운 것

Container는 정지해도 삭제되는 것은 아니다.

⸻

74. docker rm

사용한 명령:

docker rm 컨테이너 ID

의미

Container를 삭제합니다.

배운 것

Container를 삭제해도 Image는 남는다.

⸻

75. Docker에서 가장 중요한 이해

이번 학습으로 이해한 것.

Image와 Container는 별개이다.

Image는 설계도.

Container는 실제로 동작하는 실체.

Container를 삭제해도 Image는 남아 있기 때문에, 같은 Image로부터 몇번이라도 새로운 Container를 작성할 수 있다.

⸻

이번에 배운 내용

· 도커의 목적

· 도커의 기본 개념

· 이미지

· Container

· 도커 풀

· 도커 이미지

· 도커 실행

· 도커 ps

· 도커 ps -a

・docker stop

· 도커 시작

· 도커 rm

・포트 매핑(8080:80)

· Image와 Container의 차이
