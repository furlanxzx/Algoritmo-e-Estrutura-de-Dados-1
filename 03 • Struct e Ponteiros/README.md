<h1 align="center">03 • Struct e Ponteiros</h1>

<p align="center">
  <a href="../01%20•%20Definições%20Iniciais%20e%20Notação%20Assintótica/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Arquivos-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/03%20%E2%80%A2%20Struct%20e%20Ponteiros/README.md">
    <img src="https://img.shields.io/badge/Avancar-Array_e_Ponteiros_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

Diferente dos vetores (*arrays*), que armazenam uma sequência de dados do **mesmo tipo**, as **estruturas** (`struct`) permitem agrupar variáveis de **tipos diferentes** sob um único nome.

---

## 📌 Conceitos Fundamentais

* **Registro:** É o nome dado à estrutura completa em outras linguagens de programação.
* **Membros ou Campos:** São as variáveis individuais contidas dentro da estrutura.

---

##  Sintaxe e Formas de Declaração

Existem três formas principais de definir e utilizar estruturas em C:

### 1. Declaração Padrão (`struct`)
Define o modelo da estrutura e permite declarar variáveis usando a palavra-chave `struct`.

```c
// Definição do tipo
struct Cliente {
    char nome[30];
    char rua[50];
    int idade;
};

// Declaração de variáveis
struct Cliente voce;
struct Cliente eu;
```

---

### 2. Uso com `typedef` (Recomendado)
O `typedef` cria um **apelido** (*alias*) para o tipo de dado, eliminando a necessidade de repetir a palavra `struct` em toda declaração.

```c
typedef struct {
    int a1;
    int a2;
} TipoVetor;

// Declaração limpa de variáveis
TipoVetor v1, v2;
```

---

### 3. Estruturas Anônimas
Cria variáveis diretamente na definição da estrutura sem dar um nome ao tipo.

> ⚠️ **Atenção:** Estruturas anônimas **não podem** ser reutilizadas em outras partes do código para declarar novas variáveis.

```c
struct {
    int a1;
    int a2;
} vetor1, vetor2; // Apenas vetor1 e vetor2 existirão com este tipo
```

---

##  Acesso aos Campos

O acesso a cada membro de uma estrutura é feito através do **operador ponto (`.`)**.

```c
TipoVetor v1;
v1.a1 = 10; // Atribui valor ao campo 'a1'
v1.a2 = 20; // Atribui valor ao campo 'a2'
```

---

## Estruturas Aninhadas (Struct dentro de Struct)

Um membro de uma estrutura pode ser outra estrutura, desde que a estrutura interna tenha sido declarada previamente.

```c
typedef struct {
    char rua[35];
    int numero;
    char cep[10];
} Endereco;

typedef struct {
    char nome[45];
    Endereco ender; // Struct Endereco como membro
    float salario;
} Ficha;

int main() {
    Ficha pessoa;
    
    // Acesso encadeado usando o operador ponto
    pessoa.ender.numero = 100;
    
    return 0;
}
```

---

## 📝 Exercícios Práticos

---

### 🟢 Exercício 1: Criação de Tipos e Aninhamento

> **Enunciado:**  
> Escreva um trecho de código para fazer a criação dos novos tipos de dados conforme solicitado:
> 1. **Horário:** composto de hora, minutos e segundos.
> 2. **Data:** composto de dia, mês e ano.
> 3. **Compromisso:** composto de uma data, um horário e um texto descritivo.

<details>
<summary><b>💡 Clique aqui para ver a solução em C</b></summary>

<br>

