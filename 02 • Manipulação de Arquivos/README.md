<h1 align="center"> Manipulação de Arquivos — Conceitos Fundamentais</h1>

<p align="center">
  <a href="../01%20•%20Definições%20Iniciais%20e%20Notação%20Assintótica/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Capitulo_01-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/03%20%E2%80%A2%20Struct%20e%20Ponteiros/README.md">
    <img src="https://img.shields.io/badge/Avancar-Struct_e_Ponteiros_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## <mark> 1 - Tipos de Arquivos 📁</mark>

Os arquivos no sistema operacional são organizados de duas formas principais de acordo com a estrutura dos seus dados:

* **Arquivo Texto:** Armazena caracteres legíveis diretamente por seres humanos (ex: padrão ASCII ou UTF-8). Podem ser visualizados e editados em editores de texto simples (como Bloco de Notas ou VS Code).
* **Arquivo Binário:** Armazena dados como uma sequência bruta de bytes, seguindo exatamente o mesmo formato em que as informações ficam organizadas na memória RAM. São interpretados apenas pelos programas que os geraram (ex: arquivos executáveis `.exe`, imagens, vídeos, arquivos compactados).

> 💡 **Papel do Programador:** Ao trabalhar com arquivos binários, cabe ao programador mapear corretamente cada conjunto de bytes para o tipo de dado correto (como `int`, `float` ou `struct`) durante a leitura e escrita.

---

## <mark> 2 - Descritor de Arquivos (`FILE *`) </mark>

Para que o programa consiga interagir com um arquivo salvo no armazenamento secundário (HD/SSD), a linguagem C utiliza a estrutura de dados **`FILE`** (disponível na biblioteca `<stdio.h>`).

* **Arquivo Físico:** O arquivo real gravado no disco rígido/memória secundária.
* **Arquivo Lógico:** As variáveis mantidas em memória primária (RAM) manipuladas pelo código.
* **Descritor de Arquivo:** O ponteiro do tipo `FILE*` que atua como ponte de comunicação entre o arquivo lógico e o arquivo físico.

**Declaração :**
```c
FILE *parq; // Ponteiro para a estrutura que gerencia o arquivo
```

---

## <mark> 3 - Abertura de Arquivos (`fopen`) </mark>

A função `fopen()` conecta o **descritor de arquivo** (`FILE*`) ao arquivo físico no disco.

**Sintaxe:**
```c
parq = fopen("caminho_do_arquivo", "modo");
```

###  Modos de Abertura

| Modo | Tipo | Finalidade | Comportamento |
| :---: | :---: | :--- | :--- |
| **`r`** | Texto | Leitura (*Read*) | Abre um arquivo existente. Retorna `NULL` se não existir. |
| **`w`** | Texto | Escrita (*Write*) | Cria um arquivo novo. **Sobrescreve** se o arquivo já existir. |
| **`a`** | Texto | Anexar (*Append*) | Adiciona novos dados ao **final** do arquivo mantendo o conteúdo anterior. |
| **`rb`** | Binário | Leitura Binária | Abre arquivo binário existente para leitura. |
| **`wb`** | Binário | Escrita Binária | Cria ou sobrescreve arquivo binário para escrita. |
| **`ab`** | Binário | Anexo Binário | Adiciona novos bytes ao final de um arquivo binário. |
| **`r+`** / **`w+`** / **`a+`** | Texto | Múltiplo | Habilita operações combinadas de **leitura e escrita**. |
| **`r+b`** / **`w+b`** / **`a+b`** | Binário | Múltiplo Binário | Leitura e escrita combinadas no formato binário (também aceita `rb+`, `wb+`, `ab+`). |

---

## <mark> 4 - Validação e Tratamento de Erros </mark>

Quando o sistema operacional não consegue abrir um arquivo (por exemplo, ao tentar ler um arquivo inexistente ou sem permissão de acesso), a função `fopen()` retorna o ponteiro nulo (`NULL`).

