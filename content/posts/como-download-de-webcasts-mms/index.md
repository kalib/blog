---
title: "Como? Download De Webcasts Mms E Rtmp?"
date: 2011-04-20T15:26:57-05:00
draft: false
tags: [
"Arch Linux",
"Impressões",
"Linux",
"Software Livre",
]
categories: ["Tutoriais"]
description: "Cansado de apenas poder assistir aquele webcast ou vídeo-aula online? E se estiver em um local sem conec﻿tividade? Porque dor de cabeça? Sou usuário Linux! Obrigado por ter a ideia de criar o Linux e facilitar nossas vidas Linus. ;]"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
---
**C**ansado de apenas poder assistir aquele webcast ou vídeo-aula online? E se estiver em um local sem conec﻿tividade? Porque dor de cabeça? Sou usuário Linux! Obrigado por ter a ideia de criar o Linux e facilitar nossas vidas Linus. ;]

<!--more-->

**R**ecentemente me deparei com esta problemática. Como realizar o download deste tipo de conteúdo?

**A**o checar o código fonte da página que exibia o vídeo, vi que eram streamings dos protocolos mms ou rtmp, o que, aparentemente, dificultava o download.

**P**ara quem mais estiver passando por este problema, segue a solução. Aliás, as soluções.

**U**ma vez que, através do código fonte você consiga identificar a URL completa do arquivo mmc ou rmtp, fica fácil utilizar alguma das metodologias a seguir.

**A** primeira opção é a aplicação **Mimms**.

**E**sta é a forma mais simples e prática com arquivos do tipo mms.

**P**ara usuários do [Arch Linux](https://archlinux.org), já existe um pacote pronto no [AUR](https://aur.archlinux.org/packages.php?ID=16412).

**A** instalação, segue o padrão de arquivos baixados do AUR:

**1-** Download do [Tarball](https://aur.archlinux.org/packages/mimms/mimms.tar.gz);

**2-** Descompatar:

```bash
$ tar -xvzf mimms.tar.gz
```

**3-** Acessar o diretório e efetuar a compilação do pacote:

```bash
$ cd mimms && makepkg
```

**4-** Instalar o pacote que lhe foi gerado:

```bash
# pacman -U mimms-3.2.1-2-any.pkg.tar.xz
```

**F**eito. Agora é só executar da seguinte forma:

```bash
$ mimms mms://url_de_origem_do_streaming/arquivo_streaming.wmv
```

**S**erá que isso funciona mesmo? Experimente:

```bash
$ mimms [mms://wms.andrew.cmu.edu/001/pausch.wmv](mms://wms.andrew.cmu.edu/001/pausch.wmv)
```

**O**i, simples assim.

**O**utra alternativa?

**U**tilizando o **Mencoder**:

**N**ão é tão simples quanto o mimms, mas também é rápido e prático.

**O** comando que deve ser utilizado?

```bash
mencoder mms://url_de_origem_do_streaming/arquivo_streaming.wmv -o arquivo_streaming.wmv -oac copy -ovc copy
```

**U**m pouco mais complexo, certo? Mas funciona.

**E** quanto aos arquivos de streaming que utilizam o protocolo rtmp?

**N**este caso a opção é utilizarmos o **rtmpdump**.

**S**im, claro.. Se você utiliza o Arch Linux, sua vida continua simplificada:

```bash
# pacman -S rtmpdump
```

**P**ronto, está instalado. ;]

**A** execução do mesmo também é bastante simples, precisando apenas especificar a origem e o destino do streaming em questão. Segue sintaxe:

```bash
rtmpdump -r "rtmpe://url_de_origem_do_streaming/arquivo_streaming.wmv" -o arquivo.streaming.flv --resume
```

**H**ave fun! ;]

**A**braços!
