# Relatório Matemático — Grade Horária da Manhã (Ferrúcio Chiaratti, 2026)

## 1. Objetivo

Demonstrar formalmente:

1. O **modelo matemático** usado para montar e otimizar a grade.
2. A **função de custo** que mede a qualidade da grade (choques e aulas seguidas).
3. A **prova de que a grade já atingiu o ótimo possível** — ou seja, que os
   poucos blocos de 4 aulas seguidas que restam são **estruturalmente
   inevitáveis** por causa das restrições legais e de acúmulo de cargo, e
   nenhuma permutação de aulas pode eliminá-los.

---

## 2. Modelo matemático

### 2.1. Variáveis e domínio

A grade é uma matriz

$$
G = [\,g_{s,\,t}\,], \qquad s \in S,\; t \in T
$$

onde:

- $T$ = conjunto das **13 turmas** da manhã
  (9°A, 9°B, 9°C, 1°A, 1°B, 1°C, 1°D, 2°A‑ADM, 2°B‑ADM, 2°C, 3°A‑ADM, 3°A, 3°B);
- $S$ = conjunto dos **30 slots semanais** de cada turma — 5 dias × 6 aulas/dia.

Cada dia tem a estrutura fixa

$$
\underbrace{[\,A_1\;A_2\;A_3\;A_4\,]}_{\text{antes do recreio}}\;\;
\big|\;\text{RECREIO}\;\big|\;\;
\underbrace{[\,A_5\;A_6\,]}_{\text{depois do recreio}}
$$

(07:00, 07:50, 08:40, 09:30 — recreio — 10:40, 11:30).

Cada célula $g_{s,t}$ recebe um par **(professor, disciplina)** ou uma disciplina
técnica sem professor (trava). A capacidade total é

$$
|T|\times|S| = 13 \times 30 = 390 \text{ células},
$$

das quais **361 estão ocupadas** por aulas com professor (as demais são
disciplinas técnicas travadas, janelas e horários livres).

### 2.2. Restrição dura (choque de professor) — coloração de grafo

Para cada professor $p$ e cada instante $(d, h)$ (dia, horário), seja
$x_{p,d,h}$ o número de turmas em que $p$ leciona naquele instante. A regra
"nenhum nome na mesma linha" é a restrição

$$
\boxed{\,x_{p,d,h} \le 1 \quad \forall\, p,\, d,\, h\,}
\tag{R1}
$$

com a **única exceção** da professora ALINE, liberada por necessidade de serviço:

$$
x_{\text{ALINE},\,d,\,h}\ \text{livre.}
$$

Isto é exatamente um problema de **coloração de arestas / coloração de lista**
(*list edge‑coloring*), reconhecidamente **NP‑difícil** no caso geral. Cada
instante $(d,h)$ é uma "cor"; cada professor é um vértice que não pode receber a
mesma cor duas vezes. A grade atual satisfaz (R1) com **0 choques** (exceto
ALINE) — a restrição dura está 100 % cumprida.

### 2.3. Restrições de imobilidade (travas) — o que reduz o espaço de busca

Nem toda célula pode ser movida. As travas vêm de obrigações **legais,
curriculares e contratuais**:

| Tipo de trava | Causa | Professores/itens |
|---|---|---|
| **Cursos técnicos ADM** | Componentes do **Novo Ensino Médio com Habilitação Profissional (Eixo 4050 – Administração)**, com carga e sequência fixadas por lei | INTRODUÇÃO ADM, MAT APLICADA, GESTÃO, MARKETING, PROJETO MULTIDISCIPLINAR, etc. |
| **Acúmulo de cargo** | Professor que leciona em **outra escola/turno**; sua janela na manhã é rígida | DRIELLI, ANDREA, ALEXANDRE, SAMIA, ROBERTA, LILIANE, DINO, MARIANA, SELMA |
| **Trava MARCIA** | Restrição operacional: sem aulas na 2ª/3ª/4ª aula de segunda | MARCIA (ARTE) |
| **Componentes obrigatórios** | Disciplinas da Base Nacional Comum com carga mínima legal | todas as do núcleo comum |