**É indispensável checar se a abertura foi bem-sucedida antes de realizar qualquer operação:**

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *parq;

    // Tenta abrir o arquivo para leitura
    parq = fopen("teste.txt", "r");

    // Validação do ponteiro
    if (parq == NULL) {
        printf("Erro: Não foi possível abrir o arquivo para leitura!\n");
        exit(1); // Encerra a execução do programa
    } else {
        printf("Arquivo aberto com sucesso para leitura.\n");
    }

    // ... operações de leitura/escrita ...

    fclose(parq); // Libera o arquivo
    return 0;
}
```

---

## <mark> 5 - Fechamento de Arquivos (`fclose`) </mark>

Quando o programa conclui o uso do arquivo, ele deve ser fechado explicitamente através da função `fclose()`.

**Sintaxe:**
```c
fclose(parq);
```

###  Por que fechar o arquivo é essencial?
1. **Garantia de Salvamento:** Descarrega os dados contidos nos *buffers* temporários da RAM diretamente para o disco rígido.
2. **Integridade:** Evita corrupção dos dados caso o programa seja encerrado de forma inesperada.
3. **Gerenciamento de Recursos:** Libera o arquivo e a memória no sistema operacional para que outros processos ou programas possam acessá-lo.

---
## <mark> 6 - Leitura de Dados em Arquivos Texto </mark>

Ao abrir um arquivo para leitura, o sistema operacional cria e mantém um **indicador de posição interna** (cursor). Compreender como esse cursor se comporta é fundamental para ler os dados de maneira correta.

### <mark> 6.1 - O Cursor Interno e a Função `rewind()` </mark>

* **Posição Inicial:** Quando um arquivo é aberto no modo de leitura (`"r"`), o cursor é automaticamente posicionado no início do arquivo (byte 0).
* **Avanço Automático:** A cada operação de leitura realizada (`fgetc`, `fgets` ou `fscanf`), o cursor avança para o próximo caractere ou bloco de dados a ser lido.
* **Reposicionamento com `rewind()`:** Se for necessário reler o arquivo do início sem precisar fechá-lo e abri-lo novamente, utiliza-se a função `rewind()`.

```c
rewind(parq); // Reposiciona o cursor diretamente no início do arquivo
```

---

## <mark> 7 - Principais Funções de Leitura </mark>

| Função | Sintaxe | O que faz | Uso Recomendado |
| :---: | :--- | :--- | :--- |
| **`fgetc`** | `int fgetc(FILE *fp);` | Lê **um único caractere** por vez e avança o cursor. Retorna o caractere lido ou `EOF`. | Leitura caractere por caractere ou análise de sintaxe. |
| **`fgets`** | `char* fgets(char *s, int n, FILE *fp);` | Lê uma **cadeia de caracteres (string)** até encontrar `\n` ou atingir o limite `n - 1`. Retorna a string ou `NULL`. | Leitura de linhas inteiras com segurança contra transbordamento (*buffer overflow*). |
| **`fscanf`** | `int fscanf(FILE *fp, "formatos", ...);` | Lê **dados formatados** (inteiros, floats, strings) do arquivo, funcionando como o `scanf`. | Leitura de arquivos com estrutura fixa de colunas/tabelas. |

---

##   Detalhamento das Funções  

### 1. `fgetc()` — Leitura de Caractere Único
Lê apenas o caractere atual apontado pelo cursor e retorna seu valor.

```c
char letra;
letra = fgetc(fp); // Lê o próximo caractere do arquivo 'fp'
```

---

### 2. `fgets()` — Leitura por Linhas
Lê até `n - 1` caracteres de uma linha, inserindo automaticamente o caractere nulo `\0` no final. Se encontrar uma quebra de linha (`\n`), a leitura para e o `\n` é mantido na string.

```c
char linha[256];
// Lê até 255 caracteres da linha (reservando 1 byte para o '\0')
if (fgets(linha, 256, fp) != NULL) {
    printf("Linha lida: %s", linha);
}
```

---

### 3. `fscanf()` e a Constante `EOF`
Permite ler variáveis de tipos específicos diretamente do arquivo. Quando a função atinge o fim do arquivo, ela retorna a constante de sistema **`EOF`** (*End Of File*).

```c
char c;
// Lê caractere por caractere até atingir o Fim do Arquivo (EOF)
while (fscanf(f, "%c", &c) != EOF) {
    printf("%c", c);
}
```

---

##  📊 Exemplo Prático Integrado

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *f = fopen("teste.txt", "r"); // fazemos a abertura do arquivo

    if (f == NULL) {
        printf("Erro ao abrir o arquivo!\n"); // verificando se o arquivo foi aberto corretamente
        return 1;
    }

    char c;
    printf("Lendo caractere por caractere\n");
    while (fscanf(f, "%c", &c) != EOF) {
        printf("%c", c);
    }

    // volta o cursor para o início para reler de outra forma
    rewind(f);

    printf("\n Lendo o primeiro caractere com fgetc\n");
    char primeiro = fgetc(f);
    printf("Primeiro caractere: %c\n", primeiro);

    fclose(f);
    return 0;
}
```
---

## <mark> 8 - Escrevendo Dados em Arquivo Texto </mark>

| Função | Sintaxe / Assinatura | O que faz | Uso Recomendado |
| :---: | :--- | :--- | :--- |
| **`fprintf`** | `int fprintf(FILE *fp, "formatos", ...);` | Escreve **dados formatados** no arquivo. | Gravação de variáveis e textos. |
| **`fputc`** | `int fputc(int c, FILE *fp);` | Escreve **um único caractere** no arquivo. | Gravação caractere por caractere. |

