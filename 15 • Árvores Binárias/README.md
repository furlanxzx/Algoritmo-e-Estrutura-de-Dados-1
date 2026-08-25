# Árvores Binárias

> 📚 Material baseado nas aulas da Prof.ª Regina — Algoritmos e Estrutura de Dados

---

## 1. Introdução

> ➤ Dentre as árvores, as **binárias** são, sem dúvida, as mais comuns.

Uma árvore binária é simplesmente uma árvore com uma restrição a mais: **cada nó tem no máximo 2 filhos**. Mas essa restrição traz um detalhe importante:

> ➤ Deve-se observar que a **ordem** em que estão posicionadas as sub-árvores em relação à raiz é **fundamental**.
> - ✓ Uma vez que cada nó tem no máximo dois filhos, cada um destes nós (se houver) são identificados segundo a sua **posição relativa à raiz**.
> - ✓ Distingue-se, então, o nó filho à **esquerda** do nó filho à **direita** e, consequentemente, a sub-árvore à esquerda da sub-árvore à direita.

### Definição formal

> ➤ Uma **árvore binária** é uma árvore que pode ser **nula**, ou então tem as seguintes características:
> - ✓ existe um nó especial denominado **raiz**;
> - ✓ os demais nós são particionados em **T₁, T₂** — estruturas disjuntas de árvores binárias;
> - ✓ T₁ é denominada **sub-árvore esquerda** e T₂, **sub-árvore direita** da raiz.

> ➤ Árvore binária é um caso especial de árvore em que **nenhum nó tem grau superior a 2**.
>
> ➤ Adicionalmente, para árvores binárias existe um **"senso de posição"**, ou seja, distingue-se entre uma sub-árvore esquerda e uma direita.

> 💡 **Por que essa distinção de posição importa tanto?** Porque numa árvore genérica (a que vimos na introdução), a ordem dos filhos geralmente não muda o "significado" da árvore. Já numa árvore binária, **trocar um filho de lado muda a árvore**! Isso vai ficar bem claro no exemplo a seguir.

### Exemplo — quando duas árvores parecidas não são iguais

```
        a              a              a
       / \             \             /
      b   c              b          b
     /   / \
    d   e   f
```

> ➤ As duas últimas árvores, considerando-as **binárias**, são **diferentes**, pois a primeira tem sub-árvore direita vazia e a segunda tem sub-árvore esquerda vazia.
>
> ➤ Se as considerarmos como **árvores (genéricas)**, elas serão idênticas, apenas desenhadas de maneira diferente.

Ou seja: no mundo das árvores genéricas, "b é filho de a" já basta. No mundo das árvores **binárias**, não basta saber que b é filho de a — precisamos saber **de que lado** ele está, porque isso é parte da identidade da árvore.

---

## 2. Implementação

### 2.1 Estrutura do nó

> ➤ Análogo ao que fizemos para as demais estruturas de dados, podemos definir um tipo para representar uma árvore binária.
>
> ➤ Cada nó deve armazenar três informações:
> - ✓ a informação propriamente dita;
> - ✓ dois ponteiros para as sub-árvores, à esquerda e à direita.

```c
typedef struct arv {
    char info;
    struct arv* esq;
    struct arv* dir;
} TArv;
typedef TArv *PArv;
```

Visualmente, cada nó é dividido em 3 campos: o dado (`Item`) e dois ponteiros de ligação — um para a sub-árvore esquerda (`Esq`) e um para a sub-árvore direita (`Dir`):

```
     ┌──────────────────────────────┐
     │  Esq  │    info    │   Dir   │
     └──────────────────────────────┘
        │                      │
        ▼                      ▼
   sub-árvore              sub-árvore
    esquerda                 direita
```

> ➤ Da mesma forma que uma lista encadeada é representada por um ponteiro para o primeiro nó, a estrutura da árvore como um todo é representada por um **ponteiro para o nó raiz**.
>
> ➤ Como acontece com qualquer TAD (tipo abstrato de dados), as operações que fazem sentido para uma árvore binária dependem essencialmente da forma de utilização que se pretende fazer da árvore.

