「openshift.example.com」というドメインの逆引きレコードを作成する。

# 逆引きゾーンを作成
Corefileに以下の内容を追記し、ドメイン「openshift.example.com」の逆引きレコードを逆引きゾーンファイルに記載する。

<pre class="file" data-filename="Corefile" data-target="append">
10.0.2.0/24 {
    file reverse.zone
}
</pre>

# ゾーンファイルを作成
以下のリンクをクリックし、ゾーンファイルを作成する。

`reverse.zone`{{open}}

10.0.2.dbに以下の内容を追記する。

<pre class="file" data-filename="reverse.zone" data-target="replace">
$TTL 86400
@    IN SOA dns.openshift.example.com. root.localhost (
                                2017042745 ; serial
                                7200       ; refresh (2 hours)
                                3600       ; retry (1 hour)
                                1209600    ; expire (2 weeks)
                                3600       ; minimum (1 hour)
                                )


@                IN      NS     dns.openshift.example.com.

99               IN      PTR    dns.openshift.example.com.
11               IN      PTR    master.openshift.example.com.
12               IN      PTR    node1.openshift.example.com.
13               IN      PTR    node2.openshift.example.com.
</pre>

# 検証
上記作成した設定ファイルでDNSサーバを起動する。

`./coredns`{{execute T1}}

別のターミナルを起動し、IPアドレス「10.0.2.11」でドメイン「master.openshift.example.com」を逆引きする。

`nslookup 10.0.2.11`{{execute T2}}

最後に以下のリンクをクリックし、、CoreDNSを停止する。

`echo "Stop CoreDNS"`{{execute T1 interrupt}}
