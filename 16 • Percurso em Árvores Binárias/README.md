<h1 align="center">Percurso em Árvores Binárias</h1>

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/15%20%E2%80%A2%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Árvores_Binárias-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/17%20%E2%80%A2%20%C3%81rvore%20de%20Express%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/Avancar-Percurso_em_Árvore_de_Expressão_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

---

## <mark> 1 - Introdução </mark>

No módulo anterior, implementamos a função `imprime` para uma árvore binária:

```c
void imprime(PArv a) {
    if (a != NULL) {
        printf("%c ", a->info);
        imprime(a->esq);
        imprime(a->dir);
    }
}
```

> -> A programação da operação `imprime`, vista anteriormente, seguiu a ordem empregada na definição de árvore binária para decidir a ordem em que as três ações seriam executadas.
>
> -> Entretanto, dependendo da aplicação em vista, esta ordem poderia não ser a preferível, podendo ser utilizada uma ordem diferente desta.

Ou seja: não existe só **um jeito certo** de percorrer uma árvore. A ordem que escolhemos (tratar a raiz primeiro? por último? no meio?) muda dependendo do que queremos fazer com a árvore.

> -> Donald E. Knuth popularizou três ordens de percurso em árvores binárias:
> -  **pré-ordem** ou prefixa,
> -  **simétrica** ou infixa ou em-ordem e
> -  **pós-ordem** ou posfixa.

---

## <mark> 2 - As três ordens de percurso </mark>

> -> Muitas operações em árvores binárias envolvem o percurso de todas as sub-árvores, executando alguma ação de tratamento em cada nó, de forma que é comum percorrer uma árvore em uma das seguintes ordens:
>
> -  **pré-ordem ou prefixa**: trata *raiz*, percorre *sae*, percorre *sad*;
> -  **em-ordem, ordem simétrica ou infixa**: percorre *sae*, trata *raiz*, percorre *sad*;
> -  **pós-ordem ou posfixa**: percorre *sae*, percorre *sad*, trata *raiz*.

(*sae* = sub-árvore esquerda, *sad* = sub-árvore direita)

A única coisa que muda entre as três é **em que momento a raiz é "tratada"** em relação às duas sub-árvores:

| Ordem | Quando trata a raiz | Regra mnemônica |
|---|---|---|
| **Pré-ordem** | **Antes** de visitar os filhos | Raiz vem **primeiro** |
| **Em-ordem** | **Entre** visitar a sub-árvore esquerda e a direita | Raiz fica **no meio** |
| **Pós-ordem** | **Depois** de visitar os filhos | Raiz vem **por último** |

> 💡 **Dica pra nunca mais esquecer:** o nome da ordem indica a posição da palavra "raiz" na sequência "esquerda-raiz-direita". *PRÉ*-ordem → raiz *antes* de tudo. *PÓS*-ordem → raiz *depois* de tudo. *EM*-ordem (ou infixa) → raiz *no meio*, entre esquerda e direita — funciona exatamente como uma expressão matemática escrita normalmente (`operando esquerdo` `operador` `operando direito`), por isso o nome "infixa".

### Implementação em código

Como já vimos, essas funções são naturalmente recursivas — tratam o caso `NULL` e depois só reorganizam a ordem das 3 ações (tratar, ir pra esquerda, ir pra direita):

```c
// Pré-ordem: raiz, esquerda, direita
void preordem(PArv a) {
    if (a != NULL) {
        printf("%c ", a->info);  // trata a raiz
        preordem(a->esq);
        preordem(a->dir);
    }
}

// Em-ordem: esquerda, raiz, direita
void emordem(PArv a) {
    if (a != NULL) {
        emordem(a->esq);
        printf("%c ", a->info);  // trata a raiz
        emordem(a->dir);
    }
}

// Pós-ordem: esquerda, direita, raiz
void posordem(PArv a) {
    if (a != NULL) {
        posordem(a->esq);
        posordem(a->dir);
        printf("%c ", a->info);  // trata a raiz
    }
}
```

>  Repare que as três funções são **idênticas em estrutura** — a única diferença é a posição da linha `printf(...)` em relação às duas chamadas recursivas. Isso mostra bem como a ordem de percurso é só uma questão de "quando" tratar o nó atual, não uma lógica totalmente diferente.

---

## <mark> 3 - Exemplo comentado </mark>

Vamos percorrer a árvore abaixo nas três ordens, para fixar bem o raciocínio:

```
                A
              ┌─┴─┐
              B   C
            ┌─┘   └─┐
            D   E    F   G
           ┌┘         └┐
           H            I
```

(Legenda: B tem filhos D à esquerda e E à direita; D tem apenas H como filho esquerdo; C tem filhos F à esquerda e G à direita; F tem apenas I como filho esquerdo.)

