<h1 align="center"> Array e Ponteiros</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/03%20%E2%80%A2%20Struct%20e%20Ponteiros/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Struct_e_Ponteiros-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/05%20%E2%80%A2%20Pilha%20Sequencial/README.md">
    <img src="https://img.shields.io/badge/Avancar-Pilha_Sequencial_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## Vetores como Ponteiro

Ponteiros e vetores estão **fortemente relacionados** na linguagem C.

O nome de um vetor é um ponteiro que aponta para a **primeira posição** do vetor, e todas as operações já mencionadas para ponteiros podem ser executadas com um nome de vetor.

Por exemplo, a declaração

```c
int v[100];
```

declara um vetor de inteiros de 100 posições, e a partir desta declaração temos que `v` é um ponteiro equivalente ao da declaração abaixo, no sentido de que ambos guardam um **endereço para inteiro**:

```c
int *v;
```

### Um ponteiro pode apontar para um vetor de elementos

```c
int a[10], *p;
p = &a[0];   // ou   p = a;
```

```
p ──┐
    ▼
a: [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
    0  1  2  3  4  5  6  7  8  9
```

Assim, podemos acessar `a[0]` por `*p`; por exemplo, podemos armazenar o valor `5` em `a[0]` com:

```c
*p = 5;
```

```
p ──┐
    ▼
a: [5][ ][ ][ ][ ][ ][ ][ ][ ][ ]
    0  1  2  3  4  5  6  7  8  9
```

### Aritmética de Ponteiros

Utilizando aritmética de ponteiros é possível acessar outros elementos do vetor.

A linguagem C suporta **3 formas** de aritmética de ponteiros:

- ✔️ adicionar um inteiro a um ponteiro;
- ✔️ subtrair um inteiro de um ponteiro;
- ✔️ subtrair um ponteiro de outro ponteiro.

```c
int a[10], *p, *q, i;
```

---

## Processamento de Vetores

Programa para somar elementos de um vetor, usando aritmética de ponteiros no lugar de indexação:

```c
#define N  10
...
int a[N], sum, *p;
...
sum = 0;
for (p = &a[0]; p < &a[N]; p++)
    sum = sum + *p;
```

📌 Repare que a condição de parada do `for` compara **endereços** (`p < &a[N]`), não índices — o loop percorre o vetor "andando" o ponteiro `p` de posição em posição.

---

## Exemplo de Aritmética de Ponteiros

```c
#include <stdio.h>
#define TAM 5
int main(){
    int v[TAM]={5,7,8,2,1};
    int i;

    for(i=0; i<TAM; i++)
        printf("%d ",v[i]); /* indexação */
    printf("\n");

    for(i=0; i<TAM; i++)
        printf("%d ",*(v+i)); /*aritmética de ponteiros*/
    printf("\n");

    return 0;
}
```

> Em ambos os casos temos a impressão de **todo o vetor**.

Ou seja, `v[i]` e `*(v+i)` são **equivalentes** — só duas formas diferentes de escrever a mesma coisa.

---

## Combinando Operadores `*` e `++`

Programa para somar elementos de um vetor, combinando os operadores `*` (desreferência) e `++` (incremento) numa única expressão:

```c
#define N 10
...
int a[N], sum, *p;
...
sum = 0;
p = &a[0];
while (p<&a[N])
    sum = sum + *p++;
```

📌 **Cuidado com a precedência!** `*p++` significa `*(p++)`, ou seja: primeiro pega o valor apontado por `p` (`*p`), depois **incrementa o ponteiro** `p` (não o valor apontado). O operador `++` tem precedência maior que `*`, mas como é um incremento **pós-fixado**, o valor usado na expressão é o de `p` *antes* do incremento.

---

## Nome do Vetor como Ponteiro

O nome de um vetor pode ser usado como ponteiro para o primeiro elemento do vetor:

```c
int a[10];
*a = 7;       // armazena 7 em a[0]
*(a+1) = 12;  // armazena 12 em a[1]
```

Em geral, `a+i` é o mesmo que `&a[i]` (endereço de `a[i]`) e `*(a+i)` é o equivalente a `a[i]`.

---

## Mais Exemplos

### Invertendo uma série numérica com ponteiros

