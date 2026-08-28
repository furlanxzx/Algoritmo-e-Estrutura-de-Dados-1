<h1 align="center">  Definições Iniciais e Notação Assintótica</h1>
<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/tree/main/01%20%E2%80%A2%20Defini%C3%A7%C3%B5es%20Iniciais%20e%20Nota%C3%A7%C3%A3o%20Assint%C3%B3tica#readme">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Capitulo_01-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/tree/main">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/02%20%E2%80%A2%20Manipula%C3%A7%C3%A3o%20de%20Arquivos/README.md">
    <img src="https://img.shields.io/badge/Avancar-Arquivos_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

## 📌 Introdução - Conceitos Iniciais
* **Algoritmo:** Sequência finita de passos/instruções executáveis para resolver um problema (ex: receita de bolo ou a operação $a + b$).
* **Estrutura de Dados:** A forma como organizamos e armazenamos os dados na memória para que possam ser usados de maneira eficiente.
* **Programa:** $\text{Programa} = \text{Algoritmo} + \text{Estrutura de Dados}$. É a tradução dessa lógica abstrata para uma linguagem de programação que o computador consiga executar.

---

## <mark> 1 - Tipos de Dados</mark>

| Tipo | O que é | Exemplo / Aplicação |
| :--- | :--- | :--- |
| **Simples (Primitivo)** | Valores únicos e indivisíveis. | `int`, `float`, `char`, `double` |
| **Estruturado** | Agrupamento de vários dados simples. | `vetor` (array), `struct` |
| **Abstrato (TDA)** | Modelo definido pelas **operações** (o *que* faz), escondendo a implementação (o *como* faz). | `Pilha`, `Fila`, `Lista Encadeada` |

---

## <mark> 1.1 - Detalhes sobre o TDA:</mark>

A ideia é simples: **o TDA separa o uso da implementação**.

* **O que faz (Interface / Operações):** É a lista de comandos que você disponibiliza para quem for usar o seu código (ex: `inserir()`, `remover()`, `buscar()`). Quem usa só precisa saber a função de cada comando.

* **Como faz (Implementação):** É o código em C "escondido" por trás dessas funções (se usou ponteiros, vetores, alocação dinâmica, etc.). Quem usa a estrutura não precisa se preocupar com isso.

>  **Na prática da disciplina:**  
> Vocês enxergarão essa ideia com total clareza nos próximos tópicos de **Pilha**, **Fila** e **Listas Encadeadas**. Nesses módulos, quem utiliza a estrutura precisa apenas da lista de comandos oferecidos — a implementação em C fica totalmente escondida por trás dos panos. É exatamente essa separação entre o que é visível ao público e o que está codificado por dentro que chamamos de **Tipo de Dado Abstrato**.
---

## <mark> 2 - Introdução à Análise de Algoritmos</mark>

Analisar um algoritmo serve para prever seu consumo de **tempo** e **memória**.

* **Objetivo:** Encontrar o algoritmo **ótimo** (o menor custo teórico possível para resolver um problema).
* **Tempo Real (Segundos):** **Não serve como padrão**, pois muda conforme o hardware, compilador e memória do computador.
* **Modelo Matemático:** A forma correta de medir. Em vez de segundos, contamos apenas as **operações principais** (ex: quantas comparações um algoritmo de ordenação faz).

> [!IMPORTANT]
> **Comentário:**
> Esse módulo é o mais extenso, aos alunos que fazem com a professora Regina, ela informou em sala no início de 2026 que não cobraria nenhuma análise de notação assintótica complexa (eu pessoalmente nunca vi cair, mas isso está sujeito a mudança dependendo do humor dela) e que se ela fosse cobrar algo seria em um algoritmo simples, onde a complexidade seria **$O(n)$** ou **$O(n^2)$** (explicação do que é isso mais pra baixo), aconselho focar mais o tempo em Manipulação de Arquivos, Ponteiros, Pilha e Fila, em todo caso, se alguém quiser o conteúdo completo vai estar aqui explicado.

---
##  <mark> 2.1 - Função de Complexidade $f(n)$</mark>

A **Função de Complexidade $f(n)$** mede o custo de um algoritmo em função do tamanho da entrada ($n$).

