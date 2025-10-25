---
title: "Limpando O Cache De Forma Rápida"
date: 2008-07-09T16:38:37-04:00
draft: false
tags: [
"Linux",
"Redes",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Aqueles que são administradores de redes e servidores já devem ter passado por problemas parecidos com este. Nesta dica irei apresentar um comando simples para limpar sua memória em cache."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
Aqueles que são administradores de redes e servidores já devem ter passado por problemas parecidos com este.

<!--more-->

Nesta dica irei apresentar um comando simples para limpar sua memória em cache.

Resolvi escrever sobre isto por ter visto duas vezes na mesma semana problemas relacionados a isso. Um foi aqui mesmo onde trabalho, onde estávamos percebendo que o consumo de memória em um dos servidores estava muito alto, porém com pouca atividade no mesmo, no qual percebi que nada mais era do que muita memória alocada em cache sem necessidade no momento. Desenvolvedores java...vai entender.. hauhauha (brincadeira.. :p).

Outro caso foi uma dúvida que surgiu, bem parecida, em um fórum do qual faço parte, onde um rapaz estava passando pelo mesmo problema no servidor dele... Com consumo exagerado de memória. Ele até colou o resultado do top no qual podíamos ver claramente que não haviam processos consumindo tudo aquilo de memória, e mais uma vez pudemos ver que o grande vilão era o cache, o que lhe passava essa impressão de memória totalmente consumida.

O comando para se limpar este chache é o seguinte:

```bash
# echo 3 > /proc/sys/vm/drop_caches
```

Exemplo:

Aqui vai uma saída do meu top antes de rodar o comando. ( Reparem no consumo de memória armazenada em Cache na última linha!)

```bash
top - 09:40:03 up 1:42, 1 user, load average: 0.06, 0.20, 0.20  
Tasks: 83 total, 2 running, 80 sleeping, 0 stopped, 1 zombie  
Cpu(s): 4.3%us, 0.5%sy, 0.2%ni, 95.0%id, 0.0%wa, 0.0%hi, 0.0%si, 0.0%st  
Mem: 1944240k total, 898916k used, 245324k free, 51176k buffers  
Swap: 996020k total, 0k used, 996020k free, 969000k cached
```

Repare agora o resultado obtido pelo top depois de executar o comando para limpar o cache:

```bash
top - 09:45:03 up 1:47, 1 user, load average: 0.32, 0.16, 0.17  
Tasks: 85 total, 3 running, 81 sleeping, 0 stopped, 1 zombie  
Cpu(s): 11.2%us, 1.5%sy, 0.0%ni, 63.4%id, 23.9%wa, 0.0%hi, 0.0%si, 0.0%st  
Mem: 1944240k total, 329412k used, 1614828k free, 768k buffers  
Swap: 996020k total, 0k used, 996020k free, 69088k cached
```

A memória armazenada em cache caiu de ~969mb para ~69mb. ;]

Bingo!
