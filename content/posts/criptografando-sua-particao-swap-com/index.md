---
title: "Criptografando Sua Partição Swap Com O Ecryptfs"
date: 2014-01-03T19:28:47-05:00
draft: false
tags: [
"Seguranç",
"Software Livre",
"Redes",
"Linux",
"Arch Linux",
]
categories: ["Tutoriais"]
description: "Entenda a importância de criptografar a partição de swap e como fazê-lo com o ecryptfs."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**C**riptografia (Do grego *kryptós*, "escondido", e *gráphein*, "escrita") nada mais é do que o estudo e aplicação de técnicas com o intuito de ocultar informações, transformando a informação original em algo ilegível, exceto para seu destinatário final, o qual possui a chave ou método que será utilizado para tornar a informação legível novamente.

<!--more-->

**A**pesar de muitas pessoas acharem que criptografia é algo novo, o uso de mensagens cifradas é bastante antigo. Em tempos passados a cifragem de mensagens era utilizada, principalmente em assuntos de guerra. De acordo com estudos históricos, o primeiro uso documentado da criptografia foi em torno de *1900 a.c.*, no Egito, quando um escriba usou hieróglifos fora do padrão numa inscrição.

**A**o que interessa...

**M**uitas pessoas, por alguma razão (como por exemplo performance ou não gostar da ideia de digitar gigantescas senhas duranto o boot), acabam não utilizando uma LVM encriptada. Para estas pessoas existe a opção de criptografar arquivos e diretórios após a instalação do SO através de softwares como o eCryptfs. Mas, além disto, existe um outro aspecto interessante e importante que, por muitas vezes (para não dizer quase sempre) é esquecido: criptografar a swap.

**S**e você é usuário GNU/Linux, muito provavelmente já sabe o que é Swap. Ela é especialmente utilizada em computadores mais antigos e que possuem pouca memória RAM. Nestes casos, um espaço do disco rígido é utilizado como swap para servir de auxílio à memória RAM em diferentes tarefas quando a mesma não for o suficiente para realizar aquelas tarefas. Por questões óbvias, a swap não é tão rápida quanto a memória principal e, além disso, a swap também possui seus pontos *críticos* em relação a segurança. A questão mais crítica da swap em termos de segurança é o fato de ela, conforme já dito, não estar localizada em sua RAM, mas sim em seu disco rígido, escrevendo informações nesta partição, deixando assim rastros de suas atividades no próprio disco rígido. Por exemplo, caso você utilize softwares de criptografia em seu computador, mas não possua sua swap criptografada, você corre o risco de ter as senhas de chaves que você digita salvas em sua swap (disco rígido). Algo não desejado, certo?

**P**ara criptografar a sua swap, primeiramente instale os pacotes necessários:

**No Arch Linux**

```bash
 [kalib@tuxcaverna ~]$ sudo pacman -S ecryptfs-utils cryptsetup
```

**No Debian ou derivados**

```bash
 [kalib@tuxcaverna ~]$ sudo apt-get install ecryptfs-utils cryptsetup
```

**E**m seguida, precisamos literalmente criptografar a swap utilizando o seguinte comando:

```bash
 [kalib@tuxcaverna ~]$ sudo ecryptfs-setup-swap
```

**N**este momento, sua partição swap será desmontada, encriptada e montada novamente.

**P**ara certificar-se de que o procedimento funcionou, digite o seguinte comando:

```bash
 [kalib@tuxcaverna ~]$ sudo blkid | grep swap
```

**V**ocê deverá receber as informações sobre sua partição de swap, inclusive o indicativo *cryptswap*.

**C**omo medida preventiva contra erros durante o boot, edite o arquivo */etc/fstab*, retirando a entrada da swap não encriptada, deixando em seu lugar a entrada para a swap encriptada.

**H**appy Hacking!
