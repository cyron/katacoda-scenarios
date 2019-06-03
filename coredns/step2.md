CoreDNSのデフォルト設定ファイルは「Corefile」となり、実行するときに「-conf」で指定する必要がある。
また、DNSサーバをテストするために、digなどのツールをインストール必要がある。

`yum -y install bind-utils`{{execute}}

# 最小限の設定ファイルを作成
以下のリンクをクリックし、Corefileを作成する。

`Corefile`{{open}}

Corefileに以下の内容を追記し、すべての名前解決を「/etc/resolv.conf」に記載されているDNSサーバ(8.8.8.8)に委譲する。

<pre class="file" data-filename="Corefile" data-target="append">. {
    forward . 8.8.8.8
}
</pre>

「/etc/resolv.conf」を以下のコマンドで修正し、ローカルホスト(CoreDNS)を参照する。

`echo "nameserver localhost" > /etc/resolv.conf`{{execute}}

# CoreDNSを起動
上記作成した設定ファイルでDNSサーバを起動する。

`./coredns`{{execute}}

# digで検証
まず、別のターミナルを起動してください。

次はCoreDNSに対して解決する。

`dig +noall +answer google.com A`{{execute T2}}

最後にCoreDNSを停止する。

`echo "Send Ctrl+C before running Terminal"`{{execute T1 interrupt}}
