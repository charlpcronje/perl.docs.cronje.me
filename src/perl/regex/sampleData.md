---
title: Sample Data for Regex | CRONje.ME
label: Sample Data for Regex
order: 30
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

> Here is the sample data that I used to create the example referenced here at *2. Example:* [Regular Expressions for SSH Agent](regexForSSHAgent.md)

```shell
FortiWeb commands

FortiWeb $ get system status
International Version: FortiWeb-VM 6.39,build1117(GA),201125
Serial-Number: FVVM010000201278
license type: remote
Bios version: 04000002
Log hard disk: Available
Hostname: FortiWeb
Operation Mode: Reverse Proxy
FIPS-CC mode: disabled
Current HA mode: standalone
Database Status: Available


FortiWeb $ get system ha
mode                : standalone

FortiWeb $ get system ha-status
HA is disabled

FortiWeb $ get system performance
CPU states:    12% used, 88% idle
Memory states: 81% used
Up:            7 days,  8 hours,  44 minutes.

FortiWeb $ diagnose policy detail-stats list
<policy name>    the policy name
root.isa


FortiWeb $ diagnose policy detail-stats list root.isa
policy(isa)
worker num(1)
worker[0]
        accept_fd:51579
        connect_fd:44875
        closefd:        c2s:51562 s2c:44859
        epollerr:       c2s:1479 s2c:19387
        recverr:        c2s:1 s2c:0
        senderr:        c2s:1 s2c:0
        recv_bytes:     c2s:75496633 s2c:5228718006
        send_bytes:     c2s:1577475441 s2c:73023095
        ssl_accept:92672
        ssl_connect:0
        ssl_handleshake_err:    c2s:1709 s2c:0
        ssl_recverr:    c2s:1 s2c:0
        ssl_senderr:    c2s:1 s2c:0
        session_live:51579
        http_request:187159
        http_response:187155
        waf_accept:1091026
        waf_deny:3885
        waf_followup:181905

FortiWeb $ diagnose policy total-detail-stats list
<policy name>    the policy name
root.isa


FortiWeb $ diagnose policy total-detail-stats list root.isa
policy(isa)

accept_fd
           0             2             2             0             8             6             0             5             1             7
connect_fd
           0             1             2             0             5             6             0             4             1             7
ssl_accept
           0             3             4             0            10            12             0             9             2            14
ssl_connect
           0             0             0             0             0             0             0             0             0             0
session_live
           0             2             2             0             8             6             0             5             1             7
http_request
           5            13            11             7            16            20             6            16             4            46
http_response
           5            13            12             7            14            22             6            14             7            45
waf_accept
          13            36            30            19           150            59            15            39            14           128
waf_deny
           0             0             0             0             3             0             0             0             0             0
waf_followup
           5            13            12             7             8            22             6            14             7            45
closefd Client to FWB
           2             3             2             3             7             3             1             4             1             4
closefd FWB to Server
           2             2             2             3             4             3             1             3             1             4
epollerr Client to FWB
           0             0             0             0             0             1             0             0             0             0
epollerr FWB to Server
           2             2             0             3             0             0             1             0             1             2
recv_err Client to FWB
           0             0             0             0             0             0             0             0             0             0
recv_err FWB to Server
           0             0             0             0             0             0             0             0             0             0
send_err Client to FWB
           0             0             0             0             0             0             0             0             0             0
send_err FWB to Server
           0             0             0             0             0             0             0             0             0             0
recv_bytes Client to FWB
        1929          5137          4337          2787          6880          8133          2230          6158          1612         18128
recv_bytes FWB to Server
        3977          9077          9255          5137        329403         13722          5097         48314          4600       6351060
send_bytes Client to FWB
        4587         10778         10739          6221        331790         16866          5829         50348          5569        966959
send_bytes FWB to Server
        1929          5137          4280          2787          5186          8133          2230          6044          1612         18128
ssl_hskerr Client to FWB
           0             0             0             0             0             0             0             1             0             0
ssl_hskerr FWB to Server
           0             0             0             0             0             0             0             0             0             0
ssl_recverr Client to FWB
           0             0             0             0             0             0             0             0             0             0
ssl_recverr FWB to Server
           0             0             0             0             0             0             0             0             0             0
ssl_senderr Client to FWB
           0             0             0             0             0             0             0             0             0             0
ssl_senderr FWB to Server
           0             0             0             0             0             0             0             0             0             0


FortiWeb $ diagnose policy traffic list root.isa
policy(isa)
total = 60
           0             0           581             0             0             0
           0             0             0             0             0             0
           0             0             0           579             0             0
         447             0             0             0             0             0
           0             0             0             0             0             0
           0             0             0             0          1880             0
           0             0             0          1737          8684           706
           0             0             0             0             0             0
           0             0             0             0             0             0
         581          2347           705             0             0             0

throughput is 0 Bytes per second


FortiWeb $ diagnose policy total-conn-psec list
total = 60
         1           0           0           0           1           0
         0           0           0           0           0           0
         0           0           0           0           0           1
         0           0           0           0           0           0
         0           0           1           0           0           0
         0           0           0           0           0           1
         0           0           1           0           0           0
         0           0           0           0           0           0
         0           0           0           0           0           0
         0           0           0           0           0           0

throughput is 0 conns per second

FortiWeb $ get system interface
 name    interface name
 port1
 port2
 port3
 port4
 port5
 port6
 port7
 port8
 port9
 port10


FortiWeb $ get system interface port1
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : down
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port2
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : up
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port3
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : down
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port4
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : up
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port5
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : down
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port6
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : up
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port7
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : down
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port8
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : up
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port9
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : down
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ get system interface port10
type                : physical
ip                  : 192.168.4.150/25
ip6                 : ::/0
allowaccess         : ping ssh http https
status              : up
mode                : static
ip6-mode            : static
description         :
ip6-allowaccess     :
wccp                : disable
azure-endpoint      : 0
mtu                 : 1500
dynamic_gateway     :
dynamic_dns1        :
dynamic_dns2        :


FortiWeb $ diagnose hardware mem list
MemTotal:        4038456 kB
MemFree:          336492 kB
MemAvailable:     528248 kB
Buffers:          108196 kB
Cached:           255728 kB
SwapCached:            0 kB
Active:          2747408 kB
Inactive:         227068 kB
Active(anon):    2612296 kB
Inactive(anon):   146288 kB
Active(file):     135112 kB
Inactive(file):    80780 kB
Unevictable:        1140 kB
Mlocked:            1140 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:               108 kB
Writeback:             0 kB
AnonPages:       2611716 kB
Mapped:            58436 kB
Shmem:            148000 kB
KReclaimable:      16624 kB
Slab:              85684 kB
SReclaimable:      16624 kB
SUnreclaim:        69060 kB
KernelStack:        4032 kB
PageTables:        12536 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     2019228 kB
Committed_AS:    4936780 kB
VmallocTotal:   34359738367 kB
VmallocUsed:        6380 kB
VmallocChunk:          0 kB
Percpu:              496 kB
DirectMap4k:      157632 kB
DirectMap2M:     4036608 kB


FortiWeb $ diagnose hardware nic list
lo
gre0
port1
port2
port3
port4
port5
port6
port7
port8
port9
erspan0
gretap0
port10
port_tn

FortiWeb $ diagnose hardware nic list
<nic>    the network interface name


FortiWeb $ diagnose hardware nic list port1
driver                                  vmxnet3
version                                 1.4.17.0-k-NAPI
firmware-version
bus-info                                0000:03:00.0

Supported ports                         TP
Supported link modes                    1000baseT/Full
                                        10000baseT/Full
Supports auto-negotiation:              No
Advertised link modes:                  Not reported
Advertised auto-negotiation:            No

Speed:                                  10000Mb/s
Duplex:                                 Full
Port:                                   Twisted Pair
PHYAD                                   0
Transceiver:                            internal
Auto-negotiation                        off
Supports Wake-on                        uag
Wake-on                                 d
Link detected                           yes

Link encap                              Ethernet
HWaddr                                  00:0C:29:AA:CF:E0
INET addr                               192.168.4.150
Bcast                                   192.168.4.255
Mask                                    255.255.255.128
FLAG                                    UP BROADCAST RUNNING MULTICAST
MTU                                     1500
MEtric                                  1
Outfill                                 538976266
Keepalive                               538976288

RX packets                              3922557
RX errors                               0
RX dropped                              107
RX overruns                             0
RX frame                                0
TX packets                              5227882
TX errors                               0
TX dropped                              0
TX overruns                             0
TX carrier                              0
TX collisions                           0
TX queuelen                             1000
RX bytes                                5947528926 (5672.0 Mb)
TX bytes                                5807463565 (5538.4 Mb)
Adaptive RX                             off
Adaptive TX                             off
stats-block-usecs                       0
sample-interval                         0
pkt-rate-low                            0
pkt-rate-high                           0
rx-usecs                                0
rx-frames                               0
rx-usecs-irq                            0
rx-frames-irq                           0
tx-usecs                                0
tx-frames                               0
tx-usecs-irq                            0
tx-frames-irq                           0


FortiWeb $ diagnose hardware harddisk list
name    size(M)
sda      34359.74

FortiWeb $ diagnose hardware logdisk info
disk number: 1
disk[0] size: 33.55GB
partition number: 1
mount status: read-write

FortiWeb $ diagnose hardware cpu

FortiWeb $ diagnose hardware cpu list
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2407 0 @ 2.20GHz
stepping        : 7
microcode       : 0x70d
cpu MHz         : 2200.000
cache size      : 10240 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc cpuid aperfmperf pni pclmulqdq ssse3 cx16 sse4_1 sse4_2 popcnt aes hypervisor lahf_lm xsaveopt dtherm arat pln pts
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit
bogomips        : 4400.00
clflush size    : 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
power management:


FortiWeb $ get router statuc
Parsing error at 'statuc'. err=1
Command fail. CLI parsing error.

FortiWeb $ get router static
== [ 1 ]
        dst: 0.0.0.0/0
        gateway: 192.168.4.129


FortiWeb $ diagnose network ip list
lo: 127.0.0.1/24
port1: 192.168.4.150/25
port1: 192.168.4.160/32
port1: 192.168.4.151/25
lo: ::1/128
port1: fe80::20c:29ff:feaa:cfe0/64
port5: fe80::20c:29ff:feaa:cf08/64
port9: fe80::20c:29ff:feaa:cf30/64
port2: fe80::20c:29ff:feaa:cfea/64
port6: fe80::20c:29ff:feaa:cf12/64
port10: fe80::20c:29ff:feaa:cf3a/64
port3: fe80::20c:29ff:feaa:cff4/64
port7: fe80::20c:29ff:feaa:cf1c/64
port4: fe80::20c:29ff:feaa:cffe/64
port8: fe80::20c:29ff:feaa:cf26/64
port_tn: fe80::872:f3ff:fe00:ae2f/64
```