```c
#include <stdio.h>

// 1. Tipo Horario
typedef struct {
    int hora;
    int minutos;
    int segundos;
} Horario;

// 2. Tipo Data
typedef struct {
    int dia;
    int mes;
    int ano;
} Data;

// 3. Tipo Compromisso (com structs aninhadas)
typedef struct {
    Data data;
    Horario horario;
    char descricao[100];
} Compromisso;

int main() {
    Compromisso reuniao;

    // Exemplo de preenchimento dos dados
    reuniao.data.dia = 15;
    reuniao.data.mes = 9;
    reuniao.data.ano = 2026;

    reuniao.horario.hora = 14;
    reuniao.horario.minutos = 30;
    reuniao.horario.segundos = 0;

    return 0;
}
```

</details>

---

### 🟡 Exercício 2: Vetor de Estruturas (Cadastro de CDs)

> **Enunciado:**  
> Faça um programa que utilize uma estrutura do tipo `CD` e faça a leitura de dados de 10 CDs (utilizando um vetor de estruturas). A estrutura `CD` deve conter os seguintes campos:
> * Nome da banda
> * Data do lançamento do CD (dia, mês e ano)
> * Data da contratação da empresa (dia, mês e ano)
> * Valor do CD
> * Número de membros da banda
> * Produtora do CD

<details>
<summary><b>💡 Clique aqui para ver a solução em C</b></summary>

<br>

```c
#include <stdio.h>

#define QTD_CDS 10

// Struct reutilizável para datas
typedef struct {
    int dia;
    int mes;
    int ano;
} Data;

// Struct principal para o CD
typedef struct {
    char nome_banda[50];
    Data data_lancamento;
    Data data_contratacao;
    float valor;
    int num_membros;
    char produtora[50];
} CD;

int main() {
    CD colecao[QTD_CDS];

    printf("=== CADASTRO DE %d CDs ===\n\n", QTD_CDS);

    for (int i = 0; i < QTD_CDS; i++) {
        printf("--- CD nº %d ---\n", i + 1);

        printf("Nome da banda: ");
        scanf(" %[^\n]", colecao[i].nome_banda);

        printf("Data de lancamento (dia mes ano): ");
        scanf("%d %d %d", &colecao[i].data_lancamento.dia, 
                          &colecao[i].data_lancamento.mes, 
                          &colecao[i].data_lancamento.ano);

        printf("Data de contratacao (dia mes ano): ");
        scanf("%d %d %d", &colecao[i].data_contratacao.dia, 
                          &colecao[i].data_contratacao.mes, 
                          &colecao[i].data_contratacao.ano);
```
</details>

---
<h1 align="center">Ponteiros</h1>

Um **ponteiro** é uma variável especial criada para **armazenar o endereço de memória** de outra variável, em vez de guardar um valor comum (como um número ou caractere).

---

## 📌 Declarando um Ponteiro

Para indicar que uma variável é um ponteiro, utilizamos o símbolo asterisco (`*`) na sua declaração.

```c
int *p; // 'p' é um ponteiro que guardará o endereço de uma variável do tipo int
```

> **Dica de Leitura:** Leia `int *p;` como: *"A partir do endereço `p`, existe um inteiro"*.

É possível declarar ponteiros junto com variáveis comuns na mesma linha:
```c
int i, *p, j, v[10], *q; // 'i' e 'j' são inteiros; 'p' e 'q' são ponteiros para inteiro
```

---

## ⚡ Os Dois Operadores Fundamentais

| Operador | Nome | O que faz? | Exemplo |
| :--- | :--- | :--- | :--- |
| **`&`** | **Endereço de** | Retorna a posição de memória de uma variável. | `&x` (endereço onde `x` está guardado) |
| **`*`** | **Indireção / Desreferenciação** | Acessa o **conteúdo** do endereço guardado pelo ponteiro. | `*p` (conteúdo presente no endereço `p`) |

---

##  Entendendo Ponteiros na Memória

Ao declarar `int *p;`, o computador reserva um espaço para `p`, mas ele **ainda não aponta para lugar nenhum válido** (guarda lixo de memória).

```text
+-------+
|   ?   |   p (ponteiro não inicializado)
+-------+
```

Ao fazer `p = &i;`, o ponteiro `p` passa a guardar a localização de `i`:

```text
+-------+          +-------+
|   •---|--------> |   ?   |
+-------+          +-------+
    p                  i
```

---

## Manipulando Valores via Ponteiro

Quando `p` aponta para `i`, dizemos que `*p` é um ***alias* (apelido)** para `i`. Qualquer alteração via `*p` modifica diretamente o valor contido em `i`.

### Exemplo Passo a Passo:

```c
int i;
int *p;

p = &i; // p aponta para i
```
```text
+-------+          +-------+
|   •---|--------> |   ?   |
+-------+          +-------+
    p                  i
```

```c
i = 1; // Atribui 1 à variável i diretamente
```
```text
+-------+          +-------+
|   •---|--------> |   1   |
+-------+          +-------+
    p                  i
```

```c
*p = 2; // Altera o valor no endereço apontado por p (altera 'i')
```
```text
+-------+          +-------+
|   •---|--------> |   2   |
+-------+          +-------+
    p                  i
```

Ao executar `printf("%d", i);` ou `printf("%d", *p);`, a saída em ambos será **`2`**.

---

## Múltiplos Ponteiros para o Mesmo Endereço

Vários ponteiros podem apontar simultaneamente para a mesma variável:

```c
int i, *p, *q;

p = &i; // p recebe o endereço de i
q = p;  // q recebe o endereço guardado em p (também aponta para i)
```

```text
+-------+
|   •---|---+
+-------+   |      +-------+
    p       +----> |   ?   |
            |      +-------+
+-------+   |          i
|   •---|---+
+-------+
    q
```

* Tanto `*p = 1;` quanto `*q = 2;` mudarão o valor dentro da única variável **`i`**.

---

## ⚠️ Diferença entre `p = q` e `*q = *p`

Esse é um dos pontos onde as pessoas mais se confundem. Observe a diferença:

### 1. `p = q` (Cópia de Endereço)
Faz com que o ponteiro `p` passe a apontar para o **mesmo lugar** que o ponteiro `q`.

```text
p = &i;  q = &j;
p = q;   // p agora aponta para j

  p ----+
        |---> [   ] j
  q ----+     [   ] i
```

---

### 2. `*q = *p` (Cópia de Valor)
Não muda os ponteiros! Copia o **conteúdo** da memória apontada por `p` para dentro do endereço apontado por `q`.

```c
int i = 1, j;
int *p = &i, *q = &j;

*q = *p; // Copia o valor de 'i' (1) para o espaço de 'j'
```

```text
+-------+        +-------+
|   •---|------> |   1   |  (i)
+-------+        +-------+
    p

+-------+        +-------+
|   •---|------> |   1   |  (j recebeu o valor 1)
+-------+        +-------+
    q
```

---

## ⚠️ Boas Práticas e Cuidados Importantes

1. **Nunca desreferencie um ponteiro não inicializado:**
   ```c
   int *p;
   printf("%d\n", *p); // ❌ ERRO! Tenta ler uma memória aleatória. Pode travar o programa.
   ```

2. **Nunca atribua um número inteiro diretamente a um ponteiro:**
   ```c
   int *p;
   p = 1000; // ❌ ERRO! Você está mandando o ponteiro apontar para o endereço 1000 da memória.
   ```

3. **Inicialize com `NULL` se não for usá-lo imediatamente:**
   Se um ponteiro não tem para onde apontar no momento da criação, defina-o como `NULL` (requer `<stdlib.h>`).
   ```c
   int *p = NULL; // Indica que o ponteiro é explicitamente "vazio" no momento
   ```
   ---
<h1 align="center">Alocação Dinâmica</h1>

A **alocação dinâmica** permite reservar memória durante a execução do programa (*runtime*), ao contrário da alocação estática (como vetores com tamanho fixo). Todas as funções de alocação dinâmica pertencem à biblioteca `<stdlib.h>`.

---

