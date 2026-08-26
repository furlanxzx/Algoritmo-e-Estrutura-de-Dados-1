# Árvore Genérica

> Material de estudos de Algoritmos e Estruturas de Dados I, baseado nas aulas da Prof. Regina Célia Coelho.
---

## Introdução

Como vimos, numa **árvore binária** o número de filhos de um nó é limitado em no máximo dois.

No caso da **árvore genérica**, essa restrição não existe:

> ✅ Cada nó pode ter um número arbitrário de filhos.

Essa estrutura pode ser usada, por exemplo, para representar uma **árvore de diretórios**.

### Definição recursiva

As funções para manipular uma árvore genérica também serão implementadas de forma **recursiva**, e são baseadas na seguinte definição:

> Uma árvore genérica é composta por:
> - um **nó raiz**;
> - **zero ou mais sub-árvores**.

Em qualquer definição recursiva deve haver uma "condição de contorno", que permite a definição de estruturas finitas. No nosso caso, a definição de uma árvore se encerra nas **folhas**, que são identificadas como sendo nós com **zero sub-árvores**.

---

## Estrutura para Árvore Genérica

A árvore genérica utiliza uma **"lista de filhos"**: um nó aponta apenas para seu primeiro filho (`prim`), e cada um de seus filhos, exceto o último, aponta para o próximo irmão (`prox`).

A declaração de um nó é:

```c
typedef struct arvGen {
    char info;
    struct arvgen *prim;  // lista de filhos
    struct arvgen *prox;  // lista de irmãos
}TArvGen;

typedef TArvGen *PArvGen;
```

---

## Função Cria

A função para criar uma folha deve alocar o nó e inicializar seus campos, atribuindo `NULL` para os campos `prim` e `prox`, pois trata-se de um nó folha.

```c
PArvGen cria (char c)
{
    PArvGen a = (PArvGen) malloc(sizeof(TArvGen));
    a->info = c;
    a->prim = NULL;
    a->prox = NULL;
    return a;
}
```

---

## Função Insere

Como não vamos atribuir nenhum significado especial para a posição de um nó filho, a operação de inserção pode inserir a sub-árvore em qualquer posição.

Neste caso, vamos optar por inserir **sempre no início da lista** que, como já vimos, é a maneira mais simples de inserir um novo elemento numa lista encadeada.

Vamos inserir a subárvore `sa` na árvore `a`, ou seja, `sa` passa a ser o início da lista de filhos de `a`, portanto será inserido à **esquerda** de `a`, já que à direita deve estar a lista de irmãos de `a`.

```c
void insere (PArvGen a, PArvGen sa) {
    sa->prox = a->prim;
    a->prim = sa;
}
```

---

## Função Imprime

Para imprimir as informações associadas aos nós da árvore, temos duas opções para percorrer a árvore:

- **pré-ordem**: primeiro a raiz e depois as sub-árvores; ou
- **pós-ordem**: primeiro as sub-árvores e depois a raiz.

Note que neste caso não faz sentido a ordem infixa, uma vez que o número de sub-árvores é variável. Para essa função, vamos optar por imprimir o conteúdo dos nós em **pré-ordem**.

```c
void imprime (PArvGen a)
{
    PArvGen p;
    printf("%c ", a->info);
    imprime (a->prim);
    imprime (a->prox);
}
```

---

## Função Libera

O único cuidado que precisamos tomar na programação dessa função é a de liberar as sub-árvores antes de liberar o espaço associado a um nó (isto é, usar **pós-ordem**).

```c
void libera (PArvGen a)
{
    if (a != NULL) {
        libera(a->prim);
        libera(a->prox);
        free(a);
    }
}
```

---

## Exercícios

### Exercício 1 🟢

Faça uma função que verifica a ocorrência de uma dada informação na árvore (se existe uma dada informação na árvore).

<details>
<summary>Ver resposta</summary>

A ideia é a mesma da função `imprime`, mas ao invés de imprimir, comparamos o `info` do nó atual com o valor buscado. Se não achar no nó atual, buscamos recursivamente na lista de filhos (`prim`) e na lista de irmãos (`prox`).

