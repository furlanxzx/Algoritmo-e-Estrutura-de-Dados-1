<h1 align="center">Lista Circular</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/08%20%E2%80%A2%20Lista%20Encadeada%20Simples/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Encadeada_Simples-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/10%20%E2%80%A2%20Lista%20Duplamente%20Encadeada/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Duplamente_Encadeada_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

### Definição
*   Numa lista circular, o último elemento tem como próximo o primeiro elemento da lista, formando um ciclo.
*   A rigor, neste caso, não faz sentido falarmos em primeiro ou último elemento.
*   A lista pode ser representada por um ponteiro para um elemento inicial qualquer da lista.

---

### 📌 Convenção: Ponteiro para o Último
*   Uma convenção interessante numa lista circular é guardarmos o endereço do **último nó** ao invés do primeiro. 
*   Para saber qual é o primeiro, basta pegar o endereço do nó seguinte ao último (`ultimo->prox`).
*   Esta convenção tem a vantagem de poder incluir ou remover um elemento convenientemente a partir do início ou final de uma lista.
*   Além disso, é estabelecida a convenção de que um ponteiro nulo representa uma lista circular vazia.

---

### 🖨️ Função para a Impressão

Pense em como poderia ser a impressão em uma lista circular. Como não há um fim marcado por `NULL`, iteramos até alcançar novamente o início.

```c
void imprime_circular (PLista ultimo) {
    PLista p;
    if (ultimo != NULL) {
        p = ultimo->prox;
        /* percorre os elementos até alcançar novamente o início */
        do {
            printf("%d\n", p->info);
            p = p->prox;
        } while (p != ultimo->prox);
    }
}
```

---

## 📝 Lista de Exercícios de Fixação

**Nota:** Para todas as resoluções, assumimos a estrutura padrão `typedef struct lista { int info; struct lista* prox; } TLista; typedef TLista* PLista;`.

### Operações Básicas
> **A.** Faça a inserção em lista circular na primeira posição da lista (lembre-se que a inserção pode ser feita em qualquer lugar).

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
PLista insere_inicio_circular(PLista ultimo, int v) {
    PLista novo = (PLista) malloc(sizeof(TLista));
    novo->info = v;
    
    if (ultimo == NULL) { /* Lista estava vazia */
        novo->prox = novo;
        return novo;      /* Novo nó é o último (e o primeiro) */
    }
    
    /* Insere logo após o último (ou seja, na primeira posição) */
    novo->prox = ultimo->prox;
    ultimo->prox = novo;
    
    return ultimo; /* O último não muda, pois inserimos no início */
}
```
</details>

> **B.** Faça uma função para liberar todo o espaço alocado em uma lista circular.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
void libera_circular(PLista ultimo) {
    if (ultimo == NULL) return;
    
    PLista p = ultimo->prox; /* Começa do primeiro */
    
    /* Transforma em lista simples quebrando o ciclo para facilitar a liberação */
    ultimo->prox = NULL; 
    
    while (p != NULL) {
        PLista t = p;
        p = p->prox;
        free(t);
    }
}
```
</details>

### Exercício 1
> Faça a função para remover um nó com um dado valor em lista circular.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
PLista remove_valor_circular(PLista ultimo, int v) {
    if (ultimo == NULL) return NULL;
    
    PLista p = ultimo->prox;
    PLista ant = ultimo;
    
    do {
        if (p->info == v) {
            /* Caso 1: É o único elemento da lista */
            if (p == ultimo && p->prox == ultimo) {
                free(p);
                return NULL;
            }
            
            /* Caso Geral: Remove o nó 'p' */
            ant->prox = p->prox;
            
            /* Caso 2: O elemento removido era o 'ultimo' */
            if (p == ultimo) {
                ultimo = ant; /* Atualiza o ponteiro do último */
            }
            
            free(p);
            return ultimo;
        }
        ant = p;
        p = p->prox;
    } while (p != ultimo->prox);
    
    return ultimo; /* Valor não encontrado */
}
```
</details>

### Exercício 2
> Faça as funções Push() e Pop() utilizando listas circulares. Considere `ultimo` um ponteiro para o último nó da lista circular e considere que o primeiro nó seja o topo da pilha.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
/* Push: Inserir no topo da pilha (início da lista circular) */
PLista push_circular(PLista ultimo, int v) {
    PLista novo = (PLista) malloc(sizeof(TLista));
    novo->info = v;
    if (ultimo == NULL) {
        novo->prox = novo;
        return novo;
    }
    novo->prox = ultimo->prox;
    ultimo->prox = novo;
    return ultimo;
}

/* Pop: Remover do topo da pilha (início da lista circular) */
PLista pop_circular(PLista ultimo, int *v_retornado) {
    if (ultimo == NULL) return NULL;
    
    PLista topo = ultimo->prox;
    *v_retornado = topo->info;
    
    if (topo == ultimo) { /* Único elemento */
        free(topo);
        return NULL;
    }
    
    ultimo->prox = topo->prox;
    free(topo);
    return ultimo;
}
```
</details>

