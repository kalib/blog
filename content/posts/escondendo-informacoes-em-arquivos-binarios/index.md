---
title: "Escondendo Informações Em Arquivos Binários"
date: 2008-10-24T14:33:15-04:00
draft: false
tags: [
"Arch Linux",
"Cultura Hacker",
"Linux",
"Segurança",
"Software Livre",
]
categories: ["Tutoriais"]
description: "No Linux sabemos que tudo é um arquivo. Nada depende exclusivamente de extensão ou tipo. Até mesmo os famosos diretórios na verdade não passam de arquivos. O que mostrarei aqui é uma dica rápida e simples de como se aproveitar disto para ocultar de forma simples e rápida alguma informação de texto em um arquivo binário qualquer que o seja. Por mais incrível que possa parecer, não precisaremos de nenhuma mágica ou ferramenta especial para isto. Quem utiliza Linux sabe que o cat ou mesmo o echo são ferramentas padrão em qualquer distribuição. SIM. Precisaremos apenas delas para fazer o truque. Não acredita?"
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
Saudações galera...

Recentemente um amigo, Michel, me perguntou algo sobre a possibilidade de ocultar informações em arquivos binários, como uma música ou mesmo uma foto. Eu lhe respondi que era possível, e bem simples em termos de conceito, bem como lhe prometi postar esta dica.

<!--more-->

No Linux sabemos que tudo é um arquivo. Nada depende exclusivamente de extensão ou tipo. Até mesmo os famosos "diretórios" na verdade não passam de arquivos. O que mostrarei aqui é uma dica rápida e simples de como se aproveitar disto para ocultar de forma simples e rápida alguma informação de texto em um arquivo binário qualquer que o seja. Por mais incrível que possa parecer, não precisaremos de nenhuma mágica ou ferramenta especial para isto. Quem utiliza Linux sabe que o cat ou mesmo o echo são ferramentas padrão em qualquer distribuição. SIM. Precisaremos apenas delas para fazer o "truque". Não acredita? o.O

Vamos lá...

Antes de mais nada selecione o arquivo que será utilizado para "camuflar" nossa informação. Escolha uma imagem qualquer por exemplo ou outro tipo como áudio, vídeo, compactado, etc... Supondo que você escolheu o arquivo foto.jpg, passaremos ao segundo passo.

Supondo que eu queira ocultar a frase "Bem vindo ao Linux" na foto.jpg, o comando seria o seguinte:

```bash
$ echo "Bem vindo ao Linux" >> foto.jpg
```

Simples não é?! Nada de truques ou mágicas. Pode tentar abrir a foto com algum editor de imagens como o Gimp, ou mesmo com um vizualizador de sua preferência. A foto parecerá intacta e normal. Agora tente executá-la com um editor de textes como o cat por exemplo.

```bash
$ cat foto.jpg
```

Você verá um conjunto de caracteres estranhos, dados sobre a foto em si, porém role até o final do arquivo. Depois de todos os caracteres estranhos, você encontrará a informação que ocultou anteriormente, no nosso caso é a frase "Bem Vindo ao Linux".

Simples não é? Lembrando que o mesmo conceito se aplica caso você deseje utilizar ao invés de uma única frase, todo um texto. Crie o texto com um editor de textos qualquer como o vim por exemplo e salve com o nome texto.

Em seguida aplique o comando utilizando desta vez o cat, ao invés do echo:

```bash
$ cat texto >> foto.jpg
```
Volte a repetir os testes já citados anteriormente para ter certeza do sucesso no processo.

Como eu disse..tudo muito simples e sem mistérios.

Respondido Michel? ;]

Abraço pessoal
