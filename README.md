# nakalab_docker_container `Ubuntu Humble`
　ROS2 Humble 用 Docker イメージビルダー。Ubuntu で使用することが前提となっています。

## QuickStart
- CPU のみ使用する場合
```bash
docker compose up -d --build humble-cpu
```

- GPU を使用する場合
    使用するコンピューターに Nvidia 製 GPU が搭載されていることが前提です。
```bash
docker compose up -d --build humble-gpu
```

## Option
　コンテナのビルドに対していくつかのオプションを用意しています。ここではあなたの環境に沿ったコンテナを構築するためのオプションを段階的に紹介します。

---

　はじめに [`.env`](/.env) を開き、コンテナ内で使用するユーザー設定を行います。**`USER_NAME`**　と **`GROUP_NAME`** にお好きな名前を入力してください。デフォルトは `nakalab` です。<br>
　また、`WS_NAME` の名前を変更することでコンテナ内に生成されるワークスペース名を設定することができます。<br>
　念の為 `PASSWORD` にもあなたのパスワードを設定するといいでしょう。しかしながらコンテナ内でパスワードが要求されることはありません。

```

　つぎに **`ROS_DOMAIN_ID`** を設定します。デフォルトではコメントアウトされていますが、お使いのコンピューターのホスト上に ROS2 がインストールされていない場合やホスト上で ROS_DOMAIN_ID の設定を行っていない場合はこのコメントを解除してこの変数が使用できるようにしてください。

```

　[`ws_requirement.sh`](/ws_requirement.sh) にインストールしたいパッケージをコマンドで記述できます。このファイルは試験的に導入したのですが、このファイルに書かれているサンプルコードを参考にしてワークスペースにインストールしたいパッケージを記述してください。別の手段として `pkg` ディレクトリにクローンする方法もありますが、前述の手法を使用したほうが **ビルド時間が短縮されます** 。

```　

　最後に細かい設定などは [`docker-compose.yaml`](/docker-compose.yaml) を参照してください。各サービスからビルドされるイメージ名などもお好きな名前にすると管理がしやすいでしょう。
また、Dockerfile を編集したい場合 GPU、CPU オプションで分割されていますので Dockerfile の拡張子に注目してください。

---

 最後にコンテナをビルドします。以下のコマンドを実行すればビルドからコンテナ内へのエントリーができます。
以下のコマンドは CPU 版のコンテナを起動しています。
```bash
docker compose up -d humble-cpu ;docker compose exec humble-cpu /bin/bash
```

## トラブルシューティング
### `WARN[0000] The "ROS_DOMAIN_ID" variable is not set. Defaulting to a blank string.` と表示される
　この警告が表示されていることはホスト上で ROS_DOMAIN_ID が定義されていないことを示しています。この状態では **コンテナ内のノードが通信することができません** 。そのためホスト上でこの変数をエクスポートするか [`.env`](/.env) を編集してこの変数を有効化してください。

### `.../socket .... がない`、などの `PULSE_SERVER` 関連のエラーが発生する
　ホスト上で pulseaudio をインストールしてください。
```bash
sudo apt install -y pulseaudio
```
