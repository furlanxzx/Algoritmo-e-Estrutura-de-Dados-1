## 🧠 Conceito: A Política FIFO

Diferente da pilha (que vimos anteriormente usando a lógica LIFO), as filas funcionam baseadas na política **FIFO** (*First-In First-Out*, ou "O Primeiro a Entrar é o Primeiro a Sair"). 
*   **Dinâmica:** Em uma fila, nós sempre acrescentamos novos itens em uma extremidade (no final) e retiramos itens da extremidade oposta (do início).
*   **Ordem de Chegada:** Os elementos são rigorosamente ordenados pelo tempo de chegada, ou seja, o elemento que está há mais tempo aguardando é o único que pode ser consultado ou removido.

---

## 🚀 Aplicações Práticas e Operações

As filas são extremamente comuns no nosso dia a dia e fundamentais na computação. Veja algumas aplicações clássicas:
*   Fila de processos gerenciados pelo Sistema Operacional.
*   Fila de impressão e buffer de teclas acionadas no teclado.
*   Sistemas de atendimento (bancos, check-in de voos).

Para manipularmos essa estrutura, precisamos garantir as seguintes operações básicas:
*   Criar uma fila vazia e Liberar a fila (memória).
*   Inserir um elemento no fim e Retirar um do início.
*   Listar um elemento específico ou listar toda a fila.

---

## 🏗️ Estruturando a Fila Dinâmica em C

Para construir uma fila dinâmica eficientemente, precisamos de **duas estruturas (`structs`)**. A primeira define o nó (igual ao da lista encadeada), e a segunda é uma estrutura de controle que guarda os ponteiros para o início e o fim da fila, facilitando a inserção rápida no final.

```c
// 1. Estrutura de cada Nó (Elemento da fila)
typedef struct no {
    int info;
    struct no* prox;
} TNo;
typedef TNo *PNo;

// 2. Estrutura de Controle da Fila (Guarda as pontas)
typedef struct fila {
    PNo ini; // Aponta para o primeiro elemento (onde removemos)
    PNo fim; // Aponta para o último elemento (onde inserimos)
} TFila;
typedef TFila *PFila;
```

---

## 🎬 Inicializando a Fila

A primeira função prática é a de criação. A função `cria()` serve para alocar dinamicamente apenas a estrutura de controle (`TFila`) e preparar o terreno, definindo que a fila começa vazia (com início e fim apontando para `NULL`).

```c
PFila cria() {
    // Aloca espaço para a estrutura controladora da fila
    PFila f = (PFila) malloc(sizeof(TFila));
    
    // Como a fila está vazia, o início e o fim são nulos
    f->ini = f->fim = NULL;
    
    // Retorna o ponteiro de controle para a função principal
    return (f);
}
```

---
## 📝 Resolução dos Exercícios Práticos: Fila Dinâmica

---

### Exercício 1: Operações Básicas
**Nível de Dificuldade:** 🟢 Fácil
> * Escreva as funções de inserção, remoção, impressão e liberação de uma fila encadeada. 
> * Lembre que a inserção deve sempre ser no fim da fila e a remoção, no início.

<details>
<summary><b>👀 Ver resposta (Código em C das Operações)</b></summary>

Este código consolida os algoritmos apresentados nos slides da aula para manipular a fila perfeitamente:

```c
/* Insere no fim da fila */
PFila insere (PFila f, int v) {
    PNo novo = (PNo) malloc(sizeof(TNo)); //
    novo->info = v;                       //
    novo->prox = NULL;                    //
    
    if (f->fim != NULL) /* verifica se lista não estava vazia */ //
        f->fim->prox = novo;              //
    else 
        f->ini = novo; /* fila vazia */   //
        
    f->fim = novo;                        //
    return (f);                           //
}

/* Retira do início da fila */
PFila retira (PFila f, int *v) {
    PNo p;                                //
    if (f->ini==NULL) /* fila vazia */    //
        printf("\nFila vazia!!\n");       //
    else {
        *v = f->ini->info;                //
        p = f->ini;                       //
        if (f->ini == f->fim) { /* só tem um nó na fila */ //
            f->ini = f->fim = NULL;       //
        }
        else
            f->ini = p->prox; /* ou f->ini = f->ini->prox; */ //
        free(p);                          //
    }
    return f;                             //
}

/* Imprime fila (sempre do início para o fim) */
void imprime(PFila f) {
    PNo p;                                //
    if (f->ini==NULL)  /* fila vazia */   //
        printf("\nFila vazia!!\n");       //
    else {
        for (p = f->ini; p!=NULL; p = p->prox) //
            printf("%d ",p->info);        //
    }
}

/* Libera: quase igual à que libera lista */
void libera (PFila f) {
    PNo p;                                //
    for (p = f->ini; p!=NULL; p = f->ini) { //
        f->ini = p->prox;  // ou f->ini = f->ini->prox; //
        free(p);                          //
    }
    free(f);                              //
}
```
</details>

---

### Exercício 2: Separação de Filas
**Nível de Dificuldade:** 🟡 Médio
> * Faça uma função que receba 3 filas (f, f_pares e f_impares) e separe todos os valores guardados em f de tal forma que os valores pares sejam movidos para f_pares e os ímpares, para f_impares. 
> * No final, f deve estar vazia. 
> * Considere que f_pares e f_impares ainda não existem.

<details>
<summary><b>👀 Ver resposta (Código em C)</b></summary>