```c
//Inverte uma série numérica (versão com ponteiros)
#include <stdio.h>
#define N 10
int main (void){
    int a[N], *p;
    printf("Digite %d números: ", N);
    for (p = a; p < a + N; p++)
        scanf("%d", p); //veja que não usamos &
    printf("Em ordem inversa:");
    for (p = a + N - 1; p >= a; p--)
        printf(" %d", *p);
    printf("\n");
    return 0;
}
```

📌 Note que no `scanf`, `p` já **é** um endereço (é um ponteiro), então não precisamos do `&` que normalmente usamos com variáveis comuns (`scanf("%d", &x)`).

### Quatro formas equivalentes de percorrer um vetor

```c
int main(void) {
    float v[] = {1.0, 2.0, 3.0, 4.0, 5.0, 6.0, 7.0, 8.0, 9.0};
    int i;
    float *p;
    for (i=0; i<9; i++) printf("%.1f ", v[i]);           printf("\n\n");
    for (i=0; i<9; i++) printf("%.1f ", *(v+i));          printf("\n\n");
    for (i=0, p=v; i<9; i++, p++) printf("%.1f ", *p);    printf("\n\n");
    for (i=0, p=v; i<9; i++, p++) printf("%.1f ", p[0]);  printf("\n\n");
    for (i=0, p=v; i<9; i++) printf("%.1f ", p[i]);
    return 0;
}
```

Observe como o ponteiro `p` recebe seu valor inicial (`p = v`, o próprio nome do vetor) e a maneira como ele é incrementado (`p++`) — todas as cinco formas acima imprimem exatamente a mesma sequência de valores.

---

## Vetor Declarado x Ponteiro: a Diferença Fundamental

Existe uma diferença fundamental entre declarar um conjunto de dados como um **vetor** ou usando um **ponteiro**.

- Na declaração de vetor, o **compilador automaticamente reserva um bloco de memória** para que o vetor seja armazenado.
- Quando apenas um ponteiro é declarado, a única coisa que o compilador faz é **alocar o ponteiro** para apontar para a memória, **sem que espaço seja reservado**.

Ou seja: `int *p;` cria só a "etiqueta" que vai guardar um endereço — ainda não existe memória de fato para `p` apontar, até que façamos `p` apontar para algo (um vetor já existente, ou memória alocada dinamicamente, como veremos mais adiante).

---

## Operações Inválidas com Vetores

```c
int vetor[10]; /*as operações de aritmética de ponteiro
                  são INVÁLIDAS*/
int *p, i;
p = &i;
vetor = vetor + 2;    /* ERRADO: vetor não é variável */
vetor++;               /* ERRADO: vetor não é variável */
vetor = p;              /* ERRADO: vetor não é variável */

/* as operações abaixo são válidas */
p = vetor;     /* CERTO: p é variável */
p = vetor+2;   /* CERTO: p é variável */
```

📌 **Por quê?** O nome do vetor (`vetor`) é um endereço **constante** — ele sempre aponta para o início daquele bloco de memória reservado na declaração, e isso não pode ser alterado. Já um ponteiro comum (`p`) é uma **variável de verdade**, que pode ser reatribuída livremente para apontar para onde quisermos.

---

## Ponteiros e Matrizes

As propriedades e operações de ponteiros são as mesmas de um vetor unidimensional, pois em C **matrizes são armazenadas na memória como vetores** (todas as linhas emendadas, uma atrás da outra):

```
[  linha 1  ][  linha 2  ]...[ linha n-1 ]
```

**Exemplo:** inicializar com zeros os elementos de uma matriz bidimensional:

```c
int a[N_LIN][N_COL], *p;
...
for (p=&a[0][0]; p<=&a[N_LIN-1][N_COL-1]; p++)
    *p = 0;
```

`p` aponta para `a[0][0]`, depois para `a[0][1]`... e assim por diante, "andando" pela matriz inteira como se fosse um único vetor grande.

### Processando apenas uma linha da matriz

Para processar somente a linha `i` de uma matriz `a`:

```c
p = &a[i][0]   // é equivalente a   p = a[i]
```

— em ambos os casos, `p` aponta para o primeiro elemento da linha `i`.

Para zerar os elementos da linha `i`, basta fazer:

```c
int a[N_LIN][N_COL], *p, i;
...
for (p = a[i]; p < a[i] + N_COL; p++)
    *p = 0;
```

