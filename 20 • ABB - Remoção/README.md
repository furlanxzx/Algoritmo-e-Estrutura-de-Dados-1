<h1 align="center">ABB - Remoção</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/19%20%E2%80%A2%20ABB%20-%20Inser%C3%A7%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-ABB_Inserção-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/01%20%E2%80%A2%20Defini%C3%A7%C3%B5es%20Iniciais%20e%20Nota%C3%A7%C3%A3o%20Assint%C3%B3tica/README.md">
    <img src="https://img.shields.io/badge/Avancar-Definições_Iniciais_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## Remoção

Se o nó a ser retirado de uma árvore for uma **folha**, basta atualizar o link do seu pai para que não aponte mais para o nó a ser retirado.

No caso de retirada de um nó que é **raiz** (ou de qualquer nó, na verdade — a mesma lógica vale para qualquer nó da árvore, não só a raiz), deve-se adotar o seguinte procedimento:

- ✔️ **a raiz não possui filhos**: a solução é trivial (basta removê-la);
- ✔️ **a raiz possui um único filho**: podemos remover o nó raiz, substituindo-o pelo seu nó filho;
- ✔️ **a raiz possui dois filhos**: não é possível que os dois filhos assumam o lugar do pai. Nesse caso:
  - escolhemos o nó que armazena o **maior elemento na sub-árvore esquerda** (ou o **menor na sub-árvore direita**);
  - este nó será removido e o elemento armazenado por ele entrará na raiz a ser removida.

> 💡 Essa troca funciona porque tanto o maior elemento da sub-árvore esquerda quanto o menor da sub-árvore direita são, por definição, os **"vizinhos" mais próximos** do nó removido em termos de valor — colocar qualquer um deles no lugar do nó removido mantém a propriedade de ABB intacta.

---

## Os Quatro Casos de Remoção

São **quatro** as possibilidades existentes para retirada. Considere que o nó a ser retirado pode ser:

1. Um nó **sem descendentes** (folha);
2. Um nó **sem descendentes à direita**, mas **com descendentes à esquerda**;
3. Um nó **sem descendentes à esquerda**, mas **com descendentes à direita**;
4. Um nó **com 2 filhos**.

No programa, isso pode ser representado por 4 testes de seleção (`if`):

```c
if ((a->esq == NULL) && (a->dir == NULL)) {
    // nó sem descendentes
    ... (2 linhas)
}
else if ((a->esq != NULL) && (a->dir == NULL)) {
    // somente descendentes à esq.
    ... (3 linhas)
}
else if ((a->esq == NULL) && (a->dir != NULL)) {
    // somente descendentes à dir.
    ... (3 linhas)
}
else {
    // nó com 2 filhos
    ... (algumas linhas)
}
```

### Exercício — Algoritmo de Remoção 🔴

> Considerando a teoria vista sobre a remoção, tente escrever o algoritmo de remoção.

<details>
<summary>Ver resposta</summary>

Vamos usar o mesmo esqueleto de 4 testes visto acima, preenchendo cada caso. A função recebe a raiz da (sub)árvore e a chave a ser removida, e retorna a nova raiz dessa (sub)árvore — assim como fizemos em `insere`, é esse valor de retorno que permite "religar" a árvore corretamente após a remoção.

Chamei a função de `retira` (em vez de `remove`) para não conflitar com a função `remove` da biblioteca padrão do C (usada para apagar arquivos).

```c
PABB retira (PABB a, int x)
{
    if (a == NULL)
        return NULL; // elemento não encontrado, nada a fazer

    if (x < a->info)
        a->esq = retira(a->esq, x);
    else if (x > a->info)
        a->dir = retira(a->dir, x);
    else {
        // achamos o nó a ser removido (x == a->info)

        if ((a->esq == NULL) && (a->dir == NULL)) {
            // nó sem descendentes
            free(a);
            return NULL;
        }
        else if ((a->esq != NULL) && (a->dir == NULL)) {
            // somente descendentes à esq.
            PABB filho = a->esq;
            free(a);
            return filho;
        }
        else if ((a->esq == NULL) && (a->dir != NULL)) {
            // somente descendentes à dir.
            PABB filho = a->dir;
            free(a);
            return filho;
        }
        else {
            // nó com 2 filhos:
            // busca o MAIOR elemento da sub-árvore esquerda
            PABB maior = a->esq;
            while (maior->dir != NULL)
                maior = maior->dir;

            // copia o valor encontrado para o nó atual
            a->info = maior->info;

            // remove o nó "maior" da sub-árvore esquerda
            // (ele agora está duplicado na árvore, então precisa sair)
            a->esq = retira(a->esq, maior->info);
        }
    }

    return a;
}
```

