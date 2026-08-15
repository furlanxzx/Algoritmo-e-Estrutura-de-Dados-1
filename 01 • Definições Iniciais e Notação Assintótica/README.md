# 01 • Definições Iniciais e Notação Assintótica

## 📌 Introdução - Conceitos Iniciais
* **Algoritmo:** Sequência finita de passos/instruções executáveis para resolver um problema (ex: receita de bolo ou a operação $a + b$).
* **Estrutura de Dados:** A forma como organizamos e armazenamos os dados na memória para que possam ser usados de maneira eficiente.
* **Programa:** $\text{Programa} = \text{Algoritmo} + \text{Estrutura de Dados}$. É a tradução dessa lógica abstrata para uma linguagem de programação que o computador consiga executar.

---

## 📊 Tipos de Dados

| Tipo | O que é | Exemplo / Aplicação |
| :--- | :--- | :--- |
| **Simples (Primitivo)** | Valores únicos e indivisíveis. | `int`, `float`, `char`, `double` |
| **Estruturado** | Agrupamento de vários dados simples. | `vetor` (array), `struct` |
| **Abstrato (TDA)** | Modelo definido pelas **operações** (o *que* faz), escondendo a implementação (o *como* faz). | Pilha, Fila, Lista Encadeada |

---

## 💡 Detalhes sobre o TDA:

A ideia é simples: **o TDA separa o uso da implementação**.

* **O que faz (Interface / Operações):** É a lista de comandos que você disponibiliza para quem for usar o seu código (ex: `inserir()`, `remover()`, `buscar()`). Quem usa só precisa saber a função de cada comando.

* **Como faz (Implementação):** É o código em C "escondido" por trás dessas funções (se usou ponteiros, vetores, alocação dinâmica, etc.). Quem usa a estrutura não precisa se preocupar com isso.

>  **Na prática da disciplina:**  
> Vocês enxergarão essa ideia com total clareza nos próximos tópicos de **Pilha**, **Fila** e **Listas Encadeadas**. Nesses módulos, quem utiliza a estrutura precisa apenas da lista de comandos oferecidos — a implementação em C fica totalmente escondida por trás dos panos. É exatamente essa separação entre o que é visível ao público e o que está codificado por dentro que chamamos de **Tipo de Dado Abstrato**.
---





