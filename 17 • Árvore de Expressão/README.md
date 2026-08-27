<h1 align="center">Árvore de Expressão</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/16%20%E2%80%A2%20Percurso%20em%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Percurso_em_Árvores_Binárias-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/18%20%E2%80%A2%20%C3%81rvores%20Gen%C3%A9ricas/README.md">
    <img src="https://img.shields.io/badge/Avancar-Árvores_Genéricas_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## 1. Introdução

> ➤ Como vimos, numa árvore binária o número de filhos dos nós é limitado em no máximo dois.
>
> ➤ No caso da árvore genérica, esta restrição não existe.
> - ✓ Cada nó pode ter um número arbitrário de filhos.
>
> ➤ Essa estrutura pode ser usada, por exemplo, para representar uma **árvore de diretórios**.

Faz todo sentido: uma pasta no seu computador pode ter 0, 1, 5, 50 subpastas — não existe limite de 2. É exatamente esse tipo de situação que a árvore genérica resolve.

> ➤ Como veremos, as funções para manipular uma árvore genérica também serão implementadas de forma **recursiva**, e são baseadas na seguinte definição:
> - ✓ uma árvore genérica é composta por:
>   - um **nó raiz**;
>   - **zero ou mais sub-árvores**.
>
> ➤ Em qualquer definição recursiva deve haver uma **"condição de contorno"**, que permita a definição de estruturas finitas, e, no nosso caso, a definição de uma árvore se encerra nas **folhas**, que são identificadas como sendo nós com **zero sub-árvores**.

---

## 2. Estrutura para Árvore Genérica

Aqui está o ponto mais interessante desse conteúdo. Se cada nó pode ter uma quantidade **qualquer** de filhos, como representamos isso com uma struct de tamanho fixo?

> ➤ Árvore genérica utiliza uma **"lista de filhos"**: um nó aponta apenas para seu **primeiro filho** (`prim`), e cada um de seus filhos, exceto o último, aponta para o **próximo irmão** (`prox`). A declaração de um nó pode ser:

```c
typedef struct arvGen {
   char info;
   struct arvgen *prim;  //lista de filhos
   struct arvgen *prox;  //lista de irmãos
} TArvGen;

typedef TArvGen *PArvGen;
```

Essa técnica é conhecida como **"primeiro filho / próximo irmão"** (*first child, next sibling*), e é a forma mais elegante de representar uma árvore com número variável de filhos usando apenas **dois ponteiros por nó** — a mesma quantidade que usamos na árvore binária!

**A ideia central:** em vez de um nó apontar diretamente para *todos* os seus filhos, ele aponta só para o **primeiro** filho. Os demais filhos formam uma **lista encadeada entre si**, ligados pelo campo `prox` (que, nesse contexto, significa "próximo irmão", não "próximo nó da árvore").

Visualmente, uma árvore onde A tem 3 filhos (B, C, D) fica assim:

```
                    A
                    │ prim
                    ▼
                    B ──prox──▶ C ──prox──▶ D ──prox──▶ NULL
                    │ prim      │ prim      │ prim
                    ▼           ▼           ▼
                   NULL        NULL        NULL
```

Ou seja: `A->prim` aponta para B. `B->prox` aponta para C (o irmão seguinte). `C->prox` aponta para D. E `D->prox` é `NULL`, porque D é o último filho. Nenhum desses três (B, C, D) tem filho próprio, então o `prim` de cada um é `NULL`.

> 💡 **Por que isso é tão engenhoso?** Porque transforma um problema de "grau variável" (um nó pode ter qualquer quantidade de filhos) em um problema que já sabemos resolver muito bem: uma **lista encadeada** de irmãos, e uma árvore **binária-like** de "desce" (`prim`) e "lado" (`prox`). Isso vai ficar ainda mais claro na seção 6, onde mostramos que essa estrutura é, na prática, **idêntica** a uma árvore binária.

---

## 3. Função Cria

> ➤ A função para criar uma folha deve alocar o nó e inicializar seus campos, atribuindo `NULL` para os campos `prim` e `prox`, pois trata-se de um nó folha.

```c
PArvGen cria (char c)
{
   PArvGen a =(PArvGen) malloc(sizeof(TArvGen));
   a->info = c;
   a->prim = NULL;
   a->prox = NULL;
   return a;
}
```

Repare que `cria` sempre gera um nó **isolado** (sem filhos e sem irmãos) — é depois, usando a função `insere`, que vamos conectando esses nós entre si para montar a árvore completa.

---

