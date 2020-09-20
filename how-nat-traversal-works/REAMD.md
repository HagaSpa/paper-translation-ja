# How NAT traversal works
https://tailscale.com/blog/how-nat-traversal-works/

## Figuring out firewalls
* ファイアウォールに機能を持つサービスとして以下のようなものがある。
    * Windows Defender firewall
    * Ubuntu’s ufw (using iptables/nftables)
    * BSD’s pf (also used by macOS)
    * AWS  Security Groups
* これらの設定としてはinboundは全てブロックし、outboundは全て許可するのが一般的。
    * inboundはファイアウォール外から内へのアクセス
    * outboundはファイアウォール内から外へのアクセス
    * 例外としてinboundのSSHのみ許可する場合などもよくある。（現職でもそう）
* この概念はあくまでプロトコル上のものであり、実際のネットワーク上では双方向にアクセスが生まれている。
    * 物理的にファイアウォールがあるわけではなくて、２つの機器間での双方向の通信の間に論理的な概念としてファイアウォールがある。
* そのため各アクセスがinboundか？outboundか？はどのようにしてファイアウォールは判断するのか？
* これを解決するためにStateFul Firewallがある。
    * これは過去のパケットを記憶しており、その情報を用いてinboundかoutboundを判断できたり、各通信に対するコントロールが可能である。
* UDPをコントロールする場合以下を行う
    * 過去にあるアドレス:ポートがoutboundとして通信を行なっていた場合、その宛先からの戻りをinboundとして許可する。
    * あまり見ないが誰でもinboundとして通信を行ったアドレスと会話できる場合もある。


## Firewall face-off
