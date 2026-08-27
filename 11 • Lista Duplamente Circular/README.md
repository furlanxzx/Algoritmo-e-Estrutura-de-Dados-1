<h1 align="center">Lista Duplamente Circular</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/10%20%E2%80%A2%20Lista%20Duplamente%20Encadeada/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Duplamente_Encadeada-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/12%20%E2%80%A2%20Matrizes%20Esparsas/README.md">
    <img src="https://img.shields.io/badge/Avancar-Matrizes_Esparsas_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## 1. Introdução

Até agora você provavelmente já viu dois tipos de lista:

- **Lista simplesmente encadeada**: cada nó aponta só para o **próximo**. Você só consegue andar em um sentido (para frente).
- **Lista circular simples**: igual à anterior, mas o último nó, em vez de apontar para `NULL`, aponta de volta para o primeiro, fechando um "círculo".

Agora vamos juntar duas ideias:

> ➤ Uma **lista circular** também pode ser construída com **encadeamento duplo**.
>
> ➤ Neste caso, o **último elemento** da lista passa a ter como **próximo o primeiro**, que, por sua vez, passa a ter o **último como anterior**.
>
> ➤ Com essa construção podemos **percorrer a lista nos dois sentidos**, a partir de um ponteiro para um elemento qualquer.

Ou seja: uma **lista duplamente encadeada e circular** é uma lista em que cada nó tem **dois ponteiros** (um para o anterior, um para o próximo) e, além disso, **não existe começo nem fim de fato** — a lista "se fecha" em um círculo.

### Por que isso é útil?

- Você pode andar para **frente** (`prox`) ou para **trás** (`ant`) a partir de qualquer nó.
- Você pode chegar do último elemento até o primeiro em **apenas um passo** (e vice-versa), sem precisar percorrer a lista toda.
- É a estrutura usada, por exemplo, para implementar players de música em modo "repetir tudo", filas circulares, escalonadores de processos, entre outros.

---

## 2. Representação em memória

Cada nó (ou "elo") da lista guarda três informações:

1. O valor armazenado (`info`)
2. Um ponteiro para o nó **anterior** (`ant`)
3. Um ponteiro para o nó **próximo** (`prox`)

```c
typedef struct no {
    int info;
    struct no *ant;
    struct no *prox;
} No;

typedef No *PLista2;
```

> 💡 **Atenção com o detalhe da convenção usada pela Regina:**
> O ponteiro que representa "a lista" (`PLista2 l`) **não aponta para o primeiro elemento**, e sim para o **último**. Isso significa que:
>
> - `l` → último nó da lista
> - `l->prox` → primeiro nó da lista
> - `l->ant` → penúltimo nó da lista
>
> Essa escolha é proposital: guardar o ponteiro para o **último** nó permite acessar tanto o último (`l`) quanto o primeiro (`l->prox`) em **tempo constante O(1)**, o que facilita muito operações de inserção no início e no fim da lista.

Visualmente, uma lista circular duplamente encadeada com os valores `10, 20, 30` fica assim:

```
        ┌────────────────────────────────────────┐
        │                                          │
        ▼                                          │
      ┌────┐  prox   ┌────┐  prox   ┌────┐  prox   │
      │ 10 │ ──────▶ │ 20 │ ──────▶ │ 30 │ ─────────┘
      └────┘ ◀────── └────┘ ◀────── └────┘
              ant              ant

      ▲ l->prox (primeiro)              ▲ l (último)
```

Repare que **não existe `NULL`** em nenhum ponteiro — todo mundo aponta para alguém.

---

## 3. Percorrendo a lista circular duplamente encadeada

Como não existe um `NULL` marcando o fim, **não podemos usar aquele `while (p != NULL)`** que usávamos nas listas simples. Se fizéssemos isso aqui, o laço nunca pararia (loop infinito), porque a lista é um círculo!

A saída é usar um **`do...while`**, guardando o ponteiro de partida e parando **quando voltarmos a ele**:

```c
void imprime_circular_rev (PLista2 l){
   PLista2 p = l; //guarda endereço do ultimo no

   if (p!=NULL) {
      do {
         printf("%d\n", p->info);
         p = p->ant;      //ou p = p->prox;
      } while (p != l);
   }
}
```

**Explicando linha a linha, para quem está vendo isso pela primeira vez:**

