以下の手順でCoreDNSをダウンロードし、インストールする。

# ダウンロード
まずGitHubからCoreDNSのバイナリをダウンロードする。

`curl -LO https://github.com/coredns/coredns/releases/download/v1.5.0/coredns_1.5.0_linux_amd64.tgz`{{execute}}

# 解凍
次は解凍し、パスを通す場所に配置する。

`tar -zxvf coredns_1.5.0_linux_amd64.tgz`{{execute}}

# 確認
最後CoreDNSが実行できるかを確認する。

`./coredns -version`{{execute}}