### Pré-ordem (raiz → esquerda → direita)

Começamos em A, tratamos A, depois vamos inteiramente para a sub-árvore esquerda (B) antes de sequer olhar para a direita (C):

> **Pré-ordem: A, B, D, H, E, C, F, I, G**

**Passo a passo:** trata A → entra na sub-árvore de B → trata B → entra na sub-árvore de D → trata D → entra na sub-árvore de H → trata H (H é folha, volta) → volta pra D (não tem filho direito, volta) → volta pra B, agora vai para a direita → trata E (folha) → volta pra A, agora vai para a direita → trata C → entra na sub-árvore de F → trata F → trata I (folha) → volta pra C, vai para a direita → trata G (folha). Fim.

### Em-ordem (esquerda → raiz → direita)

Aqui, só tratamos um nó depois de **esgotar completamente** sua sub-árvore esquerda:

> **Em-ordem: H, D, B, E, A, I, F, C, G**

**Por que H vem primeiro?** Porque, para tratar A, primeiro precisamos esgotar toda a sub-árvore esquerda de A (a de B). Para tratar B, precisamos esgotar toda a sub-árvore esquerda de B (a de D). Para tratar D, precisamos esgotar a sub-árvore esquerda de D (a de H). H não tem filho esquerdo, então H é tratado imediatamente — e é o primeiro de todos!

### Pós-ordem (esquerda → direita → raiz)

Aqui, a raiz só é tratada **depois** de esgotarmos completamente as duas sub-árvores:

> **Pós-ordem: H, D, E, B, I, F, G, C, A**

**Por que A vem por último?** Porque A só pode ser tratado depois que toda a árvore (esquerda e direita) já tiver sido percorrida — e isso vale recursivamente para todo nó: repare que **C também só é tratado depois de F, I e G**, e **B só é tratado depois de D, H e E**.

---

## <mark> 4. Outro exemplo rápido </mark>

Considerando a árvore abaixo:

```
        A
      ┌─┴─┐
      C    B
     ┌┴┐  ┌┴┐
     D E  F G
```

| Ordem | Sequência |
|---|---|
| Prefixa | A, C, D, E, B, F, G |
| Infixa | D, C, E, A, F, B, G |
| Posfixa | D, E, C, F, G, B, A |

Esse exemplo é mais direto porque **todos os nós internos têm exatamente 2 filhos** — não há ambiguidade de "filho único", como tivemos no exemplo da seção anterior.

---

## 📝 Exercícios Práticos

> Dada as seguintes árvores, faça para cada uma delas a visitação dos nós utilizando a ordem prefixa, infixa e posfixa.

<br>

### Árvore (a) 🟢 

```
              A
            ┌─┴─┐
            C   B
           ┌┴┐ ┌┴┐
           D E F G
          ┌┴┐
          H I
```

<details>
<summary>💡 Ver resolução</summary>

**Prefixa (raiz, esquerda, direita):**

Trata A → entra em C → trata C → entra em D → trata D → trata H → trata I → volta, trata E → volta pra A, entra em B → trata B → trata F → trata G.

> **Prefixa: A, C, D, H, I, E, B, F, G**

**Infixa (esquerda, raiz, direita):**

Esgota a esquerda de D (H) → trata D → trata I → volta, trata C → trata E → volta pra A, trata A → esgota esquerda de B (F) → trata B → trata G.

> **Infixa: H, D, I, C, E, A, F, B, G**

**Posfixa (esquerda, direita, raiz):**

Esgota H, I → trata D → trata E → trata C → esgota F, G → trata B → trata A.

> **Posfixa: H, I, D, E, C, F, G, B, A**

</details>

<br>

### Árvore (b) 🟡 

Essa árvore tem alguns nós com **apenas um filho** (E tem só um filho, K, e K também tem só um filho, M) — preste atenção redobrada nesses casos, já que aqui não tem "o outro lado" pra percorrer:

```
              A
            ┌─┴──┐
            C     B
          ┌─┴┐  ┌─┴┐
          D  E  F  G
         ┌┴┐ │ ┌┴┐
         H I K J L
              │
              M
```

<details>
<summary>💡 Ver resolução</summary>

> ⚠️ Nesta árvore, assumimos que os filhos únicos (o `K` de `E`, e o `M` de `K`) estão posicionados à **esquerda**. 

**Prefixa (raiz, esquerda, direita):**

Trata A → entra em C → trata C → trata D, H, I → volta, trata E → trata K (único filho) → trata M (único filho de K) → volta tudo até A → entra em B → trata B → trata F, J, L → trata G.

> **Prefixa: A, C, D, H, I, E, K, M, B, F, J, L, G**

**Infixa (esquerda, raiz, direita):**