* **Complexidade de Tempo:** Conta quantas **operações relevantes** o código executa (não mede segundos reais).
* **Complexidade de Espaço:** Mede a **memória** necessária para rodar o algoritmo.

---

##  <mark> 2.2 - Cenários de Execução</mark>

Um mesmo algoritmo pode ter desempenhos diferentes dependendo da organização dos dados de entrada:

| Cenário | O que representa | Importância |
| :--- | :--- | :--- |
| **Melhor Caso** | Menor tempo/custo possível para a entrada $n$. | Pouco prático, serve apenas como limite inferior. |
| **Pior Caso** | Maior tempo/custo possível para a entrada $n$. | **O mais importante!** Garante o limite máximo (teto) de custo. |
| **Caso Médio** | Média ponderada do tempo para todas as entradas de tamanho $n$. | Exige cálculo de **probabilidade** ($p_i$) de cada entrada ocorrer. |

---

##  📊 Exemplo 1: Pesquisa Sequencial (Busca Simples)

Imagine que você tem uma folha de papel com $n$ nomes anotados em ordem aleatória (uma **lista não ordenada**). Você precisa encontrar um nome específico nessa lista (esse dado procurado é o que chamamos de **chave**).

O algoritmo mais simples para isso é a **pesquisa sequencial**: olhar item por item, do primeiro ao último, até encontrar.

* **Operação Relevante:** Contamos quantas **comparações** são feitas (ou seja, quantas vezes olhamos para um nome e perguntamos: *"É este o nome que procuro?"*).
* **Melhor Caso:** $f(n) = 1$  
  *(Muita sorte: o nome procurado é logo o primeiro da lista).*
* **Pior Caso:** $f(n) = n$  
  *(Azar completo: o nome é o último da lista, ou nem está lá, te obrigando a olhar os $n$ nomes).*

---

### <mark> 2.3 - Calculando o Caso Médio</mark>

Se o nome estiver na lista e tiver a **mesma chance** de estar em qualquer posição, qual é a média de nomes que precisaremos checar?

1. Como existem $n$ posições possíveis, a chance de o elemento estar em uma posição específica é de $p_i = \frac{1}{n}$.
2. O custo médio é a soma do número de checagens ($1, 2, 3 \dots n$) multiplicada pela chance de cada uma acontecer:
   $$f(n) = \left(1 \times \frac{1}{n}\right) + \left(2 \times \frac{1}{n}\right) + \left(3 \times \frac{1}{n}\right) + \dots + \left(n \times \frac{1}{n}\right)$$

3. Colocamos o termo $\frac{1}{n}$ em evidência:
   $$f(n) = \frac{1}{n} \times (1 + 2 + 3 + \dots + n)$$

4. Aplicamos a fórmula da soma de $1$ até $n$ (Soma de Gauss: $1 + 2 + \dots + n = \frac{n(n + 1)}{2}$):
   $$f(n) = \frac{1}{n} \times \frac{n(n + 1)}{2} = \frac{n + 1}{2}$$

> 💡 **O que isso significa na prática?**  
> Se você tiver $100$ nomes na lista, em média precisará olhar $\frac{100 + 1}{2} = 50,5$ nomes. Ou seja: no caso médio de uma busca simples, você examina **aproximadamente a metade dos dados** ($\approx \frac{n}{2}$).

---

## 📊 Exemplo 2: Encontrando o Maior e o Menor Elemento (MaxMin1)

O problema consiste em encontrar, simultaneamente, o **maior** e o **menor** valor dentro de um vetor $A$ contendo $n$ elementos.

```c
void MaxMin1(int A[], int n, int *Max, int *Min) {
    int i;
    *Max = A[0]; 
    *Min = A[0];

    for (i = 1; i < n; i++) {
        if (A[i] > *Max)
            *Max = A[i];
        if (A[i] < *Min)
            *Min = A[i];
    }
}
```
Análise de Complexidade Passo a Passo

Para descobrir a **função de complexidade $f(n)$**, precisamos apenas contar quantas **comparações entre números** o computador precisa fazer.

1. **Quantos números o laço analisa?**  
   Antes de começar o laço `for`, o algoritmo pega o **primeiro número** (`A[0]`) e define que ele é tanto o maior quanto o menor temporariamente. Como o primeiro já foi usado, sobram **$n - 1$ números** para testar.