```c
// Assumindo o uso da função cria(), insere() e retira() definidas anteriormente
void separa_filas(PFila f, PFila *f_pares, PFila *f_impares) {
    *f_pares = cria(); // Cria a fila de pares
    *f_impares = cria(); // Cria a fila de ímpares
    
    int valor;
    
    // Enquanto a fila original não estiver vazia
    while (f->ini != NULL) {
        retira(f, &valor); // Retira da original
        
        if (valor % 2 == 0) {
            insere(*f_pares, valor); // Se par, vai para f_pares
        } else {
            insere(*f_impares, valor); // Se ímpar, vai para f_impares
        }
    }
}
```
</details>

---

### Exercício 3: Fila de Atendimento com Tempo de Espera
**Nível de Dificuldade:** 🔴 Difícil
> * Em um centro de atendimento, os pacientes aguardam em uma fila única e são atendidos na ordem de chegada. 
> * O sistema funciona em iterações discretas, que representam unidades de tempo. 
> * A cada iteração, o usuário pode: inserir um novo paciente na fila, ou atender (remover) o paciente do início da fila.
> * Cada paciente possui um tempo de espera, inicialmente igual a 0 quando entra na fila. 
> * A cada iteração do programa, todos os pacientes que permanecem na fila têm seu tempo de espera incrementado em uma unidade.
> * Quando um paciente é atendido, o sistema deve exibir: Seu identificador (ID) e O tempo total de espera corresponde ao número de iterações em que ele permaneceu na fila.
> * O programa deve apresentar o seguinte menu: 1 - Inserir paciente, 2 - Atender paciente, 3 - Exibir fila, 0 - Sair.

<details>
<summary><b>👀 Ver resposta (Lógica Estrutural em C)</b></summary>

Para resolver este problema, a estrutura do nó precisará ser adaptada para guardar duas informações: o ID do paciente e o tempo de espera.

```c
typedef struct noPaciente {
    int id;
    int tempo_espera;
    struct noPaciente* prox;
} TNoPaciente;

// Função auxiliar para incrementar o tempo a cada iteração
void incrementa_tempo(PFila f) {
    TNoPaciente* atual = f->ini;
    while (atual != NULL) {
        atual->tempo_espera++;
        atual = atual->prox;
    }
}

// A lógica principal no main() seria um loop iterativo:
/*
   int opcao;
   do {
       printf("1 - Inserir paciente\n2 - Atender paciente\n3 - Exibir fila\n0 - Sair\n");
       scanf("%d", &opcao);
       
       if (opcao == 1) {
           // ler id, inserir na fila com tempo 0
           incrementa_tempo(fila); // incrementa quem já estava lá
       } else if (opcao == 2) {
           // retirar do início, imprimir ID e tempo_espera
           incrementa_tempo(fila); 
       } else if (opcao == 3) {
           // imprimir ID e tempo de todos
       }
   } while (opcao != 0);
*/
```
</details>

---

### Exercícios Adicionais: Fila de Supermercado (Prioridade)
**Nível de Dificuldade:** 🔴 Difícil
> * Em um supermercado, há vários guichês, mas os clientes sempre esperam numa fila única e são atendidos conforme a disponibilidade. 
> * Sendo assim, cada pessoa que chega entra no final desta fila. 
> * No entanto, cada vez que chega uma pessoa com atendimento prioritário (por exemplo, um idoso), ela deverá ser atendida antes de qualquer outra pessoa não prioritária, respeitando, porém, a ordem de chegada das pessoas com atendimento prioritário na fila. 
> * Sendo assim, ao chegar a primeira pessoa com atendimento prioritário, ela deverá entrar no início da fila. 
> * Ao chegar a próxima pessoa com atendimento prioritário, esta deverá entrar logo atrás da primeira que chegou necessitando de atendimento prioritário, e assim por diante. 
> * Armazene apenas o tipo da pessoa como informação, pois isso será necessário para o controle da fila. 
> * Considere que cada tipo é representado por uma única letra: P para prioritário e O para outros. 
> * Faça um programa que controle a entrada das pessoas na fila e o atendimento de cada uma. 
> * A cada passo do laço, o programa deverá perguntar ao usuário se quer inserir uma pessoa na fila ou retirar e imprimir a fila a cada iteração. 
> * O programa deverá terminar quando não houver mais pessoas na fila.

<details>
<summary><b>👀 Ver resposta (Dica de Resolução)</b></summary>

Para este exercício, a manipulação direta dos ponteiros será necessária para inserir um elemento "no meio" da fila (após o último 'P' e antes do primeiro 'O'). 

```c
// Lógica simplificada de inserção com prioridade
void insere_prioridade(PFila f, char tipo) {
    PNo novo = (PNo) malloc(sizeof(TNo));
    novo->info = tipo; // Assumindo que info agora é char
    novo->prox = NULL;
    
    if (f->ini == NULL) { // Fila vazia
        f->ini = f->fim = novo;
    } else if (tipo == 'O') { // Inserção normal no fim
        f->fim->prox = novo;
        f->fim = novo;
    } else if (tipo == 'P') { // Inserção de Prioridade
        if (f->ini->info == 'O') {
            // Se o primeiro já é 'O', o novo 'P' vira o novo início
            novo->prox = f->ini;
            f->ini = novo;
        } else {
            // Percorre até achar o último 'P'
            PNo atual = f->ini;
            while (atual->prox != NULL && atual->prox->info == 'P') {
                atual = atual->prox;
            }
            // Insere o novo 'P' logo após o último 'P' encontrado
            novo->prox = atual->prox;
            atual->prox = novo;
            
            // Se o novo 'P' foi parar no final, atualiza o ponteiro de fim
            if (novo->prox == NULL) {
                f->fim = novo;
            }
        }
    }
}
```
</details>
