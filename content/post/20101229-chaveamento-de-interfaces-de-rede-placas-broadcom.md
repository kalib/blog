---
title: "Chaveamento De Interfaces De Rede - Placas Broadcom"
date: 2010-12-29T21:38:01-05:00
draft: false
image: "/img/broadcomchip.jpg"
tags: [
"Arch Linux",
"Impressões",
"Linux",
"Redes",
"Software Livre",
]
description: "Como diria a Feiticeira (não lembro o nome real dela), não é feitiçaria, é tecnologia!"
---
**C**omo diria a Feiticeira (não lembro o nome real dela), não é feitiçaria, é tecnologia!

**C**omo já expliquei no outro post, tive problemas com meu notebook e por isso passei muito tempo afastado do blog. Bom, recentemente comprei outro e cá estou novamente em casa.

**M**as, como nem tudo é um mar de rosas... toda mudança trás impactos positivos e negativos.


![Vostro 3300](/img/vostro3300.jpg)


**A** máquina escolhida foi um Dell Vostro 3300. Com certeza uma excelente máquina e que eu não deixaria de recomendar para ninguém.

**P**ara quem comprou algum Dell recentemente, ou pensou em comprar, não deve ser uma novidade que estas máquinas, em sua grande maioria, estão trazendo chipsets wireless da Broadcom, ao invés dos Intel.

**Q**uando pensei em comprar, refleti bastante sobre isto, visto que os drivers para broadcom no linux já tiveram um passado um tanto quanto nebuloso em se tratando de Wireless.

**R**esolvi encarar e não me arrependo. A performance está excelente. O driver realmente funcionou como deveria funcionar porém tive um pequeno problema.

**U**tilizo [KDE](https://www.kde.org) como gerenciador de janelas em meu [ArchLinux](https://www.archlinux.org) e [Wicd](https://wicd.sourceforge.net/) como gerenciador de redes. Estava tudo funcionando bem mas ao reiniciar a máquina um certo dia percebo que o Wicd não havia encontrado nenhuma rede wireless. o.O

**R**einicio novamente e ele volta a encontrar. Feitiçaria? Fiquei curioso e resolvi reiniciar novamente e... funcionou. Pensei: "Foi apenas um susto..ela devia querer descansar.."

**M**as no dia seguinte ao ligar em casa percebi que, novamente, a interface wireless não foi encontrada pelo Wicd. Lembrei do fato anterior e reiniciei novamente. A interface continuava sem funcionar no Wicd.

**L**hes poupando dos testes que resolvi fazer para isolar o problema, descobri que as vezes a minha interface wireless estava subindo como eth0, as vezes subindo como eth1. Isso, obviamente, deixava o Wicd confuso, pois na configuração dele é necessário especificar qual será a interface wireless e qual será a wired (cabeada). Portanto, quando o notebook subia com a wireless na eth1, o Wicd funcionava numa boa, mas quando subia na eth0 o Wicd continuava tentando escanear redes wireless com a eth1, portanto não funcionava.

**A**pós algumas pesquisas descobri que este é um acontecimento comum em relação ao driver da broadcom. Este chaveamento ou swap de interfaces de rede acontece realmente de forma aleatória.

**C**omo resolver? Amarrando a interface ao nome, literalmente.

**A** placa começou a funcionar da forma esperada após vincular via regra o MAC da interface ao nome que eu desejava para ela. Desta forma, na hora de subir, ambas passam a subir de acordo com a regra que eu especifiquei, o que resolveu o problema pois a interface wireless passou a sempre subir com o mesmo nome, evitando que o Wicd tentasse escanear com uma interface que não existe.

**S**em mais papo furado.. mãos à obra!

**A**ntes de mais nada é necessário criar, caso você já não possua, o arquivo _/etc/udev/rules.d/10-network.rules_ acrescentando o seguinte conteúdo:

_SUBSYSTEM=="net", ATTR{address}=="aa:bb:cc:dd:ee:ff", NAME="eth0"_
_ SUBSYSTEM=="net", ATTR{address}=="aa:bb:cc:dd:ee:ff", NAME="eth1"_

o.O wtf?

**B**om, com isto você estará deixando claro para o sistema qual o nome que deverá ser atribuído para cada interface.

**V**ocê deverá apenas substituir o "aa:bb:cc:dd:ee:ff" e "ff:ee:dd:cc:bb:aa" pelos respectivos endereços MAC das interfaces.

**O** valor dos campos NAME podem ser alterados de acordo com sua vontade, por exemplo: eth0 para a cabeada e wlan0 para a wireless.

**C**aso não saiba como conseguir o endereço MAC de suas interfaces, utilize o seguinte comando:

_# ifconfig | grep HW_

**I**sso lhe dará um retorno parecido com o seguinte:

_eth0  Link encap:Ethernet  HWaddr **A4:BA:DB:D7:41:C0**
eth1  Link encap:Ethernet  HWaddr **1C:65:9D:6E:65:1B**_

**O**s endereços MAC são os valores que destaquei acima.

**ATENÇÃO:** Apesar de o MAC constantemente ser escrito com letras em maiúsculo como apresentado acima, no caso do arquivo 10-network.rules você deverá utilizar letras em minúsculo. Exato, o arquivo é case sensitive, portanto só funcionará desta forma.

**C**aso você receba uma saída parecida com a minha e queira se certificar de qual dos dois endereços MAC é o que realmente representa a interface wireless pode testar com os seguintes comando:

_# iwlist eth0 scanning_

e...﻿

_# iwlist eth1 scanning_

**V**ocê perceberá que terá resultados diferentes. O que retornar um scan completo de redes wireless disponíveis, obviamente, é a interface Wireless. ;]

**A**pós ter escolhido o nome que deseja para cada uma de suas interfaces pode salvar o arquivo de regra.

**ATENÇÃO²:** Certifique-se de atualizar o seu arquivo _/etc/rc.conf_ para evitar que os nomes das interfaces estejam diferentes das que você criou no arquivo de regras.

**É** isto.. agora sua interface funcinoará da forma esperada.

**A**braços!
