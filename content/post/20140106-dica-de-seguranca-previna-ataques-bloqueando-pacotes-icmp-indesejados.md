---
title: "Dica De Segurança: Previna Ataques Bloqueando Pacotes Icmp Indesejados"
date: 2014-01-10T19:23:19-05:00
draft: false
image: "/img/you_shall_not_pass.jpg"
tags: [
"Segurança",
"Software Livre",
"Redes",
"Linux",
]
description: "Entenda a importância de bloquear pacotes ou mensagens ICMP indesejadas."
---
**O** protocolo *ICMP* pode ser utilizado para facilitar diversas rotinas e tarefas importantes de um administrador de redes, tais como na utilização de ferramentas como o *ping* e o *traceroute*, mas também pode ser manipulado por pessoas mal intencionadas que podem manipular mensagens ou pacotes ICMP para mapear sua rede.

**É** comum ver administradores de rede se preocupando e fazendo um ótimo trabalho em termos de filtrar o tráfego *TCP* e/ou *UDP*, porém quase sempre esquecem de dar a mesma atenção ao tráfego *ICMP*, sendo este tão crítico quanto os dois anteriores. Uma vez que este protocolo pode ser utilizado para mapear e realizar ataques em sua rede, ele não pode simplesmente ser deixado de lado.

**I**CMP, sigla para *Internet Control Message Protocol*, é um protocolo integrante do Protocolo IP utilizado para fornecer relatórios de erros à fonte original. Seu tráfego é, basicamente, baseado em mensagens trocadas entre hots, gateways, etc., cujo intuito é, principalmente, reportar erros, como por exemplo um pacote IP que não consegue chegar ao seu destino.

**P**or padrão, alguns servidores e firewalls bloqueiam as respostas ICMP como medida de segurança, tentando assim bloquear os ataques que consistem na sobrecarga da memória, enviando dados (em ping) até o sistema não ter a capacidade de administrar suas próprias funções. Bom, ao mesmo tempo em que é um mecanismo de defesa interessante, este bloqueio total acaba comprometendo e atrapalhando diversas atividades do administrador de redes, não sendo portanto a estratégia mais inteligente a ser adotada.

**A**o invés de bloquear o ICMP por completo, é mais interessante conhecer o que é bom e o que é ruim em termos de mensagens ICMP, de forma que sejam realizados filtros corretos. A importância desta gangorra é não permitir que o lado *ruim* do ICMP, como por exemplo ICMP Smurf, Ping of death, ataques com ICMP flood ou ICMP nuke, prejudiquem o administrador de redes que pode tirar proveito de boas ferramentas que se utilizam do ICMP, como o Ping e o Traceroute.

**A** estratégia mais simples, portanto, é utilizar uma regra geral contendo exceções.

1. Bloquear todos os tipos de tráfego ICMP;
2. Permitir ping -- CMP Echo-Request outbound (saída) e Echo-Reply inbound (entrada);
3. Permitir traceroute -- TTL-Exceeded e Port-Unreachable inbound (entrada);
4. Permitir path MTU -- ICMP Fragmentation-DF-Set inbound (entrada).

**É** claro que este é apenas um exemplo, visto que você poderá permitir mais ou menos, de acordo com a sua necessidade.

**N**ão deixemos pobre a nossa configuração, facilitando as coisas para ataques quando isto pode ser facilmente bloqueado.

**H**appy Hacking!
