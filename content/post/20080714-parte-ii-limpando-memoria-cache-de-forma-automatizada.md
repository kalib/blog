---
title: "Parte II Limpando Memória Cache De Forma Automatizada"
date: 2008-07-14T16:15:30-04:00
draft: false
image: "/img/ram.jpg"
tags: [
"Linux",
"Redes",
"Software Livre",
]
description: "Muitos me pediram por aqui ou mesmo por email para explicar como seria a implementação desta técnica de forma automatizada."
---
Muitos me pediram por aqui ou mesmo por email para explicar como seria a implementação desta técnica de forma automatizada.  



Bom, utilizo isto para o servidor que citei no artigo anterior, utilizado pelos nossos amigos desenvolvedores Java. ;] Nada pessoal eim?!




Aqui precisaremos apenas de um mínimo de intimidade com shell script e um pouco de conhecimento sobre o agendamento de tarefas no linux através do cron.




Mãos a obra...




A missão: Uma vez que nossos amigos não conseguem trabalhar de forma harmônica com a alocação e desalocação de memória em nossos servidores, iremos agendar a limpeza de cache para todos os dias no começo do expediente (8:00) e após o almoço. Lembrando que este é apenas um exemplo, mas você pode adaptar os horários de acordo com sua real necessidade.




Soldados Disponíveis: Shell Script e Cron




Plano: Um pequeno e simples script em shell será executado nos dias e horários informados anteriormente de forma a fazer uma limpeza no cache.




Execução:




Primeiro criaremos o script que fará a ação de limpeza do cache. Para isso abra o editor de textos de sua preferência. Particularmente prefiro o vim, mas este pode ser substituido por qualquer outro.




No seu corpo insira o seguinte conteúdo:




> #!/bin/bash  

#limpando cache
>
>

>
> #o seguinte comando é o responsável pela limpeza  

echo 3 > /proc/sys/vm/drop_caches
>
>





Feito isto, salve o arquivo com o nome de limpacache.sh




Sim, isto é tudo o que o seu script precisa. Com o script criado, você deverá agora lhe dar condições de execução. Utilize o seguinte comando:




> # chmod a+x limpacache.sh
>
>

>
>





Agora que ele está pronto e com permissão de execução, iremos agendar a execução do mesmo. No terminal digite:




> # crontab -e
>
>





Isto irá abrir um arquivo no qual você deverá fazer o agendamento de sua tarefa. No mesmo insira o seguinte conteúdo:




> # mm HH DD MM DS tarefa  

00 08 * * * /usr/bin/scripts/limpacache.sh  

00 14 * * * /usr/bin/scripts/limpacache.sh
>
>





Ps: O caminho /etc/scripts/ precisa ser configurado de acordo com o caminho utilizado por você. ;]




Pronto. Pode salvar e encerrar este aquivo.




Traduzindo o comando as linhas do cron que utilizamos:




mm: minutos  

HH: horas  

DD: dia  

MM: mês  

DS: dia da semana  

/usr/bin/scripts/limpacache.sh: tarefa a ser realizada




Feito isto, o plano está concretizado. Seu script será executado todos os dias nestes dois horários.




Sinta-se livre agora para customizar os dias e horários da maneira que for mais conveniente para você.




Abraços
