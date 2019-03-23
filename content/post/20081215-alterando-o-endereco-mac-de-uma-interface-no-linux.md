---
title: "Alterando O Endereço Mac De Uma Interface No Linux"
date: 2008-12-15T11:39:10-04:00
draft: false
image: "/img/placaderede.jpeg"
tags: [
"Linux",
"Redes",
"Segurança",
"Software Livre",
]
description: "Nesta curta dica irei apresentar uma dica simples e rápida de como alterar o endereço MAC de uma interface de rede no Linux."
---
Nesta curta dica irei apresentar uma dica simples e rápida de como alterar o endereço MAC de uma interface de rede no Linux.




MAC = Media Access Control  

Assim como nós, seres humanos, possuímos um número de registro físico como o RG, as interfaces de dispositivos de rede também possuem um registro físico que lhes é dedicado já na hora de sua fabricação. Este endereço físico se chama MAC e é formado por 48 bits em forma de hexadecimal.




Este protocolo é responsável pelo controle de acesso à rede Ethernet. Um exemplo de endereço MAC seria:




> 00:A0:D1:58:DF:BC
>
>





No caso, não existem duas interfaces de rede no mundo com o mesmo endereço MAC. Este valor é único AO SAIR DE FÁBRICA. Mas, como nem tudo na vida são rosas...




Existem alguns casos nos quais precisamos identificar, ou mesmo alterar, endereços MAC. Um exemplo de caso em que se é preciso alterar o endereço MAC seria o seguinte:




Supondo que eu seja um técnico e estou querendo dar suporte à máquina de um amigo. Este trás sua máquina até minha casa. Minha internet recebe um ip por dhcp de forma amarrada ao meu endereço MAC. Neste caso, para ter acesso à internet pela máquina deste colega para atualizações, eu precisaria momentaneamente alterar o endereço MAC de sua interface de rede.




No Linux podemos descobrir qual o endereço MAC de uma interface com o comando ifconfig [interface], como no exemplo a seguir:




# ifconfig eth0




Me será retornado um conjunto de informações sobre a interface, incluindo o endereço MAC da mesma, como a seguir:




> eth0      Link encap:Ethernet  HWaddr 00:A0:D1:58:DF:BC inet addr:192.168.1.105  Bcast:192.168.1.255  Mask:255.255.255.0 inet6 addr: fe80::2a0:d1ff:fe58:dfbc/64 Scope:Link UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1 RX packets:54461 errors:0 dropped:0 overruns:0 frame:0 TX packets:46066 errors:0 dropped:0 overruns:0 carrier:0 collisions:0 txqueuelen:1000 RX bytes:68669660 (65.4 Mb)  TX bytes:5002980 (4.7 Mb) Interrupt:20 Base address:0x4800
>
>





O procedimento para se mudar este endereço MAC é:




1- Desabilitar a interface:  

# ifconfig eth0 down




2- Alterar o MAC:  

# ifconfig eth0 hw ether XX:XX:XX:XX:XX:XX




3- Subir novamente a interface:  

# ifconfig eth0 up




Simples não?!




Feito isto, pode conferir a alteração com o comando ifconfig eth0 novamente. ;]




Abraços pessoal...