### Exercício 3
> Faça as funções de inserção e remoção em Fila utilizando lista circular.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
/* Enqueue (Inserção na Fila): Insere no final da lista circular */
PLista enqueue_circular(PLista ultimo, int v) {
    PLista novo = (PLista) malloc(sizeof(TLista));
    novo->info = v;
    if (ultimo == NULL) {
        novo->prox = novo;
        return novo;
    }
    novo->prox = ultimo->prox;
    ultimo->prox = novo;
    
    /* A diferença para o Push é que o novo nó passa a ser o último! */
    return novo; 
}

/* Dequeue (Remoção na Fila): Remove do início da lista (mesma lógica do Pop) */
PLista dequeue_circular(PLista ultimo, int *v_retornado) {
    if (ultimo == NULL) return NULL;
    
    PLista frente = ultimo->prox;
    *v_retornado = frente->info;
    
    if (frente == ultimo) { /* Único elemento */
        free(frente);
        return NULL;
    }
    
    ultimo->prox = frente->prox;
    free(frente);
    return ultimo;
}
```
</details>

---

## 📝 Exercícios de Fixação: Listas Circulares (Avançado)

**Nota:** As resoluções utilizam a convenção de que a lista circular é manipulada através de um ponteiro para o **último** nó.

### Exercício 3: Múltiplas Operações
> Escreva funções para efetuar cada uma das operações a seguir para listas circulares: 
> * Incluir um elemento no meio de uma lista 
> * Concatenar duas listas 
> * Inverter uma lista 
> * Eliminar o último elemento da lista 
> * Eliminar o enésimo elemento da lista 
> * Combinar duas listas ordenadas numa única lista ordenada 
> * Colocar os elementos de uma lista em ordem crescente 
> * Retornar o número de elementos da lista

<details>
<summary><b>👀 Ver resposta (Principais Operações)</b></summary>

Como o exercício pede muitas funções, aqui estão as implementações mais cruciais para a estrutura circular (Contagem, Concatenação e Remoção do último).

```c
/* 1. Retornar o número de elementos */
int conta_nos(PLista ultimo) {
    if (ultimo == NULL) return 0;
    int cont = 0;
    PLista p = ultimo->prox;
    do {
        cont++;
        p = p->prox;
    } while (p != ultimo->prox);
    return cont;
}

/* 2. Concatenar duas listas circulares */
PLista concatena_circulares(PLista u1, PLista u2) {
    if (u1 == NULL) return u2;
    if (u2 == NULL) return u1;
    
    /* O último da u1 aponta para o primeiro da u2 */
    /* E o último da u2 aponta para o primeiro da u1 */
    PLista primeiro_u1 = u1->prox;
    u1->prox = u2->prox;
    u2->prox = primeiro_u1;
    
    return u2; /* O novo último geral passa a ser o último da segunda lista */
}

