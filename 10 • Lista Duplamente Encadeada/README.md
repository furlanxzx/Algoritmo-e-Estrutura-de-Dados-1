<h1 align="center">Lista Duplamente Encadeada</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/09%20%E2%80%A2%20Lista%20Circular/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Circular-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/11%20%E2%80%A2%20Lista%20Duplamente%20Circular/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Duplamente_Circular_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

### Introdução e Motivação
*   **A limitação da lista simples:** A estrutura vista anteriormente forma um encadeamento simples. Como cada elemento armazena apenas um ponteiro para o próximo, não conseguimos percorrer a lista eficientemente de trás para frente (do final para o início).
*   **Dificuldade na remoção:** O encadeamento simples dificulta a retirada de elementos. Mesmo se tivermos o ponteiro do elemento a ser retirado, precisamos percorrer a lista desde o início para encontrar o seu elemento anterior e ajustar os ponteiros.
*   **A solução:** Para resolver isso, criamos a **lista duplamente encadeada**, onde cada elemento possui dois ponteiros: um apontando para o próximo elemento e outro apontando para o elemento anterior. Isso permite acesso rápido a ambos os nós adjacentes e facilita a inserção ou remoção no meio da lista sem precisar guardar ponteiros auxiliares durante a busca.

---

### Estrutura e Definição
*   O primeiro elemento da lista não possui um elemento anterior (seu ponteiro `ant` vale `NULL`).
*   A estrutura em C (usando `int` como informação) fica assim:

```c
typedef struct lista2 {
    int info;
    struct lista2* ant;
    struct lista2* prox;
} TLista2;

typedef TLista2 *PLista2;
```

---

### 🛠️ Operações Básicas (dos slides)

**Inserção no Início:**
```c
PLista2 insere (PLista2 l, int v) {
    PLista2 novo = (PLista2) malloc(sizeof(TLista2));
    novo->info = v;
    novo->prox = l;
    novo->ant = NULL; /* Como é inserido no início, não tem anterior */
    
    /* Se a lista não estava vazia, o antigo primeiro nó passa a ter um anterior */
    if (l != NULL) {
        l->ant = novo;
    }
    return novo;
}
```

**Busca:**
```c
PLista2 busca (PLista2 l, int v) {
    PLista2 p;
    for (p = l; p != NULL; p = p->prox) {
        if (p->info == v)
            return p;
    }
    return NULL; /* não achou o elemento */
}
```

---

## 📝 Lista de Exercícios de Fixação

Com o encadeamento duplo, a remoção exige que atualizemos os ponteiros `prox` do nó anterior e `ant` do nó posterior, caso eles existam. A vantagem é que podemos fazer isso conhecendo apenas o ponteiro do elemento a ser removido.

### Exercício 1
> Faça a função de remoção de um nó em uma lista duplamente encadeada.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
PLista2 remove_no(PLista2 l, int v) {
    /* 1. Busca o elemento (usando a função de busca mostrada antes) */
    PLista2 p = busca(l, v);
    
    if (p == NULL) {
        return l; /* Elemento não encontrado, retorna a lista original */
    }
    
    /* 2. Verifica se é o primeiro elemento da lista */
    if (l == p) {
        l = p->prox; /* O início da lista passa a ser o segundo elemento */
    } else {
        /* O 'prox' do elemento anterior passa a apontar para o próximo de 'p' */
        p->ant->prox = p->prox; 
    }
    
    /* 3. Verifica se não é o último elemento da lista */
    if (p->prox != NULL) {
        /* O 'ant' do elemento seguinte passa a apontar para o anterior de 'p' */
        p->prox->ant = p->ant;
    }
    
    /* 4. Libera a memória e retorna o novo início da lista */
    free(p);
    return l;
}
```
</details>

### Exercício 2
> Escreva uma função que remova de uma lista duplamente encadeada todos os elementos que contêm o valor de *y*.

<details>
<summary><b>👀 Ver resposta</b></summary>

```c
PLista2 remove_todos_y(PLista2 l, int y) {
    PLista2 p = l;
    
    while (p != NULL) {
        PLista2 prox_no = p->prox; /* Guardamos o próximo nó antes de possivelmente liberar o 'p' */
        
        if (p->info == y) {
            /* Mesma lógica de acerto de ponteiros do Exercício 1 */
            if (l == p) {
                l = p->prox;
            } else {
                p->ant->prox = p->prox;
            }
            
            if (p->prox != NULL) {
                p->prox->ant = p->ant;
            }
            
            free(p);
        }
        p = prox_no; /* Avança para o próximo nó da iteração */
    }
    
    return l;
}
```
</details>

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/09%20%E2%80%A2%20Lista%20Circular/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Circular-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/11%20%E2%80%A2%20Lista%20Duplamente%20Circular/README.md">
    <img src="https://img.shields.io/badge/Avancar-Lista_Duplamente_Circular_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