## 4. Função Insere

> ➤ Como não vamos atribuir nenhum significado especial para a posição de um nó filho, a operação de inserção pode inserir a sub-árvore em **qualquer posição**.
>
> ➤ Neste caso, vamos optar por inserir sempre no **início da lista** que, como já vimos, é a maneira mais simples de inserir um novo elemento numa lista encadeada.

> ➤ Vamos inserir a subárvore `sa` na árvore `a`, ou seja, `sa` é o início da lista de filhos de `a`, portanto será inserido à esquerda de `a`, já que à direita deve estar a lista de irmãos de `a`.

```c
void insere (PArvGen a, PArvGen sa) {
   sa->prox = a->prim;
   a->prim = sa;
}
```

**Explicando:** antes de inserir, `a->prim` apontava para o antigo primeiro filho de `a` (ou `NULL`, se `a` não tinha filhos ainda). A função faz `sa` "assumir o lugar" desse primeiro filho: primeiro liga o `prox` de `sa` para quem *era* o primeiro filho (`sa->prox = a->prim`), e só depois atualiza `a->prim` para apontar para `sa`. É exatamente o mesmo padrão de "inserção no início" que já vimos em listas encadeadas simples!

```
Antes:      a ──prim──▶ B ──prox──▶ C

insere(a, sa):

Depois:     a ──prim──▶ sa ──prox──▶ B ──prox──▶ C
```

---

## 5. Função Imprime

> ➤ Para imprimir as informações associadas aos nós da árvore, temos duas opções para percorrer a árvore:
> - ✓ **pré-ordem**, primeiro a raiz e depois as sub-árvores, ou
> - ✓ **pós-ordem**, primeiro as sub-árvores e depois a raiz.
>
> ➤ Note que neste caso **não faz sentido a ordem infixa**, uma vez que o número de sub-árvores é variável. Para essa função, vamos optar por imprimir o conteúdo dos nós em pré-ordem.

```c
void imprime (PArvGen a)
{
   PArvGen p;
   printf("%c ", a->info);
   imprime (a->prim);
   imprime(a->prox);
}
```

> ⚠️ **Atenção — esse código como está no slide tem um problema:** ele não verifica se `a` é `NULL` antes de acessar `a->info`. Como toda folha e todo último irmão têm `prim`/`prox` valendo `NULL`, essa função vai tentar fazer `a->info` com `a = NULL` assim que chegar numa dessas pontas — e isso trava o programa (erro de segmentação). A versão correta, com o **caso base** tratado, é:

```c
void imprime (PArvGen a)
{
   if (a != NULL) {
      printf("%c ", a->info);
      imprime(a->prim);
      imprime(a->prox);
   }
}
```

> 🧠 **Por que essa função consegue imprimir a árvore inteira, mesmo só "descendo" e "andando pro lado"?** Porque, ao imprimir `a`, ela primeiro imprime **toda a sub-árvore de `a->prim`** (que, por sua vez, imprime a si mesma, seus próprios filhos, e depois — através do `prox` — todos os seus irmãos e as sub-árvores deles). É uma recursão dupla que cobre "desce" e "vai pro lado" ao mesmo tempo. Esse padrão (`trata(a); percorre(a->prim); percorre(a->prox);`) vai se repetir em praticamente **todas** as funções que faremos daqui pra frente.

---

## 6. Conversão em Árvore Binária

Essa é uma observação muito bonita da estrutura que acabamos de montar:

> ➤ Note que as raízes das árvores binárias resultantes não possuem filhos à direita.
>
> ➤ Isto deve-se ao fato da raiz da árvore transformada não possuir nenhum irmão.
>
> ➤ Este fato pode ser utilizado para transformar **florestas** numa única árvore binária.
>
> ➤ Para isto, transforma-se inicialmente cada árvore da floresta em árvore binária e, depois, une-se estas árvores binárias por meio dos ponteiros direitos dos nós-raízes.

Repare que a struct que criamos (`prim`, `prox`) é **estruturalmente idêntica** à struct de árvore binária que vimos no módulo anterior (`esq`, `dir`) — só trocamos os nomes dos campos! Isso significa que **toda árvore genérica pode ser enxergada como uma árvore binária**, bastando reinterpretar:

- `prim` (primeiro filho) ↔ `esq` (filho esquerdo)
- `prox` (próximo irmão) ↔ `dir` (filho direito)

```
Árvore genérica (A tem filhos B, C, D):

        A
      ┌─┼─┐
      B C D

Reinterpretada como árvore binária (via prim/prox):

        A
       /
      B
       \
        C
         \
          D
```

