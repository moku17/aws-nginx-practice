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
