<h1 align="center">Lista Encadeada Simples</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/07%20%E2%80%A2%20Fila%20Din%C3%A2mica/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Fila_Dinâmica-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/09%20%E2%80%A2%20Lista%20Circular/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Circular_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

Uma lista encadeada é uma sequência de nós ligados entre si por ponteiros (cada nó guarda o endereço do próximo nó).
*   Cada nó pode ser inserido ou retirado de qualquer posição da lista.
*   Cada nó pode ser consultado em qualquer ordem.

Para manipularmos uma lista encadeada, precisamos fazer as funções de: **Inserção**, **Busca**, **Remoção**, **Impressão** e **Liberação**.

---

### 1. Declaração da Struct

Para criarmos a lista, primeiro definimos a estrutura do nó, que conterá a informação (um inteiro, neste caso) e o ponteiro para o próximo elemento.

```c
typedef struct lista {
    int info;
    struct lista* prox;
} TLista;

typedef TLista *PLista;
```

---

### 2. Procurando um elemento na lista (Busca)

Vamos começar pela função de busca. Ela percorre a lista através de um laço `for` até encontrar o valor desejado ou chegar ao final da lista (`NULL`).

```c
PLista busca (PLista l, int v)
{
    PLista p;
    for (p=l; p!=NULL && p->info != v; p=p->prox);
    return p;
}
```

---

### 3. Inserindo na Lista Encadeada (Ordenada)

A função de inserção depende de como se quer inserir cada elemento (no início, fim ou no meio, dependendo da necessidade). Por exemplo, se quisermos montar uma lista ordenada, temos que procurar a posição em que o elemento deve ser inserido.

Abaixo, a função que insere de forma ordenada crescentemente:

```c
PLista Insere_ord (PLista l, int dado){
    PLista novo, // novo elemento
           ant,  // ponteiro auxiliar para a posição anterior
           paux; // ponteiro auxiliar para a posição atual

    /* aloca um novo nó */
    novo = (PLista) malloc(sizeof(TLista));
    novo->info = dado; /* insere a informação no novo nó */
    
    /* procurando a posição de inserção */
    for(ant=NULL, paux=l; paux!=NULL && paux->info<dado; ant=paux, paux = paux->prox);

    /* encadeia o elemento */
    if (ant == NULL){
        /* se o anterior não existe, será inserido na 1ª posição */
        novo->prox = l;
        return(novo);
    }
    
    /* senão, o elemento será inserido no meio da lista */
    novo->prox = ant->prox;
    ant->prox = novo;
    return (l);
}
```

---

### 4. Retirando da Lista

Para retirar, precisamos procurar o elemento e manter um ponteiro para o elemento anterior, pois precisaremos "pular" o nó removido na hora de refazer o encadeamento.

```c
PLista retira (PLista l, int v){
    PLista ant; /* ponteiro para elemento anterior */
    PLista p;   /* ponteiro para percorrer a lista */
    
    /* procura elemento na lista, guardando anterior */
    for (ant=NULL, p=l; p!=NULL && p->info!=v; ant=p, p = p->prox);

    /* verifica se achou elemento */
    if (p == NULL)
        return l; /* não achou: retorna lista original */

    /* retira elemento */
    if (ant == NULL)
        /* retira elemento do inicio */
        l = p->prox;
    else
        /* retira elemento do meio da lista */
        ant->prox = p->prox;
        
    free(p);
    return l;
}
```

---

### 📝 Exercício Prático

**Nível de Dificuldade:** 🟢 Fácil
> Faça a função que imprima toda a lista e a função que libera os espaços alocados.

<details>
<summary><b>👀 Ver resposta (Código em C)</b></summary>

```c
/* Função para imprimir toda a lista do início ao fim */
void imprime_lista(PLista l) {
    PLista p;
    // Percorre a lista até encontrar NULL
    for (p = l; p != NULL; p = p->prox) {
        printf("%d ", p->info);
    }
    printf("\n");
}

/* Função para liberar os espaços alocados dinamicamente */
void libera_lista(PLista l) {
    PLista p = l;
    PLista aux; // Ponteiro auxiliar para não perder a referência
    
    while (p != NULL) {
        aux = p->prox; /* guarda a referência para o próximo nó */
        free(p);       /* libera a memória do nó atual */
        p = aux;       /* faz p apontar para o próximo nó */
    }
}
```
</details>

---

## 🔄 Implementações Recursivas em Listas Encadeadas

Uma lista encadeada é representada por:
*   uma lista vazia; ou
*   um elemento seguido de uma (sub-)lista.

Neste caso, o segundo elemento da lista representa o primeiro elemento da sub-lista.

---

### 1. Função Imprimir (Recursiva)

Na abordagem recursiva, processamos o nó atual e fazemos a chamada da função passando o próximo nó como parâmetro.

```c
void imprime_rec (PLista l)
{
    if (l==NULL)
        return;
    /* imprime primeiro elemento */
    printf("info: %d\n",l->info);

    /* imprime sub-lista */
    imprime_rec(l->prox);
}
```