1. `PLista2 p = l;` → começamos a percorrer a partir do último nó (que é o que `l` representa).
2. `if (p != NULL)` → só faz sentido percorrer se a lista não estiver vazia.
3. `do { ... } while (p != l);` → o `do-while` garante que o **primeiro nó visitado sempre entra no laço** (mesmo que ele seja igual a `l`), e só paramos quando `p` **voltar** a ser `l` — ou seja, quando o círculo se fechar.
4. `p = p->ant;` → aqui estamos percorrendo de trás para frente (por isso a função se chama `imprime_circular_rev`, de "reversa"). Se trocarmos para `p = p->prox;`, percorremos na ordem normal, do primeiro para o último.

> ⚠️ **Erro clássico de iniciante:** usar `while (p != NULL)` em lista circular. Como nenhum ponteiro é `NULL`, o programa entra em loop infinito. Sempre use `do-while` comparando com o ponteiro de partida.

---

## 4. Exercícios

<br>

### Exercício 1 🟡 (médio)

> Escreva funções de **inserção** e **remoção** em uma lista duplamente encadeada circular. Na inserção, considere deixar a lista em **ordem crescente**. Na remoção, considere tirar **todas as ocorrências** de um dado valor em lista ordenada.

<details>
<summary>💡 Ver resolução</summary>

**Ideia da inserção:** como a lista é ordenada, precisamos percorrer até achar o primeiro elemento **maior ou igual** ao valor que queremos inserir, e colocar o novo nó **antes** dele. Precisamos tratar 3 casos especiais: lista vazia, inserção no início e inserção no fim (que muda quem é `l`, já que `l` aponta pro último).

```c
PLista2 insere_ordenado(PLista2 l, int valor) {
    PLista2 novo = (PLista2) malloc(sizeof(No));
    novo->info = valor;

    // Caso 1: lista vazia
    if (l == NULL) {
        novo->prox = novo;
        novo->ant = novo;
        return novo; // o novo nó é o único, logo também é o "último"
    }

    PLista2 primeiro = l->prox;
    PLista2 p = primeiro;

    // Percorre a lista procurando a posição correta
    do {
        if (p->info >= valor) break;
        p = p->prox;
    } while (p != primeiro);

    // Caso 2: valor é o maior de todos -> insere depois do ultimo (l)
    if (p->info < valor) {
        novo->prox = primeiro;
        novo->ant = l;
        l->prox = novo;
        primeiro->ant = novo;
        return novo; // novo nó passa a ser o último
    }

    // Caso 3: insere antes de p (no início ou no meio)
    novo->prox = p;
    novo->ant = p->ant;
    p->ant->prox = novo;
    p->ant = novo;

    // Se inserimos antes do primeiro, o "ultimo" (l) continua o mesmo
    return l;
}
```

**Ideia da remoção:** primeiro contamos quantos nós existem (para saber quantas voltas dar no `for`), depois percorremos guardando sempre o `próximo` **antes** de eventualmente apagar o nó atual — assim não perdemos a referência ao removê-lo.

```c
PLista2 remove_todas_ocorrencias(PLista2 l, int valor) {
    if (l == NULL) return NULL;

    PLista2 p = l->prox; // primeiro nó
    int n = 0;

    // conta quantos nós existem na lista
    PLista2 aux = p;
    do {
        n++;
        aux = aux->prox;
    } while (aux != p);

    for (int i = 0; i < n; i++) {
        PLista2 prox = p->prox; // guarda o proximo antes de mexer

        if (p->info == valor) {
            if (p->prox == p) {
                // era o único elemento da lista
                free(p);
                return NULL;
            }
            p->ant->prox = p->prox;
            p->prox->ant = p->ant;

            if (p == l) l = p->ant; // atualiza o "ultimo", se necessario

            free(p);
        }
        p = prox;
    }

    return l;
}
```

</details>

<br>

### Exercício 2 🟢 (fácil)

> Escreva uma função (`locate(l,p)`) que retorna o apontador para o item na p-ésima posição de L. Retorne `NULL` se a lista tiver menos que P elementos.

<details>
<summary>💡 Ver resolução</summary>

Aqui é só percorrer a partir do primeiro (`l->prox`), contando as posições, até chegar na posição `p` ou dar a volta completa (o que significa que a lista tem menos de `p` elementos).

```c
PLista2 locate(PLista2 l, int p) {
    if (l == NULL || p <= 0) return NULL;

    PLista2 atual = l->prox; // posição 1
    int cont = 1;

    do {
        if (cont == p) return atual;
        atual = atual->prox;
        cont++;
    } while (atual != l->prox);

    return NULL; // deu a volta na lista e não achou -> lista tem menos que p itens
}
```