## 🛠️ Funções Principais (`<stdlib.h>`)

| Função | Assinatura | Descrição |
| :--- | :--- | :--- |
| **`malloc`** | `void *malloc(size_t size);` | Aloca um bloco contínuo de memória com o tamanho especificado em bytes. Conteúdo inicial indeterminado (lixo de memória). |
| **`calloc`** | `void *calloc(size_t n_element, size_t size);` | Aloca memória para N elementos de determinado tamanho e **inicializa todos os bytes com zero**. |
| **`realloc`** | `void *realloc(void *ptr, size_t size);` | Altera o tamanho de um bloco de memória previamente alocado. |
| **`free`** | `void free(void *ptr);` | Libera o bloco de memória alocado dinamicamente de volta para o sistema operacional. |

---

##  Conceitos Fundamentais

### 1. O Retorno `void *` e o Cast Tipo de Ponteiro
A função `malloc` aloca bytes sem saber o tipo de dado que será gravado neles, retornando um ponteiro genérico (`void *`).
Em C, fazemos um ***cast*** (conversão forçada de tipo) para o ponteiro correto:

```c
char *c = (char *) malloc(1); // Cast (char *) converte a saída de malloc
```

### 2. Verificação de Sucesso (`NULL`)
Se o computador não tiver memória suficiente disponível, o `malloc` retorna o ponteiro nulo (`NULL`). **Sempre verifique esse retorno** antes de usar o ponteiro!

```c
if (c == NULL) { // Equivalente a: if (!c)
    printf("Erro: Memória insuficiente!\n");
    exit(1);
}
```

### 3. Liberação de Memória (`free`)
Memória alocada dinamicamente **não é liberada automaticamente** ao final da função. O uso da função `free()` é obrigatório para evitar vazamento de memória (*memory leak*).

```c
free(c); // Libera o espaço apontado por c
```

---

## Exemplo Básico de Uso

```c
#include <stdio.h>
#include <stdlib.h>

int main(void) 
{
    // 1. Declarar o ponteiro
    char *c; 

    // 2. Alocar 1 byte na memória e fazer o cast
    c = (char *) malloc(sizeof(char));

    // 3. Testar se a alocação funcionou
    if (c == NULL) {
        printf("Não foi possível alocar a memória.\n");
        exit(1);
    }

    // 4. Usar a memória alocada
    *c = 'd';
    printf("Valor armazenado: %c\n", *c);

    // 5. Liberar a memória após o uso
    free(c);

    return 0;
}
```

---

## 🎯 Exercício Resolvido

> **Proposta:** Escreva uma função que receba um caractere e o transforme em uma string de comprimento 1 contendo esse caractere. Faça a função `main` para testar, imprimindo a string criada e o seu tamanho.

⚠️ **Atenção:** Em C, uma string precisa do caractere nulo `'\0'` no final para indicar seu término. Portanto, uma "string de comprimento 1" precisa de **2 bytes** alocados (1 para o caractere + 1 para o `'\0'`).

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Função que converte um char em uma string alocada dinamicamente
char* char_para_string(char c) 
{
    // Aloca 2 bytes: 1 para o caractere e 1 para o delimitador '\0'
    char *str = (char *) malloc(2 * sizeof(char));

    if (str == NULL) {
        printf("Erro ao alocar memória!\n");
        exit(1);
    }

    str[0] = c;    // Define o caractere
    str[1] = '\0'; // Finaliza a string

    return str;
}

