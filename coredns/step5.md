最後に、CoreDNSをSystemdでサービス化する。
残念ながら、KataCoda上ではSystemdが起動できなさそうだ。。。

# 設定ファイルを修正
Corefileを以下のように修正する。

<pre class="file" data-filename="Corefile" data-target="replace">
openshift.example.com {
    file /etc/coredns/example.zone
}

10.0.2.0/24 {
    file /etc/coredns/reverse.zone
}

. {
    forward . 8.8.8.8
    log
    errors
    cache
}
</pre>

# バイナリファイルと設定ファイルを配置
CoreDNSのバイナリファイルを「/usr/local/bin」にコピーする。

`cp ./coredns /usr/local/bin/`{{execute}}

「/etc/coredns」フォルダを作成し、設定ファイルとゾーンファイルをコピーする。

`mkdir /etc/coredns`{{execute}}

`cp Corefile example.zone reverse.zone /etc/coredns`{{execute}}

# ユニットファイルを作成
以下のリンクをクリックし、ユニットファイルを作成する。

`coredns.service`{{open}}

<pre class="file" data-filename="coredns.service" data-target="append">
[Unit]
Description=CoreDNS

[Service]
EnvironmentFile=
ExecStart=/usr/local/bin/coredns -conf /etc/coredns/Corefile

[Install]
WantedBy=multi-user.target
</pre>

ユニットファイルを「/etc/systemd/system」にコピーする。

`cp coredns.service /etc/systemd/system/`{{execute}}

# サービスを起動
サービスを有効化し、起動する。

`systemctl enable coredns`{{execute}}

`systemctl start coredns`{{execute}}

`systemctl status coredns`{{execute}}
