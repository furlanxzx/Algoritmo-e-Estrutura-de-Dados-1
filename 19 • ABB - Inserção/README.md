<h1 align="center">ABB - Inserção</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/18%20%E2%80%A2%20%C3%81rvores%20Gen%C3%A9ricas/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Árvores_Genéricas-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/20%20%E2%80%A2%20ABB%20-%20Remo%C3%A7%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/Avancar-ABB_Remoção_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## <mark> 1 - Definição </mark>

Uma **árvore binária de busca**, além da relação hierárquica entre os nós, possui uma **ordem** entre os nós filhos.

Uma árvore binária de busca, cuja raiz armazena o elemento **R**, é denominada **árvore binária de pesquisa (busca)** se:

-  todo elemento armazenado na sub-árvore à **esquerda** é **menor ou igual** a **R**;
-  todo elemento armazenado na sub-árvore à **direita** é **maior** do que **R**;
-  a sub-árvore direita e esquerda também são **ABB**.

> Uma árvore binária de busca (ABB), visitada em ordem **infixa** (Esq, Raiz, Dir), resulta em uma **lista de dados em ordem crescente**.

⚠️ **Não permite chaves duplicadas.**

---

## <mark> 2 - Estrutura para ABB </mark>

Diferente da árvore genérica (que usa lista de filhos com `prim`/`prox`), a ABB é uma árvore binária "de verdade": cada nó tem no máximo **dois** filhos, um à esquerda (`esq`) e um à direita (`dir`).

```c
typedef struct nodoABB {
    int info;
    struct nodoABB *esq;
    struct nodoABB *dir;
} TABB;

typedef TABB *PABB;
```

---

## <mark> 3 - Inserção </mark>

Inserir um elemento **X** em uma ABB **vazia** é trivial.

Se a árvore não se encontra vazia, deve-se fazer o seguinte:

1. compara-se o elemento a inserir com o elemento raiz da árvore;
2. se o elemento for **menor**, ele é inserido na sub-árvore **esquerda**; senão, é inserido na sub-árvore **direita**.

### Exercício — Função Insere 🟡

> Tente fazer uma função para inserir em uma ABB!
>
> Lembre-se:
> - Se a árvore estiver vazia, é só inserir o elemento;
> - Senão, você deverá percorrer a árvore para encontrar a posição onde o elemento deverá ser inserido e só então poderá inseri-lo.

<details>
<summary>Ver resposta</summary>

Assim como nas funções da árvore genérica, a inserção na ABB é feita de forma **recursiva**: a cada chamada, decidimos se descemos para a esquerda ou para a direita, até encontrar uma posição vazia (`NULL`), que é onde o novo nó será alocado.

```c
PABB insere (PABB a, int x)
{
    if (a == NULL) {
        a = (PABB) malloc(sizeof(TABB));
        a->info = x;
        a->esq = NULL;
        a->dir = NULL;
    }
    else if (x < a->info)
        a->esq = insere(a->esq, x);
    else
        a->dir = insere(a->dir, x);

    return a;
}
```

📌 Note que a função **retorna** `a` — isso é o que permite "religar" o novo nó na árvore. Na chamada recursiva `a->esq = insere(a->esq, x)`, se `a->esq` for `NULL`, a função cria o novo nó e o retorna, e essa atribuição é o que efetivamente pendura o novo nó na árvore.

</details>

---

## Exemplo

Insira os seguintes números numa ABB, na ordem dada:

```
17, 99, 13, 1, 3, 100, 400
```

Seguindo a regra (menor → esquerda, maior ou igual → direita), a árvore fica assim ao final:

- **17** (raiz)
  - esq: **13**
    - esq: **1**
      - dir: **3**
  - dir: **99**
    - dir: **100**
      - dir: **400**

Note como `99`, `100` e `400` formam uma "escada" só de filhos à direita — isso acontece porque cada novo número inserido é maior que todos os anteriores daquele ramo. Esse é exatamente o tipo de situação que deixa a árvore **desbalanceada**.

---

## <mark> 4 - Busca </mark>

Se a árvore for nula, nada a fazer. Caso contrário, o processo de pesquisa é o **mesmo** utilizado para inserção.