Como a **raiz** de uma árvore não tem irmãos (não existe "irmão da raiz"), o campo `prox` da raiz é sempre `NULL` — e é exatamente por isso que, na representação binária, **a raiz nunca tem filho à direita**.

> 💡 **Aplicação prática:** e se tivermos uma **floresta** (várias árvores separadas, sem uma raiz em comum)? Como cada raiz convertida "sobra" com o ponteiro direito livre (`NULL`), podemos usar exatamente esse espaço livre para **encadear as raízes entre si**! Ou seja: a raiz da primeira árvore aponta, pelo seu `dir`, para a raiz da segunda árvore, que aponta para a raiz da terceira, e assim por diante — transformando várias árvores separadas em **uma única estrutura binária**, sem perder nenhuma informação.

---

## 7. Exercícios

<br>

### Exercício 1 🟢 (fácil)

> Faça uma função que verifica a ocorrência de uma dada informação na árvore (se existe uma dada informação na árvore).

<details>
<summary>💡 Ver resolução</summary>

Seguindo o mesmo padrão da `imprime`: tratamos o caso base (`NULL` → não encontrado), depois checamos o nó atual, e se não for ele, buscamos recursivamente tanto nos filhos (`prim`) quanto nos irmãos (`prox`).

```c
int busca(PArvGen a, char c) {
    if (a == NULL) return 0; // não encontrado

    if (a->info == c) return 1; // achou

    return busca(a->prim, c) || busca(a->prox, c);
}
```

> 🧠 Por que buscar em `a->prim` **e** em `a->prox`? Porque, para não deixar nenhum nó da árvore de fora, a busca precisa "descer" (olhar os filhos de `a`, começando por `a->prim`) **e** "andar pro lado" (olhar os próximos irmãos de `a`, via `a->prox`). O operador `||` garante que a busca para assim que encontrar o valor em qualquer um dos dois lados.

</details>

<br>

### Exercício 2 🟡 (médio)

> Faça uma função com o protótipo a seguir para testar se duas árvores são iguais.
>
> ```c
> int igual(PArvGen a, PArvGen b);
> ```

<details>
<summary>💡 Ver resolução</summary>

Duas árvores são iguais quando: **ambas são vazias**, ou **ambas têm a mesma informação na raiz, a mesma sub-árvore de filhos (`prim`), e a mesma lista de irmãos (`prox`)**.

```c
int igual(PArvGen a, PArvGen b) {
    if (a == NULL && b == NULL) return 1;   // ambas vazias -> iguais
    if (a == NULL || b == NULL) return 0;   // só uma é vazia -> diferentes

    if (a->info != b->info) return 0;       // conteúdo diferente

    return igual(a->prim, b->prim) && igual(a->prox, b->prox);
}
```

> 🧠 Repare que comparar `a->prox` com `b->prox` não é "só" comparar o próximo irmão — como a comparação é recursiva, isso acaba verificando **toda a lista de irmãos restante** dos dois lados, de uma vez só. O mesmo vale para `a->prim`: comparamos não só o primeiro filho, mas (recursivamente) **toda a sub-árvore de filhos**.

</details>

<br>

### Exercício 3 🟡 (médio)

> Faça uma função com o protótipo a seguir para criar dinamicamente uma cópia da árvore.
>
> ```c
> PArvGen copia(PArvGen a);
> ```

<details>
<summary>💡 Ver resolução</summary>

A ideia é criar um novo nó com o mesmo conteúdo do nó atual, e então **copiar recursivamente** tanto a sub-árvore de filhos quanto a lista de irmãos, conectando as cópias entre si.

```c
PArvGen copia(PArvGen a) {
    if (a == NULL) return NULL;

    PArvGen novo = cria(a->info);
    novo->prim = copia(a->prim);
    novo->prox = copia(a->prox);

    return novo;
}
```

> 🧠 Essa função reaproveita a `cria` que já tínhamos pronta — só que, em vez de deixar `prim` e `prox` como `NULL` (que é o que `cria` faz por padrão), sobrescrevemos os dois logo em seguida com as cópias das respectivas sub-árvores. É um padrão muito comum: usar uma função "base" já existente, e complementar o que ela não faz.

</details>

<br>

### Exercício 4 🔴 (difícil)

> Faça uma função que insira um valor em uma árvore genérica. Este valor deverá ser passado como parâmetro da função, assim como o valor de seu pai.

<details>
<summary>💡 Ver resolução</summary>