2. **Quantas comparações acontecem a cada volta?**  
   Para cada número testado no laço, o algoritmo faz **2 perguntas obrigatórias**:
   * **1ª comparação:** *"Este número é maior que o Max atual?"* (`if (A[i] > *Max)`)
   * **2ª comparação:** *"Este número é menor que o Min atual?"* (`if (A[i] < *Min)`)

3. **Calculando o Custo Total:**  
   Se o algoritmo faz **2 comparações** para cada um dos **$n - 1$ números** restantes:
   $$f(n) = 2 \times (n - 1)$$

---

### <mark> 2.4 -  Por que o Melhor, Pior e Caso Médio são iguais aqui?</mark>

Veja que o código **não tem atalhos**: mesmo se a primeira pergunta for verdadeira (*"o número é maior"*), ele ainda é obrigado a executar a segunda pergunta (*"o número é menor"*).

Portanto, não importa se a sua lista de números está ordenada, invertida ou totalmente aleatória: o computador **sempre** fará exatamente o mesmo número de comparações.

* **Melhor caso:** $f(n) = 2(n - 1)$
* **Pior caso:** $f(n) = 2(n - 1)$
* **Caso médio:** $f(n) = 2(n - 1)$

---

## 📊 Exemplo 3: Otimizando o MaxMin (MaxMin2)

No algoritmo anterior (`MaxMin1`), percebemos uma ineficiência: se um número é **maior** que o máximo atual, é impossível ele ser ao mesmo tempo **menor** que o mínimo. 

A solução do `MaxMin2` é trocar o segundo `if` por um `else if` (um atalho condicional).

```c
void MaxMin2(int A[], int n, int *Max, int *Min) {
    int i;
    *Max = A[0]; 
    *Min = A[0];

    for (i = 1; i < n; i++) {
        if (A[i] > *Max) {
            *Max = A[i];
        } else if (A[i] < *Min) {
            *Min = A[i];
        }
    }
}
```

---

### <mark> 2.5 - Por que a ordem dos dados agora faz diferença?</mark>

Diferente do `MaxMin1`, onde fazíamos 2 perguntas para cada número não importando a situação, o uso do `else if` faz com que o comportamento mude de acordo com a organização da lista:

* **Melhor Caso: $f(n) = n - 1$**
  * **Cenário:** Elementos em **ordem crescente** (ex: `[10, 20, 30, 40]`).
  * **O que acontece:** Cada novo número analisado é sempre maior que o `Max` anterior. O primeiro `if` é **verdadeiro**, e o computador ignora completamente o `else if`.
  * **Custo:** Faz apenas **1 comparação** por elemento para os $n - 1$ números restantes.

* **Pior Caso: $f(n) = 2(n - 1)$**
  * **Cenário:** Elementos em **ordem decrescente** (ex: `[40, 30, 20, 10]`).
  * **O que acontece:** Cada novo número é menor que o anterior. O primeiro `if` dá **falso**, obrigando o computador a testar a segunda pergunta no `else if`.
  * **Custo:** Faz **2 comparações** por elemento para os $n - 1$ números restantes.

> 💡 **Resumo da melhoria:**  
> No pior cenário, o `MaxMin2` empata com o `MaxMin1`. Porém, no melhor cenário, ele executa **metade das comparações**, mostrando como um simples atalho no código melhora o desempenho médio!

---

## 📊 Exemplo 4: Divisão por Pares (MaxMin3)

Para otimizar ainda mais o algoritmo, o `MaxMin3` utiliza uma estratégia matemática esperta: em vez de testar cada número individualmente contra o maior e o menor, ele processa os elementos **aos pares** (de 2 em 2).

### <mark> 2.6 - A Ideia Intuitiva </mark>

Se pegarmos dois números quaisquer, basta **1 comparação** para descobrir qual é o maior e qual é o menor da dupla:
1. O **maior do par** só precisa disputar o título com o `Max` global.
2. O **menor do par** só precisa disputar com o `Min` global.

Assim, para cada **2 elementos**, fazemos apenas **3 comparações** no total (em vez de 4 comparações do método tradicional).

---

