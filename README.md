# 📊 iFood Data Analysis — Segmentação Inteligente e Análise de Resposta a Campanhas Promocionais

> Case adaptado pela FNAT (Fundação de Negócios, Analytics e Tecnologia) a partir de um processo seletivo real do iFood, com foco em análise inferencial e experimental de campanhas de marketing.

---

## 🧭 Contexto

Uma rede varejista do setor **premium** enfrenta o desafio de compreender a efetividade de suas campanhas promocionais. Com um histórico de **cinco campanhas distintas** e dados detalhados sobre clientes e comportamento de compra, a diretoria busca:

- Identificar quais campanhas geraram resultados expressivos
- Entender quais não tiveram impacto
- Otimizar futuros investimentos em marketing com base em evidências

O portfólio da empresa inclui produtos alimentícios de alta qualidade, bebidas importadas, vinhos, queijos, carnes nobres e itens gourmet — posicionados para consumidores de renda média-alta que valorizam sofisticação e experiência de compra.

---

## 🎯 Objetivo

Analisar os dados de campanhas usando técnicas de **teste A/B**, **cálculo de tamanho de amostra** e **análise estatística inferencial**, conectando teoria estatística à prática de marketing analítico.

---

## 📁 Estrutura do Projeto

```
ifood-crm-analyst/
│
├── README.md
├── ifood_data_analysis.xlsx      # Análise completa em Excel
├── CAPA                      # Apresentação do projeto
├── ifood_df                  # Base de dados (2.205 clientes, 40 variáveis)
├── Cálculo Tamanho Amostra   # Metodologia e resultado do n amostral
├── Amostra 1–5               # 5 amostras aleatórias simples (n=339 cada)
├── Análise Amostra           # Validação de representatividade amostral
├── IC                        # Intervalo de Confiança 95%
├── Valores de Z              # Tabela da distribuição normal padrão
├── Teste AB                  # Resultados comparativos Campanha 3 vs. 4
└── Teste de Hipóteses        # Validação estatística do Teste A/B


```

---

## 📦 Dataset

A base de dados contém informações de **2.205 clientes** com **40 variáveis** cobrindo perfil demográfico, comportamento de compra e resposta a campanhas.

| Variável | Tipo | Descrição |
|---|---|---|
| `Income` | float | Renda anual do domicílio |
| `Kidhome` | int | Número de crianças pequenas em casa |
| `Teenhome` | int | Número de adolescentes em casa |
| `Recency` | int | Dias desde a última compra |
| `MntWines` | int | Gasto com vinhos (últimos 2 anos) |
| `MntFruits` | int | Gasto com frutas (últimos 2 anos) |
| `MntMeatProducts` | int | Gasto com carnes (últimos 2 anos) |
| `MntFishProducts` | int | Gasto com peixes (últimos 2 anos) |
| `MntSweetProducts` | int | Gasto com doces (últimos 2 anos) |
| `MntGoldProds` | int | Gasto com produtos premium (últimos 2 anos) |
| `NumDealsPurchases` | int | Compras feitas com desconto |
| `NumCatalogPurchases` | int | Compras por catálogo |
| `NumStorePurchases` | int | Compras em loja física |
| `NumWebPurchases` | int | Compras via site |
| `NumWebVisitsMonth` | int | Visitas ao site no último mês |
| `AcceptedCmp1`–`AcceptedCmp5` | int | Aceitação por campanha (1=sim, 0=não) |
| `Response` | int | Aceitação da última campanha (variável-alvo) |
| `Complain` | int | Reclamação nos últimos 2 anos |
| `Age` | int | Idade estimada do cliente |
| `Customer_Days` | int | Dias desde o cadastro |
| `Marital_*` | int | Dummies de estado civil |
| `Education_*` | int | Dummies de nível educacional |
| `MntTotal` | int | Gasto total em todas as categorias |
| `MntRegularProds` | int | Gasto em produtos exceto premium |
| `AcceptedCmpOverall` | int | Total de campanhas aceitas (0–5) |
| `Grupo` | str | Grupo do teste A/B (A ou B) |

---

## 🏷️ Campanhas

| Campanha | Nome | Estratégia |
|---|---|---|
| Campanha 1 | Família Premium | Produtos gourmet e vinhos para clientes de maior poder aquisitivo |
| Campanha 2 | Sorteio de Alto Valor | Sorteio sem descontos, gasto mínimo elevado — baixa conversão |
| Campanha 3 | Descontos em Essenciais | Descontos agressivos em produtos cotidianos para clientes sensíveis a preço |
| Campanha 4 | Premium Moderado | Descontos moderados em produtos de maior valor agregado |
| Campanha 5 | Experiência Familiar Gourmet | Vinhos, queijos e itens premium com posicionamento aspiracional |