> 🧠 **Dica:** essa função vai ser muito útil no próximo exercício — sempre que puder reaproveitar uma função já pronta, faça isso! É mais fácil de entender e de manter o código.

</details>

<br>

### Exercício 3 🟡 (médio)

> Escreva uma função que insere o item E na posição P da lista L. Use a função `locate(l,p)` neste procedimento. Retorne erro se a lista tiver menos que P itens.

<details>
<summary>💡 Ver resolução</summary>

```c
PLista2 insere_posicao(PLista2 l, int e, int p) {
    PLista2 novo = (PLista2) malloc(sizeof(No));
    novo->info = e;

    // Caso especial: lista vazia só aceita inserção na posição 1
    if (l == NULL) {
        if (p != 1) {
            printf("Erro: lista vazia so aceita insercao na posicao 1\n");
            free(novo);
            return NULL;
        }
        novo->prox = novo;
        novo->ant = novo;
        return novo;
    }

    PLista2 ref = locate(l, p); // reaproveitando a funcao do exercicio anterior

    if (ref == NULL) {
        printf("Erro: a lista tem menos que %d itens\n", p);
        free(novo);
        return l;
    }

    // Insere o novo nó ANTES de ref
    novo->prox = ref;
    novo->ant = ref->ant;
    ref->ant->prox = novo;
    ref->ant = novo;

    return l; // como inserimos antes de alguem que ja existia, "l" nao muda
}
```

> 🧠 Repare que, como sempre inserimos **antes** do nó retornado por `locate`, nunca precisamos atualizar quem é o "último" (`l`) — a única forma de `l` mudar seria inserir depois dele, o que essa função não faz.

</details>

<br>

### Exercício 4 🔴 (difícil)

> Faça um programa completo utilizando lista duplamente encadeada e circular que permita a inserção de nomes de alunos juntamente com três notas relacionadas a ele. O programa deve também permitir a remoção e busca de um aluno, assim como a impressão dos registros cadastrados. Também deve permitir a alteração de notas de algum aluno já cadastrado.

<details>
<summary>💡 Ver resolução</summary>

Esse é o exercício mais completo: precisamos adaptar a estrutura do nó para guardar um **aluno** (nome + 3 notas) em vez de um `int`, e criar um pequeno menu para testar todas as operações.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct aluno {
    char nome[50];
    float notas[3];
} Aluno;

typedef struct no {
    Aluno info;
    struct no *ant;
    struct no *prox;
} No;

typedef No *PLista2;

// ---------- INSERÇÃO (sempre no final, antes do primeiro) ----------
PLista2 insere_aluno(PLista2 l, char nome[], float n1, float n2, float n3) {
    PLista2 novo = (PLista2) malloc(sizeof(No));
    strcpy(novo->info.nome, nome);
    novo->info.notas[0] = n1;
    novo->info.notas[1] = n2;
    novo->info.notas[2] = n3;

    if (l == NULL) {
        novo->prox = novo;
        novo->ant = novo;
        return novo;
    }

    PLista2 primeiro = l->prox;
    novo->prox = primeiro;
    novo->ant = l;
    l->prox = novo;
    primeiro->ant = novo;

    return novo; // o novo aluno passa a ser o "ultimo"
}

// ---------- BUSCA por nome ----------
PLista2 busca_aluno(PLista2 l, char nome[]) {
    if (l == NULL) return NULL;

    PLista2 p = l->prox;
    do {
        if (strcmp(p->info.nome, nome) == 0) return p;
        p = p->prox;
    } while (p != l->prox);

    return NULL; // nao encontrado
}

// ---------- REMOÇÃO por nome ----------
PLista2 remove_aluno(PLista2 l, char nome[]) {
    PLista2 p = busca_aluno(l, nome);
    if (p == NULL) {
        printf("Aluno '%s' nao encontrado.\n", nome);
        return l;
    }

    if (p->prox == p) { // único aluno cadastrado
        free(p);
        return NULL;
    }

    p->ant->prox = p->prox;
    p->prox->ant = p->ant;

    if (p == l) l = p->ant; // atualiza o "ultimo", se necessario

    free(p);
    printf("Aluno '%s' removido com sucesso.\n", nome);
    return l;
}

// ---------- ALTERAÇÃO de notas ----------
void altera_notas(PLista2 l, char nome[], float n1, float n2, float n3) {
    PLista2 p = busca_aluno(l, nome);
    if (p == NULL) {
        printf("Aluno '%s' nao encontrado.\n", nome);
        return;
    }
    p->info.notas[0] = n1;
    p->info.notas[1] = n2;
    p->info.notas[2] = n3;
    printf("Notas de '%s' atualizadas.\n", nome);
}