```c
void MaxMin3(int A[], int *Max, int *Min, int n) {
    int i, FimDoAnel;

    // Se a quantidade de elementos for ímpar, duplica o último item para formar um par
    if (n % 2 > 0) {
        A[n] = A[n - 1]; 
        FimDoAnel = n;
    } else {
        FimDoAnel = n - 1;
    }

    // Inicializa Max e Min comparando os dois primeiros elementos
    if (A[0] > A[1]) {
        *Max = A[0];
        *Min = A[1];
    } else {
        *Max = A[1];
        *Min = A[0];
    }

    // Processa os elementos restantes de 2 em 2 (i = i + 2)
    i = 2;
    while (i < FimDoAnel) {
        if (A[i] > A[i + 1]) {
            if (A[i] > *Max)     *Max = A[i];     // Compara o maior do par com Max
            if (A[i + 1] < *Min) *Min = A[i + 1]; // Compara o menor do par com Min
        } else {
            if (A[i] < *Min)     *Min = A[i];     // Compara o menor do par com Min
            if (A[i + 1] > *Max) *Max = A[i + 1]; // Compara o maior do par com Max
        }
        i = i + 2;
    }
}
```

---

###  <mark> 2.7 - Análise de Complexidade Passo a Passo </mark>

A contagem de comparações da estrutura principal funciona assim:

1. **Separação em Pares:**  
   Compara os dois elementos de cada dupla entre si.  
   $$\text{Custo} = \frac{n}{2} \text{ comparações}$$

2. **Ajuste do Máximo:**  
   Compara o vencedor de cada dupla com o `Max` atual.  
   $$\text{Custo} = \frac{n - 2}{2} \text{ comparações}$$

3. **Ajuste do Mínimo:**  
   Compara o perdedor de cada dupla com o `Min` atual.  
   $$\text{Custo} = \frac{n - 2}{2} \text{ comparações}$$

####  Soma Total da Função $f(n)$:
$$f(n) = \frac{n}{2} + \frac{n - 2}{2} + \frac{n - 2}{2}$$

$$f(n) = \frac{n + (n - 2) + (n - 2)}{2} = \frac{3n - 4}{2} = \frac{3n}{2} - 2$$

---

### <mark> 2.8 - Comparando o Ganho de Desempenho </mark>

Assim como no `MaxMin1`, este algoritmo executa o mesmo fluxo de instruções independentemente da ordem dos dados. Portanto, a fórmula **$f(n) = \frac{3n}{2} - 2$** vale para o **melhor caso**, **pior caso** e **caso médio**.

Para entender o ganho prático em uma lista com **$n = 100$ elementos**:

* **MaxMin1 (Pior/Médio/Melhor):** $2(100 - 1) = 198$ comparações.
* **MaxMin3 (Pior/Médio/Melhor):** $\frac{3(100)}{2} - 2 = 148$ comparações.

O algoritmo reduziu em **25%** o número total de operações necessárias.

---

## 📊 Comparação Final entre MaxMin1, MaxMin2 e MaxMin3

Agora que vimos os três algoritmos, vamos comparar o número de comparações de cada um lado a lado:

| Algoritmo | Melhor Caso $f(n)$ | Pior Caso $f(n)$ |
| :--- | :---: | :---: |
| **MaxMin1** | $2(n - 1)$ | $2(n - 1)$ |
| **MaxMin2** | $n - 1$ | $2(n - 1)$ |
| **MaxMin3** | $\frac{3n}{2} - 2$ | $\frac{3n}{2} - 2$ |

###  Conclusões Importantes:
* **MaxMin2 vs MaxMin1:** O `MaxMin2` nunca é pior que o `MaxMin1`. Na maioria das vezes (e especialmente no melhor caso), ele faz menos comparações.
* **MaxMin3 vs MaxMin2:** O `MaxMin3` é superior ao `MaxMin2` no pior caso (reduz de $2n$ para $1.5n$ comparações). No caso médio, ambos têm desempenhos muito próximos.

---

## <mark> 2.9 - Comportamento Assintótico de Funções </mark>

Quando analisamos algoritmos, o tamanho da entrada $n$ (quantidade de dados) determina o nível de dificuldade do problema.

