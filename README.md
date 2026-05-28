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
