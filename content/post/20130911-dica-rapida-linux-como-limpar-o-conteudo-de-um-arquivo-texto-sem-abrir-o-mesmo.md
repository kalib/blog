---
title: "Dica Rápida Linux: Como Limpar O Conteúdo De Um Arquivo Texto Sem Abrir O Mesmo"
date: 2013-09-11T00:22:04-05:00
draft: false
image: "/img/erase.jpg"
tags: [
"Linux",
"Software Livre",
"Arch Linux",
]
description: "Possui um arquivo texto com vários Gigas e deseja limpar seu conteúdo sem abrir o mesmo? Saiba como..."
---
**A**rquivos de log, por vezes, podem ser muito extensos. Tão extensos que torna-se praticamente impossível a leitura de seu conteúdo com um editor de textos como o vim ou vi. Além disso, muitos deles podem chegar a ocupar Gigas e mais Gigas de seu espaço em disco, o que pode acabar sendo um desperdício de espaço em seu sistema de arquivos.

**H**oje passei por um problema deste tipo. Ao atualizar meu Arch Linux, percebi que comecei a receber erros devido ao meu espaço em disco o qual era apontado como "insuficiente". O fato é que é muito comum em distribuições Linux utilizarmos um particionamento de forma a isolarmos um pouco cada partição para fins específicos. No meu caso, verifiquei que a minha partição raíz estava mesmo completamente cheia.

```
 [kalib@tuxcaverna ~]$ df -h /
 Sist. Arq.      Tam. Usado Disp. Uso% Montado em
 /dev/sda3        30G   30G   0M  100% /
```

**P**ara resolver meu problema comecei limpando os pacotes que ficam em cache desnecessariamente através do pacman:

```
 [kalib@tuxcaverna ~]$ pacman -Sc
```

**E**m seguida resolvi verificar quanto de espaço eu estava consumindo com logs de sistema:

```
 [root@tuxcaverna ~]# cd /var/log
 [root@tuxcaverna ~]# du -h .
```

**O** comando acima me mostrou que meu diretório de logs estava consumindo mais de 13G. Insano, certo? Na verdade, não. É comum que ao longo dos meses, ou mesmo anos, o sistema acumule logs e mais logs. Uma quantia absurda e, muitas vezes, desnecessária de informações. Como trata-se de meu notebook pessoal e não estou tendo qualquer erro em meu sistema, resolvi limpar estes logs, portanto o primeiro passo é identificar os maiores arquivos. No meu caso foram os arquivos:

```
 everything.log    kernel.log    daemon.log
```

**U**ma vez que cada um destes arquivos possuía mais de 3G, resolvi limpá-los completamente. A saída mais rápida e KISS? */dev/null neles*.

```
 [root@tuxcaverna ~]# cat /dev/null >everything.log
```

**I**sto limpará completamente o conteúdo do arquivo de texto informado. Vejamos o simples exemplo a seguir:

**V**amos criar um arquivo de texto vazio chamado *teste.txt*.

```
 [root@tuxcaverna ~]# touch teste.txt
```

**E**m seguida, vamos inserir algum conteúdo no mesmo:

```
 [root@tuxcaverna ~]# echo "Testando" >teste.txt
```

**V**erifiquemos se meu texto realmente foi inserido no arquivo:

```
 [root@tuxcaverna ~]# cat teste.txt
 Testando
```

**T**udo certo. Agora vamos limpar o mesmo:

```
 [root@tuxcaverna ~]# cat /dev/null >teste.txt
```

**A**gora, vamos confirmar se o mesmo foi mesmo zerado:

```
 [root@tuxcaverna ~]# cat teste.txt
 [root@tuxcaverna ~]#
```

**M**issão cumprida. Apenas para constar a diferença:

```
 [kalib@tuxcaverna ~]$ df -h /
 Sist. Arq.      Tam. Usado Disp. Uso% Montado em
 /dev/sda3        30G   16G   13G  56% /
```