### Ponteiro para Matriz

Aprofundando a relação entre ponteiros e matrizes:

- `&A[0][0]+1` equivale ao endereço de `A[0][1]`.
- `A[0]+1` equivale ao endereço de `A[0][1]`.
- `A+1` equivale ao endereço de `A[1][0]`.
- `&A[0]+1` equivale ao endereço de `A[1][0]`.

Logo:

```c
A[i][j] = (*(A+i))[j]
        = *((*(A+i))+j)
        = *(A[i]+j)
```

📌 Ou seja, indexar uma matriz com `A[i][j]` é só uma forma mais legível de escrever uma sequência de desreferências de ponteiro — por baixo dos panos, é tudo aritmética de endereços.

#### Utilizando ponteiros ao invés de notação de matrizes

Como uma matriz é armazenada na memória como um vetor único (linhas emendadas), podemos alocá-la dinamicamente como um vetor de uma dimensão e acessar o elemento `[i][j]` calculando manualmente sua posição: `i*COL+j`.

```c
#define LIN 3
#define COL 4
int main(){
    int *matriz, i, j;
    matriz = (int *)malloc(LIN*COL*sizeof(int));
    if (!matriz){
        printf("Nao consegui alocar a memoria suficiente.\n");
        exit(1);
    }
    for(i=0; i<LIN; i++){
        for (j=0; j<COL; j++){
            printf("Elemento %d %d = ", i, j);
            scanf("%d", matriz+(i*COL+j));
        }
    }
    free(matriz);
    return (0);
}
```

📌 Note que `matriz+(i*COL+j)` é o endereço do elemento que estaria na posição `[i][j]` de uma matriz de `COL` colunas — essa é a técnica que usaremos mais adiante para alocar matrizes dinamicamente com um único `malloc`.

---

## Alocação Dinâmica de Vetores

Suponha que queremos tirar a média de `n` notas. Pedimos o valor de `n` e então as `n` notas, certo? E como guardaríamos?

Até agora tínhamos que declarar um vetor muito grande e torcer para que `n` não fosse maior que o vetor definido.

✅ **Alocação dinâmica acaba com esse problema!**

### Como fazer

Primeiro é preciso declarar um **ponteiro** para o tipo desejado:

```c
float *v;
```

Lembra de `float v[10]`? O compilador aloca espaço na memória suficiente para guardar 10 `float`s, guardando o endereço do primeiro elemento do vetor `v`. Isso significa que, em `float v[10]`, `v` nada mais é que `float *`.

Dessa forma, se declararmos um ponteiro para `float`, tudo que temos que fazer para transformá-lo em um vetor é **apontá-lo para um grupo sequencial de `float`s na memória** — e é exatamente isso que o `malloc` faz.

```c
v = (float *) malloc(n * sizeof(float));
```

Note que pegamos o tamanho de um `float` e multiplicamos pelo número de `float`s (`n`) que o vetor conterá, ou seja, calculamos o tamanho **em bytes** de `n` `float`s.

Como já visto anteriormente, ao fazermos `v[i]`, estamos fazendo, na verdade, `*(v+i)`. Portanto, é possível tratar um ponteiro alocado dinamicamente como um **vetor comum**.

Por fim, porém não menos importante, para **desalocar** nosso vetor, basta fazer:

```c
free(v);
```

### Exemplo — vetor de inteiros alocado dinamicamente

```c
#include <stdio.h>
#include <stdlib.h>
int main(void) {
    int *p, i, n;
    printf("Entre com o tamanho do vetor: ");
    scanf("%d",&n);
    p = (int *)malloc(n * sizeof(int));
    for (i = 0; i < n; i++)
        p[i] = i;
    for (i = 0; i < n; i++)
        printf("%d ",p[i]);

    printf("\n");
    free(p);
    return 0;
}
```

### Exemplo — vetor de notas, com verificação de falha do `malloc`

```c
int main() {
    float *v; /* vetor de notas */
    int i, n; // contador e No. de elementos do vetor

    printf("Qual o numero de notas? ");
    scanf("%d",&n);
    v = (float *)malloc(n * sizeof(float));
    if (v==NULL) {
        printf("Nao foi possivel alocar o vetor\n");
        exit(0);
    }
    for (i = 0; i < n; i++) scanf("%f", &v[i]);
    for (i = 0; i < n; i++) // imprime o vetor
        printf("Nota: %f\n ", v[i]);
    free(v);
    return 0;
}
```

