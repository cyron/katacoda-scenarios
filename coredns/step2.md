CoreDNSのデフォルト設定ファイルは「Corefile」となり、それを作成すれば、毎回実行するときに「-conf」を指定しなくてもいい。
また、DNSサーバをテストするために、digなどのツールをインストール必要がある。

`yum -y install bind-utils`{{execute}}

# 最小限の設定ファイルを作成
まずCorefileを作成し、すべての名前解決を「/etc/resolv.conf」に記載されているDNSサーバ(8.8.8.8)に委譲する。

<pre class="file" data-filename="Corefile" data-target="replace">. {
    forward . /etc/resolv.conf
    log
}
</pre>