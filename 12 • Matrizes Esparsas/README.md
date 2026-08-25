# Matrizes Esparsas

> 📚 Material baseado nas aulas da Prof.ª Regina — Algoritmos e Estrutura de Dados

---

## 1. Definição

Uma **matriz esparsa** é uma matriz de grande dimensão (centenas ou milhares de linhas e colunas) em que a **maioria dos elementos vale zero**.

> ➤ Este tipo de matriz surge em diversas aplicações, principalmente na física, na matemática e na economia.
>
> ✓ Também tem aplicação em computação: no **armazenamento de dados**.
>
> ➤ Se tentarmos representar uma matriz deste tamanho pelo modo convencional (`array`), é provável que o compilador não permita a sua declaração e, mesmo que o faça, **desperdiçaremos muita memória**.

### Por que isso é um problema?

Imagine uma matriz `1000 x 1000` em que só existem 20 valores diferentes de zero. Se declararmos um `array` normal `int m[1000][1000]`, estamos reservando espaço para **1.000.000 de inteiros**, sendo que **999.980** deles são zero e não servem para nada. É um desperdício gigantesco de memória.

**A ideia central da matriz esparsa é:** por que guardar os zeros, se eles nem carregam informação relevante? Vamos guardar **só os valores diferentes de zero**.

### Exemplo

```
        ┌                      ┐
        │  6   0   3   0   0  0 │
ME  =   │  0   0   0   2   0  3 │
        │  0   1   0   0   0  0 │
        └                      ┘
```

Se guardarmos apenas o **valor**, perdemos a informação de **onde** ele estava na matriz. Por isso, para cada elemento não nulo, guardamos uma **tripla**: `(linha, coluna, valor)`.

```
vetme = [(1, 1, 6) (1, 3, 3) (2, 4, 2) (2, 6, 3) (3, 2, 1)]
```

> ➤ Note que cada elemento deste vetor é constituído por 3 valores:
> - ✓ a linha do elemento,
> - ✓ a coluna e
> - ✓ o valor armazenado na matriz.
>
> ➤ `vetme` trata-se de um vetor em que cada elemento é uma tupla, e a construção desta tupla pode ser realizada utilizando o tipo `struct`.

---

## 2. Implementação

Em vez de um vetor de tuplas, a forma mais comum (e mais flexível) de implementar matrizes esparsas é usando **listas encadeadas**: guardamos um vetor de ponteiros — um ponteiro **por linha** — e cada ponteiro aponta para uma lista encadeada com os elementos não nulos **daquela linha**.

> ➤ Uma forma de implementar matrizes esparsas utilizando listas encadeadas é guardar apenas uma **lista de linhas** e, no registro, **não armazenar o número da linha** (ele já é dado pelo índice do vetor), ou seja, utilizar um **vetor de ponteiros**.

```c
#define MAX_LINHA 100

typedef struct no *pme;
struct no {
    int col, val;
    pme prox;
};
typedef pme matriz[MAX_LINHA];
```

Repare que cada nó guarda `col` (coluna) e `val` (valor) — **não** guarda a linha, porque a linha já está implícita: é o índice do vetor `matriz`.

Visualmente, a matriz `ME` do exemplo acima ficaria assim:

```
m[0] ──▶ (col:1, val:6) ──▶ (col:3, val:3) ──▶ NULL
m[1] ──▶ (col:4, val:2) ──▶ (col:6, val:3) ──▶ NULL
m[2] ──▶ (col:2, val:1) ──▶ NULL
```

> 💡 Note que os índices aqui começam em 0 (como é padrão em C), enquanto no slide a numeração das linhas/colunas começou em 1 — é só uma questão de convenção, o importante é manter a consistência dentro do seu programa.

### Programa de teste (fornecido em aula)

Este é o `main()` que a professora usa para testar a implementação — ele monta a matriz perguntando valor por valor ao usuário, e só chama `insere` quando o valor digitado é diferente de zero:

```c
int main( ) {
  matriz m;
  int lin, col, i, j, val;
  do{
    printf("Quantidade de linhas (menor que %d: ", MAX_LINHA+1);
    scanf("%d", &lin);
  }while (lin>0 && lin<= MAX_LINHA)
  printf("Quantidade de colunas: ");
  scanf("%d", &col);
  inicializa(m,lin);
  for(i=0; i<lin; i++)
    for(j=0; j<col; j++) {
        printf("m[%d][%d]= ", i, j);
        scanf("%d",&val);
        if(val!=0)  insere(m,i,j,val);
    }
    imprime(m,lin,col);
    libera(m,lin);
    return(0);  }
```

Para esse programa funcionar, faltam justamente as quatro funções (`inicializa`, `insere`, `imprime`, `libera`) — e é exatamente isso que o primeiro exercício pede!

---

## 3. Exercícios

<br>

### Exercício 1 🟢 (fácil)

> Faça um programa para manipulação de matrizes esparsas. Inclua as funções `Inicializa`, `Insere` (considere a inserção no final da lista), `Imprime` e `Libera`.

<details>
<summary>💡 Ver resolução</summary>

**`inicializa`**: simplesmente zera (coloca `NULL`) todos os ponteiros do vetor de linhas — afinal, no começo, nenhuma linha tem elementos.

```c
void inicializa(matriz m, int lin) {
    int i;
    for (i = 0; i < lin; i++)
        m[i] = NULL;
}
```

**`insere`**: cria um novo nó com a coluna e o valor, e o encaixa no **final** da lista daquela linha. Se a lista da linha ainda estiver vazia, o novo nó vira o primeiro.

```c
void insere(matriz m, int i, int j, int val) {
    pme novo = (pme) malloc(sizeof(struct no));
    novo->col = j;
    novo->val = val;
    novo->prox = NULL;

    if (m[i] == NULL) {
        m[i] = novo; // primeiro elemento da linha
    } else {
        pme p = m[i];
        while (p->prox != NULL) p = p->prox; // anda ate o ultimo
        p->prox = novo;
    }
}
```

> 🧠 Como os valores são inseridos digitando coluna por coluna em ordem crescente (olha o `for(j=0; j<col; j++)` do `main`), inserir sempre no final garante que a lista de cada linha fica **automaticamente ordenada por coluna**. Isso vai ser muito útil nos próximos exercícios!

**`imprime`**: para cada linha, percorremos a lista e, para cada coluna de 0 até `col-1`, verificamos se o próximo nó da lista "bate" com aquela coluna. Se sim, imprimimos o valor e avançamos o ponteiro da lista; se não, imprimimos zero.

```c
void imprime(matriz m, int lin, int col) {
    int i, j;
    for (i = 0; i < lin; i++) {
        pme p = m[i];
        for (j = 0; j < col; j++) {
            if (p != NULL && p->col == j) {
                printf("%d ", p->val);
                p = p->prox;
            } else {
                printf("0 ");
            }
        }
        printf("\n");
    }
}
```

**`libera`**: percorre cada linha liberando nó por nó, sempre guardando o próximo antes de dar `free`.

```c
void libera(matriz m, int lin) {
    int i;
    pme p, aux;
    for (i = 0; i < lin; i++) {
        p = m[i];
        while (p != NULL) {
            aux = p;
            p = p->prox;
            free(aux);
        }
        m[i] = NULL;
    }
}
```

</details>

<br>

### Exercício 2 🟡 (médio)

> Escreva uma função que calcule a **transposta** de uma matriz esparsa.

<details>
<summary>💡 Ver resolução</summary>

A transposta troca linha por coluna: o elemento que estava em `(i, j)` vai para `(j, i)`. Como a matriz transposta tem `col` linhas (o número de colunas vira o número de linhas), começamos inicializando o resultado com esse novo tamanho.

```c
void transposta(matriz m, int lin, int col, matriz mt) {
    int i;

    inicializa(mt, col); // a transposta tem "col" linhas

    for (i = 0; i < lin; i++) {
        pme p = m[i];
        while (p != NULL) {
            insere(mt, p->col, i, p->val); // (linha, coluna) viram (coluna, linha)
            p = p->prox;
        }
    }
}
```

