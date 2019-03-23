---
title: "Bleachbit: Limpeza De Disco No Linux"
date: 2013-10-23T00:10:09-05:00
draft: false
image: "/img/disk-cleanup.jpg"
tags: [
"Linux",
"Software Livre",
"Segurança",
]
description: "Como realizar uma limpeza segura em seu disco rígido utilizando o BleachBit no Linux."
---
**Q**uando o assunto é segurança da informação, muitas vezes o óbvio é deixado de lado. Se pensa em anti-vírus, firewall, IDS (Intrusion Detection System), etc. Mas e o básico e óbvio? Raramente vejo profissionais preocupados com o lixo existente nos discos rígidos. Com *lixo* eu quero dizer aqueles arquivos temporários, antigos arquivos de cache que já não são necessários, ou mesmo rastros de informações de arquivos que já foram deletados. Sim, o fato de você deletar arquivos não significa que as informações daqueles arquivos foram completamente apagadas do disco. A *"deleção regular de arquivos"* não está apagando *os dados deletados*. Ela está apenas removendo os links dos inodes de arquivos, tornando assim possível a recuperação dos arquivos *deletados* com algum software forense.

**E**xistem várias alternativas para remover seus arquivos de forma segura. As alternativas poderão variar de acordo com o sistema de arquivos utilizado em seu disco, é claro. Uma das formas mais simples que conheço é através da ferramenta **BleachBit**.

**B**leachBit é um software livre criado para limpar e liberar espaço em disco, gerenciando assim melhor a sua privacidade e otimizando sistemas computacionais. Criado em 2008 para sistemas GNU/Linux, partiu de um controverso e polêmico debate sobre a necessidade de o GNU/Linux necessitar de uma ferramenta para limpeza de registros. Posteriormente o BleachBit passou a suportar também sistemas Microsoft Windows.

**C**om esta ferramenta você poderá não apenas limpar seu espaço em disco, mas também limpar vários arquivos típicos do sistema que não são necessários na maioria dos casos a longo prazo, arquivos estes que podem dar ao intruso informações desnecessárias sobre suas atividades. Não desejamos isto...

**A** instalação do mesmo é feita de forma simples e convencional. No caso do archlinux, o pacote se encontra disponível no repositório [community], portanto pode ser instalado regularmente através do pacman.

```
 [kalib@tuxcaverna ~]$ sudo pacman -Ss bleachbit
 [sudo] password for kalib:
 community/bleachbit 0.9.6-1
    Deletes unneeded files to free disk space and maintain privacy

 [kalib@tuxcaverna ~]$ sudo pacman -S bleachbit
```

**N**a maioria das distribuições baseadas em Debian, tal como o Ubuntu, o pacote poderá ser instalado através do aptitude ou apt-get:

```
 $ sudo apt-get update
 $ sudo apt-get install bleachbit
```

**O** BleachBit possui uma interface gráfica simples, na qual você poderá selecionar o que você deseja destruir. Lembre-se que algumas funções são experimentais e podem causar problemas em seu sistema. Mas não há necessidade de se preocupar: O BleachBit lhe informa sobre isto e lhe dá a chance de cancelar a operação selecionada. ;]

**O** BleachBit possui uma vasta lista de *cleaners*, como ele chama as ferramentas internas de limpeza. Dentre a longa lista, podemos citar alguns como: *Adanaxis, Adobe Reader, aMSN, aMule, APT, Audacious, Bash, Beagle, Chromium, Downloader for X, Deep scan, Easytag, ELinks, emesene, Epiphany, Evolution, Exaile, Filezilla, Firefox, Flash, gedit, gFTP, GIMP, etc.* A lista é realmente longa. [Lista completa de cleaners e demais recursos.](https://bleachbit.sourceforge.net/features)

**P**ara executar o programa, basta procurar o mesmo através do menu de aplicativos de seu sistema, ou através do comando:

```
 [kalib@tuxcaverna ~]$ bleachbit
```

**H**ave fun!