int main(void) 
{
    char caractere = 'A';

    // Chama a função e recebe o ponteiro da string alocada
    char *str_resultado = char_para_string(caractere);

    // Imprime o conteúdo e o tamanho usando strlen
    printf("String resultante: \"%s\"\n", str_resultado);
    printf("Tamanho da string: %lu\n", strlen(str_resultado));

    // Libera a memória alocada dinamicamente pela função
    free(str_resultado);

    return 0;
}
```
---
> [!IMPORTANT]
> **Atenção aos próximos tópicos!**
> A alocação dinâmica (com foco no `malloc`) é a ferramenta fundamental para a construção de **Listas**, **Pilhas** e **Filas**. Certifique-se de praticar bastante este módulo antes de avançar!

---
<h1 align="center">Ponteiro para Estruturas</h1>

Unir ponteiros e registros (`struct`) é o passo definitivo para construir estruturas de dados avançadas. Essa combinação permite manipular dados complexos na memória sem realizar cópias desnecessárias e viabiliza a criação de nós encadeados.

---

## 📌 Declaração e Sintaxe

Podemos declarar um ponteiro que aponta para uma estrutura de duas formas:

* **Sintaxe direta:**
  ```c
  struct ponto *pp; // pp é um ponteiro para uma struct ponto
  ```
* **Usando `typedef` para simplificar:**
  ```c
  typedef struct ponto *Ponto; // Cria o tipo 'Ponto', que já é um ponteiro
  Ponto pp;

  //*Eu pessoalmente prefiro dessa forma, como a professora também só usava dessa maneira, passei a usar também.
  ```

---

## 🎯 O Operador Seta (`->`)

Para acessar os campos de uma estrutura através de um ponteiro, utilizamos o operador **`->`** (seta). Ele substitui de forma simplificada a combinação do operador de desreferência `*` com o ponto `.`.

| Forma Longa (com parênteses) | Forma Simplificada (Recomendada) | O que faz? |
| :--- | :--- | :--- |
| `(*pp).x = 12.0;` | `pp->x = 12.0;` | Acessa o campo `x` do ponto apontado por `pp` |
| `(*pp).y = 5.5;` | `pp->y = 5.5;` | Acessa o campo `y` do ponto apontado por `pp` |

> **Por que os parênteses na forma longa?** O operador ponto `.` tem prioridade sobre o asterisco `*`. Sem os parênteses, `*pp.x` causaria um erro de compilação, pois o C tentaria acessar o campo `.x` antes de desreferenciar o ponteiro.

---

##  Passagem de Structs para Funções

| Tipo de Passagem | Como funciona | Vantagens / Desvantagens |
| :--- | :--- | :--- |
| **Por Valor** `(struct ponto p)` | Copia **todos** os campos da estrutura para a memória da função. | ❌ Ineficiente para structs grandes.<br>❌ Não permite alterar os valores originais. |
| **Por Ponteiro / Referência** `(struct ponto *p)` | Passa apenas o **endereço de memória** (4 a 8 bytes). | ✅ Alta eficiência de desempenho e memória.<br>✅ Permite alterar os valores originais. |

---

##  Alocação Dinâmica de Structs

Podemos alocar o espaço exato de uma estrutura em tempo de execução usando `malloc(sizeof(struct ...))`.

```c
#include <stdio.h>
#include <stdlib.h>

// Definição da estrutura
struct ponto {
    double x;
    double y;
};

int main(void) 
{
    // 1. Declaração do ponteiro para a estrutura
    struct ponto *p;

    // 2. Alocação dinâmica de memória
    p = (struct ponto *) malloc(sizeof(struct ponto));

    // 3. Verificação de segurança
    if (p == NULL) {
        printf("Erro ao alocar memória para a struct!\n");
        exit(1);
    }

    // 4. Atribuição de valores usando a seta (->)
    p->x = 12.0;
    p->y = 3.5;

    // 5. Exibição dos dados
    printf("Ponto alocado em: (%f, %f)\n", p->x, p->y);

    // 6. Liberação da memória
    free(p);

    return 0;
}
```

---

> 📌 **Lembrete Importante:** Esta sintaxe de **Ponteiro + Struct + `malloc` + `->`** é a fundação para a criação de **Pilhas, Filas e Listas Encadeadas**. Domine bem o uso da seta (`->`), pois ela será a sua principal ferramenta daqui em diante!