Esse exercício tem duas partes escondidas: primeiro precisamos **encontrar o nó pai** (dado seu valor), e só depois **inserir o novo nó** como filho dele — reaproveitando a função `insere` que já temos pronta.

**Passo 1 — função auxiliar para localizar um nó pelo valor** (parecida com a `busca` do Exercício 1, mas retornando o **ponteiro** para o nó, em vez de só dizer se existe):

```c
PArvGen busca_no(PArvGen a, char valor) {
    if (a == NULL) return NULL;

    if (a->info == valor) return a;

    PArvGen achou = busca_no(a->prim, valor);
    if (achou != NULL) return achou;

    return busca_no(a->prox, valor);
}
```

**Passo 2 — função que insere o novo valor como filho do nó pai encontrado:**

```c
void insere_valor(PArvGen raiz, char valorPai, char novoValor) {
    PArvGen pai = busca_no(raiz, valorPai);

    if (pai == NULL) {
        printf("Erro: no pai '%c' nao encontrado na arvore.\n", valorPai);
        return;
    }

    PArvGen novo = cria(novoValor);
    insere(pai, novo); // reaproveita a funcao insere que ja tinhamos
}
```

> 🧠 Esse é um ótimo exemplo de **composição de funções**: em vez de reescrever tudo do zero, quebramos o problema em "achar o pai" (`busca_no`) + "inserir nele" (`insere`, que já existia). Isso deixa o código mais curto, mais fácil de entender e de testar cada parte separadamente.

</details>

<br>

### Exercício 5 🟡 (médio)

> Escrevas as funções pedidas em cada item (obedeça os protótipos):
> - calcule a soma do conteúdo de todos os nós: `int soma(PArvGen a)`
> - calcule a quantidade total de nós na árvore: `int num_nos(PArvGen a)`
> - retorna o primeiro filho de a: `PArvGen retornaPrim (PArvGen a)`
> - retorna o irmão de a: `PArvGen retornaProx (PArvGen a)`
> - retorna o conteúdo de a: `int retornaConteudo (PArvGen a)`
> - insere sa como filho de a: `void insere (PArvGen a, PArvGen sa);`

<details>
<summary>💡 Ver resolução</summary>

As duas últimas (`retornaConteudo` e `insere`) são só "encapsulamentos" simples de algo que já vimos. Já `soma` e `num_nos` seguem o mesmo padrão recursivo da `imprime` (percorre `prim` e `prox`, combinando os resultados):

```c
int soma(PArvGen a) {
    if (a == NULL) return 0;
    return a->info + soma(a->prim) + soma(a->prox);
}
```

> 💡 Como `info` é do tipo `char`, essa soma na verdade soma os **códigos ASCII** dos caracteres armazenados. Se a ideia for somar valores numéricos "de verdade", basta trocar o tipo do campo `info` para `int` na struct — a lógica da função continua exatamente a mesma.

```c
int num_nos(PArvGen a) {
    if (a == NULL) return 0;
    return 1 + num_nos(a->prim) + num_nos(a->prox);
}
```

```c
PArvGen retornaPrim(PArvGen a) {
    return a->prim;
}

PArvGen retornaProx(PArvGen a) {
    return a->prox;
}

int retornaConteudo(PArvGen a) {
    return a->info;
}

// a função insere já foi implementada na seção 4:
void insere(PArvGen a, PArvGen sa) {
    sa->prox = a->prim;
    a->prim = sa;
}
```

> 🧠 Funções como `retornaPrim`, `retornaProx` e `retornaConteudo` parecem "bobas" à primeira vista, mas cumprem um papel importante em um TAD (tipo abstrato de dados): elas **escondem** os detalhes internos da struct de quem usa a árvore. Se um dia a struct mudar (por exemplo, o campo `info` virar `dado`), só essas funções precisam ser ajustadas — quem usa a árvore, chamando `retornaConteudo(a)`, nem percebe a diferença.

</details>

<br>

### Exercício 6 🔴 (difícil)

> Faça um programa para a manipulação de árvores genéricas que leia um arquivo texto contendo uma sequência de nomes e coloque os nomes em diferentes níveis da árvore de acordo com a inicial do nome. Exibir na tela a lista de nomes, agrupados de acordo com a letra inicial do nome.

<details>
<summary>💡 Ver resolução</summary>

Esse exercício pede para guardar **nomes completos** (strings), não só um caractere — então, só para esse programa, vamos adaptar a struct para guardar uma `string` em vez de um único `char`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

