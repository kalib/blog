---
title: "Goroutines - O que são e como utilizar"
date: 2025-10-25T12:27:17-04:00
draft: false
tags: [
"Golang",
"Goroutines",
"Google"
]
categories: ["Tutoriais"]
description: "Uma das características mais poderosas da linguagem Go é seu modelo de concorrência nativo, centrado no conceito de goroutines. Enquanto outras linguagens dependem de bibliotecas externas ou de complexos sistemas de threads do sistema operacional, Go traz a concorrência como cidadã de primeira classe."
images: []
resources:
- name: "featured-image"
  src: "featured-image.jpg"
lightgallery: true
---

Uma das características mais poderosas da linguagem [Go](https://go.dev/) é seu modelo de concorrência nativo, centrado no conceito de **goroutines**. Enquanto outras linguagens dependem de bibliotecas externas ou de complexos sistemas de threads do sistema operacional, Go traz a concorrência como cidadã de primeira classe.

<!--more-->

# Que tal voltar um pouco?

Se você ainda não conhece muito sobre esta magnífica linguagem de programação criada pelo Google, sinta-se convidado a voltar um pouco no tempo e conferir posts que escrevi anteriormente:

- [Go Ou Go!, Linguagem Do Google Cria Primeira Intriga](/posts/go-ou-go-linguagem-do/)
- [Go Ou Golang: Porquê Adotei Go Como Minha Linguagem Favorita](/posts/go-ou-golang-porque-adotei/)

## Introdução

Goroutines são importantes porque permitem que os programas executem tarefas simultaneamente de forma eficiente, aproveitando melhor os recursos do processador. Elas são mais leves que as threads do sistema operacional, com baixo custo de criação e gerenciamento, o que possibilita a execução de milhares delas e facilita a criação de softwares concorrentes e de alta performance.

## O que são Goroutines?

Goroutines são funções ou métodos que são executados concorrentemente com outras funções. Elas são threads leves gerenciadas pelo runtime do Go, não pelo sistema operacional.

### Características e Vantagens Principais:

- **Leves:** Consomem apenas 2KB de stack inicialmente (comparado com 1-2MB de threads no SO);

- **Gerenciadas pelo Go runtime:** Escalonadas pelo próprio Go, não pelo SO. O Go runtime as aloca utilizando um mecanismo chamado Go scheduler (agendador);

- **Eficientes:** Você pode criar milhares sem impactar performance, o que as torna baratas por tempo de execução;

- **Execução concorrente:** Quando você executa `go minhaFuncao()`, a função minhaFuncao começa a ser executada em segundo plano, enquanto o restante do programa continua;

- **Compartilhamento de memória:** Goroutines rodam no mesmo espaço de endereço, o que significa que elas podem acessar a mesma memória.

- **Sincronização:** Para evitar problemas como condições de corrida (race conditions), o Go oferece mecanismos de sincronização como channels e WaitGroups;

    - **Channels:** Permitem que goroutines se comuniquem de forma segura, enviando dados de uma para outra;

    - **WaitGroups:** Permitem que você espere que um conjunto de goroutines termine antes de continuar.

### Como Funcionam

Go implementa um modelo **M:N** onde:

- **M** goroutines são multiplexadas em **N** threads do sistema operacional;

- O scheduler do Go gerencia essa distribuição automaticamente;

- Utiliza o conceito de channel para comunicação segura entre goroutines;

#### O modelo M:N

O modelo de threading M:N trás uma abordagem inovadora, que redefine a maneira como a simultaneidade é gerenciada em linguagens de programação. Em essência, o modelo M:N mapeia dinamicamente um conjunto de threads de nível de kernel, permitindo uma alocação flexível e adaptável de recursos. Ao contrário dos modelos de threading tradicionais, em que a relação entre threads de nível de usuário e threads de nível de kernel é fixa (**N:1**), o modelo M:N otimiza a utilização de recursos ajustando dinamicamente o mapeamento com base na carga de trabalho e nas condições do sistema.

## Exemplos Práticos

### Exemplo 1: Programa Síncrono vs Concorrente

Repare que no exemplo a seguir, ambas as funções fazem exatamente a mesma coisa:

- Recebem uma string nome como argumento;

- Entram em um loop que executa 3 passos;

- Em cada passo, imprimem o nome da tarefa e o número do passo;

- Pausam a execução por 1 segundo `(time.Sleep(1 * time.Second))` para simular uma tarefa que leva tempo;

Elas são idênticas em conteúdo, mas são chamadas de maneiras diferentes na função main.

```go
package main

// Importa o pacote fmt para formatação de I/O (impressão)
// e o pacote time para medir o tempo e pausar a execução
import (
    "fmt"
    "time"
)

// Versão síncrona
func tarefaSincrona(nome string) {
    for i := 1; i <= 3; i++ {
        fmt.Printf("%s: passo %d\n", nome, i)
        time.Sleep(1 * time.Second)
    }
}

// Versão concorrente
func tarefaConcorrente(nome string) {
    for i := 1; i <= 3; i++ {
        fmt.Printf("%s: passo %d\n", nome, i)
        time.Sleep(1 * time.Second)
    }
}

func main() {
    fmt.Println("=== Versão Síncrona ===")
    inicio := time.Now()
    tarefaSincrona("Tarefa A")
    tarefaSincrona("Tarefa B")
    fmt.Printf("Tempo total: %v\n", time.Since(inicio))

    fmt.Println("\n=== Versão Concorrente ===")
    inicio = time.Now()
    go tarefaConcorrente("Tarefa C")  // 'go' cria uma goroutine
    go tarefaConcorrente("Tarefa D")  // outra goroutine
    time.Sleep(4 * time.Second)       // espera as goroutines terminarem
    fmt.Printf("Tempo total: %v\n", time.Since(inicio))
}
```

O resultado da execução deste código será o seguinte:

```
=== Versão Síncrona ===
Tarefa A: passo 1
Tarefa A: passo 2
Tarefa A: passo 3
Tarefa B: passo 1
Tarefa B: passo 2
Tarefa B: passo 3
Tempo total: 6s

=== Versão Concorrente ===
Tarefa C: passo 1
Tarefa D: passo 1
Tarefa C: passo 2
Tarefa D: passo 2
Tarefa C: passo 3
Tarefa D: passo 3
Tempo total: 3s
```

Podemos perceber que a versão Concorrente (utiilizando Goroutines) levou a metade do tempo.

**Execução Síncrona:**

O Go executa a `tarefaSincrona("Tarefa A")` completamente antes de começar a `tarefaSincrona("Tarefa B")`.

Como cada tarefa leva 3 segundos *(3 passos de 1 segundo cada)*, o tempo total de execução será de aproximadamente 6 segundos *(3 segundos para A + 3 segundos para B)*.

As tarefas esperam a anterior terminar antes de iniciar.

**Execução Concorrente:**

O uso da palavra-chave `go` antes da chamada da função transforma a execução em uma goroutine. Uma goroutine é um thread leve gerenciado pelo runtime do Go, permitindo que as tarefas rodem concorrentemente (aparentemente ao mesmo tempo).

`Tarefa C` e `Tarefa D` são iniciadas quase que instantaneamente e rodam lado a lado.

A linha `time.Sleep(4 * time.Second)` é crucial na execução concorrente aqui. A função principal *(main)* de um programa Go **não** espera que as goroutines terminem. Se o main terminasse imediatamente, as goroutines Tarefa C e Tarefa D seriam interrompidas antes de concluir. Este *sleep* força a função main a esperar 4 segundos, dando tempo suficiente (cada tarefa leva 3 segundos) para que ambas as tarefas concorrentes terminem. (Alerta de Spoiler: No **Exemplo 3** veremos uma solução melhor para isto.)

Como as tarefas rodam concorrentemente, a duração total é determinada pela tarefa mais longa. Ambas levam 3 segundos, então o tempo total de execução será de aproximadamente 3 a 4 segundos.

As tarefas se intercalam e trabalham em paralelo, reduzindo o tempo total. A saída no console mostrará as mensagens de "Tarefa C" e "Tarefa D" misturadas/intercaladas.

### Exemplo 2: Usando Channels para Comunicação

Este código é um exemplo clássico de Pool de Workers usando Goroutines e **Channels** para implementar um padrão de concorrência. Ele permite que você execute várias tarefas simultaneamente de forma eficiente.

De forma resumida, temos uma fila de tarefas (`jobs`) e um grupo de trabalhadores (`workers`) que pegam e processam essas tarefas em paralelo, depositando os resultados em outra fila (`results`).

```go
package main

import (
    "fmt"
    "time"
)

// Goroutine que envia dados para um channel
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        fmt.Printf("Worker %d processando job %d\n", id, j)
        time.Sleep(1 * time.Second)  // Simula trabalho
        results <- j * 2             // Envia resultado
    }
}

func main() {
    const numJobs = 5
    jobs := make(chan int, numJobs)
    results := make(chan int, numJobs)

    // Inicia 3 workers
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }

    // Envia jobs
    for j := 1; j <= numJobs; j++ {
        jobs <- j
    }
    close(jobs)

    // Coleta resultados
    for a := 1; a <= numJobs; a++ {
        resultado := <-results
        fmt.Printf("Resultado recebido: %d\n", resultado)
    }
}
```

No código acima, `Channels` são o meio pelo qual as goroutines se comunicam. Eles podem ser vistos como filas ou tubos por onde os dados são enviados e recebidos.

- `jobs := make(chan int, numJobs)`: Este é o canal de tarefas.

    - É um canal de inteiros.

    - O `<—chan int` na função `worker` indica que o canal é somente para receber (`jobs`).

- `results := make(chan int, numJobs)`: Este é o canal de resultados.

    - O `chan—> int` na função `worker` indica que o canal é somente para enviar (`results`).

- Ambos são criados com `buffer` (capacidade para 5 itens), o que significa que podem armazenar até 5 valores antes que o remetente precise esperar o receptor.

Já os `Workers` ou trabalhadores... As `Goroutines` são os `lightweight workers` (trabalhadores leves) do Go.

- A função `worker` é o código que cada trabalhador executa.

- O bloco que inicia os `workers`:

    ```go
    for w := 1; w <= 3; w++ {
        go worker(w, jobs, results)
    }
    ```

    - Isto inicia 3 goroutines independentes (Workers 1, 2 e 3) que rodarão concorrentemente.

### Exemplo 3: WaitGroup para Sincronização

O código a seguir exemplifica a utilização do pacote `sync`, especificamente o tipo `sync.WaitGroup`, para gerenciar e sincronizar a execução de múltiplas goroutines. Ele resolve o problema de garantir que a função principal (`main`) espere que todas as tarefas concorrentes terminem antes de encerrar o programa.

Se lembrarmos bem, no **Exemplo 1** utilizamos `time.Sleep()` na função `main` para esperar pelas goroutines, o que é uma solução ruim e imprecisa. O `sync.WaitGroup` oferece a solução correta e robusta.

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func download(site string, wg *sync.WaitGroup) {
    defer wg.Done()  // Marca como concluído ao final

    fmt.Printf("Iniciando download de %s\n", site)
    time.Sleep(2 * time.Second)  // Simula download
    fmt.Printf("Download de %s concluído\n", site)
}

func main() {
    var wg sync.WaitGroup
    sites := []string{"site1.com", "site2.com", "site3.com", "site4.com"}

    for _, site := range sites {
        wg.Add(1)  // Incrementa o contador
        go download(site, &wg)
    }

    fmt.Println("Aguardando downloads...")
    wg.Wait()  // Espera todas as goroutines terminarem
    fmt.Println("Todos downloads concluídos!")
}
```

O `WaitGroup` é essencialmente um contador interno que você gerencia com três métodos:

- `wg.Add(n int)`: Adiciona `n` ao contador do WaitGroup. Chamamos isso antes de iniciar uma nova goroutine para avisar que há mais uma tarefa a ser esperada.

- `wg.Done()`: Subtrai 1 do contador. É tipicamente chamado ao final da goroutine (frequentemente usando `defer`).

- `wg.Wait()`: Bloqueia a execução da goroutine que o chamou (neste caso, a `main`) até que o contador do WaitGroup chegue a zero.

Em resumo, o código executa os **4 downloads concorrentemente**. Em vez de levar **8 segundos** (*4 sites vezes 2 segundos cada, sequencialmente*), o tempo total de execução será de aproximadamente **2 segundos**, pois todas as tarefas de download rodam ao mesmo tempo. A saída no console mostrará as mensagens de "Iniciando download" e "Download... concluído" intercaladas.

O `WaitGroup` é a forma idiomática e mais segura no Go para esperar por um conjunto de goroutines.

## Conclusão

Goroutines tornam a concorrência acessível e eficiente em Go. Combinadas com channels para comunicação e sync primitives (como waitgroup) para coordenação, elas formam uma base sólida para construir sistemas concorrentes robustos.

Principais vantagens:

:(fas fa-rocket fa-fw): Fácil de usar (apenas adicione go antes da função)

:(fas fa-rocket fa-fw): Eficiente em memória e CPU

:(fas fa-rocket fa-fw): Integração nativa com channels

:(fas fa-rocket fa-fw): Gerenciamento automático pelo runtime

Até a próxima.