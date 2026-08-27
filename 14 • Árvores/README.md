<h1 align="center">Árvores</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/13%20%E2%80%A2%20Grafos/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Grafos-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/15%20%E2%80%A2%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/Avancar-Árvores_Binárias_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## <mark> 1 - Introdução </mark>

>  Uma das mais importantes classes de estruturas de dados em computação são as **árvores**.
>
>  Aproveitando-se de sua organização **hierárquica**, muitas aplicações são realizadas usando-se algoritmos relativamente simples, recursivos e de eficiência bastante razoável.

Até agora você estudou estruturas **lineares**: listas, pilhas, filas — onde cada elemento tem, no máximo, um "próximo" e um "anterior". A árvore quebra essa linearidade: ela representa uma **hierarquia**, onde um elemento pode se ramificar em vários outros.

Pense em situações do dia a dia que já são naturalmente hierárquicas:

- O **organograma de uma empresa** (diretor → gerentes → funcionários)
- A **divisão de um livro** em capítulos, seções e subseções
- A **árvore genealógica** de uma família
- O **sistema de pastas** do seu computador (pasta → subpastas → arquivos)

Todos esses exemplos têm algo em comum: existe **um elemento "no topo"**, e a partir dele tudo se ramifica.

---

## <mark> 2. Definição </mark>

>  Uma árvore é uma estrutura de dados que se caracteriza por uma **relação de hierarquia** entre os elementos que a compõem.

De um modo mais formal, uma árvore é um **conjunto finito de um ou mais nós (vértices)**, tais que:

-  existe um nó denominado **raiz**;
-  os demais nós formam `m >= 0` conjuntos separados `s1, s2, ..., sm`, tais que cada um desses conjuntos também é uma árvore (denominada **sub-árvore**).

>  Repare que essa definição é **recursiva**: uma árvore é feita de nós que, por sua vez, são raízes de outras árvores menores (sub-árvores). É exatamente por isso que muitos algoritmos sobre árvores são naturalmente escritos de forma recursiva — a própria estrutura de dados "pede" esse tipo de solução.

Vamos usar esta árvore como exemplo ao longo de todo o material. Ela tem a seguinte estrutura:

- `A` é a raiz, e é pai de `B`, `C` e `D`;
- `B` é pai de `E` e `F`;
- `C` é pai de `G`;
- `D` é pai de `H`, `I` e `J`;
- `E` é pai de `K`;
- `H` é pai de `L`.

Você vai ver essa mesma árvore reaparecer nas próximas seções, escrita de outras formas.

---

## <mark> 3 - Representações de uma árvore </mark>

A mesma árvore pode ser desenhada/escrita de formas diferentes. Isso é útil porque, dependendo da situação (explicar no quadro, guardar em um arquivo de texto, ou implementar no código), uma representação pode ser mais conveniente que outra.

### <mark> 3.1 Grafo (a forma mais visual) </mark>

Essa é a representação mais intuitiva, e provavelmente a primeira que vem à cabeça quando você pensa em "árvore": círculos representando os nós, ligados por linhas que indicam a relação de "pai para filho", com a raiz desenhada no topo e os galhos se abrindo para baixo.

### <mark> 3.2 Parênteses aninhados </mark>

Cada nó é seguido, entre parênteses, pelos seus filhos:

```
(A(B(E(K)F)C(G)D(H(L)IJ)))
```

Leia assim: `A` tem filhos `B`, `C` e `D`. `B` tem filhos `E` e `F`. `E` tem filho `K`. E assim por diante. Essa representação é bem compacta e é útil, por exemplo, para guardar uma árvore em uma única linha de texto.

### <mark> 3.3 Paragrafação </mark>

Os nós são escritos um por linha, e a indentação (nível de recuo) indica a profundidade de cada nó na árvore:

```
A
  B
    E
      K
    F
  C
    G
  D
    H
      L
    I
    J
```

> 💡 Essa é a representação que o comando `tree` do terminal usa para mostrar pastas e arquivos, e também é como o Windows Explorer / macOS Finder mostram uma árvore de diretórios expandida.

### Qual representação usar?

| Representação | Quando é útil |
|---|---|
| Grafo | Explicar visualmente, entender a estrutura de relance |
| Parênteses aninhados | Guardar a árvore inteira em uma linha de texto/arquivo |
| Paragrafação | Mostrar hierarquia de forma legível (ex: estrutura de pastas) |

---

## <mark> 4 - Conceitos básicos </mark>

### <mark> Aresta </mark>