typedef struct arvGen {
   char info[50];
   struct arvGen *prim;
   struct arvGen *prox;
} TArvGen;

typedef TArvGen *PArvGen;

PArvGen cria(char *s) {
    PArvGen a = (PArvGen) malloc(sizeof(TArvGen));
    strcpy(a->info, s);
    a->prim = NULL;
    a->prox = NULL;
    return a;
}

void insere(PArvGen a, PArvGen sa) {
    sa->prox = a->prim;
    a->prim = sa;
}
```

**A estrutura da árvore que vamos montar:**

```
        (raiz, "vazia")
       /      |       \
      A       C         M
    / | \    / | \     / | \
 Abel Ana Anibal ...  ...
```

A raiz não representa nada (é só um "ponto de partida"); o **nível 1** guarda as letras iniciais encontradas (A, C, M, P...); o **nível 2** guarda os nomes que começam com aquela letra.

**Função para localizar (ou criar) o nó de uma letra, entre os filhos da raiz:**

```c
PArvGen busca_irmao(PArvGen lista, char *chave) {
    while (lista != NULL) {
        if (strcmp(lista->info, chave) == 0) return lista;
        lista = lista->prox;
    }
    return NULL; // não achou entre os irmãos
}
```

**Programa principal:**

```c
int main() {
    FILE *arq = fopen("nomes.txt", "r");
    if (arq == NULL) {
        printf("Erro ao abrir o arquivo.\n");
        return 1;
    }

    PArvGen raiz = cria(""); // raiz "vazia", só para servir de ponto de partida
    char nome[50];

    while (fscanf(arq, "%s", nome) == 1) {
        char letra[2];
        letra[0] = toupper(nome[0]);
        letra[1] = '\0';

        // procura se já existe um "grupo" para essa letra
        PArvGen noLetra = busca_irmao(raiz->prim, letra);

        if (noLetra == NULL) {
            noLetra = cria(letra);
            insere(raiz, noLetra); // cria o grupo da letra, filho da raiz
        }

        PArvGen noNome = cria(nome);
        insere(noLetra, noNome); // insere o nome dentro do grupo da letra
    }

    fclose(arq);

    // Exibe agrupado
    PArvGen letra = raiz->prim;
    while (letra != NULL) {
        printf("%s\n", letra->info);
        PArvGen nome = letra->prim;
        while (nome != NULL) {
            printf("   %s", nome->info);
            nome = nome->prox;
        }
        printf("\n");
        letra = letra->prox;
    }

    return 0;
}
```

> 🧠 **Por que usar `busca_irmao` em vez do `busca_no` do Exercício 4?** Porque aqui só precisamos procurar entre os **filhos diretos da raiz** (as letras já criadas) — não faz sentido procurar "descendo" na árvore, já que as letras estão sempre no mesmo nível. Por isso a função só anda pelo `prox`, sem tocar no `prim`.
>
> ⚠️ Como a função `insere` sempre insere no **início** da lista, tanto as letras quanto os nomes dentro de cada letra vão aparecer na ordem **inversa** de leitura do arquivo. Se a ordem alfabética/cronológica for importante, uma alternativa é criar uma função `insere_fim` (que insere no final da lista de filhos, como fizemos com listas encadeadas simples lá no início do material) e usá-la aqui no lugar da `insere`.

</details>

<br>

### Exercício 7 🔴 (difícil)

> Faça um programa para a manipulação de árvores genéricas que leia um arquivo texto com uma sequência de linhas compostas de um identificador e um dado (ex: `1.3.2- Estado`), e gere uma árvore genérica em que o identificador de cada linha serve para indicar onde esta linha será inserida na hierarquia da árvore. A seguir, gere um arquivo texto, percorrendo a árvore do modo prefixo.

<details>
<summary>💡 Ver resolução</summary>

A ideia-chave aqui: o **identificador** (`1`, `1.1`, `1.3.2`...) descreve o **caminho** do nó dentro da árvore — cada ponto (`.`) representa "descer mais um nível". Por exemplo, `1.3.2` é o 2º dado dentro do 3º item dentro do item `1`.

Vamos assumir que o arquivo já vem **em ordem** (os pais sempre aparecem antes dos filhos — como no exemplo dos slides). Com isso, conseguimos montar a árvore usando um **vetor de "últimos nós inseridos em cada nível"**: sempre que lemos uma linha de profundidade `d`, o pai dela é o último nó que inserimos no nível `d - 1`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct arvGen {
   char info[100];
   struct arvGen *prim;
   struct arvGen *prox;
} TArvGen;

typedef TArvGen *PArvGen;

PArvGen cria(char *s) {
    PArvGen a = (PArvGen) malloc(sizeof(TArvGen));
    strcpy(a->info, s);
    a->prim = NULL;
    a->prox = NULL;
    return a;
}

// insere sa como o ULTIMO filho de a (preserva a ordem de leitura do arquivo)
void insere_fim(PArvGen a, PArvGen sa) {
    if (a->prim == NULL) {
        a->prim = sa;
    } else {
        PArvGen p = a->prim;
        while (p->prox != NULL) p = p->prox;
        p->prox = sa;
    }
}

// conta quantos niveis tem o identificador, contando os pontos
int calcula_profundidade(char *identificador) {
    int prof = 1;
    for (int i = 0; identificador[i] != '\0'; i++)
        if (identificador[i] == '.') prof++;
    return prof;
}
```