### <mark> Por que focamos em valores GRANDES de $n$? </mark>
* **Para $n$ pequeno (ex: $n = 5$ ou $n = 10$):** Qualquer algoritmo roda em frações de milissegundo, mesmo os mais ineficientes. A escolha do algoritmo não faz diferença prática.
* **Para $n$ grande (ex: $n = 1.000.000$):** A diferença entre um algoritmo eficiente e um ineficiente pode ser a diferença entre rodar em **1 segundo** ou demorar **3 dias**.

> 📌 **Comportamento Assintótico:** É o estudo de como o custo de um algoritmo cresce quando a quantidade de dados ($n$) tende ao infinito.

---

## <mark> 3 - Dominação Assintótica e Notação $O$ </mark>

Para comparar algoritmos sem nos preocuparmos com detalhes irrelevantes de hardware, usamos a **Dominação Assintótica**.

## <mark> 3.1 - Definição Formal </mark>
Dizemos que uma função $f(n)$ **domina assintoticamente** $g(n)$ se, para valores grandes de $n$, a função $c \cdot f(n)$ fica sempre acima de $g(n)$.

$$\text{Existe uma constante } c > 0 \text{ e um ponto inicial } m \text{ tais que: } |g(n)| \le c \cdot |f(n)| \quad \forall n \ge m$$

---

## <mark> 3.2 - A Notação Big-O: $g(n) = O(f(n))$ </mark>
Lê-se: *"g(n) é da ordem de no máximo f(n)"*.

A Notação $O$ estabelece um **teto máximo** (limite superior) para o tempo de execução. Ela garante que o algoritmo **nunca será pior** do que aquela taxa de crescimento.

* **Exemplo:** Se um programa tem tempo de execução $T(n) = O(n^2)$, significa que, conforme $n$ cresce, o tempo total não vai crescer mais rápido do que uma curva quadrática $n^2$.

---

##  <mark> 3.3 - Regras Práticas para Simplificar a Notação $O$ </mark>

Para quem está aprendendo, a regra de ouro para encontrar o Big-O de qualquer expressão matemática é:
1. **Ignore constantes multiplicativas** (ex: $3n \rightarrow n$).
2. **Mantenha apenas o termo de MAIOR crescimento** (jogue fora os termos menores).

###  Exemplos Passo a Passo:

1. **Exemplo 1:** $g(n) = (n + 1)^2$
   * Desenvolvendo a equação: $g(n) = n^2 + 2n + 1$
   * O termo que cresce mais rápido é $n^2$.
   * **Resultado:** $g(n) = O(n^2)$

2. **Exemplo 2:** $g(n) = 3n^3 + 2n^2 + n$
   * Ignoramos as constantes ($3$ e $2$) e os termos menores ($2n^2$ e $n$).
   * O termo dominante é $n^3$.
   * **Resultado:** $g(n) = O(n^3)$

---

##  <mark> 3.4 - Logaritmos na Notação $O$ </mark>

Na Notação $O$, a base do logaritmo **não importa**: $\log_2 n$, $\log_5 n$ e $\log_{10} n$ pertencem todos à mesma classe $O(\log n)$.

### Por que a base do logaritmo não importa?
Pela propriedade da mudança de base matemática:
$$\log_b n = \log_c n \times \log_b c$$

Como $\log_b c$ é apenas um **número constante**, ele é ignorado na análise assintótica.

> **Exemplo:** $g(n) = \log_5 n \Rightarrow O(\log n)$

---

## <mark> 3.5 - Operações Regras com a Notação $O$ </mark>

Quando um programa possui vários blocos de código executados em sequência, o tempo total é a soma do tempo de cada bloco.

### Propriedades da Notação $O$:

| Operação | Resultado | O que significa? |
| :--- | :---: | :--- |
| $c \times O(f(n))$ | $O(f(n))$ | Multiplicar por constante não muda o Big-O. |
| $O(f(n)) + O(f(n))$ | $O(f(n))$ | Somar funções de mesma ordem mantém a mesma ordem. |
| **$O(f(n)) + O(g(n))$** | **$O(\max(f(n), g(n)))$** | **Regra da Soma: O pior trecho domina o código!** |
| $O(f(n)) \times O(g(n))$ | $O(f(n) \cdot g(n))$ | Laços aninhados (um `for` dentro de outro) multiplicam os custos. |

