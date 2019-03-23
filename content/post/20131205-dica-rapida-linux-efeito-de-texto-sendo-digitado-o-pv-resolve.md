---
title: "Dica Rápida Linux: Efeito De Texto Sendo Digitado O Pv Resolve"
date: 2013-12-05T21:19:14-05:00
draft: false
image: "/img/monitor.jpg"
tags: [
"Linux",
"Software Livre",
]
description: "Utilizando o comando pv para dar o efeito de texto sendo digitado, assim como nos filmes e séries de TV."
---
**C**ertamente você já cansou de ver em filmes e/ou séries de TV cenas nas quais um monitor apresenta um texto que, aparentemente, está sendo digitado em tempo real. É claro que eles não possuem uma pessoa digitando aquele texto ou série de comandos no momento da gravação. Então, que tal aprender uma forma de fazer isto no Linux?

**O** comando *pv* realiza perfeitamente este trabalho, podendo inclusive interagir com outros aplicativos e comandos.

**M**ão na massa...

**A**ntes de mais nada você precisará instalar o pv em sua distribuição. No Arch Linux, eu utilizo o pacman da forma convencional:

```
 [kalib@tuxcaverna ~]$ sudo pacman -S pv
```

**O** pv está disponível nos repositórios de praticamente todas as distribuições, portanto utilize o gerenciador de pacotes de sua preferência para instalá-lo.

**A** utilização é simples, bastando que você utilize algum comando que, de alguma forma, exponha algum texto na tela e em seguida redirecione esta saída para o pv. O pv possui diversos parâmetros, mas eu gosto particularmente de utilizar -qL, onde o *q* significa "quiet" e o *L* significa latência, em seguida insiro um valor para a latência. Vamos ao exemplo:

```
 [kalib@tuxcaverna ~]$ echo "Primeiro teste com pv" | pv -qL 20
 Primeiro teste com pv
```

**S**e você digitar o mesmo comando, verá que ele irá escrever o texto na tela de forma "automática": "Primeiro teste com pv".

**É** claro, em uma gravação de Hollywood a linha na qual o comando foi passado não deveria aparecer, no caso: *[kalib@tuxcaverna ~]$ echo "Primeiro teste com pv" | pv -qL 20*. Que tal inserir um *clear* antes de nosso comando para limpar a tela antes da execução desejada?

```
 [kalib@tuxcaverna ~]$ clear && echo "Primeiro teste com pv" | pv -qL 20
```

**P**erceba que desta vez o comando digitado não aparece na tela. A única informação que será exibida será *Primeiro teste com pv*.

**D**iminuindo ou aumentando o valor da latência você diminuirá ou aumentará a velocidade de *digitação* do texto que você escolheu.

**C**omo informei no início, você pode unir o pv com outros programas ou comandos. Que tal fazer com que um texto um pouco maior seja exibido?

**P**ara este teste, eu criei um arquivo texto chamado *testepv*, conforme pode ser visto abaixo:

```
 [kalib@tuxcaverna ~]$ cat testepv

 Não obstante, a contínua expansão de nossa atividade oferece uma interessante oportunidade para verificação de todos os recursos funcionais envolvidos.
 A prática cotidiana prova que o desenvolvimento contínuo de distintas formas de atuação nos obriga à análise de alternativas às soluções ortodoxas.
 Por conseguinte, a competitividade nas transações comerciais estende o alcance e a importância da gestão inovadora da qual fazemos parte.
```

**N**este caso, o texto pode ser digitado *automaticamente* com o pv, da seguinte forma:

```
 [kalib@tuxcaverna ~]$ clear && cat testepv | pv -qL 20
```

**L**egal? Que tal utilizarmos algo ainda melhor? Já ouviu falar no *figlet*? É outro comando/aplicativo Linux que muitas pessoas desconhecem. Comece instalando-o em seu sistema, caso você já não o possua. O figlet *desenha* o seu texto de uma forma um pouco mais *enfeitada*, se comparado ao puro *cat* ou *echo*. Exemplo:

```
 [kalib@tuxcaverna ~]$ figlet "Teste do Figlet"
  _____         _             _         _____ _       _      _   
 |_   _|__  ___| |_ ___    __| | ___   |  ___(_) __ _| | ___| |_
   | |/ _ \/ __| __/ _ \  / _` |/ _ \  | |_  | |/ _` | |/ _ \ __|
   | |  __/\__ \ ||  __/ | (_| | (_) | |  _| | | (_| | |  __/ |_
   |_|\___||___/\__\___|  \__,_|\___/  |_|   |_|\__, |_|\___|\__|
                                                |___/            
```

**N**esse caso, vamos fazer com que o efeito *figlet* também pareça estar sendo digitado automaticamente e em tempo real:

```
 [kalib@tuxcaverna ~]$ clear && figlet "Teste do Figlet" | pv -qL 30
```

**R**esultado interessante, certo? Da mesma forma, o pv pode ser utilizado com diversos outros aplicativos que trazem alguma saída no terminal, como o cowsay e muitos outros. A sua criatividade é o limite.

**H**ave fun! \,,/_