###  Exemplo: Copiando Conteúdo entre Arquivos

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fr = fopen("teste.txt", "r"); //fazendo abertura dos arquivos
    FILE *fw = fopen("saida.txt", "w");

    if (fr == NULL || fw == NULL) {
        printf("Erro ao abrir os arquivos.\n"); //verificando se houve algum erro na abertura
        return 1;
    }

    char c;
    while (fscanf(fr, "%c", &c) != EOF) { //fazendo a cópia do conteúdo de um arquivo para o outro
        fprintf(fw, "%c", c);
    }

    fclose(fr);
    fclose(fw); //lembre-se de fechar o arquivo sempre! (professor(a) desconta nota por isso)
    return 0;
}
```

---

## 📝 Exercícios Práticos


### 🟢 Exercício 1: Contador de Linhas

> **Enunciado:**  
> Faça um programa que leia um arquivo texto qualquer e retorne a sua quantidade de linhas.

#### Solução:

<details>
<summary><b>💡 Clique aqui para ver a solução </b></summary>

<br>

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fp = fopen("arquivo.txt", "r");

    if (fp == NULL) {
        printf("Erro ao abrir o arquivo para leitura.\n");
        return 1;
    }

    int linhas = 0;
    char c;

    while ((c = fgetc(fp)) != EOF) {
        if (c == '\n') {
            linhas++;
        }
    }

    rewind(fp);
    if (fgetc(fp) != EOF && linhas == 0) {
        linhas = 1;
    }

    printf("Total de linhas: %d\n", linhas);

    fclose(fp);
    return 0;
}
```

</details>

---

### 🟡 Exercício 2: Processador de Operações Numéricas

> **Enunciado:**  
> Faça um programa que leia um arquivo de texto chamado `entrada.txt`, que contém uma lista de números inteiros (um por linha) e também uma linha adicional no início que especifica uma operação (`soma`, `média` ou `máximo`). O programa deve realizar a operação especificada na primeira linha sobre os números do arquivo.  
> 
> Grave o resultado em um novo arquivo chamado `saida.txt`. Se a operação não for reconhecida, o programa deve salvar uma mensagem de erro em `saida.txt`. Caso o arquivo `entrada.txt` não exista, o programa deve imprimir uma mensagem de erro no console e sair.
> 
> **Exemplo de `entrada.txt`:**
> ```text
> soma
> 10
> 20
> 30
> 40
> ```
> 
> **Saídas esperadas em `saida.txt`:**
> * Para **`soma`**: `Resultado: 100`
> * Para **`média`**: `Resultado: 25.00`
> * Para **`máximo`**: `Resultado: 40`
> * Para **operação não reconhecida**: `Erro: Operação não reconhecida`

#### Solução:

<details>
<summary><b>💡 Clique aqui para ver a solução </b></summary>

<br>

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    FILE *entrada = fopen("entrada.txt", "r"); //note que estamos abrindo o arquivo com "r" pois vamos somente fazer a leitura dele (e não alterá-lo)

    if (entrada == NULL) {
        printf("Erro: Não foi possível abrir o arquivo entrada.txt!\n");
        return 1;
    }

    FILE *saida = fopen("saida.txt", "w"); //já aqui, estamos abrindo o arquivo com "w" pois vamos escrever nele.
    if (saida == NULL) {
        printf("Erro: Não foi possível criar o arquivo saida.txt!\n");
        fclose(entrada);
        return 1;
    }

    char operacao[30];
    
    // lê o nome da operação na primeira linha
    if (fscanf(entrada, "%s", operacao) == EOF) {
        fprintf(saida, "Erro: Arquivo de entrada vazio\n");
        fclose(entrada);
        fclose(saida);
        return 0;
    }

    int val;
    long soma = 0;
    int qtd = 0;
    int max;
    int primeiro = 1;

    // processa a sequência numérica
    while (fscanf(entrada, "%d", &val) != EOF) {
        soma += val;
        
        if (primeiro || val > max) {
            max = val;
            primeiro = 0;
        }
        qtd++;
    }

    // escreve o resultado no arquivo de saída
    if (strcmp(operacao, "soma") == 0) {
        fprintf(saida, "Resultado: %ld\n", soma);
    } 
    else if (strcmp(operacao, "media") == 0 || strcmp(operacao, "média") == 0) {
        if (qtd > 0) {
            double media = (double)soma / qtd;
            fprintf(saida, "Resultado: %.2f\n", media);
        } else {
            fprintf(saida, "Erro: Nenhum número encontrado para cálculo de média\n");
        }
    } 
    else if (strcmp(operacao, "maximo") == 0 || strcmp(operacao, "máximo") == 0) {
        if (qtd > 0) {
            fprintf(saida, "Resultado: %d\n", max);
        } else {
            fprintf(saida, "Erro: Nenhum número encontrado para cálculo do máximo\n");
        }
    } 
    else {
        fprintf(saida, "Erro: Operação não reconhecida\n");
    }

    fclose(entrada);
    fclose(saida);

    printf("Processamento concluído com sucesso. Verifique o arquivo saida.txt.\n");
    return 0;
}
```

</details>

---

<p align="center">
  <a href="../01%20•%20Definições%20Iniciais%20e%20Notação%20Assintótica/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Capitulo_01-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/03%20%E2%80%A2%20Struct%20e%20Ponteiros/README.md">
    <img src="https://img.shields.io/badge/Avancar-Struct_e_Ponteiros_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
