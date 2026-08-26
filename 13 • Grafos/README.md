# Grafos

> Material de estudos de Algoritmos e Estruturas de Dados I.
>

## Índice

- [Definição de Grafo](#definição-de-grafo)
- [Grafo Não Direcionado](#grafo-não-direcionado)
- [Grafo Direcionado (Dígrafo)](#grafo-direcionado-dígrafo)
- [Vértices Adjacentes](#vértices-adjacentes)
- [Grau de um Vértice](#grau-de-um-vértice)
- [Ordem de um Grafo](#ordem-de-um-grafo)
- [Passeio](#passeio)
- [Comprimento de um Passeio](#comprimento-de-um-passeio)
- [Caminho](#caminho)
- [Ciclo](#ciclo)
- [Árvore](#árvore)
- [Floresta](#floresta)
- [Matriz de Adjacência](#matriz-de-adjacência)
- [Lista de Adjacência](#lista-de-adjacência)

---

## Definição de Grafo

Um **grafo** é uma estrutura de dados definida por um par:

```
G = (V, E)
```

onde:

- **V** é um conjunto finito e não vazio de **vértices** (também chamados de **nós**);
- **E** é um conjunto de **arestas** (também chamadas de **arcos**, no caso de grafos direcionados), onde cada aresta conecta um par de vértices de **V**.

Grafos são usados para modelar **relações** entre objetos: cidades e estradas, pessoas e amizades, computadores e conexões de rede, tarefas e suas dependências, etc.

---

## Grafo Não Direcionado

Em um grafo **não direcionado**, as arestas **não têm sentido** — uma aresta entre os vértices `u` e `v` representa uma conexão "de mão dupla".

Representamos uma aresta como um **par não ordenado**: `(u, v) = (v, u)`.

**Exemplo:**

```
A ─── B
│     │
C ─── D
```

Aqui, `E = {(A,B), (A,C), (B,D), (C,D)}`. Dizer que existe a aresta `(A,B)` é o mesmo que dizer que existe a aresta `(B,A)`.

---

## Grafo Direcionado (Dígrafo)

Em um grafo **direcionado** (ou **dígrafo**), cada aresta tem um **sentido** — ela sai de um vértice de **origem** e chega em um vértice de **destino**.

Representamos uma aresta como um **par ordenado**: `(u, v) ≠ (v, u)`, onde `u` é a origem e `v` é o destino.

**Exemplo:**

```
A ──▶ B
▲     │
│     ▼
C ◀── D
```

Aqui, `E = {(A,B), (D,C), (C,A), (B,D)}`. A existência da aresta `(A,B)` **não** implica a existência da aresta `(B,A)` — para ir de `B` até `A`, é preciso passar por `D` e `C`.

---

## Vértices Adjacentes

Dois vértices são **adjacentes** (ou **vizinhos**) quando existe uma aresta que os conecta diretamente.

- Em um grafo **não direcionado**: `u` e `v` são adjacentes se `(u,v) ∈ E`.
- Em um grafo **direcionado**: costuma-se dizer que `v` é **sucessor** de `u` se existe a aresta `(u,v)`, e que `u` é **predecessor** de `v`. Alguns autores só chamam de "adjacentes" quando existe aresta em pelo menos um dos dois sentidos.

No exemplo do grafo não direcionado acima, `A` e `B` são adjacentes, mas `A` e `D` **não** são (não existe aresta direta entre eles, embora exista um caminho passando por `B` ou por `C`).

---

## Grau de um Vértice

O **grau** de um vértice é o número de arestas que **incidem** sobre ele (ou seja, o número de conexões que ele possui).

### Em grafos não direcionados

O grau de um vértice `v`, denotado `grau(v)` ou `d(v)`, é simplesmente a quantidade de arestas conectadas a ele.

No exemplo do grafo não direcionado acima: `grau(A) = 2`, `grau(B) = 2`, `grau(C) = 2`, `grau(D) = 2`.

> 📌 **Propriedade importante:** a soma dos graus de todos os vértices de um grafo é sempre **igual ao dobro do número de arestas** (`Σ grau(v) = 2|E|`), já que cada aresta contribui com `+1` de grau para **dois** vértices.

### Em grafos direcionados

Como as arestas têm sentido, dividimos o grau em dois:

- **Grau de saída** (`grau⁺(v)` ou *out-degree*): número de arestas que **saem** de `v`;
- **Grau de entrada** (`grau⁻(v)` ou *in-degree*): número de arestas que **chegam** em `v`.

O grau total do vértice é a soma dos dois: `grau(v) = grau⁺(v) + grau⁻(v)`.

No exemplo do dígrafo acima: `grau⁺(A) = 1` (só a aresta `A→B`), `grau⁻(A) = 1` (só a aresta `C→A`).

---

## Ordem de um Grafo

A **ordem** de um grafo é o número de **vértices** que ele possui, ou seja, `|V|`.

> 📌 Não confunda com o **tamanho** do grafo, que é o número de **arestas** (`|E|`). No exemplo do grafo não direcionado acima, a ordem é `4` (vértices `A, B, C, D`) e o tamanho é `4` (arestas).

---

## Passeio

Um **passeio** (*walk*, em inglês) é uma sequência **alternada** de vértices e arestas:

```
v₀, e₁, v₁, e₂, v₂, ..., eₙ, vₙ
```

onde cada aresta `eᵢ` conecta o vértice `vᵢ₋₁` ao vértice `vᵢ`.

Num passeio, **tanto vértices quanto arestas podem se repetir** — não há nenhuma restrição de "não passar duas vezes pelo mesmo lugar".

**Exemplo** (usando o grafo não direcionado do início): `A, B, D, C, A, B` é um passeio válido — ele passa por `A` e por `B` duas vezes.

---

## Comprimento de um Passeio

O **comprimento** de um passeio é o **número de arestas** percorridas nele (não o número de vértices!).

No exemplo acima, o passeio `A, B, D, C, A, B` tem comprimento **5** (são 5 arestas percorridas: `A-B`, `B-D`, `D-C`, `C-A`, `A-B`), mesmo passando por 6 posições de vértices na sequência.

---

## Caminho

Um **caminho** (*path*, em inglês) é um **passeio em que nenhum vértice se repete** (e, como consequência, nenhuma aresta se repete também).

Ou seja, todo caminho é um passeio, mas nem todo passeio é um caminho — o caminho é a versão "sem repetições" do passeio.

**Exemplo:** `A, B, D, C` é um caminho (nenhum vértice se repete), enquanto `A, B, D, C, A, B` (visto acima) é um passeio, mas **não** é um caminho, pois `A` e `B` aparecem duas vezes.

---

## Ciclo

Um **ciclo** é um **caminho fechado**: um passeio onde o vértice inicial é igual ao vértice final (`v₀ = vₙ`), e nenhum outro vértice se repete ao longo do percurso (nem nenhuma aresta).

**Exemplo:** no grafo não direcionado do início (`A-B-D-C-A`), o percurso `A, B, D, C, A` é um ciclo — começa e termina em `A`, sem repetir nenhum outro vértice pelo caminho.

Um grafo que **não** contém nenhum ciclo é chamado de **acíclico** — conceito fundamental para a próxima definição.

---

## Árvore

Uma **árvore**, no contexto de grafos, é um grafo não direcionado que é, ao mesmo tempo:

- ✔️ **conexo** (existe um caminho entre qualquer par de vértices); e
- ✔️ **acíclico** (não contém nenhum ciclo).

> 📌 **Propriedade importante:** uma árvore com `n` vértices possui **exatamente `n - 1` arestas** — nem mais, nem menos. Se tivesse menos arestas, não seria possível conectar todos os vértices; se tivesse mais, obrigatoriamente haveria um ciclo.

**Exemplo:**

```
    A
   / \
  B   C
 /
D
```

Este é um exemplo de árvore: 4 vértices, 3 arestas, conexo e sem ciclos. Esse é exatamente o mesmo conceito de árvore que já vimos em estruturas como a **árvore binária de busca** e a **árvore genérica** — só que agora enxergado sob a ótica mais geral da teoria de grafos (uma ABB, por exemplo, é uma árvore em que, além disso, cada vértice tem no máximo 2 filhos e existe uma ordem entre eles).

---

## Floresta

Uma **floresta** é um grafo **acíclico**, mas **não necessariamente conexo** — ou seja, é um conjunto de uma ou mais **árvores disjuntas** (sem nenhuma aresta conectando uma árvore à outra).

Em outras palavras:

- ✔️ toda **árvore** é uma floresta (uma floresta com um único componente conexo);
- ✔️ mas nem toda **floresta** é uma árvore (só é árvore se tiver exatamente um componente conexo).

**Exemplo:**

```
    A          E
   / \         │
  B   C        F

  G───H
```

Esse é um exemplo de floresta com **3 componentes** (3 árvores disjuntas): a árvore `{A,B,C}`, a árvore `{E,F}` e a árvore `{G,H}`. Como não há nenhuma aresta conectando esses grupos entre si, o grafo inteiro não é conexo — logo, não é uma árvore, mas é uma floresta.

### Relação entre Árvore e Floresta

Uma forma bem intuitiva de enxergar essa relação: se pegarmos uma **árvore** e **removermos qualquer uma de suas arestas**, ela deixa de ser conexa e se **divide em duas árvores menores** — ou seja, o resultado é uma **floresta com 2 componentes**.

De modo geral, remover `k` arestas de uma árvore resulta em uma floresta com `k + 1` componentes (desde que as arestas removidas sejam distintas).

---

## Matriz de Adjacência

A **matriz de adjacência** é uma das duas formas mais comuns de representar um grafo em um programa.

Para um grafo com `n` vértices, a matriz de adjacência é uma matriz **n × n**, onde:

```
M[i][j] = 1, se existe aresta entre o vértice i e o vértice j
M[i][j] = 0, caso contrário
```

(Em grafos com **peso** nas arestas, ao invés de `1`, armazena-se o próprio peso da aresta.)

```c
#define N_VERTICES 4

int matrizAdj[N_VERTICES][N_VERTICES];
```

**Exemplo**, para o grafo não direcionado inicial (`A=0, B=1, C=2, D=3`):

|       | A | B | C | D |
|-------|---|---|---|---|
| **A** | 0 | 1 | 1 | 0 |
| **B** | 1 | 0 | 0 | 1 |
| **C** | 1 | 0 | 0 | 1 |
| **D** | 0 | 1 | 1 | 0 |

> 📌 **Grafo não direcionado → matriz simétrica.** Note que `M[i][j] = M[j][i]` sempre — se existe aresta de `A` para `B`, existe (a mesma aresta) de `B` para `A`. Já em um **grafo direcionado**, a matriz **não** é necessariamente simétrica, pois `M[i][j]` representa a aresta de `i` para `j`, que pode não existir no sentido contrário.

**Vantagens:** verificar se dois vértices são adjacentes é muito rápido — `M[i][j]` — feito em tempo constante.

**Desvantagens:** ocupa espaço `O(n²)` **mesmo que o grafo tenha poucas arestas** — para grafos **esparsos** (com poucas arestas em relação ao número de vértices), isso desperdiça bastante memória.

---

## Lista de Adjacência

A **lista de adjacência** é a outra forma clássica de representar um grafo: para cada vértice, mantemos uma **lista** (geralmente uma lista encadeada) com todos os vértices que são adjacentes a ele.

```c
typedef struct nodoAdj {
    int vertice;
    struct nodoAdj *prox;
} TNodoAdj;

typedef TNodoAdj *PNodoAdj;

// vetor onde cada posição é a "cabeça" da lista de adjacência daquele vértice
PNodoAdj listaAdj[N_VERTICES];
```

**Exemplo**, para o mesmo grafo não direcionado (`A=0, B=1, C=2, D=3`):

```
listaAdj[A] → B → C
listaAdj[B] → A → D
listaAdj[C] → A → D
listaAdj[D] → B → C
```

**Vantagens:** ocupa espaço proporcional ao número de arestas, `O(n + m)` (onde `m = |E|`) — muito mais eficiente que a matriz para grafos esparsos.

**Desvantagens:** verificar se dois vértices são adjacentes é mais lento — é preciso **percorrer a lista** do vértice até encontrar (ou não) o outro vértice, o que pode custar até `O(grau(v))` no pior caso, ao invés do tempo constante da matriz.

### Matriz x Lista — quando usar cada uma?

| Critério | Matriz de Adjacência | Lista de Adjacência |
|---|---|---|
| Espaço ocupado | `O(n²)` | `O(n + m)` |
| Verificar se `(u,v)` é aresta | `O(1)` | `O(grau(u))` |
| Percorrer todos os vizinhos de `v` | `O(n)` | `O(grau(v))` |
| Melhor para... | grafos **densos** (muitas arestas) | grafos **esparsos** (poucas arestas) |

Na prática, a **lista de adjacência** costuma ser a escolha mais comum, já que a maioria dos grafos do mundo real (redes sociais, mapas de estradas, etc.) é esparsa.