📌 **Por que o `else` final não precisa de `free`/`return` direto?** Porque, no caso de 2 filhos, nós não removemos o nó `a` fisicamente — apenas copiamos para ele o valor do "maior da esquerda" e, em seguida, chamamos `retira` recursivamente para remover o nó que tinha esse valor (que, por sua vez, vai cair em um dos três primeiros casos, já que o maior elemento de uma sub-árvore nunca tem filho à direita).

</details>

---

## Exercícios

### Exercício 1 🔴

Escreva uma função para verificar se uma árvore binária é ABB.

<details>
<summary>Ver resposta</summary>

**Erro comum:** um erro clássico é verificar apenas se `a->esq->info < a->info` e `a->dir->info > a->info` olhando só para os filhos diretos. Isso **não é suficiente**! É preciso garantir que **toda** a sub-árvore esquerda seja menor que `a->info` e **toda** a sub-árvore direita seja maior — não só os filhos imediatos.

A forma correta é carregar, em cada chamada recursiva, os **limites** (mínimo e máximo) que os valores daquele ramo podem assumir:

```c
int eABB (PABB a, int minVal, int maxVal)
{
    if (a == NULL)
        return 1; // árvore vazia é ABB por definição

    if (a->info < minVal || a->info > maxVal)
        return 0; // valor fora dos limites permitidos

    return eABB(a->esq, minVal, a->info - 1) &&
           eABB(a->dir, a->info + 1, maxVal);
}
```

A chamada inicial deve ser feita com os limites mais abertos possíveis:

```c
eABB(raiz, INT_MIN, INT_MAX)
```

(usando `#include <limits.h>` para `INT_MIN` e `INT_MAX`).

**Por que isso funciona?** A cada passo, ao descer para a sub-árvore esquerda, sabemos que todo valor ali deve ser **menor** que `a->info`, então o novo limite máximo passa a ser `a->info - 1`. Simetricamente, ao descer para a direita, o novo limite mínimo passa a ser `a->info + 1`. Assim, um valor "fora de lugar" — mesmo que respeite seu pai imediato, mas viole um ancestral mais distante — é detectado.

</details>

---

### Exercício 2 🟡

Escreva uma função para excluir todas as folhas de uma ABB.

<details>
<summary>Ver resposta</summary>

A função recebe a raiz e retorna a nova raiz (importante para tratar o caso especial de uma árvore com um único nó, que também é uma folha).

```c
PABB excluiFolhas (PABB a)
{
    if (a == NULL)
        return NULL;

    if ((a->esq == NULL) && (a->dir == NULL)) {
        // a é uma folha: remove
        free(a);
        return NULL;
    }

    a->esq = excluiFolhas(a->esq);
    a->dir = excluiFolhas(a->dir);

    return a;
}
```

⚠️ **Atenção:** a verificação "`a` é folha?" acontece **antes** de mexermos nos filhos de `a`. Isso garante que só removemos os nós que **já eram** folhas antes da chamada — e não nós que "viraram" folha depois que seus próprios filhos foram removidos nesta mesma execução.

</details>

---

### Exercício 3 🟢

Escreva uma função que conte o número de folhas em uma ABB.

<details>
<summary>Ver resposta</summary>

```c
int contaFolhas (PABB a)
{
    if (a == NULL)
        return 0;

    if ((a->esq == NULL) && (a->dir == NULL))
        return 1;

    return contaFolhas(a->esq) + contaFolhas(a->dir);
}
```

</details>

---

### Exercício 4 🟡

Escreva uma função que remova um nó contendo uma dada chave **e seus descendentes**.

<details>
<summary>Ver resposta</summary>

⚠️ Esse exercício é diferente da função `retira` que fizemos antes! Ali, ao remover um nó, **preservávamos** seus descendentes (reorganizando a árvore). Aqui, quando encontramos o nó com a chave buscada, queremos apagar **ele e toda a sub-árvore abaixo dele**.

Para isso, usamos uma função auxiliar (igual à `libera` que já vimos na árvore genérica) que libera uma sub-árvore inteira em pós-ordem:

```c
void liberaABB (PABB a)
{
    if (a != NULL) {
        liberaABB(a->esq);
        liberaABB(a->dir);
        free(a);
    }
}

PABB removeSubArvore (PABB a, int x)
{
    if (a == NULL)
        return NULL; // chave não encontrada, nada a fazer

    if (x < a->info) {
        a->esq = removeSubArvore(a->esq, x);
        return a;
    }
    else if (x > a->info) {
        a->dir = removeSubArvore(a->dir, x);
        return a;
    }
    else {
        // achamos o nó: libera ele e toda a sua sub-árvore
        liberaABB(a);
        return NULL;
    }
}
```

</details>

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/19%20%E2%80%A2%20ABB%20-%20Inser%C3%A7%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-ABB_Inserção-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/01%20%E2%80%A2%20Defini%C3%A7%C3%B5es%20Iniciais%20e%20Nota%C3%A7%C3%A3o%20Assint%C3%B3tica/README.md">
    <img src="https://img.shields.io/badge/Avancar-Definições_Iniciais_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