```c
int existe (PArvGen a, char c)
{
    if (a == NULL)
        return 0;

    if (a->info == c)
        return 1;

    if (existe(a->prim, c))
        return 1;

    return existe(a->prox, c);
}
```

</details>

---

### Exercício 2 🔴

Faça uma função com o protótipo a seguir para testar se duas árvores são iguais.

```c
int igual(PArvGen a, PArvGen b);
```

<details>
<summary>Ver resposta</summary>

Duas árvores são iguais se:
1. Ambas forem `NULL` (caso base, ok); ou
2. Ambas existirem, tiverem o **mesmo `info`**, **a mesma lista de filhos** (`prim`) e **a mesma lista de irmãos** (`prox`), comparadas recursivamente.

Se apenas uma das duas for `NULL`, as árvores já são diferentes.

```c
int igual (PArvGen a, PArvGen b)
{
    if (a == NULL && b == NULL)
        return 1;

    if (a == NULL || b == NULL)
        return 0;

    if (a->info != b->info)
        return 0;

    return igual(a->prim, b->prim) && igual(a->prox, b->prox);
}
```

**Por que comparar `prim` e `prox` ao mesmo tempo?**
Porque a estrutura de "lista de filhos" transforma a comparação de duas árvores genéricas numa comparação de duas listas encadeadas de sub-árvores. Ao descer por `prim` comparamos os filhos nível a nível, e ao descer por `prox` comparamos os irmãos um a um — se ambas as recursões baterem, a árvore inteira é igual.

</details>

---

### Exercício 3 🟡

Faça uma função com o protótipo a seguir para criar dinamicamente uma cópia da árvore.

```c
PArvGen copia(PArvGen a);
```

<details>
<summary>Ver resposta</summary>

Usamos a própria função `cria` para alocar cada nó novo, copiando o `info`, e chamamos `copia` recursivamente para `prim` e `prox`.

```c
PArvGen copia (PArvGen a)
{
    if (a == NULL)
        return NULL;

    PArvGen novo = cria(a->info);
    novo->prim = copia(a->prim);
    novo->prox = copia(a->prox);

    return novo;
}
```

</details>

---

### Exercício 4 🔴

Faça uma função que insira um valor em uma árvore genérica. Este valor deverá ser passado como parâmetro da função, assim como o valor de seu pai.

<details>
<summary>Ver resposta</summary>

Como recebemos apenas o **valor** do pai (e não um ponteiro para ele), primeiro precisamos **buscar** o nó cujo `info` é igual ao valor do pai — uma função auxiliar parecida com o `existe` do Exercício 1, mas que retorna o **ponteiro** para o nó em vez de `0`/`1`. Encontrado o nó pai, usamos a função `insere` já pronta para colocar o novo valor como filho dele.

```c
PArvGen busca (PArvGen a, char c)
{
    PArvGen p;

    if (a == NULL)
        return NULL;

    if (a->info == c)
        return a;

    p = busca(a->prim, c);
    if (p != NULL)
        return p;

    return busca(a->prox, c);
}

void insereValor (PArvGen a, char valor, char pai)
{
    PArvGen noPai = busca(a, pai);

    if (noPai != NULL)
        insere(noPai, cria(valor));
}
```

⚠️ Se o `pai` informado não existir na árvore, a função simplesmente não insere nada (`noPai == NULL`).

</details>

---

### Exercício 5

Escreva as funções pedidas em cada item (obedeça os protótipos):

> ℹ️ **Nota:** para os itens `soma` e `retornaConteudo` fazerem sentido, vamos considerar que o campo `info` da struct é do tipo `int` neste exercício (em vez de `char`), mantendo a mesma estrutura com `prim`/`prox`.

#### a) calcule a soma do conteúdo de todos os nós — `int soma(PArvGen a)` 🟢

<details>
<summary>Ver resposta</summary>

```c
int soma (PArvGen a)
{
    if (a == NULL)
        return 0;

    return a->info + soma(a->prim) + soma(a->prox);
}
```

</details>

#### b) calcule a quantidade total de nós na árvore — `int num_nos(PArvGen a)` 🟢