### Exercício — Função Pesquisa 🟢

> Faça a função de busca em ABB.

<details>
<summary>Ver resposta</summary>

A lógica é idêntica à da inserção: comparamos o valor buscado com o `info` do nó atual e decidimos se descemos à esquerda ou à direita. A diferença é que, ao invés de criar um nó quando `a == NULL`, apenas retornamos que o elemento **não foi encontrado**.

```c
int pesquisa (PABB a, int x)
{
    if (a == NULL)
        return 0;

    if (a->info == x)
        return 1;

    if (x < a->info)
        return pesquisa(a->esq, x);
    else
        return pesquisa(a->dir, x);
}
```

</details>

---

## 📝 Exercícios Práticos

### 🟢 Exercício 1 

Insira a seguinte sequência de números em uma ABB:

```
42  51  19  37  86  71  10  75  22  31
```

<details>
<summary>Ver resposta</summary>

Inserindo um a um, seguindo a regra "menor → esquerda, maior ou igual → direita":

- **42** (raiz)
  - esq: **19**
    - esq: **10**
    - dir: **37**
      - esq: **22**
        - dir: **31**
  - dir: **51**
    - dir: **86**
      - esq: **71**
        - dir: **75**

**Passo a passo resumido:**
| Inserindo | Caminho percorrido |
|---|---|
| 42 | vira raiz |
| 51 | 51 ≥ 42 → dir de 42 |
| 19 | 19 < 42 → esq de 42 |
| 37 | 37 < 42 → esq; 37 ≥ 19 → dir de 19 |
| 86 | 86 ≥ 42 → dir; 86 ≥ 51 → dir de 51 |
| 71 | 71 ≥ 42 → dir; 71 ≥ 51 → dir; 71 < 86 → esq de 86 |
| 10 | 10 < 42 → esq; 10 < 19 → esq de 19 |
| 75 | 75 ≥ 42 → dir; 75 ≥ 51 → dir; 75 < 86 → esq; 75 ≥ 71 → dir de 71 |
| 22 | 22 < 42 → esq; 22 ≥ 19 → dir; 22 < 37 → esq de 37 |
| 31 | 31 < 42 → esq; 31 ≥ 19 → dir; 31 < 37 → esq; 31 ≥ 22 → dir de 22 |

</details>

---

### 🟡 Exercício 2 

Construa uma ABB com o conjunto de números:

```
{100, 23, 13, 15, 66, 43, 134, 87, 166, 786, 214, 221, 7, 1880, 200}
```

<details>
<summary>Ver resposta</summary>

 Como um **conjunto** não tem ordem definida, vamos considerar a inserção na **mesma ordem em que os números aparecem** listados no enunciado — é assim que a árvore final é determinada de forma única.

- **100** (raiz)
  - esq: **23**
    - esq: **13**
      - esq: **7**
      - dir: **15**
    - dir: **66**
      - esq: **43**
      - dir: **87**
  - dir: **134**
    - dir: **166**
      - dir: **786**
        - esq: **214**
          - esq: **200**
          - dir: **221**
        - dir: **1880**

**Repare que:**
- `134` só tem filho à **direita** (não há nenhum número entre 100 e 134 no restante da lista);
- o ramo direito da árvore (a partir de `786`) fica bem mais "cheio" que o esquerdo — a árvore está desbalanceada, com números concentrados desigualmente.

</details>

---

### 🔴 Exercício 3 

Considere uma ABB *A* de números inteiros contendo todos os números entre 1 e 1000. Dentre as sequências abaixo, indique qual(is) pode(m) corresponder a uma sequência de elementos de *A* visitados em ordem **prefixa** durante a operação de **pesquisa do elemento 363**. Para cada uma das sequências não válidas, indique o problema.

```
i)   2, 252, 401, 398, 330, 344, 397, 363
ii)  924, 220, 911, 244, 898, 258, 362, 363
iii) 925, 202, 911, 240, 912, 245, 363
iv)  2, 399, 387, 219, 266, 382, 381, 278, 363
v)   935, 278, 347, 621, 299, 392, 358, 363
```

<details>
<summary>Ver resposta</summary>