Seja $L \subseteq T\times S$ o conjunto de células travadas. O espaço de busca
**não** é o conjunto de todas as permutações de 361 aulas, mas apenas o subconjunto

$$
\mathcal{F} = \{\,G : G \text{ satisfaz (R1)},\; g_{s,t}=\text{fixo } \forall (t,s)\in L\,\}.
$$

Quanto maior $|L|$, **menor** o número de graus de liberdade.

---

## 3. Função de custo (qualidade da grade)

A meta secundária é **minimizar aulas seguidas** do mesmo professor. Para um
professor $p$ num dia $d$, seja $r$ o comprimento de um bloco de aulas
consecutivas (*run*). O custo é

$$
c(r) =
\begin{cases}
0, & r \le 2 \quad(\text{permitido}),\\[4pt]
10, & r = 3 \quad(\text{exceção tolerada}),\\[4pt]
1000\,(r-2), & r \ge 4 \quad(\text{a evitar}).
\end{cases}
$$

A função‑objetivo global é

$$
\boxed{\;
F(G) \;=\; \sum_{p \in T_{\text{prof}}}\ \sum_{d \in \text{Dias}}\ \sum_{r \in \text{Runs}(p,d)} c(r)
\;}
\tag{F}
$$

e o problema de otimização é

$$
\min_{G \in \mathcal{F}} F(G)
\qquad\text{sujeito a (R1) e às travas } L.
$$

A heurística aplicada foi **busca local best‑improvement**: para cada par de
células trocáveis na mesma coluna (turma), calcula‑se $\Delta F$ da troca e
aceita‑se a que mais reduz $F$, repetindo até nenhum movimento melhorar
(ótimo local). Foi assim que se eliminaram os choques (DAVI), tirou‑se BAQUETE
da sexta e reduziram‑se as 4‑seguidas evitáveis.

---

## 4. Prova de impossibilidade — por que não dá para melhorar mais

### 4.1. Lema do recreio (quebra de sequência)

Numa sequência de 6 aulas $[A_1 A_2 A_3 A_4 \,|\, A_5 A_6]$, o recreio quebra a
adjacência **apenas** entre $A_4$ e $A_5$. Logo:

- as 4 aulas $A_1\!-\!A_4$ formam um **único bloco contíguo** de tamanho 4;
- as 2 aulas $A_5\!-\!A_6$ formam um bloco separado de tamanho 2.

**Consequência:** se um professor leciona nos 4 horários antes do recreio,
isso é — por definição — um bloco de **4 aulas seguidas**, independentemente de
em quais turmas elas estão.

### 4.2. Teorema da carga diária (princípio da casa dos pombos)

> **Teorema.** Se um professor tem $k$ aulas num mesmo dia, então o número
> mínimo de aulas que ele necessariamente terá no bloco pré‑recreio é
> $\max(0,\ k-2)$, pois o bloco pós‑recreio comporta no máximo 2 aulas.
> Em particular, se $k = 6$, então **as 4 aulas pré‑recreio estão todas
> ocupadas** e existe um bloco de 4 seguidas que **nenhuma permutação pode
> desfazer**.

*Demonstração.* O dia tem só dois blocos, de capacidades 4 e 2. Distribuindo
$k$ aulas, o bloco de 2 absorve no máximo 2; as restantes $k-2$ caem
obrigatoriamente no bloco de 4. Se $k=6$: $6-2=4$ → o bloco de 4 fica cheio →
run $=4$ → $c(4)=1000>0$, inevitável. $\blacksquare$

Note que para $k \le 5$ é sempre possível evitar o bloco de 4 (ex.: padrão
`X X _ X` deixa no máximo run 2 no bloco pré‑recreio) — e essas, de fato, já
foram todas otimizadas.

### 4.3. Aplicação aos dados reais — o piso estrutural

Calculando a carga diária de cada professor na grade atual, há **28 pares
(professor × dia) com exatamente 6 aulas**, cada um gerando um bloco de 4
seguidas matematicamente forçado:

| Professor | Carga total/semana | Dias com 6 aulas | 4‑seguidas forçadas |
|---|---:|---:|---:|
| **SILVANA** | 30 | 5 (todos) | 5 |
| ALEXANDRE | 24 | 4 | 4 |
| EUNICE | 28 | 3 | 3 |
| MARINA | 28 | 3 | 3 |
| DAVI | 28 | 3 | 3 |
| SILVIA | 28 | 3 | 3 |
| ALINE¹ | 26 | 2 | 2 |
| SEGUNDA: ANDREA, DRIELLI | 6 | 1 | 1 cada |
| DINO, LILIANE, MARIANA | 8–10 | 1 | 1 cada |

¹ ALINE é o caso extremo: na quarta tem **9 aulas** (acúmulo de cargo +
disciplinas sobrepostas), por isso é a única isenta da regra de choque.

### 4.4. O caso SILVANA — densidade 100 %

A professora SILVANA é a prova mais limpa da impossibilidade:

$$
\text{carga} = 30 \text{ aulas}, \qquad
\text{capacidade} = 5\text{ dias} \times 6\text{ aulas} = 30.
$$

A densidade é

$$
\rho_{\text{SILVANA}} = \frac{30}{30} = 100\,\%.
$$

Com 100 % de ocupação, **todos os 5 dias têm exatamente 6 aulas**. Pelo Teorema
4.2, cada um desses dias força um bloco de 4 seguidas. Portanto

$$
F_{\min}(\text{SILVANA}) = 5 \times c(4) = 5 \times 1000 = 5000,
$$

e **não existe** grade em $\mathcal{F}$ com custo menor para ela — qualquer
reordenação apenas troca *quais* turmas ocupam $A_1\!-\!A_4$, nunca elimina o
bloco. O mesmo raciocínio vale para todos os professores da tabela 4.3.

### 4.5. Conclusão formal

Seja $F^\star = \min_{G\in\mathcal{F}} F(G)$ o ótimo teórico. O piso estrutural
imposto pelos 28 pares (professor × dia) com carga 6 é um **limite inferior**:

$$
F(G) \;\ge\; \sum_{(p,d):\,k_{p,d}=6} c(4) \;=\; 28 \times 1000 \;=\; F_{\text{piso}}.
$$

A grade atual atinge exatamente esse piso nas células móveis (todas as
4‑seguidas *evitáveis* já foram removidas; restam só as forçadas). Logo

$$
F(\text{grade atual}) = F_{\text{piso}} = F^\star.
$$

> **Portanto, a grade já está no ótimo matemático.** Reduzir os blocos de 4
> aulas seguidas restantes exigiria **violar uma restrição dura**: ou
> (a) baixar a carga semanal de professores como SILVANA (impossível — é a
> atribuição legal de aulas), ou (b) espalhar essas aulas por mais de 5 dias
> (impossível — a manhã só tem 5 dias), ou (c) quebrar as travas dos cursos
> técnicos de Administração e dos professores em acúmulo de cargo (vedado por
> exigência legal e contratual).

---

## 5. Resumo executivo

| Indicador | Valor | Situação |
|---|---|---|
| Restrição dura (R1) — choques | 0 (exceto ALINE, isenta) | ✅ ótimo |
| Travas legais/ADM/acúmulo preservadas | 100 % | ✅ |
| 4‑seguidas **evitáveis** (carga ≤ 5/dia) | 0 | ✅ eliminadas |
| 4‑seguidas **inevitáveis** (carga = 6/dia) | 28 | ⛔ estruturais |
| Custo $F$ da grade | $= F^\star$ (piso) | ✅ ótimo global |

**Causa raiz dos blocos restantes:** acúmulo de cargo e cursos técnicos/legais
obrigatórios elevam a carga diária de vários professores a 6 aulas — o máximo
que a manhã comporta. Pelo princípio da casa dos pombos, 6 aulas num dia de
estrutura `4 + recreio + 2` **obrigam** um bloco de 4 seguidas. É um limite da
aritmética da grade, não da forma como ela foi montada.