> 🧠 **Sobre alocação dinâmica:** essas estruturas dificultam a determinação do **pai** de um nó — afinal, os ponteiros só apontam "para baixo" (dos pais para os filhos), nunca "para cima". Se for essencial ter esse acesso, pode-se adicionar mais um campo de modo a armazenar a referência ao nó pai (algo como `struct arv *pai;`).

### 2.2 Criando árvores: a função `cria`

> ➤ As funções que manipulam árvores são, em geral, implementadas de forma **recursiva**.
>
> ➤ Para criar árvores não vazias, podemos ter uma operação que cria um nó raiz dadas a informação e suas duas sub-árvores, à esquerda e à direita.

```c
PArv cria(char c, PArv sae, PArv sad) {
    PArv p = (PArv) malloc(sizeof(TArv));
    p->info = c;
    p->esq = sae;
    p->dir = sad;
    return p;
}
```

> ➤ A função `cria` e a inicialização da árvore com `NULL` representam os **dois casos da definição recursiva** de árvore binária:
> - ✓ uma árvore binária (`PArv a`) é **vazia** (`a = NULL`) **ou** é composta por uma raiz e duas sub-árvores (`a = cria(c, sae, sad)`).
> - ✓ Com isso, podemos criar árvores mais complexas.

Repare como isso é elegante: com **uma única função**, conseguimos montar árvores de qualquer tamanho e formato, aninhando chamadas de `cria` umas dentro das outras — cada chamada resolve "uma raiz + duas sub-árvores", e cada sub-árvore, por sua vez, pode ser outra chamada de `cria` (ou `NULL`, se aquele lado não tiver filho).

**Exemplo — construindo a árvore abaixo sem usar recursividade "manual" (só encadeando chamadas de `cria`):**

```
        a
       / \
      b   c
     /   / \
    d   e   f
```

```c
int main() {
   PArv a = cria('a',
                    cria('b', NULL,
                        cria('d', NULL, NULL)
                    ),
                    cria('c',
                        cria('e', NULL, NULL),
                        cria('f', NULL, NULL)
                    )
                );
   imprime(a);
   libera(a);
   return 0;
}
```

Note que cada `cria(...)` já entrega uma sub-árvore pronta, que é passada como argumento para o `cria` de cima. É por isso que dizemos que não usamos "recursividade" aqui — cada chamada de `cria` faz um trabalho simples e direto, sem chamar a si mesma.

### 2.3 Acessando e modificando nós existentes

> ➤ Considerando a criação da árvore anterior, podemos acrescentar alguns nós, com:

```c
a->esq->esq = cria('x',
                 cria('y', NULL, NULL),
                 cria('z', NULL, NULL)
              );
```

Isso pega o nó `b` (que é `a->esq`) e diz: "o filho esquerdo de b agora é uma nova sub-árvore com raiz x e filhos y, z".

> ➤ E podemos liberar alguns outros, com:

```c
a->dir->esq = libera(a->dir->esq);
```

Isso pega o nó `c` (que é `a->dir`) e libera toda a sub-árvore que estava no seu filho esquerdo (o `e`), substituindo aquele ponteiro por `NULL` — repare que `libera` **retorna** o novo valor do ponteiro (sempre `NULL`, já que a sub-árvore deixou de existir), seguindo o mesmo padrão que já vimos em listas encadeadas.

### 2.4 Completando o `main()`: `imprime` e `libera`

Os slides usam as funções `imprime` e `libera` no `main()`, mas sem mostrar como implementá-las — então vamos completá-las aqui, já que elas são essenciais para qualquer programa com árvores. Ambas seguem o mesmo padrão recursivo da `cria`: **tratam o caso vazio, e resolvem o resto chamando a si mesmas nas sub-árvores**.

```c
void imprime(PArv a) {
    if (a != NULL) {
        printf("%c ", a->info);
        imprime(a->esq);
        imprime(a->dir);
    }
}
```

```c
PArv libera(PArv a) {
    if (a != NULL) {
        libera(a->esq);
        libera(a->dir);
        free(a);
    }
    return NULL;
}
```

