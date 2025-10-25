---
title: "Dica Rápida Linux: Troubleshooting Em Tempo De Execução Com Strace"
date: 2013-12-19T20:04:45-05:00
draft: false
tags: [
"Linux",
"Software Livre",
"Segurança",
]
categories: ["Tutoriais"]
description: "Utilizando o comando strace para realização de análise e troubleshooting em tempo de execução"
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---
**S**e algum dia você já precisou realizar uma análise para troubleshoot de algum comando em tempo de execução e não soube como fazê-lo, seus problemas acabaram. O **strace** faz justamente isso.

<!--more-->

**J**á trabalhei em servidores de clientes comprometidos pós-invasão cujos comandos padrões **Unix** haviam sido substituídos por comandos *similares* (ao menos em nome) que realizam outras tarefas sem o conhecimento dos administradores dos mesmos.

**O** strace serve justamente para estes, bem como outros, casos. Ele lhe indica exatamente tudo o que acontece *por baixo dos panos* em seu sistema.

**V**ejamos um exemplo.

```bash
 [kalib@tuxcaverna ~]$ date
 Qui Dez 19 08:41:55 BRT 2013
```

**A**gora vejamos a diferença com o uso do strace.

```bash
 [kalib@tuxcaverna ~]$ strace date
 execve("/usr/bin/date", ["date"], [/* 59 vars */]) = 0
 brk(0)                                  = 0x21df000
 access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
 open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
 fstat(3, {st_mode=S_IFREG|0644, st_size=274630, ...}) = 0
 mmap(NULL, 274630, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fe0d3f3d000
 close(3)                                = 0
 open("/usr/lib/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
 read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\20\34\2\0\0\0\0\0"..., 832) = 832
 fstat(3, {st_mode=S_IFREG|0755, st_size=2031229, ...}) = 0
 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3f3c000
 mmap(NULL, 3840528, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fe0d39b8000
 mprotect(0x7fe0d3b58000, 2097152, PROT_NONE) = 0
 mmap(0x7fe0d3d58000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a0000) = 0x7fe0d3d58000
 mmap(0x7fe0d3d5e000, 14864, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3d5e000
 close(3)                                = 0
 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3f3b000
 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3f3a000
 arch_prctl(ARCH_SET_FS, 0x7fe0d3f3b700) = 0
 mprotect(0x7fe0d3d58000, 16384, PROT_READ) = 0
 mprotect(0x60d000, 4096, PROT_READ)     = 0
 mprotect(0x7fe0d3f81000, 4096, PROT_READ) = 0
 munmap(0x7fe0d3f3d000, 274630)          = 0
 brk(0)                                  = 0x21df000
 brk(0x2200000)                          = 0x2200000
 open("/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
 fstat(3, {st_mode=S_IFREG|0644, st_size=1863120, ...}) = 0
 mmap(NULL, 1863120, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fe0d37f1000
 close(3)                                = 0
 open("/etc/localtime", O_RDONLY|O_CLOEXEC) = 3
 fstat(3, {st_mode=S_IFREG|0644, st_size=714, ...}) = 0
 fstat(3, {st_mode=S_IFREG|0644, st_size=714, ...}) = 0
 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3f80000
 read(3, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\0"..., 4096) = 714
 lseek(3, -438, SEEK_CUR)                = 276
 read(3, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\3\0\0\0\3\0\0\0\0"..., 4096) = 438
 close(3)                                = 0
 munmap(0x7fe0d3f80000, 4096)            = 0
 fstat(1, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 2), ...}) = 0
 mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe0d3f80000
 write(1, "Qui Dez 19 08:42:47 BRT 2013\n", 29Qui Dez 19 08:42:47 BRT 2013
 ) = 29
 close(1)                                = 0
 munmap(0x7fe0d3f80000, 4096)            = 0
 close(2)                                = 0
 exit_group(0)                           = ?
 +++ exited with 0 +++
```

**O** strace não é instalado por padrão em todas as distribuições, portanto é possível que você precise instalá-lo com o seu gerenciador de pacotes.

**A**lém do uso regular, ele possui diversos parâmetros que podem melhorar ou filtrar o seu uso. Você pode verificar a lista de parâmetros em seu manual de uso:

```bash
 [kalib@tuxcaverna ~]$ man strace
```