> -> Dada uma árvore qualquer, a linha que liga dois nós da árvore denomina-se **aresta**.

### <mark> Caminho </mark>

> -> Diz-se que existe **caminho** entre dois nós V e W da árvore, se a partir do nó V for possível chegar ao nó W percorrendo-se as arestas que ligam os nós intermediários entre V e W.
>
> -> Observa-se que existe **sempre** um caminho entre a raiz e qualquer nó da árvore.

### <mark> Ancestral e descendente / Pai e filho / Irmãos </mark>

> -> Se houver um caminho entre V e W, começando em V, diz-se que **V é um nó ancestral de W** e **W é um nó descendente de V**.
>
>  Se este caminho contiver uma **única aresta**, diz-se que V é o nó **pai** de W e que W é um nó **filho** de V.
>
>  Dois nós que são nós filhos do mesmo nó pai são denominados **nós irmãos**.

Usando nossa árvore de exemplo: `D` é filho de `A` (e, portanto, `A` é pai de `D`). Já `H`, `I` e `J` são irmãos entre si, pois todos são filhos de `D`. E `L` é descendente de `A`, mas não é seu filho direto — o caminho entre eles (`A → D → H → L`) passa por mais de uma aresta, então `A` é apenas ancestral de `L`, não seu pai.

> ⚠️ **Cuidado com a diferença:** "pai/filho" é uma relação **direta** (uma única aresta de distância). "Ancestral/descendente" é uma relação **mais ampla**, que vale para qualquer distância no caminho — todo pai é ancestral, mas nem todo ancestral é pai.

### <mark> Raiz </mark>

> -> Normalmente as árvores são desenhadas de forma invertida, com a **raiz em cima**.

A raiz é o único nó que **não tem pai** — é o topo da hierarquia. No nosso exemplo, `A` é a raiz.

### <mark> Folha (nó terminal) e nó não-folha </mark>

> -> Uma característica inerente a árvores é que **qualquer nó, exceto a raiz, tem um único nó pai**.
>
> -> Se um nó não possui nós descendentes, ele é chamado de **folha** ou **nó terminal** da árvore (nós com grau zero).
>
> -> Consequentemente, nó com descendentes (filhos) é denominado nó **não-folha** ou nó **não-terminal** da árvore (nós com grau maior que zero).

No nosso exemplo, os nós `K, F, G, L, I, J` são as **folhas**. Os nós que sobraram (`A, B, C, D, E, H`) são **não-folha**, porque todos têm pelo menos um filho.

### <mark> Grau de um nó </mark>

> -> **Grau** de um nó é o número de nós filhos do mesmo nó. Obviamente que um nó folha tem grau zero.

| Nó | Filhos | Grau |
|---|---|---|
| A | B, C, D | 3 |
| B | E, F | 2 |
| C | G | 1 |
| D | H, I, J | 3 |
| E | K | 1 |
| F | — | 0 (folha) |

### <mark> Nível de um nó </mark>

> -> **Nível** de um nó é o número de nós existentes no caminho entre a raiz e o próprio nó.
>
>  Por definição, dizemos que a raiz de uma árvore encontra-se no **nível 0** (alguns autores assumem a raiz como nível 1).
>
>  Os filhos da raiz estão no nível 1, os filhos dos filhos da raiz estão no nível 2, e assim sucessivamente.
>
>  Estando um nó no nível `n`, seus filhos estarão no nível `n + 1`.

Aplicando isso à nossa árvore de exemplo:

- **Nível 0:** `A`
- **Nível 1:** `B`, `C`, `D`
- **Nível 2:** `E`, `F`, `G`, `H`, `I`, `J`
- **Nível 3:** `K`, `L`

> ⚠️ **Atenção:** essa definição (raiz no nível 0) é a convenção usada pela Regina neste material, mas fique de olho, porque **alguns livros/professores começam do nível 1**. O importante é entender a lógica: "quantas arestas eu percorro da raiz até aqui" (nível 0) ou "quantos nós existem no caminho, contando a raiz" (nível 1) — são a mesma ideia, só a contagem inicial muda.

### <mark> Grau da árvore </mark>

> -> O grau da árvore é igual ao **grau do nó de maior grau** da árvore (como visto em grafos).

No nosso exemplo: `A` e `D` têm grau 3, que é o maior de todos — então a **árvore tem grau 3**.

### <mark> Árvore completa </mark>

> -> Uma árvore de grau **d** é **completa** se:
>
>  Todos os nós têm exatamente **d** filhos, exceto as folhas;
>
>  Todas as folhas estão no **mesmo nível**.

