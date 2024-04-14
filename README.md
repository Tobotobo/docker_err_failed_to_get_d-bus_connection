# docker_err_failed_to_get_d-bus_connection

## 概要
* Docker のコンテナ内で `systemctl` を使いたい。
* しかし、実行すると `Failed to get D-Bus connection: No such file or directory` エラーが発生する。
* 本リポジトリでは、上記に対する対応方法を自分用に残していく。

## 結論

* できなかった。単に複数のサービス起動したいだけなら supervisord を使おう（あきらめ）
* ↓　みたいに書いてみたけどもーよーわからん
  compose.yml
  ```
  services:
    dev:
      build: .
      volumes:
        - /sys/fs/cgroup:/sys/fs/cgroup
      privileged: true
      cgroup: host
  ```

### supervisord 使う場合のサンプルおいておく

Dockerfile
```
# Install supervisor
RUN apt-get update && apt-get install -y supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
```

supervisord.conf
```
[supervisord]
nodaemon=true

[program:sshd]
command=/usr/sbin/sshd -D
autostart=true
autorestart=true

[program:dockerd]
command=/usr/bin/dockerd
autostart=true
autorestart=true
```


## 参考サイト

[コンテナで`systemctl`を叩いたら`Failed to get D-Bus connection: No such file or directory`と言われた](https://qiita.com/fukuchi0527/items/a4bcc7a6876379582f35)

[Docker for mac (ver 4.3以降) で「Failed to get D-Bus connection」エラーが出たときの対処法](https://qiita.com/NaoyaMiyagawa/items/22a870112377a91e1c99)