/* 3. Eliminar o último elemento */
PLista remove_ultimo(PLista ultimo) {
    if (ultimo == NULL) return NULL;
    
    PLista p = ultimo->prox;
    
    /* Se tiver apenas um elemento */
    if (p == ultimo) {
        free(ultimo);
        return NULL;
    }
    
    /* Percorre até achar o penúltimo */
    while (p->prox != ultimo) {
        p = p->prox;
    }
    
    /* O penúltimo passa a apontar para o primeiro */
    p->prox = ultimo->prox;
    free(ultimo);
    
    return p; /* O penúltimo passa a ser o novo último */
}
```
</details>

---

### Exercício 4: Problema de Josephus (Simulação de Fuga)
> Um grupo de soldados está rodeado por forças inimigas... Eles formam um círculo e sorteiam num chapéu um número $n$ e um nome. Começando pelo soldado cujo nome foi sorteado, é começada a contagem no sentido horário e quando atingir o número $n$, este soldado é retirado. A contagem reinicia... até que só reste um. 
> 
> Faça um programa que simule a formação dos soldados neste círculo e a saída na forma explicada até restar apenas um.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct soldado {
    char nome[50];
    struct soldado *prox;
} TSoldado;

/* Função para resolver o problema de Josephus */
void simula_fuga(TSoldado *ultimo, char *nome_inicio, int n) {
    if (ultimo == NULL) return;

    TSoldado *p = ultimo->prox;
    TSoldado *ant = ultimo;

    /* 1. Encontra o soldado inicial */
    while (strcmp(p->nome, nome_inicio) != 0) {
        ant = p;
        p = p->prox;
    }

    /* 2. Roda a eliminação até sobrar 1 */
    while (p->prox != p) {
        /* Pula (n-1) soldados para parar no enésimo */
        for (int i = 1; i < n; i++) {
            ant = p;
            p = p->prox;
        }

        /* p agora é o soldado a ser removido */
        printf("Soldado retirado: %s\n", p->nome);
        ant->prox = p->prox;
        
        TSoldado *temp = p;
        p = p->prox; /* O próximo a iniciar a contagem */
        free(temp);
    }

    printf("\n>>> O soldado que vai buscar ajuda a cavalo é: %s <<<\n", p->nome);
    free(p); /* Libera o último sobrevivente no final da simulação */
}
```
</details>

---

### Exercício 5: Rotação de Conjuntos
> Considere um problema em que precisamos rotacionar um determinado conjunto de dados. Por exemplo, rotacionar "a b c d e" 2 vezes resulta em "c d e a b". Considerando que não é permitido apenas trocar o valor de um nó para outro (é necessário realmente trocar as posições dos nós), escreva uma função para realizar essas operações.

<details>
<summary><b>👀 Ver resposta</b></summary>

Neste exercício, usar uma lista circular simplifica o problema drasticamente! Rotacionar fisicamente os nós para a esquerda "k" vezes significa simplesmente avançar o ponteiro que marca o último elemento.

```c
/* Considerando a estrutura: typedef struct no { char info; struct no* prox; } TNo; */

TNo* rotaciona_lista(TNo* ultimo, int k) {
    if (ultimo == NULL || k <= 0) {
        return ultimo;
    }
    
    /* Na lista circular, basta avançar o ponteiro "ultimo" k posições. 
       O encadeamento do anel se mantém intacto! */
       
    for (int i = 0; i < k; i++) {
        ultimo = ultimo->prox;
    }
    
    return ultimo; /* Retorna o novo ponteiro que marca o final da lista rotacionada */
}
```
</details>## 📝 Exercícios de Fixação: Listas Circulares (Avançado)

**Nota:** As resoluções utilizam a convenção de que a lista circular é manipulada através de um ponteiro para o **último** nó.

### Exercício 3: Múltiplas Operações
> Escreva funções para efetuar cada uma das operações a seguir para listas circulares: 
> * Incluir um elemento no meio de uma lista 
> * Concatenar duas listas 
> * Inverter uma lista 
> * Eliminar o último elemento da lista 
> * Eliminar o enésimo elemento da lista 
> * Combinar duas listas ordenadas numa única lista ordenada 
> * Colocar os elementos de uma lista em ordem crescente 
> * Retornar o número de elementos da lista

<details>
<summary><b>👀 Ver resposta (Principais Operações)</b></summary>

Como o exercício pede muitas funções, aqui estão as implementações mais cruciais para a estrutura circular (Contagem, Concatenação e Remoção do último).

