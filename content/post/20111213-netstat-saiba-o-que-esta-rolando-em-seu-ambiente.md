---
title: "Netstat Saiba O Que Esta Rolando Em Seu Ambiente"
date: 2011-12-13T11:01:23-05:00
draft: false
image: "/img/cyberspace.jpg"
tags: [
"Impressões",
"Linux",
"Redes",
"Segurança",
"Software Livre",
]
description: "Que tal saber um pouco mais sobre o que lhe cerca virtualmente? Conexões de rede às quais sua máquina está ligada, tabelas de roteamento, estatísticas de interfaces, conexões mascaradas, multicasting, etc..?"
---
**S**audações pessoal,

**Q**ue tal saber um pouco mais sobre o que lhe cerca virtualmente? Conexões de rede às quais sua máquina está ligada, tabelas de roteamento, estatísticas de interfaces, conexões mascaradas, multicasting, etc.. ?

**A**pesar de muitos conhecerem o netstat, poucos sabem que ele é capaz de tudo isso e mais um pouco. O netstat é sem sombra de dúvidas uma rica ferramenta que possui inúmeros comandos e combinações que sequer cabem em um artigo simples como este post.

**A** ideia vai ser apenas apresentar algumas opções que podem ser bem interessantes no dia a dia de um administrador de redes/sysadmin.

**V**amos lá...

**1.** **Listar todas as portas, incluindo portas que estão sendo escutadas e portas que não estão:**

**1.1** Para listar TODAS as portas, podemos utilizar o parâmetro -a:

```
[root@tuxcaverna ~]# netstat -a
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 localhost.localdo:55481 *:*                     LISTEN
tcp        0      0 *:60634                 *:*                     LISTEN
tcp        0      0 *:6881                  *:*                     LISTEN
tcp        0      0 172.16.1.6:46714        gru03s08-in-f20.1:https TIME_WAIT
tcp        0      0 172.16.1.6:40899        gx-in-f138.1e1:www-http ESTABLISHED
tcp        0      0 localhost.localdo:48742 localhost.localdo:55481 ESTABLISHED
tcp        0      0 172.16.1.6:48643        125-233-152-234.d:21990 TIME_WAIT
tcp        0      0 172.16.1.6:46717        gru03s08-in-f20.1:https TIME_WAIT
tcp        0      0 172.16.1.6:57293        apache2-fungi.:www-http TIME_WAIT
tcp        0      0 172.16.1.6:36444        gru03s05-in-f22.1:https ESTABLISHED
tcp        0      0 172.16.1.6:57659        gru03s06-in-f1:www-http ESTABLISHED
tcp        0      0 localhost.localdo:55481 localhost.localdo:48742 ESTABLISHED
tcp        0      0 172.16.1.6:50581        gru03s06-in-f23.1:https ESTABLISHED
tcp        0      0 172.16.1.6:39979        sn1msg1010828.phx.:msnp ESTABLISHED
...
```

**N**ão coloquei a saída inteira, pois era bem extensa e ainda temos muitos parâmetros para ver. Onde existe XXX.XX.X.XX, obviamente, era o endereço IP que ocultei por puro protesto pela alta no preço do **amendoim**.

**1.2** Para listar todas as portas UDP, utilizamos os parâmetros -au:

```
[root@tuxcaverna ~]# netstat -au

Active Internet connections (servers and established) Proto Recv-Q Send-Q Local Address Foreign Address State udp 0 0 *:bootpc *:* udp 0 0 *:49119 *:* udp 0 0 *:mdns *:*
```

**1.3** Para listar todas as portas TCP, utilizamos os parâmetros -at:

```
[root@tuxcaverna ~]# netstat -at
Active Internet connections (servers and established) Proto Recv-Q Send-Q Local Address Foreign Address State tcp 0 0 localhost.localdo:39307 *:* LISTEN tcp 0 0 *:64758 *:* LISTEN tcp 0 0 XXX.XX.X.XX:54293 gru03s06-in-f21.1:https ESTABLISHED tcp 1 0 XXX.XX.X.XX:58732 sn1msg3020104.sn1.:msnp CLOSE_WAIT
```

**2.** **Listar os Sockets que estão no estado Listening ou escuta:**

**2.1** Para listar todas no estado Listening, utilizamos o parâmetro -l:

```
[root@tuxcaverna ~]# netstat -l

Active Internet connections (only servers) Proto Recv-Q Send-Q Local Address Foreign Address State tcp 0 0 localhost.localdo:39307 *:* LISTEN tcp 0 0 *:64758 *:* LISTEN Active UNIX domain sockets (only servers) Proto RefCnt Flags Type State I-Node Path unix 2 [ ACC ] STREAM LISTENING 10443 @/tmp/.ICE-unix/1396 unix 2 [ ACC ] STREAM LISTENING 8262 /var/run/xdmctl/dmctl/socket unix 2 [ ACC ] STREAM LISTENING 8277 /var/run/xdmctl/dmctl-:0/socket
```

**2.2** Para listar apenas as portas TCP no estado Listening, utilizamos os parâmetros -lt:

```
[root@tuxcaverna ~]# netstat -lt

Active Internet connections (only servers) Proto Recv-Q Send-Q Local Address Foreign Address State tcp 0 0 localhost.localdo:39307 *:* LISTEN tcp 0 0 *:64758 *:* LISTEN
```

**2.3** Para listar apenas as portas UNIX em estado Listening, utilizamos os parâmetrox -lx:

```
[root@tuxcaverna ~]# netstat -lx

Active UNIX domain sockets (only servers) Proto RefCnt Flags Type State I-Node Path unix 2 [ ACC ] STREAM LISTENING 10443 @/tmp/.ICE-unix/1396 unix 2 [ ACC ] STREAM LISTENING 8262 /var/run/xdmctl/dmctl/socket unix 2 [ ACC ] STREAM LISTENING 8277 /var/run/xdmctl/dmctl-:0/socket
```

**3.** **Apresentar as estatísticas para cada protocolo:**

**3.1** Para apresentar as estatísticas de todos os protocolos, utilizamos o parâmetro -s:

```
[root@tuxcaverna ~]# netstat -s

Ip: 98790 total packets received 54 with invalid headers 2 with invalid addresses 0 forwarded 0 incoming packets discarded 94778 incoming packets delivered 79070 requests sent out Icmp: 0 ICMP messages received 0 input ICMP message failed. ICMP input histogram: 0 ICMP messages sent 0 ICMP messages failed ICMP output histogram: Tcp: 2019 active connections openings 2 passive connection openings 10 failed connection attempts 161 connection resets received 14 connections established 88977 segments received 75703 segments send out 297 segments retransmited 0 bad segments received. 216 resets sent Udp: 2986 packets received 0 packets to unknown port received. 0 packet receive errors 3080 packets sent 0 receive buffer errors 0 send buffer errors

...

...
```

**3.2** Para apresentar as estatísticas do protocolo TCP (ou) UDP, utilizamos os parâmetros -st (ou) -su:

```
[root@tuxcaverna ~]# netstat -st

Tcp: 2031 active connections openings 2 passive connection openings 10 failed connection attempts 164 connection resets received 10 connections established 89257 segments received 76010 segments send out 297 segments retransmited 0 bad segments received. 219 resets sent
```

ou

```
[root@tuxcaverna ~]# netstat -su

Udp: 3012 packets received 0 packets to unknown port received. 0 packet receive errors 3107 packets sent 0 receive buffer errors 0 send buffer errors
```

**4.** **Apresentar o PID (ID do prcesso) e os nomes de programas:**

**4.1** Neste caso utilizamos o netstat com o parâmetro -p para receber a informação de "PID/Nome do Programa" na saída do netstat. Esta opção é muito útil para debugar e identificar qual programa está rodando em uma porta específica. O parâmetro -p pode ser combinado com demais parâmetros:

```
[root@tuxcaverna ~]# netstat -pt
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 XXX.XX.X.X:6881         83-228-34-45.btc-:11729 SYN_RECV    -
tcp        0      0 XXX.XX.X.X:39508        200.123.194.16:www-http ESTABLISHED 30741/chromium
tcp        0      0 XXX.XX.X.X:36321        208.46.17.59:www-http   ESTABLISHED 30741/chromium
tcp        0      0 XXX.XX.X.X:57411        65.55.142.101:https     ESTABLISHED 3292/python2
tcp        0      0 XXX.XX.X.X:6881         bd3e1c77.virtua.c:59298 ESTABLISHED 20447/qbittorrent
```

**5.** **Não resolver host, porta ou nome de usuário:**

**5.1** Utiliza-se o parâmetro -an quando não se deseja receber na saída do netstat informações de host, porta ou usuarios com seus respectivos nomes resolvidos. Ao invés destas informações virão números:

```
[root@tuxcaverna ~]# netstat -an
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:61108           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:6881            0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:49155         0.0.0.0:*               LISTEN
```

**5.2** Se você não quizer descartar as 3 informações de uma vez, pode optar por ocultar apenas uma em específico, conforme abaixo:

> _[root@tuxcaverna ~]# netstat -a --numericports_

ou

> _[root@tuxcaverna ~]# netstat -a -numeric-hosts_

ou

> _[root@tuxcaverna ~]# netstat -a numeric-users_

**6.** **Que** **tal ter uma saída contínua de informações em tempo real? Para isso usa-se o parâmetro -c, que vai atualizar as informações do netstat a cada segundo:**

```
[root@tuxcaverna ~]# netstat -c
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 XXX.XX.X.X:57411        65.55.142.101:https     ESTABLISHED
tcp        0      0 XXX.XX.X.X:6881         bd3e1c77.virtua.c:59298 ESTABLISHED
tcp        1      0 XXX.XX.X.X:49920        sn1msg2010605.phx.:msnp CLOSE_WAIT
tcp        0      0 XXX.XX.X.X:45554        111-240-177-108.dy:8921 TIME_WAIT
```


**7.** **Encontrar famílias de endereços não suportadas em seu sistema:**

**U**tilizamos o parâmetro --verbose. Repare que nas últimas linhas do retorno teremos algo como:

```
[root@tuxcaverna ~]# netstat -c
netstat: no support for `AF IPX' on this system.
netstat: no support for `AF AX25' on this system.
netstat: no support for `AF X25' on this system.
netstat: no support for `AF NETROM' on this system.
```

**8.** **Para apresentar informações de rotas do kernel utilizamos o parâmetro -r:**

```
[root@tuxcaverna ~]# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         172.16.1.1      0.0.0.0         UG        0 0          0 wlan0
172.16.1.0      *               255.255.255.0   U         0 0          0 wlan0
```

...ou, pode-se utilizar -rn para apresentar em formato numérico ao invés de nomes de hosts.
****

**9.** **Para descobrir em qual porta um determinado programa está rodando utilizamos os parâmetros -ap:**

```
[root@tuxcaverna ~]# netstat -ap
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 *:61108                 *:*                     LISTEN      3452/wish
tcp        0      0 *:6881                  *:*                     LISTEN      20447/qbittorrent
tcp        0      0 localhost.localdo:49155 *:*                     LISTEN      4010/GoogleTalkPlug
tcp        0    269 172.16.1.3:58742        52.60.in-addr.arp:12814 FIN_WAIT1   -
tcp        0      0 172.16.1.3:6881         bd3e1c77.virtua.c:59298 ESTABLISHED 20447/qbittorrent
```

**9.1** Se quizer especificar o programa, ao invés de o procurar em uma extensa lista, utilize um filtro com o grep da seguinte forma:

```
[root@tuxcaverna ~]# netstat -ap | grep chromium
unix  2      [ ACC ]     STREAM     LISTENING     252100   30741/chromium       /tmp/.org.chromium.Chromium.bSJLzX/SingletonSocket
unix  3      [ ]         STREAM     CONNECTED     309051   30741/chromium
unix  3      [ ]         STREAM     CONNECTED     308785   30741/chromium
```

**9.2** Se ao invés de especificar o programa, você quizer especificar uma porta e saber o que está rodando nela, pode-se utilizar o filtro do grep da seguinte forma:

> _[root@tuxcaverna ~]# netstat -an | grep ':80'_

**10.** **Para terminar, podemos utilizar o parâmetro -i para listar nossas interfaces de rede:**

```
[root@tuxcaverna ~]# netstat -i
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0   1500 0         0      0      0 0             0      0      0      0 BMU
lo    16436 0       662      0      0 0           662      0      0      0 LRU
wlan0  1500 0    157377      0      4 0        133487      0      0      0 BMRU
```

**S**e desejar informações mais detalhadas sobre cada interface, pode-se utilizar a combinação de parâmetros -ie:

```
[root@tuxcaverna ~]# netstat -ie
Kernel Interface table
eth0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500  metric 1
ether a4:ba:db:d7:41:c0  txqueuelen 1000  (Ethernet)
RX packets 0  bytes 0 (0.0 B)
RX errors 0  dropped 0  overruns 0  frame 0
TX packets 0  bytes 0 (0.0 B)
TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
device interrupt 47  base 0xc000
```

**H**appy Hacking...
