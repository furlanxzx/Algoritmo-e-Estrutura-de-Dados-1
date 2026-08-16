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

---