---

### 2. Função Retirar (Recursiva)

Para remover recursivamente, o caso base é a lista vazia. Se o elemento for encontrado, ele é liberado; caso contrário, a função avança para a sub-lista.

```c
PLista retira_rec (PLista l, int v)
{
    PLista t;
    if (l==NULL)
        return l; //lista vazia: retorna valor original
        
    // verifica se elemento a ser retirado é o primeiro
    if (l->info == v)
    {
        t = l; // temporário para poder liberar
        l = l->prox;
        free(t);
    }
    else
    {
        /* retira de sub-lista */
        l->prox = retira_rec(l->prox,v);
    }
    return l;
}
```

---

## 📝 Lista de Exercícios de Fixação

**Nível de Dificuldade:** 🟡 Médio a 🔴 Difícil

### Exercício 1
> Faça uma função recursiva que libera a lista e uma que busca um elemento na lista.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
/* Função recursiva para liberar a lista */
void libera_rec(PLista l) {
    if (l != NULL) {
        libera_rec(l->prox); /* Libera o resto da lista primeiro */
        free(l);             /* Depois libera o nó atual */
    }
}

/* Função recursiva de busca */
PLista busca_rec(PLista l, int v) {
    if (l == NULL) 
        return NULL;          /* Caso base: não encontrou */
    if (l->info == v) 
        return l;             /* Caso base: encontrou */
        
    return busca_rec(l->prox, v); /* Busca na sub-lista */
}
```
</details>

### Exercício 2
> Implemente uma função que verifique se duas listas encadeadas são iguais (faça na forma recursiva e não recursiva). Duas listas são consideradas iguais se têm a mesma sequência de elementos.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
/* Versão Iterativa (Não Recursiva) */
int iguais_iter(PLista l1, PLista l2) {
    while (l1 != NULL && l2 != NULL) {
        if (l1->info != l2->info) 
            return 0; /* Diferentes */
        l1 = l1->prox;
        l2 = l2->prox;
    }
    /* Retorna 1 se ambas chegaram ao fim (mesmo tamanho), senão 0 */
    return (l1 == NULL && l2 == NULL); 
}

/* Versão Recursiva */
int iguais_rec(PLista l1, PLista l2) {
    if (l1 == NULL && l2 == NULL) 
        return 1; /* Ambas vazias e iguais até o fim */
    if (l1 == NULL || l2 == NULL) 
        return 0; /* Tamanhos diferentes */
    if (l1->info != l2->info) 
        return 0; /* Elementos diferentes */
        
    return iguais_rec(l1->prox, l2->prox);
}
```
</details>

### Exercício 3
> Construa um algoritmo que retira um elemento da posição `pos1` e coloca na posição `pos2` em uma lista dinâmica. *(Assumindo posições baseadas em índice a partir de 1)*

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
PLista move_elemento(PLista l, int pos1, int pos2) {
    if (l == NULL || pos1 == pos2 || pos1 < 1 || pos2 < 1) return l;

    PLista p1 = l, ant1 = NULL;
    int i = 1;
    
    /* 1. Encontra o nó na pos1 e remove logicamente */
    while (p1 != NULL && i < pos1) {
        ant1 = p1;
        p1 = p1->prox;
        i++;
    }
    
    if (p1 == NULL) return l; /* pos1 inválida */

    /* Desconecta p1 da lista */
    if (ant1 == NULL) l = p1->prox;
    else ant1->prox = p1->prox;

    /* 2. Encontra a pos2 para inserir o nó extraído */
    PLista p2 = l, ant2 = NULL;
    i = 1;
    
    while (p2 != NULL && i < pos2) {
        ant2 = p2;
        p2 = p2->prox;
        i++;
    }

    /* Reconecta p1 na nova posição */
    if (ant2 == NULL) { 
        /* Insere no início */
        p1->prox = l;
        l = p1;
    } else {
        /* Insere no meio ou fim */
        p1->prox = ant2->prox;
        ant2->prox = p1;
    }

    return l;
}
```
</details>

### Exercício 4
> Considere uma lista encadeada `L1` representando uma sequência de caracteres. Construa uma função para imprimir a sequência de caracteres da lista `L1` na ordem inversa (não é permitido o uso de listas auxiliares).
> *Ex:* Para a lista L1={A,E,I,O,U}, a função deve imprimir “UOIEA”.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
/* Considerando uma struct modificada para char: typedef struct listaC { char info; struct listaC* prox; } TListaC; */

void imprime_inversa(TListaC *l) {
    if (l == NULL) {
        return; /* Condição de parada */
    }
    
    /* Chamada recursiva primeiro, faz o empilhamento na memória */
    imprime_inversa(l->prox);
    
    /* A impressão só ocorre na volta da recursão, saindo da última para a primeira */
    printf("%c", l->info); 
}
```
</details>

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/07%20%E2%80%A2%20Fila%20Din%C3%A2mica/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Fila_Dinâmica-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/09%20%E2%80%A2%20Lista%20Circular/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Circular_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