📌 **Boa prática:** sempre verificar se o `malloc` retornou `NULL` antes de usar o ponteiro — se a memória solicitada não estiver disponível, `malloc` retorna `NULL`, e tentar usar esse ponteiro (por exemplo, `v[0] = ...`) causaria um erro grave (acesso a endereço inválido).

---

## Exercícios

> ℹ️ O material recebido começa no **Exercício 2** — se você tiver o Exercício 1, é só mandar que eu encaixo aqui.

### Exercício 2 🟡

O que fazem os seguintes programas?

```c
#include <stdio.h>
int main() {
  int vet[] = {4,9,12};
  int i,*ptr;
  ptr = vet;
  for(i = 0 ; i < 3 ; i++)
    printf("%d ",*ptr++);
  return 0;
}
```

```c
#include <stdio.h>
int main(){
  int vet[] = {4,9,12};
  int i,*ptr;
  ptr = vet;
  for(i = 0 ; i < 3 ; i++)
    printf("%d ",(*ptr)++);
  return 0;
}
```

<details>
<summary>Ver resposta</summary>

A diferença entre os dois programas está toda na **precedência de operadores**.

**Programa 1 — `*ptr++`**

Isso é o mesmo que `*(ptr++)`: o operador `++` (pós-fixado) tem precedência maior que `*`, então primeiro se aplica o `++` a `ptr`. Mas como é pós-fixado, o valor usado na expressão é o de `ptr` **antes** do incremento — ou seja, desreferenciamos o endereço atual e **depois** avançamos o ponteiro para o próximo elemento.

Resultado: o programa imprime cada elemento do vetor, um após o outro.

```
4 9 12
```

**Programa 2 — `(*ptr)++`**

Aqui os parênteses forçam a desreferência **primeiro**: `*ptr` acessa o valor apontado (sempre `vet[0]`, já que `ptr` nunca é incrementado), e o `++` incrementa **esse valor**, não o ponteiro.

Ou seja, a cada iteração estamos incrementando `vet[0]` e `ptr` continua parado nele.

```
4 5 6
```

(primeiro imprime 4, e `vet[0]` vira 5; depois imprime 5, e `vet[0]` vira 6; depois imprime 6)

**Bônus:** se fosse escrito `*(ptr++)` explicitamente, o resultado seria **idêntico** ao do Programa 1 (`4 9 12`), pois é exatamente essa a forma como `*ptr++` já é interpretado pelo compilador.

</details>

---

### Exercício 3 🔴

Escreva uma função `int remove_dup(float v[], int *n)` que receba um vetor e verifique a existência de elementos duplicados. Caso não existam elementos duplicados retorne 0. Caso existam, remova estes elementos (deixando apenas um deles) e retorne o número de elementos removidos. O programa principal que chamará esta função deverá alocar o espaço necessário para este vetor, assim como ler o vetor inicial e imprimir o vetor resultante.

<details>
<summary>Ver resposta</summary>

A ideia é comparar cada elemento com todos os que vêm depois dele; ao encontrar um duplicado, deslocamos os elementos seguintes uma posição para a esquerda (sobrescrevendo o duplicado) e diminuímos o tamanho lógico do vetor (por isso `n` é passado como **ponteiro**, já que o tamanho muda).

```c
#include <stdio.h>
#include <stdlib.h>

int remove_dup (float v[], int *n)
{
    int removidos = 0;
    int i, j, k;

    for (i = 0; i < *n; i++) {
        for (j = i+1; j < *n; j++) {
            if (v[j] == v[i]) {
                // desloca os elementos seguintes uma posição p/ esquerda
                for (k = j; k < *n - 1; k++)
                    v[k] = v[k+1];

                (*n)--;
                removidos++;
                j--; // o elemento que "caiu" na posição j precisa ser comparado também
            }
        }
    }

    return removidos;
}

int main(void)
{
    float *v;
    int n, removidos, i;

    printf("Quantos elementos o vetor tera? ");
    scanf("%d", &n);

    v = (float *) malloc(n * sizeof(float));
    if (v == NULL) {
        printf("Nao foi possivel alocar o vetor\n");
        exit(0);
    }

    printf("Digite os %d elementos:\n", n);
    for (i = 0; i < n; i++)
        scanf("%f", &v[i]);

    removidos = remove_dup(v, &n);

    printf("Elementos removidos: %d\n", removidos);
    printf("Vetor resultante:\n");
    for (i = 0; i < n; i++)
        printf("%.2f ", v[i]);
    printf("\n");

    free(v);
    return 0;
}
```