**Como pensar:** durante a pesquisa de um valor `x` em uma ABB, o caminho percorrido a partir da raiz é **único**: em cada nó visitado, comparamos `x` com o valor do nó — se `x` for menor, descemos à esquerda; se for maior, descemos à direita; se for igual, achamos o elemento e paramos.

Isso significa que cada nó visitado **restringe** os valores possíveis dos próximos nós do caminho:
- ao descer à **esquerda** de um nó de valor `V` (porque `363 < V`), esse `V` passa a ser um **limite superior** — todo próximo nó do caminho precisa ser **menor** que `V`;
- ao descer à **direita** de um nó de valor `V` (porque `363 > V`), esse `V` passa a ser um **limite inferior** — todo próximo nó do caminho precisa ser **maior** que `V`.

Uma sequência é válida se, ao aplicar essas regras nó a nó, nenhum valor viola os limites acumulados até ali, e a sequência termina exatamente em `363`.

---

**i) 2, 252, 401, 398, 330, 344, 397, 363 → ✅ válida**

| nó | limites (inf, sup) | decisão |
|---|---|---|
| 2 | (1, 1000) | 363 > 2 → direita, inf = 2 |
| 252 | (2, 1000) | 363 > 252 → direita, inf = 252 |
| 401 | (252, 1000) | 363 < 401 → esquerda, sup = 401 |
| 398 | (252, 401) | 363 < 398 → esquerda, sup = 398 |
| 330 | (252, 398) | 363 > 330 → direita, inf = 330 |
| 344 | (330, 398) | 363 > 344 → direita, inf = 344 |
| 397 | (344, 398) | 363 < 397 → esquerda, sup = 397 |
| 363 | (344, 397) | achou! ✅ |

---

**ii) 924, 220, 911, 244, 898, 258, 362, 363 → ✅ válida**

Seguindo o mesmo processo, cada valor respeita os limites (inf, sup) acumulados a cada passo, terminando em 363. Sequência consistente com um caminho de pesquisa válido.

---

**iii) 925, 202, 911, 240, 912, 245, 363 → ❌ inválida**

| nó | limites (inf, sup) | decisão |
|---|---|---|
| 925 | (1, 1000) | 363 < 925 → esquerda, sup = 925 |
| 202 | (1, 925) | 363 > 202 → direita, inf = 202 |
| 911 | (202, 925) | 363 < 911 → esquerda, sup = 911 |
| 240 | (202, 911) | 363 > 240 → direita, inf = 240 |
| **912** | (240, **911**) | **problema!** |

O nó `912` viola o limite superior `911` estabelecido ao visitar `911` — depois de ir à esquerda de `911`, todo valor seguinte no caminho precisa ser **menor que 911**, mas `912 > 911`. Isso nunca poderia acontecer numa pesquisa real, pois `912` estaria fora da sub-árvore esquerda de `911`.

---

**iv) 2, 399, 387, 219, 266, 382, 381, 278, 363 → ✅ válida**

Assim como na sequência (i), cada valor respeita os limites acumulados, terminando em 363.

---

**v) 935, 278, 347, 621, 299, 392, 358, 363 → ❌ inválida**

| nó | limites (inf, sup) | decisão |
|---|---|---|
| 935 | (1, 1000) | 363 < 935 → esquerda, sup = 935 |
| 278 | (1, 935) | 363 > 278 → direita, inf = 278 |
| 347 | (278, 935) | 363 > 347 → direita, inf = **347** |
| 621 | (347, 935) | 363 < 621 → esquerda, sup = 621 |
| **299** | (**347**, 621) | **problema!** |

O nó `299` viola o limite inferior `347` estabelecido ao visitar `347` — depois de ir à direita de `347`, todo valor seguinte precisa ser **maior que 347**, mas `299 < 347`. Isso também nunca poderia ocorrer numa pesquisa real.

**Resumo:** válidas → i, ii, iv &nbsp;|&nbsp; inválidas → iii, v

</details>

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/18%20%E2%80%A2%20%C3%81rvores%20Gen%C3%A9ricas/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Árvores_Genéricas-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/20%20%E2%80%A2%20ABB%20-%20Remo%C3%A7%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/Avancar-ABB_Remoção_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