> 🧠 **Por que `libera` retorna `PArv` em vez de ser `void`?** Porque, como vimos no exemplo `a->dir->esq = libera(a->dir->esq);`, é conveniente que a função já devolva o novo valor (`NULL`) para ser atribuído diretamente ao ponteiro que apontava pra árvore liberada — evitando que sobre algum ponteiro "solto" apontando para memória já desalocada (o clássico *dangling pointer*).

### 2.5 Bônus: percursos (traversals)

Antes de seguir para os exercícios, vale comentar uma coisa: a função `imprime` que fizemos acima segue uma ordem específica — primeiro imprime o nó, depois a sub-árvore esquerda, depois a direita. Essa ordem tem nome: **pré-ordem**. Existem 3 formas clássicas de percorrer uma árvore binária, e você vai encontrá-las bastante nos próximos módulos:

| Percurso | Ordem | Como fica no código |
|---|---|---|
| **Pré-ordem** | nó → esquerda → direita | `imprime(nó); percorre(esq); percorre(dir);` |
| **Em ordem** (in-order) | esquerda → nó → direita | `percorre(esq); imprime(nó); percorre(dir);` |
| **Pós-ordem** | esquerda → direita → nó | `percorre(esq); percorre(dir); imprime(nó);` |

> 💡 Repare que a nossa função `libera` segue justamente a ordem **pós-ordem**: primeiro libera os filhos, só depois libera o próprio nó — faz todo sentido, já que não podemos liberar um nó e só *depois* tentar acessar seus filhos (perderíamos a referência a eles!).

---

## 3. Exercícios

<br>

### Exercício 1 🟡 (médio)

> Quantos nós, no mínimo, tem uma árvore binária de altura *h*? E no máximo? Qual o número máximo de nós do nível *k* de uma árvore binária?

<details>
<summary>💡 Ver resolução</summary>

Esse exercício é mais teórico/matemático do que de código — vamos raciocinar por partes.

**Número máximo de nós no nível k:**

No nível 0 (a raiz), só pode existir **1** nó. No nível 1, cada um dos filhos da raiz pode ter até 2 filhos, então o nível 2 pode ter no máximo 4 nós. Percebendo o padrão: cada nível pode ter, no máximo, o **dobro** de nós do nível anterior, porque cada nó tem no máximo 2 filhos.

$$\text{máximo de nós no nível } k = 2^k$$

**Número mínimo de nós numa árvore de altura h:**

O caso mínimo acontece quando a árvore é totalmente "esticada" (uma cadeia, como uma lista encadeada) — cada nó tem só 1 filho. Para alcançar a altura *h*, precisamos de exatamente um nó em cada nível, do nível 0 até o nível *h*:

$$\text{mínimo de nós para altura } h = h + 1$$

**Número máximo de nós numa árvore de altura h:**

O caso máximo acontece quando a árvore está **completamente cheia** em todos os níveis (uma árvore perfeita) — todo nó, exceto os do último nível, tem exatamente 2 filhos. Nesse caso, somamos o máximo de nós de cada nível, do nível 0 até o nível *h*:

$$\text{máximo de nós para altura } h = 2^0 + 2^1 + 2^2 + \dots + 2^h = 2^{h+1} - 1$$

**Resumo:**

| | Fórmula |
|---|---|
| Mínimo de nós para altura *h* | `h + 1` |
| Máximo de nós para altura *h* | `2^(h+1) - 1` |
| Máximo de nós no nível *k* | `2^k` |

> 🧠 Essas fórmulas são a base para entender a **eficiência** de árvores binárias: quando a árvore está bem balanceada (próxima do caso máximo), a altura cresce em `log₂(n)`, o que torna buscas muito rápidas. Quando a árvore vira uma "cadeia" (caso mínimo), ela se comporta como uma lista encadeada, perdendo toda a vantagem — isso é o que motiva o estudo de árvores balanceadas mais adiante no curso.

</details>

<br>

### Exercício 2 🟢 (fácil)

> Escreva uma função **recursiva** que conte a quantidade de nós em uma árvore binária. O protótipo da função pode ser dado por:
>
> ```c
> int quant_nos(PArv arv);
> ```