// ---------- IMPRESSÃO de todos os registros ----------
void imprime_alunos(PLista2 l) {
    if (l == NULL) {
        printf("Nenhum aluno cadastrado.\n");
        return;
    }

    PLista2 p = l->prox; // comeca do primeiro
    do {
        float media = (p->info.notas[0] + p->info.notas[1] + p->info.notas[2]) / 3;
        printf("Nome: %-20s Notas: %.1f, %.1f, %.1f   Media: %.2f\n",
               p->info.nome, p->info.notas[0], p->info.notas[1], p->info.notas[2], media);
        p = p->prox;
    } while (p != l->prox);
}

// ---------- MENU PRINCIPAL ----------
int main() {
    PLista2 lista = NULL;
    int opcao;
    char nome[50];
    float n1, n2, n3;

    do {
        printf("\n===== CADASTRO DE ALUNOS =====\n");
        printf("1 - Inserir aluno\n");
        printf("2 - Remover aluno\n");
        printf("3 - Buscar aluno\n");
        printf("4 - Alterar notas\n");
        printf("5 - Listar todos\n");
        printf("0 - Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                printf("Nome: ");
                scanf(" %[^\n]", nome);
                printf("Nota 1: "); scanf("%f", &n1);
                printf("Nota 2: "); scanf("%f", &n2);
                printf("Nota 3: "); scanf("%f", &n3);
                lista = insere_aluno(lista, nome, n1, n2, n3);
                break;

            case 2:
                printf("Nome do aluno a remover: ");
                scanf(" %[^\n]", nome);
                lista = remove_aluno(lista, nome);
                break;

            case 3:
                printf("Nome do aluno a buscar: ");
                scanf(" %[^\n]", nome);
                PLista2 encontrado = busca_aluno(lista, nome);
                if (encontrado != NULL)
                    printf("Encontrado! Notas: %.1f, %.1f, %.1f\n",
                           encontrado->info.notas[0],
                           encontrado->info.notas[1],
                           encontrado->info.notas[2]);
                else
                    printf("Aluno nao encontrado.\n");
                break;

            case 4:
                printf("Nome do aluno: ");
                scanf(" %[^\n]", nome);
                printf("Nova nota 1: "); scanf("%f", &n1);
                printf("Nova nota 2: "); scanf("%f", &n2);
                printf("Nova nota 3: "); scanf("%f", &n3);
                altera_notas(lista, nome, n1, n2, n3);
                break;

            case 5:
                imprime_alunos(lista);
                break;
        }
    } while (opcao != 0);

    return 0;
}
```

**Resumo do que cada função faz:**

| Função | O que faz | Complexidade |
|---|---|---|
| `insere_aluno` | Insere um novo aluno no final da lista (antes do primeiro) | O(1) |
| `busca_aluno` | Percorre a lista procurando pelo nome | O(n) |
| `remove_aluno` | Busca o aluno e ajusta os ponteiros vizinhos para "pular" o nó removido | O(n) |
| `altera_notas` | Busca o aluno e sobrescreve as notas | O(n) |
| `imprime_alunos` | Percorre a lista do primeiro ao último, imprimindo cada aluno | O(n) |

> 🧠 **Por que inserir sempre no final é O(1)?** Porque, como `l` aponta direto para o último nó, não precisamos percorrer a lista para achar onde inserir — já sabemos onde é o "fim" e onde é o "começo" (`l->prox`) em tempo constante.

</details>

---

## 5. Resumo rápido

| Operação | Lista simples | Lista dupl. encadeada circular |
|---|---|---|
| Percorrer para frente | ✅ | ✅ |
| Percorrer para trás | ❌ | ✅ |
| Saber quando parar de percorrer | `p == NULL` | `p == ponto_de_partida` |
| Acesso ao último elemento | O(n) | O(1) (se `l` apontar pro último) |
| Acesso ao primeiro elemento | O(1) | O(1) (`l->prox`) |

**Próximos passos sugeridos:** depois de dominar bem esses exercícios, vale a pena tentar implementar uma **pilha** ou **fila circular** usando essa mesma estrutura, só para fixar a lógica dos ponteiros `ant`/`prox` andando nos dois sentidos.

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/10%20%E2%80%A2%20Lista%20Duplamente%20Encadeada/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Lista_Duplamente_Encadeada-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/12%20%E2%80%A2%20Matrizes%20Esparsas/README.md">
    <img src="https://img.shields.io/badge/Avancar-Matrizes_Esparsas_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
