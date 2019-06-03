「openshift.example.com」というドメインの正引きレコードを作成する。

# 正引きゾーンを作成
Corefileに以下の内容を追記し、ドメイン「openshift.example.com」の名前解決を「example.zone」というゾーンファイルに記載する。

<pre class="file" data-filename="Corefile" data-target="append">
openshift.example.com {
    file example.zone
}
</pre>

# ゾーンファイルを作成
CoreDNSのゾーンファイルの記述方法は、Bindとほぼ同じである。
以下のリンクをクリックし、ゾーンファイルを作成する。

`example.zone`{{open}}

example.zoneに以下の内容を追記する。

<pre class="file" data-filename="example.zone" data-target="replace">
$TTL 86400
$ORIGIN openshift.example.com.
@       IN      SOA     dns root.localhost (
                                2017042745 ; serial
                                7200       ; refresh (2 hours)
                                3600       ; retry (1 hour)
                                1209600    ; expire (2 weeks)
                                3600       ; minimum (1 hour)
                                )

@                IN      NS     dns
dns              IN      A      10.0.2.99
master           IN      A      10.0.2.11
node1            IN      A      10.0.2.12
node2            IN      A      10.0.2.13
</pre>

# 検証
上記作成した設定ファイルでDNSサーバを起動する。

`./coredns`{{execute}}

別のターミナルを起動し、ドメイン「master.openshift.example.com」を解決してみましょう。

`dig @localhost +noall +answer master.openshift.example.com A`{{execute}}

最後に一つ目のターミナルに戻って、CoreDNSを停止する。