<details>
<summary>💡 Ver resolução</summary>

Seguindo a mesma lógica recursiva das demais funções: se a árvore é vazia, a contagem é 0. Caso contrário, é **1 (o nó atual) + a contagem da sub-árvore esquerda + a contagem da sub-árvore direita**.

```c
int quant_nos(PArv arv) {
    if (arv == NULL) return 0;
    return 1 + quant_nos(arv->esq) + quant_nos(arv->dir);
}
```

> 🧠 Esse é praticamente o "exercício modelo" para entender recursão em árvores: **caso base** (árvore vazia → 0) + **passo recursivo** que combina o resultado das duas sub-árvores. A maioria das funções sobre árvores binárias segue exatamente essa receita.

</details>

<br>

### Exercício 3 🟡 (médio)

> Escreva uma função **recursiva** que percorre uma árvore binária para determinar sua altura. O protótipo da função pode ser dado por:
>
> ```c
> int altura(PArv arv);
> ```

<details>
<summary>💡 Ver resolução</summary>

Lembrando da nossa introdução sobre árvores: a altura de uma árvore vazia é **-1** (é o caso base), e a altura de uma árvore não vazia é **1 + a maior altura entre as duas sub-árvores**.

```c
int altura(PArv arv) {
    if (arv == NULL) return -1;

    int he = altura(arv->esq);
    int hd = altura(arv->dir);

    return 1 + (he > hd ? he : hd);
}
```

> 🧠 Por que a convenção "altura de árvore vazia = -1" é tão conveniente aqui? Porque ela faz a fórmula funcionar **até para as folhas**, sem precisar de um caso especial: uma folha tem duas sub-árvores vazias (altura -1 cada), então sua altura calculada é `1 + (-1) = 0` — que é exatamente o esperado (uma folha sozinha tem altura 0).

</details>

<br>

### Exercício 4 🟡 (médio)

> Escreva uma função recursiva que verifique se existe um determinado caractere na árvore.
>
> ```c
> int busca(PArv a, char c);
> ```

<details>
<summary>💡 Ver resolução</summary>

Aqui, o caso base é a árvore vazia (não encontrou, retorna falso). Caso contrário, verificamos se o **nó atual** é o valor procurado — se não for, a busca "se espalha" para as duas sub-árvores, e basta que **uma delas** encontre o valor para a resposta ser verdadeira.

```c
int busca(PArv a, char c) {
    if (a == NULL) return 0; // não encontrado

    if (a->info == c) return 1; // achou no nó atual

    return busca(a->esq, c) || busca(a->dir, c);
}
```

> 🧠 O operador `||` (ou lógico) aqui faz um papel importante: graças ao **short-circuit** do C, se `busca(a->esq, c)` já retornar 1 (encontrou), o programa **nem chega a chamar** `busca(a->dir, c)` — economizando processamento desnecessário assim que a resposta já é conhecida.

</details>

---

## 4. Resumo rápido

| Conceito | Explicação |
|---|---|
| Árvore binária | Árvore em que cada nó tem **no máximo 2 filhos**, com posição (esquerda/direita) relevante |
| Estrutura do nó | `info` (dado) + `esq` e `dir` (ponteiros para as sub-árvores) |
| Função `cria` | Monta um nó a partir do dado e das duas sub-árvores já prontas |
| Padrão recursivo | Quase toda função sobre árvores trata o caso `NULL` primeiro, depois combina o resultado de `esq` e `dir` |
| Pré-ordem | nó → esquerda → direita |
| Em ordem | esquerda → nó → direita |
| Pós-ordem | esquerda → direita → nó |
| Mínimo de nós (altura h) | `h + 1` |
| Máximo de nós (altura h) | `2^(h+1) - 1` |
| Máximo de nós (nível k) | `2^k` |

> 🧭 **O que vem por aí:** o próximo passo natural depois de dominar a estrutura básica é a **árvore binária de busca (BST)**, que acrescenta uma regra de ordenação (tudo à esquerda é menor, tudo à direita é maior que o nó) — o que torna buscas, inserções e remoções muito eficientes.
