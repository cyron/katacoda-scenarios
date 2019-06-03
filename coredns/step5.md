最後に、CoreDNSをSystemdでサービス化する。

CoreDNSのバイナリファイルを「/usr/local/bin」にコピーする。

`cp ./coredns /usr/local/bin/`{{execute}}

「/etc/coredns」フォルダを作成し、設定ファイルとゾーンファイルをコピーする。

`mkdir /etc/coredns`{{execute}}

`cp Corefile openshift.example.com.db 10.0.2.db /etc/coredns`{{execute}}

Corefileを以下のように修正する。

<pre class="file" data-filename="Corefile" data-target="replace">
openshift.example.com {
    file /etc/coredns/openshift.example.com.db
}

10.0.2.0/24 {
    file /etc/coredns/10.0.2.db
}

. {
    forward . /etc/resolv.conf
    log
    errors
    cache
}
</pre>

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

サービスを有効化し、起動する。

`systemctl enable coredns`{{execute}}

`systemctl start coredns`{{execute}}

`systemctl status coredns`{{execute}}