Esgota esquerda de D (H) → trata D → trata I → volta, trata C → esgota esquerda de E (que é a subárvore de K, que por sua vez esgota a esquerda de K, que é M) → trata M → trata K → trata E (E não tem filho direito) → volta pra A, trata A → esgota esquerda de B (F: esquerda J, F, direita L) → trata J → trata F → trata L → trata B → trata G.

> **Infixa: H, D, I, C, M, K, E, A, J, F, L, B, G**

**Posfixa (esquerda, direita, raiz):**

Esgota H, I → trata D → esgota (M, então) K → trata E → trata C → esgota (J, L, então) F → trata G → trata B → trata A.

> **Posfixa: H, I, D, M, K, E, C, J, L, F, G, B, A**

</details>

<br>

### Árvore (c) 🔴 

```
              A
            ┌─┴──┐
            C     B
          ┌─┴┐    │
          D  E    F
         ┌┴┐     ┌┴┐
         H I     G  J
                    │
                    K
                   ┌┴┐
                   L  M
```

<details>
<summary>💡 Ver resolução</summary>

> ⚠️ Assumimos que `F` é o filho **esquerdo** de `B` (único filho) e que `K` é o filho **esquerdo** de `J` (único filho). 

**Prefixa (raiz, esquerda, direita):**

Trata A → entra em C → trata C → trata D, H, I → trata E → volta pra A, entra em B → trata B → entra em F (único filho) → trata F → trata G → entra em J → trata J → entra em K (único filho) → trata K → trata L → trata M.

> **Prefixa: A, C, D, H, I, E, B, F, G, J, K, L, M**

**Infixa (esquerda, raiz, direita):**

Esgota esquerda de D (H) → trata D → trata I → trata C → trata E → trata A → esgota esquerda de B (toda a subárvore de F) → dentro de F: esgota esquerda (G) → trata F → esgota direita (subárvore de J: esgota esquerda que é a subárvore de K: esgota L, trata K, trata M, depois trata J) → trata B (B não tem filho direito).

dentro da subárvore de **K**, K tem filhos L (esquerda) e M (direita), então a ordem infixa de K é: `L, K, M`. Como K é filho único (esquerdo) de J, e J não tem filho direito, a ordem infixa de J é: `L, K, M, J`. Como J é filho direito de F, e F já tratou sua esquerda (G) e a si mesmo, a ordem infixa de F é: `G, F, L, K, M, J`.

> **Infixa: H, D, I, C, E, A, G, F, L, K, M, J, B**

**Posfixa (esquerda, direita, raiz):**

Esgota H, I → trata D → trata E → trata C → dentro de B: esgota toda a subárvore de F antes de tratar B. Dentro de F: esgota G, depois esgota a subárvore de J (que esgota a subárvore de K: L, M, K, depois J), depois trata F. Por fim trata B. Por último, trata A.

> **Posfixa: H, I, D, E, C, G, L, M, K, J, F, B, A**

</details>

---

## <mark> 6 - Resumo rápido </mark>

| Ordem | Regra | Quando é mais útil |
|---|---|---|
| Pré-ordem (prefixa) | raiz → esquerda → direita | Copiar/recriar a estrutura da árvore; representações como parênteses aninhados |
| Em-ordem (infixa) | esquerda → raiz → direita | Em árvores binárias de busca, produz os elementos **em ordem crescente** |
| Pós-ordem (posfixa) | esquerda → direita → raiz | Liberar memória da árvore (como fizemos na função `libera`); avaliar expressões matemáticas |

>  **Dica de prova:** para não errar a ordem na hora de resolver um exercício desses, sempre desenhe a árvore primeiro (se ainda não estiver desenhada) e vá "contornando" o desenho com o dedo, seguindo a regra da ordem escolhida. Isso evita esquecer algum nó ou trocar a ordem por engano.

---

<p align="center">
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/15%20%E2%80%A2%20%C3%81rvores%20Bin%C3%A1rias/README.md">
    <img src="https://img.shields.io/badge/⬅️_Voltar-Árvores_Binárias-8b5cf6?style=plastic" alt="Voltar">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="../README.md">
    <img src="https://img.shields.io/badge/🏠_Inicio-Menu-06b6d4?style=plastic" alt="Menu">
  </a>
  &nbsp;&nbsp;&nbsp;&nbsp;
  <a href="https://github.com/furlanxzx/Algoritmo-e-Estrutura-de-Dados-1/blob/main/17%20%E2%80%A2%20%C3%81rvore%20de%20Express%C3%A3o/README.md">
    <img src="https://img.shields.io/badge/Avancar-Percurso_em_Árvore_de_Expressão_➡️-8b5cf6?style=plastic" alt="Avançar">
  </a>
</p>