**Montando a árvore a partir do arquivo:**

```c
#define MAX_NIVEL 10

int main() {
    FILE *entrada = fopen("dados.txt", "r");
    FILE *saida = fopen("saida.txt", "w");
    if (entrada == NULL || saida == NULL) {
        printf("Erro ao abrir arquivos.\n");
        return 1;
    }

    PArvGen raiz = cria("RAIZ"); // no artificial, so para dar um ponto de partida
    PArvGen ultimoNoNivel[MAX_NIVEL];
    ultimoNoNivel[0] = raiz;

    char linha[150];
    while (fgets(linha, sizeof(linha), entrada) != NULL) {
        char identificador[30], dado[100];

        // separa "1.3.2- Estado" em identificador="1.3.2" e dado="Estado"
        sscanf(linha, "%[^-]- %[^\n]", identificador, dado);

        int prof = calcula_profundidade(identificador);

        PArvGen pai = ultimoNoNivel[prof - 1];
        PArvGen novo = cria(dado);

        insere_fim(pai, novo);
        ultimoNoNivel[prof] = novo; // este e o novo "ultimo no" desse nivel
    }

    fclose(entrada);
```

**Gerando o arquivo de saída em pré-ordem:**

```c
    // percorre em pre-ordem e escreve no arquivo de saida
    void imprime_prefixo(PArvGen a, FILE *saida) {
        if (a != NULL) {
            fprintf(saida, "%s\n", a->info);
            imprime_prefixo(a->prim, saida);
            imprime_prefixo(a->prox, saida);
        }
    }

    imprime_prefixo(raiz->prim, saida); // pula a raiz artificial

    fclose(saida);
    return 0;
}
```

> 🧠 **Por que usamos um vetor `ultimoNoNivel[]` em vez de fazer uma busca a cada linha?** Porque, como o arquivo já vem em ordem hierárquica (o pai sempre aparece antes do filho), o pai de qualquer linha é **sempre** o último nó que vimos naquele nível anterior — não precisamos procurar pela árvore inteira, só consultar uma posição do vetor. Isso torna o programa bem mais rápido (O(1) por linha) do que fazer uma busca recursiva a cada inserção.
>
> ⚠️ Essa solução assume que o arquivo de entrada é bem-formado (identificadores em ordem, sem "pular" níveis) — é uma simplificação razoável dado que o enunciado já garante isso ("os dígitos de cada subitem variam apenas de 1 a 9").

</details>

---

## 8. Resumo rápido

| Conceito | Explicação |
|---|---|
| Árvore genérica | Árvore em que cada nó pode ter **qualquer número de filhos** |
| Representação | "Primeiro filho / próximo irmão" — cada nó só guarda `prim` (1º filho) e `prox` (próximo irmão) |
| Isomorfismo com árvore binária | `prim` ↔ `esq`, `prox` ↔ `dir` — a raiz nunca tem filho à direita, pois não tem irmãos |
| Padrão recursivo típico | `trata(a); percorre(a->prim); percorre(a->prox);` |
| Percursos possíveis | Só pré-ordem ou pós-ordem fazem sentido (não existe "em-ordem" com grau variável) |
| Uso prático | Árvore de diretórios, estrutura organizacional, hierarquias de qualquer tipo (como no Exercício 7) |

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/16%20%E2%80%A2%20Percurso%20em%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Percurso_em_Árvores_Binárias-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/18%20%E2%80%A2%20%C3%81rvores%20Gen%C3%A9ricas/README.md">
    <img src="https://img.shields.io/badge/Avancar-Árvores_Genéricas_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
