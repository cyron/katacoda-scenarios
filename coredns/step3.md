「openshift.example.com」というドメインの正引きレコードを作成する。

# 正引きゾーンを作成
Corefileに以下の内容を追記し、ドメイン「openshift.example.com」の名前解決を「openshift.example.com.db」というゾーンファイルに記載する。

<pre class="file" data-filename="Corefile" data-target="append">. {
    openshift.example.com {
        file ~/openshift.example.com.db
    }
}
</pre>

# ゾーンファイルを作成
CoreDNSのゾーンファイルの記述方法は、Bindとほぼ同じである。
以下のリンクをクリックし、Corefileを作成する。

`openshift.example.com.db`{{open}}

openshift.example.com.dbに以下の内容を追記する。

<pre class="file" data-filename="openshift.example.com.db" data-target="append">. {
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
}
</pre>