📌 O `j--` depois de remover é essencial: como todo mundo deslocou uma casa pra esquerda, o valor que agora ocupa a posição `j` ainda não foi comparado com `v[i]` — sem o `j--`, o loop pularia esse elemento.

</details>

---

### Exercício 4 🟡

Escreva uma função `void insert(float v[], int n, float valor, int pos)` que faça a inserção de `valor` na posição `pos` do vetor `v`, deslocando os demais elementos.

<details>
<summary>Ver resposta</summary>

⚠️ Assumimos que o vetor `v` já tem espaço alocado para `n+1` elementos (afinal, vamos inserir um a mais) — quem chama a função é responsável por garantir isso.

```c
void insert (float v[], int n, float valor, int pos)
{
    int i;

    // desloca tudo de pos em diante uma posição p/ direita,
    // começando pelo final para não sobrescrever nada
    for (i = n; i > pos; i--)
        v[i] = v[i-1];

    v[pos] = valor;
}
```

</details>

---

### Exercício 5 🟢

Faça uma função `ordem(int v, int n)` que ordene crescentemente os elementos de um vetor `v` de `n` elementos inteiros.

<details>
<summary>Ver resposta</summary>

⚠️ O protótipo do slide tem uma pequena imprecisão de digitação (`int v` ao invés de `int v[]`) — o correto, para receber um vetor, é `void ordem(int v[], int n)`.

Vamos usar o algoritmo clássico de **bubble sort** (ordenação por flutuação):

```c
void ordem (int v[], int n)
{
    int i, j, aux;

    for (i = 0; i < n-1; i++) {
        for (j = 0; j < n-1-i; j++) {
            if (v[j] > v[j+1]) {
                aux = v[j];
                v[j] = v[j+1];
                v[j+1] = aux;
            }
        }
    }
}
```

A cada passagem pelo vetor, o maior elemento "borbulha" até sua posição final correta, por isso a cada iteração externa `i` diminuímos o alcance da interna (`n-1-i`) — os últimos `i` elementos já estão garantidamente ordenados.

</details>

---

### Exercício 6 🔴

Escreva uma função `int merge(float r[], float s[], float v[], int n, int m)` que receba um vetor `r` de `n` elementos e outro vetor `s` de `m` elementos e construa um vetor `v` com os elementos de `r` e `s`, ordenado e não duplicado. A função deve retornar o tamanho do vetor `v` construído.

> Sugestão: Utilize as funções dos exercícios 4 e 5.

<details>
<summary>Ver resposta</summary>

Vamos reaproveitar a ideia da **inserção ordenada**: para cada elemento de `r` e `s`, achamos a posição correta dele no vetor `v` (já ordenado até o momento) e usamos a função `insert` do Exercício 4 para colocá-lo lá — mas só se ele ainda não estiver presente, o que garante que não haverá duplicados.

```c
// insere valor em v mantendo a ordem crescente, evitando duplicados.
// retorna 1 se inseriu, 0 se valor já existia (duplicado)
int insereOrdenado (float v[], int tam, float valor)
{
    int pos = 0;

    // acha a posição onde valor deveria entrar (mesma lógica de comparação do Ex. 5)
    while (pos < tam && v[pos] < valor)
        pos++;

    if (pos < tam && v[pos] == valor)
        return 0; // já existe, não insere

    insert(v, tam, valor, pos); // reaproveitando o Exercício 4
    return 1;
}

int merge (float r[], float s[], float v[], int n, int m)
{
    int tam = 0;
    int i;

    for (i = 0; i < n; i++)
        tam += insereOrdenado(v, tam, r[i]);

    for (i = 0; i < m; i++)
        tam += insereOrdenado(v, tam, s[i]);

    return tam;
}
```

📌 O vetor `v` construído dessa forma é exatamente a **união** dos conjuntos `r` e `s`, ordenada e sem repetições — que é justamente o assunto do próximo exercício.