Ou seja: não basta todo mundo ter o mesmo número de filhos — as folhas também precisam estar "niveladas", sem nenhum ramo terminando antes da hora. Nossa árvore de exemplo **não é completa**: por exemplo, `F` (nível 2) e `K` (nível 3) são ambas folhas, mas estão em níveis diferentes.

### <mark> Altura (ou profundidade) da árvore </mark>

> -> A **altura** (ou **profundidade**) de uma árvore é o **nível máximo** entre todos os nós da árvore ou, equivalentemente, é a altura da raiz.
>
> -> No exemplo, a árvore possui **altura 3**.
>
> -> A altura de uma árvore vazia é **-1**.

No nosso exemplo, o nó mais "fundo" é `K` ou `L`, que estão no nível 3 — por isso a altura da árvore é 3.

### <mark> Floresta </mark>

> -> Um conjunto de `n` árvores separadas (`n >= 0`) é chamado de **floresta**.
>
>  Se retirarmos a raiz de uma árvore obteremos uma floresta.

Faz sentido: se tirarmos o nó `A` (a raiz) do nosso exemplo, sobram três "pedaços" separados — a sub-árvore com raiz `B`, a sub-árvore com raiz `C` e a sub-árvore com raiz `D`. Como nenhuma delas está mais ligada a um nó em comum, o conjunto das três forma uma **floresta**.

---

## <mark> 5 - TAD (Tipo Abstrato de Dados) de Árvores </mark>

Como implementar isso em código? O grande desafio é que, diferente de uma lista (onde cada nó tem só "um próximo"), aqui **um nó pode ter qualquer quantidade de filhos** (0, 1, 2, 10...). A solução mais natural é dizer que cada nó guarda, além dos seus dados, uma **lista de filhos**:

```c
typedef struct SNo *Tarvore;

typedef struct {
  int Chave;
  /* outros componentes */
} TDados;

typedef struct SNo {
  TDados Dados;
  TLista Filhos;
} Tno;
```

**Explicando essa estrutura:**

- `TDados` representa a informação que cada nó guarda (aqui simplificado como um `Chave` inteiro, mas poderia ser um struct maior — nome, idade, o que for necessário).
- `Filhos` é do tipo `TLista` — ou seja, **cada nó guarda uma lista** com todos os seus nós filhos. Essa lista pode ter 0, 1, 2 ou quantos elementos forem necessários, o que resolve exatamente o problema de "número variável de filhos" que uma árvore genérica precisa suportar.
- `Tarvore` é definido como um **ponteiro para um nó** (`SNo *`) — a árvore inteira é representada só pelo ponteiro para sua raiz, já que a partir da raiz conseguimos alcançar todos os outros nós (lembra do conceito de "caminho" que vimos antes?).

> 💡 **Por que isso importa?** Essa é a base para a próxima aula, sobre **árvores genéricas** — onde vamos implementar de verdade essa lista de filhos (provavelmente com uma lista encadeada) e escrever funções como inserir filho, buscar um nó, calcular altura, contar folhas, percorrer a árvore, etc. Vale a pena já deixar claro na cabeça essa ideia de "nó = dado + lista de filhos", porque tudo que vem depois se apoia nela.

---

## <mark> 6 - Resumo rápido </mark>

| Termo | Significado |
|---|---|
| Nó / vértice | Cada elemento da árvore |
| Raiz | O único nó sem pai — o "topo" da hierarquia |
| Aresta | Linha que liga um nó pai a um nó filho |
| Caminho | Sequência de arestas ligando dois nós |
| Pai / filho | Relação direta (1 aresta de distância) |
| Ancestral / descendente | Relação mais ampla (qualquer distância no caminho) |
| Irmãos | Nós que compartilham o mesmo pai |
| Folha (nó terminal) | Nó sem filhos (grau 0) |
| Nó não-folha | Nó com pelo menos 1 filho |
| Grau de um nó | Quantidade de filhos daquele nó |
| Grau da árvore | O maior grau entre todos os nós |
| Nível de um nó | Quantidade de nós no caminho da raiz até ele (raiz = nível 0) |
| Altura da árvore | O maior nível entre todos os nós (árvore vazia = altura -1) |
| Árvore completa | Todo nó tem exatamente `d` filhos (exceto folhas), e todas as folhas estão no mesmo nível |
| Floresta | Conjunto de árvores separadas — surge, por exemplo, ao remover a raiz de uma árvore |

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/13%20%E2%80%A2%20Grafos/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Grafos-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/15%20%E2%80%A2%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/Avancar-Árvores_Binárias_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
