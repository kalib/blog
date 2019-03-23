---
title: "Dica Rápida Linux: Como Deletar Todos Os Arquivos Exceto Alguns Com Extensões Específicas"
date: 2013-08-23T08:46:58-05:00
draft: false
image: "/img/delete.jpg"
tags: [
"Linux",
"Software Livre",
]
description: "Como remover todos os arquivos em um diretório no linux exceto os arquivos com uma determinada extensão."
---
**É** comum termos diretórios repletos de arquivos dos mais diferentes tipos e extensões, certo?! E quando desejamos remover vários arquivos ao mesmo tempo? Bom, o linux nos permite utilizar o comando rm para remover diversos arquivos ao mesmo tempo, certo? Da mesma forma, podemos facilmente excluir todos os arquivos de um determinado tipo ou extensão, conforme exemplo abaixo:

```
 $ rm *.py
```

**O** comando acima remove todos os arquivos com extensão .py (*geralmente arquivos de código fonte Python*) do diretório atual. Mas e se desejarmos remover não apenas os arquivos de código Python, mas também os de código Ruby? Da mesma forma, podemos apenas inserir esta nova informação:

```
 $ rm *.py *.rb
```

**S**imples... nenhuma novidade. Ok, mas e se desejarmos o caminho oposto? Se ao invés de especificar o que eu *desejo deletar*, eu preferir especificar o que eu *não desejo deletar*?

**P**orque eu desejaria isso? A ideia não é complicar, mas sim simplificar. Imaginemos que eu possuo um diretório com não apenas 2 ou 3 tipos de arquivos diferentes, mas sim 10, 20 ou mesmo 30. Eu apenas desejo manter os arquivos de 2 tipos ou extensões. Já imaginaram qual seria o tamanho do comando seguindo a linha de raciocínio acima? Algo como...

```
 $ rm *.py *.rb *.txt *.doc *.odt *.png *.jpg *.sh *.bat *.c *.bkp *.ppt *.pl ... ... ... ...
```

**C**ansei antes mesmo de digitar todos. Agora deve ter ficado claro o porque de ser mais simples poder especificar apenas o que não desejo que seja deletado.

**V**amos supor que eu possuo um diretório no qual se encontram os meus arquivos de diversos tipos e extensões, conforme listados abaixo:

```
 $ ls

 arq2.avi  arq2.cdr  arq2.doc  arq2.html  arq2.mp3  arq2.php  arq2.ppt  arq2.vob  arq.c
 arq.dbf  arq.gif   arq.mov  arq.odt  arq.png  arq.rb  arq2.bat  arq2.cdx  arq2.exe
 arq2.jpg   arq2.mp4  arq2.pl   arq2.py   arq.avi   arq.cdr  arq.doc  arq.html  arq.mp3
 arq.php  arq.ppt  arq.vob  arq2.c    arq2.dbf  arq2.gif  arq2.mov   arq2.odt  arq2.png
 arq2.rb   arq.bat   arq.cdx  arq.exe  arq.jpg   arq.mp4  arq.pl   arq.py
```

**C**omo podemos ver, temos arquivos de diversas extensões. Agora, supondo que eu desejo remover todos, exceto os arquivos com as extensões .rb e .py:

```
 $ rm !(*.rb|*.py)
```

**N**osso resultado?

```
 $ ls

 arq2.py  arq2.rb  arq.py  arq.rb
```

**F**eito. Oi, simples assim...