</details>

---

### Exercício 7 🔴

A função do exercício 6 pode ser entendida como uma função que retorna a **união** entre dois conjuntos. Escreva uma função `int intersec(float r[], float s[], float v[], int n, int m)` que construa um vetor `v` com a **interseção** entre `r` e `s`, ordenados. A função deve retornar o tamanho do vetor `v` construído.

<details>
<summary>Ver resposta</summary>

A lógica muda: agora só entram em `v` os elementos que aparecem **em ambos** os vetores `r` e `s`. Continuamos usando `insert` (Exercício 4) para manter `v` ordenado e sem duplicados.

```c
int intersec (float r[], float s[], float v[], int n, int m)
{
    int tam = 0, i, j, achou, pos, existe;

    for (i = 0; i < n; i++) {
        // verifica se r[i] também está em s
        achou = 0;
        for (j = 0; j < m; j++) {
            if (r[i] == s[j]) {
                achou = 1;
                break;
            }
        }

        if (achou) {
            // verifica se r[i] já foi inserido em v (evita duplicado)
            existe = 0;
            pos = 0;
            while (pos < tam && v[pos] < r[i])
                pos++;
            if (pos < tam && v[pos] == r[i])
                existe = 1;

            if (!existe) {
                insert(v, tam, r[i], pos); // reaproveitando o Exercício 4
                tam++;
            }
        }
    }

    return tam;
}
```

</details>

---

### Exercício 8 🟢

Escreva uma função `void desordem(int v, int n)` que desordene os elementos de um vetor `v` (não necessariamente ordenado) de `n` elementos inteiros.

> Sugestão: use o seguinte algoritmo:
> ```
> para i de n-1 até 0 faça
>       j → valor aleatório entre 0 e i
>       v[i] ↔ v[j]
> fim para
> ```
> Observações:
> a) Esta rotina pode ser usada para simular o processo de embaralhar as cartas de um baralho.
> b) Utilize a função `rand()` para obter um número aleatório.

<details>
<summary>Ver resposta</summary>

⚠️ Assim como no Exercício 5, o protótipo correto é `void desordem(int v[], int n)`.

É só traduzir o algoritmo dado, diretamente, para C. Esse algoritmo é conhecido como **Fisher-Yates shuffle**, e garante que qualquer permutação do vetor é igualmente provável.

```c
#include <stdlib.h> // necessário para rand()

void desordem (int v[], int n)
{
    int i, j, aux;

    for (i = n-1; i >= 0; i--) {
        j = rand() % (i+1); // valor aleatório entre 0 e i

        aux = v[i];
        v[i] = v[j];
        v[j] = aux;
    }
}
```

📌 `rand() % (i+1)` gera um número entre `0` e `i` (inclusive), respeitando o que o algoritmo pede.

</details>

---

### Exercício 9 🟡

Escreva uma função `int find(char v[], char t[], int m, int n)` que receba um vetor `v` de `m` elementos e um vetor `t` de `n` elementos (`n < m`). Esta função deve verificar a ocorrência do padrão `t` em `v` ou não. Se houver, deve retornar a posição inicial da primeira ocorrência. Por exemplo: se `v = {As bananas do Panamá são bacanas}` e `p = {anas}` deve retornar `6`. Caso não haja ocorrência, retorne `-1`.

> Observação: Algoritmos como esses são usados em editores de texto.

<details>
<summary>Ver resposta</summary>

Esse é o problema clássico de **busca de padrão em texto** (pattern matching). A versão mais simples (busca ingênua) tenta casar o padrão `t` a partir de cada posição possível de `v`:

```c
int find (char v[], char t[], int m, int n)
{
    int i, j;

    for (i = 0; i <= m - n; i++) {
        j = 0;
        while (j < n && v[i+j] == t[j])
            j++;

        if (j == n)
            return i; // padrão inteiro casou a partir da posição i
    }

    return -1; // não encontrado
}
```

Para cada posição inicial `i` em `v`, comparamos caractere a caractere com `t`; se todos os `n` caracteres baterem (`j == n`), achamos a ocorrência. O laço externo vai só até `m - n` porque, a partir daí, não há mais espaço suficiente em `v` para caber o padrão inteiro.

</details>

