# Beginning Network Addressing PT Activity

https://danscourses.com/beginning-network-addressing-pt-activity/

## PC-A

- 適当に IP 設定をしていく。
- DHCP での再配信なら `ipconfig /release` が必要だが、今回は static な設定なので即時反映される。

## S2

知らなかったので Switch に対する IP アドレスに関する知識をおさらいしとく。

* スイッチは L2 の機器だけど管理用に IP アドレスを付与するのが一般的
* Catalyst スイッチは管理用として内部に仮想インターフェイスを持っていて、その仮想インターフェイスに IP アドレスを設定する
* 仮想インターフェイスは VLAN 毎に作成出来るが、アクティブにできるのは一つの仮想インターフェイスだけ
* このアクティブな VLAN を 管理 VLAN と呼ぶ
* デフォルトでは管理 VLAN は VLAN1 となっている

http://jukenki.com/contents/cisco/ccna-lab-scenario/lab1-switch-ip-address-setting.html

```
Switch#show running-config // sh run
!
interface Vlan1
 no ip address
 shutdown
!
```

初期状態では何も設定されていない

```
Switch#sh vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/1, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active
````

全てのポートが VLAN1 に所属している状態らしい (このポートから設定した仮想インターフェイスに接続できるという意味)。

```
Switch# configure terminal // conf t
Switch(config)# host S1
S1(config)# int vlan 1
S1(config-if)# ip add 192.168.1.2 255.255.255.0
S1(config-if)# no shut
```

指示された通りホスト名と仮想インターフェイス (vlan 1) の構成を行う。

## Router

```
# sh run
!
interface GigabitEthernet0/0
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
```

まずは g0/0 側から構成する。

```
# enable
# conf t
# hostname R1
# int g0/1
# ip address 192.168.1.1 255.255.255.0
# no shut
```

この時点で PC-A とデフォゲは疎通が出来るはずなので、ping を飛ばしてみる。

```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time=1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time=3ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 3ms, Average = 1ms
```

## 反対側も同じ用に R1/S2/PC-B

今回の場合、反対側も構成したら PC-A/B 間で疎通ができた。
これは、R1 が対向側のネットワークを直接知っていたから。