> 🧠 **Por que a lista resultante continua ordenada por coluna sem esforço extra?** Porque percorremos `m` linha por linha, em ordem crescente de `i` (0, 1, 2...). Cada vez que inserimos em `mt[p->col]`, estamos inserindo com um valor de "nova coluna" (`i`) sempre maior que o anterior para aquela linha — e como `insere` sempre bota no final, a ordem se mantém automaticamente.

</details>

<br>

### Exercício 3 🟡 (médio)

> Faça uma função que verifique se uma matriz esparsa é **simétrica**.

<details>
<summary>💡 Ver resolução</summary>

Uma matriz é simétrica quando `m[i][j] == m[j][i]` para todos os `i` e `j` (e, claro, ela precisa ser quadrada, ou seja, `lin == col`).

Primeiro criamos uma função auxiliar que busca o valor em uma posição específica `(i, j)` — se não encontrar nenhum nó com aquela coluna, o valor é zero:

```c
int busca_valor(matriz m, int i, int j) {
    pme p = m[i];
    while (p != NULL && p->col < j) p = p->prox;

    if (p != NULL && p->col == j) return p->val;
    return 0; // nao encontrado -> elemento nulo
}
```

Agora, para cada elemento não nulo `(i, j, val)`, verificamos se o elemento espelhado `(j, i)` tem o **mesmo valor**:

```c
int eh_simetrica(matriz m, int lin, int col) {
    int i;

    if (lin != col) return 0; // só faz sentido para matriz quadrada

    for (i = 0; i < lin; i++) {
        pme p = m[i];
        while (p != NULL) {
            if (busca_valor(m, p->col, i) != p->val) return 0;
            p = p->prox;
        }
    }
    return 1; // percorreu tudo e nao achou nenhuma diferenca
}
```

> 🧠 Repare que não precisamos verificar as duas direções manualmente: como o `for` externo passa por **todas** as linhas (inclusive as que fazem o papel de "j" em outra iteração), toda comparação acaba sendo feita nos dois sentidos naturalmente.

</details>

<br>

### Exercício 4 🟢 (fácil)

> Faça uma função que verifique se uma matriz esparsa é **diagonal** (ou seja, tem apenas elementos não nulos na diagonal principal).

<details>
<summary>💡 Ver resolução</summary>

Uma matriz é diagonal quando todo elemento não nulo está na diagonal principal, ou seja: na linha `i`, o **único** valor permitido diferente de zero é o da coluna `i`. Então basta percorrer cada linha e checar se algum nó tem `col` diferente de `i`.

```c
int eh_diagonal(matriz m, int lin) {
    int i;
    for (i = 0; i < lin; i++) {
        pme p = m[i];
        while (p != NULL) {
            if (p->col != i) return 0; // achou elemento fora da diagonal
            p = p->prox;
        }
    }
    return 1;
}
```

</details>

<br>

### Exercício 5 🟡 (médio)

> Faça uma função que verifique se uma matriz esparsa é **triangular inferior** (se só tem elementos diferentes de zero nas posições em que a linha >= coluna).

<details>
<summary>💡 Ver resolução</summary>

Muito parecido com o exercício anterior, mas agora, em vez de exigir `col == i`, exigimos `col <= i` (a coluna precisa estar "abaixo ou sobre" a diagonal principal).

```c
int eh_triangular_inferior(matriz m, int lin) {
    int i;
    for (i = 0; i < lin; i++) {
        pme p = m[i];
        while (p != NULL) {
            if (p->col > i) return 0; // achou elemento ACIMA da diagonal
            p = p->prox;
        }
    }
    return 1;
}
```

</details>

<br>

### Exercício 6 🔴 (difícil)

> Escreva funções que informem a linha e a coluna da matriz esparsa com **maior valor médio**.

<details>
<summary>💡 Ver resolução</summary>

Aqui a "média" de uma linha (ou coluna) é a soma de todos os seus valores dividida pelo **número total de colunas** (ou de linhas) da matriz — lembrando que os elementos que não aparecem na lista valem zero e entram na conta normalmente (só não precisamos somar zero, porque não muda o resultado).