---

### Exercício 10 🟢

Escreva uma função `void stat(float *vet, int N, float *med, float *dsvpd)` que receba um vetor de números reais `vet`, seu tamanho `N` e calcule a média aritmética `med` e o desvio padrão `dsvpd` destes valores.

Cálculo do desvio padrão (μ = `med` e vᵢ = cada elemento de `vet`):

```
dsvpd = √( Σ(vᵢ - μ)² / N )
```

<details>
<summary>Ver resposta</summary>

Como a função precisa **devolver dois valores** (média e desvio padrão) e em C uma função só retorna um valor diretamente, usamos **ponteiros** para os parâmetros de saída — daí `float *med` e `float *dsvpd`: a função escreve o resultado no endereço que o chamador passou.

```c
#include <math.h> // necessário para sqrt()

void stat (float *vet, int N, float *med, float *dsvpd)
{
    int i;
    float soma = 0, somaQuad = 0;

    for (i = 0; i < N; i++)
        soma += vet[i];

    *med = soma / N;

    for (i = 0; i < N; i++)
        somaQuad += (vet[i] - *med) * (vet[i] - *med);

    *dsvpd = sqrt(somaQuad / N);
}
```

📌 Precisamos primeiro terminar de calcular `*med` (um laço completo pelo vetor) antes de começar a calcular o desvio padrão, já que a fórmula do desvio depende da média já pronta — por isso são dois laços separados, e não um só.

</details>

---

### Exercício 11 🔴

Escreva um programa que procure em uma matriz elementos que sejam, ao mesmo tempo, o **maior da linha** e o **menor da coluna**. As dimensões da matriz devem ser solicitadas ao usuário e você deve alocar o espaço necessário para armazenar esta matriz.

<details>
<summary>Ver resposta</summary>

Esse tipo de elemento é conhecido como **ponto de sela** (*saddle point*).

Seguindo a técnica vista na seção [Ponteiro para Matriz](#ponteiro-para-matriz), vamos alocar a matriz dinamicamente como um **vetor único** (com `malloc`), acessando o elemento `[i][j]` através da fórmula `i*col+j` — já que o tamanho da matriz só é conhecido em tempo de execução (não dá pra declarar `int matriz[LIN][COL]` com `LIN`/`COL` vindos do `scanf`).

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int lin, col, i, j, k;
    int *matriz;

    printf("Numero de linhas: ");
    scanf("%d", &lin);
    printf("Numero de colunas: ");
    scanf("%d", &col);

    matriz = (int *) malloc(lin * col * sizeof(int));
    if (matriz == NULL) {
        printf("Nao foi possivel alocar a matriz\n");
        exit(1);
    }

    printf("Digite os elementos da matriz:\n");
    for (i = 0; i < lin; i++)
        for (j = 0; j < col; j++) {
            printf("Elemento [%d][%d]: ", i, j);
            scanf("%d", matriz + (i*col+j));
        }

    printf("\nElementos que sao maior da linha e menor da coluna:\n");
    for (i = 0; i < lin; i++) {
        for (j = 0; j < col; j++) {
            int atual = *(matriz + (i*col+j));
            int maiorDaLinha = 1, menorDaColuna = 1;

            // atual é o maior da linha i?
            for (k = 0; k < col; k++)
                if (*(matriz + (i*col+k)) > atual)
                    maiorDaLinha = 0;

            // atual é o menor da coluna j?
            for (k = 0; k < lin; k++)
                if (*(matriz + (k*col+j)) < atual)
                    menorDaColuna = 0;

            if (maiorDaLinha && menorDaColuna)
                printf("Elemento %d na posicao [%d][%d]\n", atual, i, j);
        }
    }

    free(matriz);
    return 0;
}
```

---
<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/03%20%E2%80%A2%20Struct%20e%20Ponteiros/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Struct_e_Ponteiros-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/05%20%E2%80%A2%20Pilha%20Sequencial/README.md">
    <img src="https://img.shields.io/badge/Avancar-Pilha_Sequencial_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

📌 Repare que usamos `scanf("%d", matriz + (i*col+j))` sem `&` — assim como vimos na seção de exemplos com ponteiros, `matriz + (i*col+j)` **já é** um endereço, então não precisamos do `&` que usaríamos com uma variável comum.

</details>