---

### 💡 Exemplo da Regra da Soma:
Imagine um programa com 3 etapas consecutivas:
1. Etapa 1: Executa em $O(n)$
2. Etapa 2: Executa em $O(n^2)$
3. Etapa 3: Executa em $O(n \log n)$

$$T(n) = O(n) + O(n^2) + O(n \log n)$$

Como o termo $n^2$ cresce muito mais rápido que $n$ e $n \log n$, dizemos que o custo total do programa é:
$$T(n) = O(\max(n, n^2, n \log n)) = O(n^2)$$

>  **Resumindo:** O tempo total do programa é sempre definido pelo **seu gargalo mais lento**.

---

##  <mark> 3.6 - Comportamento Assintótico e Constantes </mark>

O **comportamento assintótico** analisa o desempenho e o crescimento de um algoritmo conforme o volume de dados tende ao infinito ($n \to \infty$), desconsiderando constantes de proporcionalidade.

* **Constantes são ignoradas no Big-O:** Algoritmos que diferem apenas por um fator constante pertencem à mesma classe. Se $f(n) = 3g(n)$, então $O(f(n)) = O(g(n))$.
* **Importância do Tamanho ($n$):** Para entradas pequenas, as constantes importam. Para entradas grandes, a classe de crescimento assintótico domina completamente.

> 📌 **Ponto de Atenção:** Um algoritmo $O(n)$ sempre superará um $O(n^2)$ quando $n$ escala, tornando funções quadráticas inviáveis para grandes volumes de dados.

---

## <mark> 3.7 - Ponto de Virada Prático: $100n$ vs $2n^2$ </mark>

Nem sempre o algoritmo de menor Big-O é o mais rápido para **qualquer** cenário. As constantes de proporcionalidade podem inverter o resultado quando a entrada de dados é reduzida.

* **Para $n < 50$:** O algoritmo quadrático $2n^2$ executa **mais rápido** que o linear $100n$ (ex: para $n = 10$, temos $200$ operações contra $1.000$).
* **Para $n \ge 50$:** O crescimento quadrático domina, e o algoritmo $O(n)$ passa a ser infinitamente superior.

---

## <mark> 3.8 - Classes de Complexidade </mark>

| Classe | Nome | Comportamento ao Dobrar $n$ | Aplicação Típica |
| :--- | :--- | :--- | :--- |
| **$O(1)$** | Constante | Inalterado (independe de $n$) | Instruções com número fixo de execuções. |
| **$O(\log n)$** | Logarítmica | Crescimento irrisório ($\approx +1$ op) | Divisão sucessiva em subproblemas menores. |
| **$O(n)$** | Linear | Tempo dobra ($\times 2$) | Varredura ou trabalho simples sobre $n$ itens. |
| **$O(n \log n)$** | Linear-logarítmica | Pouco mais que dobra | Divisão e Conquista (ex: ordenações eficientes). |
| **$O(n^2)$** | Quadrática | Tempo quadruplica ($\times 4$) | Laços aninhados (processamento em pares). |
| **$O(n^3)$** | Cúbica | Tempo multiplica por $8$ | Três laços aninhados (viável só para $n$ pequeno). |
| **$O(2^n)$** | Exponencial | Tempo eleva ao quadrado | Força bruta (inviável na prática). |
| **$O(n!)$** | Fatorial | Explosão computacional | Permutações de força bruta (ex: Caixeiro Viajante). |

---

##  <mark> 3.9 - Hierarquia Definitiva de Eficiência </mark>

A regra para classificar o desempenho da melhor (mais rápida) para a pior (mais lenta) classe é:

$$O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(n^3) < O(2^n) < O(n!)$$

Quanto mais à esquerda da cadeia o seu código estiver, mais escalável e eficiente ele será na prática.

---
<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/tree/main/01%20%E2%80%A2%20Defini%C3%A7%C3%B5es%20Iniciais%20e%20Nota%C3%A7%C3%A3o%20Assint%C3%B3tica#readme">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Capitulo_01-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/tree/main">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/02%20%E2%80%A2%20Manipula%C3%A7%C3%A3o%20de%20Arquivos/README.md">
    <img src="https://img.shields.io/badge/Avancar-Arquivos_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>