**A dificuldade principal:** nossa estrutura é organizada por **linha**, então somar os valores de uma linha é fácil (é só percorrer a lista dela). Mas somar os valores de uma **coluna** exige percorrer **todas as linhas**, já que os elementos de uma mesma coluna estão espalhados em listas diferentes. Por isso, usamos um vetor auxiliar para acumular as somas de cada coluna **enquanto** percorremos as linhas — assim fazemos tudo em uma única varredura.

```c
void linha_coluna_maior_media(matriz m, int lin, int col, int *linha_max, int *coluna_max) {
    int i;
    float *soma_coluna = (float *) calloc(col, sizeof(float));

    float melhor_media_linha = -1;
    int melhor_linha = -1;

    // Passo 1: percorre linha por linha, calculando a soma da linha
    // e, de quebra, ja vai acumulando a soma de cada coluna
    for (i = 0; i < lin; i++) {
        pme p = m[i];
        float soma_linha = 0;

        while (p != NULL) {
            soma_linha += p->val;
            soma_coluna[p->col] += p->val;
            p = p->prox;
        }

        float media_linha = soma_linha / col;
        if (media_linha > melhor_media_linha) {
            melhor_media_linha = media_linha;
            melhor_linha = i;
        }
    }

    // Passo 2: agora que ja temos a soma de cada coluna, calculamos as medias
    float melhor_media_coluna = -1;
    int melhor_coluna = -1;

    for (i = 0; i < col; i++) {
        float media_coluna = soma_coluna[i] / lin;
        if (media_coluna > melhor_media_coluna) {
            melhor_media_coluna = media_coluna;
            melhor_coluna = i;
        }
    }

    free(soma_coluna);
    *linha_max = melhor_linha;
    *coluna_max = melhor_coluna;
}
```

Uso da função (ela retorna dois valores, por isso os parâmetros são ponteiros):

```c
int lmax, cmax;
linha_coluna_maior_media(m, lin, col, &lmax, &cmax);
printf("Linha com maior media: %d\n", lmax);
printf("Coluna com maior media: %d\n", cmax);
```

> 🧠 **Por que usar `calloc` no vetor de somas?** Porque `calloc` já inicializa toda a memória com zero — exatamente o que precisamos, já que vamos **somar** valores nesse vetor a partir de zero.

</details>

<br>

### Exercício 7 🔴 (difícil)

> Tente implementar matrizes esparsas utilizando **lista de lista**.

<details>
<summary>💡 Ver resolução</summary>

Até aqui, usamos um **vetor** de ponteiros (`matriz[MAX_LINHA]`) para representar as linhas — o que ainda tem uma limitação: precisamos definir `MAX_LINHA` de antemão, e se a matriz tiver poucas linhas, ainda estamos "desperdiçando" um pouco de espaço com o vetor.

A ideia de **lista de lista** é: em vez de um vetor de linhas, usamos **outra lista encadeada** para representar as linhas! Assim, só existe um nó de linha para linhas que **realmente têm** algum elemento não nulo — eliminando completamente o `MAX_LINHA` e qualquer desperdício.

```c
// Lista de colunas (elementos de uma linha) - igual a antes
typedef struct nocol *pcol;
struct nocol {
    int col, val;
    pcol prox;
};

// Lista de linhas - CADA NÓ representa uma linha com elementos
typedef struct nolinha *plinha;
struct nolinha {
    int linha;
    pcol colunas; // lista encadeada com os elementos nao nulos dessa linha
    plinha prox;
};
```

Visualmente, a mesma matriz de exemplo ficaria assim (repare que **não existe mais um vetor fixo** — é tudo lista, do começo ao fim):

```
m ──▶ [linha 0] ──▶ [linha 1] ──▶ [linha 2] ──▶ NULL
         │               │               │
         ▼               ▼               ▼
      (1,6)──(3,3)    (4,2)──(6,3)     (2,1)
```