<details>
<summary>Ver resposta</summary>

```c
int num_nos (PArvGen a)
{
    if (a == NULL)
        return 0;

    return 1 + num_nos(a->prim) + num_nos(a->prox);
}
```

</details>

#### c) retorna o primeiro filho de a — `PArvGen retornaPrim(PArvGen a)` 🟢

<details>
<summary>Ver resposta</summary>

```c
PArvGen retornaPrim (PArvGen a)
{
    return a->prim;
}
```

</details>

#### d) retorna o irmão de a — `PArvGen retornaProx(PArvGen a)` 🟢

<details>
<summary>Ver resposta</summary>

```c
PArvGen retornaProx (PArvGen a)
{
    return a->prox;
}
```

</details>

#### e) retorna o conteúdo de a — `int retornaConteudo(PArvGen a)` 🟢

<details>
<summary>Ver resposta</summary>

```c
int retornaConteudo (PArvGen a)
{
    return a->info;
}
```

</details>

#### f) insere sa como filho de a — `void insere(PArvGen a, PArvGen sa)` 🟢

<details>
<summary>Ver resposta</summary>

Já vimos essa função em detalhes na seção [Função Insere](#função-insere) — é a mesma implementação:

```c
void insere (PArvGen a, PArvGen sa)
{
    sa->prox = a->prim;
    a->prim = sa;
}
```

</details>

---

### Exercício 6 🔴

Faça um programa para a manipulação de árvores genéricas que leia um arquivo texto contendo uma sequência de nomes e coloque os nomes em diferentes níveis da árvore de acordo com a inicial do nome. Exibir na tela a lista de nomes, agrupados de acordo com a letra inicial do nome.

Exemplo de saída:

```
A
   Abel  Ana Anibal
C
   Carla  Carlo Cicero
M
   Maria Mario Moacir
P
   Paula Paulo Pedro
```

<details>
<summary>Ver resposta (lógica da árvore — leitura de arquivo simplificada)</summary>

**Ideia geral:** a raiz é um nó "fictício" (ou vazio) cujos filhos diretos (`prim`/`prox`) são as **letras** (A, C, M, P...). Cada nó-letra, por sua vez, tem como filhos os **nomes** que começam com aquela letra. Ou seja, temos uma árvore de **3 níveis**: raiz → letras → nomes.

Para cada nome lido do arquivo:
1. Pegamos a primeira letra do nome.
2. Buscamos, entre os filhos da raiz, se já existe um nó com aquela letra (`busca`, do Exercício 4).
3. Se não existir, criamos o nó-letra e inserimos como filho da raiz.
4. Inserimos o nome como filho do nó-letra encontrado/criado.

```c
typedef struct arvGen {
    char info[30];          // aqui info guarda uma string (letra ou nome)
    struct arvgen *prim;
    struct arvgen *prox;
}TArvGen;

typedef TArvGen *PArvGen;

// Busca entre os FILHOS de a um nó cujo info é igual a c
// (usada para localizar o nó-letra já existente)
PArvGen buscaFilho (PArvGen a, char c)
{
    PArvGen p = a->prim;

    while (p != NULL) {
        if (p->info[0] == c)
            return p;
        p = p->prox;
    }
    return NULL;
}

void insereNome (PArvGen raiz, char *nome)
{
    PArvGen noLetra = buscaFilho(raiz, nome[0]);

    if (noLetra == NULL) {
        char letra[2] = {nome[0], '\0'};
        noLetra = cria(letra);
        insere(raiz, noLetra);
    }

    insere(noLetra, cria(nome));
}
```

Depois de montada a árvore (raiz → letras → nomes), basta percorrer: para cada filho da raiz, imprime a letra (`info`) e depois percorre a lista de filhos daquele nó imprimindo os nomes — sem precisar de recursão até as folhas, já que a árvore tem profundidade fixa de 3 níveis.

A leitura do arquivo (`fopen`, `fscanf`/`fgets` em loop até o fim do arquivo, chamando `insereNome` para cada nome lido) segue o padrão básico de leitura de arquivo texto em C.

</details>

---

### Exercício 7 🔴

Faça um programa para a manipulação de árvores genéricas que leia um arquivo texto com uma sequência de linhas compostas de um identificador e um dado, e gere uma árvore genérica em que o identificador de cada linha indica onde essa linha será inserida na hierarquia da árvore. A seguir, gere um arquivo texto, percorrendo a árvore em modo prefixo.

Exemplo:

```
1-   Nome
1.1- Sexo
1.2- Idade
1.3- Nacionalidade
1.3.1- Local_de_Nascimento
1.3.2- Estado
1.3.3- CEP
1.4- Profissão
1.5- Estado_Civil
1.6- Carteira_de_Identidade
1.7- CPF
1.4.1- Salario
1.4.2- Cargo
```

<details>
<summary>Ver resposta (lógica da árvore — leitura de arquivo simplificada)</summary>

**Ideia geral:** o identificador (`1`, `1.1`, `1.3.2`...) representa o **caminho** do nó dentro da árvore, onde cada "nível" separado por `.` indica uma descida na hierarquia. Por exemplo, `1.3.2` é: filho de `1.3`, que é filho de `1`.

Para inserir uma linha, o algoritmo é:
1. "Remove" o último nível do identificador para achar o identificador do **pai** (ex: de `1.3.2` obtemos `1.3`).
2. Busca o nó **pai** na árvore pelo identificador dele (mesma ideia do `busca` do Exercício 4, mas comparando o identificador, não o dado).
3. Insere o novo nó (com o dado, ex: `Estado`) como filho do nó pai encontrado, usando a função `insere` de sempre.
4. O único caso especial é a raiz (`1`), que não tem pai — é criada isoladamente, sem chamar `insere`.

```c
typedef struct arvGen {
    char id[15];      // ex: "1.3.2"
    char dado[50];     // ex: "Estado"
    struct arvgen *prim;
    struct arvgen *prox;
}TArvGen;

typedef TArvGen *PArvGen;

PArvGen cria (char *id, char *dado)
{
    PArvGen a = (PArvGen) malloc(sizeof(TArvGen));
    strcpy(a->id, id);
    strcpy(a->dado, dado);
    a->prim = NULL;
    a->prox = NULL;
    return a;
}

void insere (PArvGen a, PArvGen sa)
{
    sa->prox = a->prim;
    a->prim = sa;
}

// Busca na árvore o nó cujo identificador é igual a id
PArvGen busca (PArvGen a, char *id)
{
    PArvGen p;

    if (a == NULL)
        return NULL;

    if (strcmp(a->id, id) == 0)
        return a;

    p = busca(a->prim, id);
    if (p != NULL)
        return p;

    return busca(a->prox, id);
}

// Retorna o identificador do pai, removendo o último nível
// ex: "1.3.2" -> "1.3"    |    "1" -> "" (é raiz)
void idPai (char *id, char *idPaiSaida)
{
    strcpy(idPaiSaida, id);
    char *ultimoPonto = strrchr(idPaiSaida, '.');
    if (ultimoPonto != NULL)
        *ultimoPonto = '\0';
    else
        idPaiSaida[0] = '\0';
}
```

Com essas peças, a inserção de cada linha do arquivo fica:

```c
PArvGen insereLinha (PArvGen raiz, char *id, char *dado)
{
    PArvGen novo = cria(id, dado);

    if (raiz == NULL)          // primeira linha (raiz da árvore)
        return novo;

    char pai[15];
    idPai(id, pai);

    PArvGen noPai = busca(raiz, pai);
    if (noPai != NULL)
        insere(noPai, novo);

    return raiz;
}
```

Por fim, para gerar o arquivo de saída em **modo prefixo**, basta adaptar a função `imprime` para escrever no arquivo em vez de imprimir na tela (primeiro o nó atual, depois `prim`, depois `prox`), respeitando a ordem pré-fixa pedida.

A leitura do arquivo de entrada (separar cada linha em `id` e `dado`, chamando `insereLinha` para cada uma) segue o padrão básico de leitura de arquivo texto em C.

</details>