> Campanhas 3 e 4 foram desenhadas intencionalmente como **Teste A/B controlado**.

---

## 🔬 Análises Realizadas

### 1. Cálculo do Tamanho de Amostra

**Objetivo:** determinar o tamanho mínimo de amostra estatisticamente válido para testar uma campanha com mínimo risco.

**Metodologia:** população infinita com ajuste de correção para população finita.

```
n₀ = Z² · p · q / e²       (população infinita)
n  = n₀ · N / (n₀ + N − 1) (ajuste para população finita)
```

| Parâmetro | Valor |
|---|---|
| Nível de confiança | 95% (Z = 1,96) |
| Margem de erro | 5% |
| Proporção esperada (p) | 0,5 (máxima variabilidade) |
| N (população) | 2.205 clientes |
| **n₀ (pop. infinita)** | **400** |
| **n (ajustado)** | **339** |

Foram extraídas **5 amostras aleatórias simples** de 339 registros cada, todas validadas como representativas da população (taxa de conversão real: 20,77%).

---

### 2. Intervalo de Confiança

Com base na Amostra 1 (n=339, 69 conversões):

| Métrica | Valor |
|---|---|
| Proporção amostral (p̂) | ~20,35% |
| Erro padrão | calculado via √(p·(1-p)/n) |
| Nível de confiança | 95% |
| IC | [limite inferior; limite superior] |

---

### 3. Teste A/B — Campanha 3 vs. Campanha 4

| Grupo | Campanha | Clientes | Aceitaram | Taxa de Conversão |
|---|---|---|---|---|
| Grupo A | Campanha 3 — Descontos Essenciais | 1.117 | 163 | **14,59%** |
| Grupo B | Campanha 4 — Premium Moderado | 1.088 | 164 | **15,07%** |

Diferença observada: **+0,48 pp** favorável ao Grupo B.

---

### 4. Teste de Hipóteses

**Teste bilateral de proporções** (bicaudal), α = 5%.

| Hipótese | Formulação |
|---|---|
| H₀ | pA = pB — as taxas de conversão são iguais |
| H₁ | pA ≠ pB — uma campanha performa melhor |

| Estatística | Valor |
|---|---|
| Proporção combinada (p̄) | 0,1483 |
| Erro padrão combinado | 0,01514 |
| Estatística Z | −0,318 |
| **p-valor (bilateral)** | **0,7507** |
| Decisão | p ≥ α → **Não rejeitar H₀** |

**Conclusão:** a diferença de 0,48 pp entre as campanhas **não é estatisticamente significativa** (p = 0,75 >> α = 0,05). Os intervalos de confiança dos dois grupos se sobrepõem amplamente, confirmando que a diferença pode ser ruído estatístico.

> **Implicação de negócio:** não há evidências suficientes para afirmar que uma campanha é superior à outra. Recomenda-se nova rodada de testes com maior poder estatístico antes de escalar qualquer das abordagens.

---

## 🛠️ Ferramentas Utilizadas

- **Microsoft Excel** — toda a análise estatística, modelagem e visualização
- Fórmulas nativas para cálculos de proporção, erro padrão, estatística Z e p-valor
- Tabela da distribuição normal padrão construída manualmente como referência

---

## 📌 Principais Aprendizados

1. **Preço agressivo não garante melhor resultado** — a diferença entre descontos em essenciais (Campanha 3) e descontos moderados em premium (Campanha 4) não foi significativa, questionando o custo-benefício de margens sacrificadas.
3. **Testes A/B precisam de poder estatístico adequado** — com p=0,75, o experimento não distinguiu as campanhas. Definir tamanho de amostra antes do teste é crítico.
4. **Marketing baseado em evidências requer disciplina metodológica** — o case demonstra a transição do marketing por intuição para decisões guiadas por dados e testes controlados.

---

## 📚 Referências

- Case original disponibilizado pelo **iFood** em processo seletivo para funções de dados
- Adaptação acadêmica: **FNAT — Fundação de Negócios, Analytics e Tecnologia**

---

*Projeto desenvolvido para fins acadêmicos com base em dados de exemplo inspirados em operações reais de CRM.*
