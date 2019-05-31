CoreDNSのデフォルト設定ファイルは「Corefile」となり、実行するときに「-conf」で指定する必要がある。
また、DNSサーバをテストするために、digなどのツールをインストール必要がある。

`yum -y install bind-utils`{{execute}}

# 最小限の設定ファイルを作成
Corefileを作成し、すべての名前解決を「/etc/resolv.conf」に記載されているDNSサーバ(8.8.8.8)に委譲する。

<pre class="file" data-filename="Corefile" data-target="append">. {
    forward . /etc/resolv.conf
}
</pre>

# CoreDNSを起動
上記作成した設定ファイルでDNSサーバを起動する。

`nohup coredns -quiet >/dev/null 2>&1 &`{{execute}}

# digで検証
まず、システムのデフォルト設定で「google.com」をdigで解決する。

`dig +noall +answer google.com A`{{execute}}

次はCoreDNSに対して解決する。

`dig @localhost +noall +answer google.com A`{{execute}}