**`insere`**: agora precisa primeiro **encontrar (ou criar) o nó da linha** `i` dentro da lista de linhas, para só depois inserir o elemento na lista de colunas daquela linha. Vamos manter as linhas em ordem crescente para facilitar buscas futuras.

```c
plinha insere(plinha m, int i, int j, int val) {
    plinha pl = m, ant = NULL;

    // Procura a linha i (ou o lugar onde ela deveria estar)
    while (pl != NULL && pl->linha < i) {
        ant = pl;
        pl = pl->prox;
    }

    // Se a linha i ainda nao existe, cria um novo no de linha
    if (pl == NULL || pl->linha != i) {
        plinha nova_linha = (plinha) malloc(sizeof(struct nolinha));
        nova_linha->linha = i;
        nova_linha->colunas = NULL;
        nova_linha->prox = pl;

        if (ant == NULL) m = nova_linha;       // nova linha é a primeira
        else ant->prox = nova_linha;

        pl = nova_linha;
    }

    // Agora insere o elemento (j, val) no final da lista de colunas da linha pl
    pcol novo = (pcol) malloc(sizeof(struct nocol));
    novo->col = j;
    novo->val = val;
    novo->prox = NULL;

    if (pl->colunas == NULL) {
        pl->colunas = novo;
    } else {
        pcol pc = pl->colunas;
        while (pc->prox != NULL) pc = pc->prox;
        pc->prox = novo;
    }

    return m; // retorna, pois "m" pode ter mudado (nova linha no inicio)
}
```

**`imprime`**: agora precisamos, para cada linha de `0` a `lin-1`, primeiro checar se ela **existe** na lista de linhas antes de tentar imprimir seus elementos.

```c
void imprime(plinha m, int lin, int col) {
    int i, j;
    plinha pl = m;

    for (i = 0; i < lin; i++) {
        // Anda na lista de linhas ate achar a linha i (se existir)
        while (pl != NULL && pl->linha < i) pl = pl->prox;

        pcol pc = (pl != NULL && pl->linha == i) ? pl->colunas : NULL;

        for (j = 0; j < col; j++) {
            if (pc != NULL && pc->col == j) {
                printf("%d ", pc->val);
                pc = pc->prox;
            } else {
                printf("0 ");
            }
        }
        printf("\n");
    }
}
```

**`libera`**: agora precisamos liberar em **dois níveis** — para cada nó de linha, liberamos toda a lista de colunas, e só depois liberamos o próprio nó de linha.

```c
plinha libera(plinha m) {
    plinha pl = m, auxl;
    pcol pc, auxc;

    while (pl != NULL) {
        pc = pl->colunas;
        while (pc != NULL) { // libera a lista de colunas dessa linha
            auxc = pc;
            pc = pc->prox;
            free(auxc);
        }
        auxl = pl;
        pl = pl->prox;
        free(auxl); // libera o no da linha
    }

    return NULL;
}
```

> 🧠 **Vantagem sobre a versão com vetor:** não precisamos mais de `MAX_LINHA`, e linhas totalmente vazias **nem existem** na estrutura — usamos memória só para o que de fato tem conteúdo, em qualquer uma das duas dimensões (linha e coluna). A desvantagem é que buscar uma linha específica deixa de ser O(1) (acesso direto por índice do vetor) e passa a ser O(número de linhas não vazias), pois agora também é uma lista.

</details>

---

## 4. Resumo rápido

| Conceito | Explicação |
|---|---|
| Matriz esparsa | Matriz grande com a maioria dos elementos igual a zero |
| Ideia central | Guardar só os elementos **não nulos**, junto com sua posição |
| Representação com vetor de listas | `matriz[MAX_LINHA]`, cada posição do vetor é uma linha (lista de `col, val`) |
| Representação com lista de listas | Uma lista de "linhas", cada uma apontando para uma lista de "colunas" — sem limite fixo de tamanho |
| Vantagem | Economia enorme de memória quando há muitos zeros |
| Desvantagem | Acesso a um elemento específico deixa de ser O(1) (como seria num array 2D) e passa a depender do tamanho das listas |