```c
/* 1. Retornar o número de elementos */
int conta_nos(PLista ultimo) {
    if (ultimo == NULL) return 0;
    int cont = 0;
    PLista p = ultimo->prox;
    do {
        cont++;
        p = p->prox;
    } while (p != ultimo->prox);
    return cont;
}

/* 2. Concatenar duas listas circulares */
PLista concatena_circulares(PLista u1, PLista u2) {
    if (u1 == NULL) return u2;
    if (u2 == NULL) return u1;
    
    /* O último da u1 aponta para o primeiro da u2 */
    /* E o último da u2 aponta para o primeiro da u1 */
    PLista primeiro_u1 = u1->prox;
    u1->prox = u2->prox;
    u2->prox = primeiro_u1;
    
    return u2; /* O novo último geral passa a ser o último da segunda lista */
}

/* 3. Eliminar o último elemento */
PLista remove_ultimo(PLista ultimo) {
    if (ultimo == NULL) return NULL;
    
    PLista p = ultimo->prox;
    
    /* Se tiver apenas um elemento */
    if (p == ultimo) {
        free(ultimo);
        return NULL;
    }
    
    /* Percorre até achar o penúltimo */
    while (p->prox != ultimo) {
        p = p->prox;
    }
    
    /* O penúltimo passa a apontar para o primeiro */
    p->prox = ultimo->prox;
    free(ultimo);
    
    return p; /* O penúltimo passa a ser o novo último */
}
```
</details>

---

### Exercício 4: Problema de Josephus (Simulação de Fuga)
> Um grupo de soldados está rodeado por forças inimigas... Eles formam um círculo e sorteiam num chapéu um número $n$ e um nome. Começando pelo soldado cujo nome foi sorteado, é começada a contagem no sentido horário e quando atingir o número $n$, este soldado é retirado. A contagem reinicia... até que só reste um. 
> 
> Faça um programa que simule a formação dos soldados neste círculo e a saída na forma explicada até restar apenas um.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct soldado {
    char nome[50];
    struct soldado *prox;
} TSoldado;

/* Função para resolver o problema de Josephus */
void simula_fuga(TSoldado *ultimo, char *nome_inicio, int n) {
    if (ultimo == NULL) return;

    TSoldado *p = ultimo->prox;
    TSoldado *ant = ultimo;

    /* 1. Encontra o soldado inicial */
    while (strcmp(p->nome, nome_inicio) != 0) {
        ant = p;
        p = p->prox;
    }

    /* 2. Roda a eliminação até sobrar 1 */
    while (p->prox != p) {
        /* Pula (n-1) soldados para parar no enésimo */
        for (int i = 1; i < n; i++) {
            ant = p;
            p = p->prox;
        }

        /* p agora é o soldado a ser removido */
        printf("Soldado retirado: %s\n", p->nome);
        ant->prox = p->prox;
        
        TSoldado *temp = p;
        p = p->prox; /* O próximo a iniciar a contagem */
        free(temp);
    }

    printf("\n>>> O soldado que vai buscar ajuda a cavalo é: %s <<<\n", p->nome);
    free(p); /* Libera o último sobrevivente no final da simulação */
}
```
</details>

---

### Exercício 5: Rotação de Conjuntos
> Considere um problema em que precisamos rotacionar um determinado conjunto de dados. Por exemplo, rotacionar "a b c d e" 2 vezes resulta em "c d e a b". Considerando que não é permitido apenas trocar o valor de um nó para outro (é necessário realmente trocar as posições dos nós), escreva uma função para realizar essas operações.

<details>
<summary><b>👀 Ver resposta</b></summary>

Neste exercício, usar uma lista circular simplifica o problema drasticamente! Rotacionar fisicamente os nós para a esquerda "k" vezes significa simplesmente avançar o ponteiro que marca o último elemento.

```c
/* Considerando a estrutura: typedef struct no { char info; struct no* prox; } TNo; */

TNo* rotaciona_lista(TNo* ultimo, int k) {
    if (ultimo == NULL || k <= 0) {
        return ultimo;
    }
    
    /* Na lista circular, basta avançar o ponteiro "ultimo" k posições. 
       O encadeamento do anel se mantém intacto! */
       
    for (int i = 0; i < k; i++) {
        ultimo = ultimo->prox;
    }
    
    return ultimo; /* Retorna o novo ponteiro que marca o final da lista rotacionada */
}
```
</details>

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/08%20%E2%80%A2%20Lista%20Encadeada%20Simples/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Encadeada_Simples-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/10%20%E2%80%A2%20Lista%20Duplamente%20Encadeada/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Duplamente_Encadeada_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
