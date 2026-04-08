# 📘 Guia de Implementação — Dashboard de KPIs de Suporte Técnico

**Projeto:** Dashboard de KPIs de Suporte Técnico (Zendesk/CRM)  
**Autora:** Joana Aghata Dias Cardoso  
**Ferramentas cobertas:** Power BI · Google Looker Studio

---

## ✅ Pré-requisitos

Antes de começar, certifique-se de ter:

- O arquivo `tickets_suporte_kpi.csv` disponível localmente.
- Acesso ao **Power BI Desktop** (gratuito) ou ao **Google Looker Studio** (gratuito via conta Google).
- Familiaridade básica com importação de dados (não é necessário saber programar).

---

## 🔵 OPÇÃO A — Power BI Desktop

### 1. Importação dos Dados

1. Abra o **Power BI Desktop**.
2. Clique em **Obter Dados** > **Texto/CSV**.
3. Selecione o arquivo `tickets_suporte_kpi.csv` e clique em **Abrir**.
4. Na janela de pré-visualização, verifique se as colunas estão separadas corretamente.
5. Clique em **Transformar Dados** para abrir o Power Query Editor.

### 2. Tratamento dos Dados (Power Query)

No Power Query Editor, execute os seguintes ajustes:

- **Coluna `Data_Criacao`:** clique com botão direito > **Alterar Tipo** > **Data/Hora**.
- **Colunas numéricas** (`Tempo_Resposta_Horas`, `Tempo_Resolucao_Horas`, `Satisfacao_Cliente`): verifique se estão como tipo **Número Decimal**.
- **Coluna `FCR`:** deve estar como **Texto** (valores: `"Sim"` / `"Não"`).
- Após os ajustes, clique em **Fechar e Aplicar**.

### 3. Criação de Medidas DAX

No painel lateral direito, clique com botão direito na tabela > **Nova Medida** e crie as seguintes:

```
// Total de Tickets
Total_Tickets = COUNT(tickets_suporte_kpi[Ticket_ID])

// Taxa de FCR (%)
FCR_Percentual =
DIVIDE(
    CALCULATE(COUNT(tickets_suporte_kpi[Ticket_ID]), tickets_suporte_kpi[FCR] = "Sim"),
    COUNT(tickets_suporte_kpi[Ticket_ID])
)

// Tempo Médio de Resposta
TMR = AVERAGE(tickets_suporte_kpi[Tempo_Resposta_Horas])

// Tempo Médio de Resolução
TMT = AVERAGE(tickets_suporte_kpi[Tempo_Resolucao_Horas])

// Média de Satisfação (CSAT)
CSAT_Medio = AVERAGE(tickets_suporte_kpi[Satisfacao_Cliente])

// Tickets Pendentes
Tickets_Pendentes =
CALCULATE(COUNT(tickets_suporte_kpi[Ticket_ID]), tickets_suporte_kpi[Status] = "Pendente")
```

### 4. Montagem do Dashboard

Organize os visuais na tela conforme o layout abaixo:

#### 🔲 Linha 1 — Cartões de Resumo (KPIs principais)
| Visual | Medida | Configuração |
|---|---|---|
| Cartão | Total_Tickets | Rótulo: "Total de Tickets" |
| Cartão | FCR_Percentual | Formato: %, Rótulo: "Taxa FCR" |
| Cartão | CSAT_Medio | Formato: 1 decimal, Rótulo: "CSAT Médio" |
| Cartão | TMR | Formato: 1 decimal, Rótulo: "TMR (horas)" |
| Cartão | Tickets_Pendentes | Rótulo: "Tickets Pendentes" |

#### 🔲 Linha 2 — Gráficos de Análise
- **Gráfico de Linhas:** Eixo X = `Data_Criacao` (hierarquia: Mês > Semana > Dia), Valores = `Total_Tickets`. Mostra tendência de volume ao longo do tempo.
- **Gráfico de Barras Horizontais:** Eixo Y = `Categoria`, Valores = `Total_Tickets`. Identifica categorias de maior incidência.

#### 🔲 Linha 3 — Distribuição e Detalhe
- **Gráfico de Rosca:** Legenda = `Canal`, Valores = `Total_Tickets`. Mostra de onde vêm os tickets (Chat, E-mail, Telefone, etc).
- **Tabela de Detalhes:** Colunas sugeridas: `Ticket_ID`, `Data_Criacao`, `Categoria`, `Canal`, `Status`, `Satisfacao_Cliente`. Adicione filtro: `Status = "Pendente"`.

### 5. Filtros e Segmentações

Adicione as seguintes **Segmentações de Dados** (slicers) na lateral do dashboard:

- **Período:** campo `Data_Criacao` > tipo "Entre datas".
- **Categoria:** campo `Categoria`.
- **Canal:** campo `Canal`.
- **Status:** campo `Status`.

### 6. Publicação (opcional)

1. Salve o arquivo como `.pbix`.
2. Clique em **Publicar** > selecione seu workspace no Power BI Service.
3. Acesse em `app.powerbi.com` para visualizar e compartilhar online.

---

## 🟢 OPÇÃO B — Google Looker Studio

### 1. Criação do Relatório

1. Acesse [lookerstudio.google.com](https://lookerstudio.google.com).
2. Clique em **Criar** > **Relatório**.
3. Selecione a fonte de dados: **Fazer upload de arquivo** > `tickets_suporte_kpi.csv`.
4. Confirme os tipos de dados:
   - `Data_Criacao` → **Data e Hora**
   - `Tempo_Resposta_Horas`, `Tempo_Resolucao_Horas`, `Satisfacao_Cliente` → **Número**
   - Demais colunas → **Texto**

### 2. Criação de Campos Calculados

No painel de dados, clique em **Adicionar campo** e crie:

```
// Taxa FCR (%)
FCR_Percentual:
COUNTIF(FCR, "Sim") / COUNT(Ticket_ID)
→ Formato: Porcentagem

// CSAT Médio
(já disponível via agregação AVG na configuração dos gráficos)

// Tickets Pendentes
Tickets_Pendentes:
COUNTIF(Status, "Pendente")
```

### 3. Montagem do Dashboard

#### Scorecard (equivalente ao Cartão do Power BI)
- Insira **5 Scorecards** para: Total de Tickets, FCR %, CSAT Médio, TMR e Tickets Pendentes.
- Em cada Scorecard: defina a **Métrica** com a agregação correta (COUNT, AVG, campo calculado).

#### Gráfico de Série Temporal
- Tipo: **Gráfico de série temporal**.
- Dimensão: `Data_Criacao` (agrupamento: Semana).
- Métrica: `Ticket_ID` (COUNT).

#### Gráfico de Barras por Categoria
- Tipo: **Gráfico de barras**.
- Dimensão: `Categoria`.
- Métrica: `Ticket_ID` (COUNT).
- Ordenar por: Métrica decrescente.

#### Gráfico de Pizza por Canal
- Tipo: **Gráfico de pizza**.
- Dimensão: `Canal`.
- Métrica: `Ticket_ID` (COUNT).

#### Tabela de Tickets Pendentes
- Tipo: **Tabela**.
- Dimensões: `Ticket_ID`, `Data_Criacao`, `Categoria`, `Canal`.
- Filtro da tabela: `Status = "Pendente"`.

### 4. Filtros Interativos

Adicione **Controles de filtro** para:
- `Data_Criacao` (intervalo de datas).
- `Categoria` (lista suspensa).
- `Canal` (lista suspensa).

### 5. Compartilhamento

1. Clique em **Compartilhar** no canto superior direito.
2. Adicione colaboradores por e-mail ou gere um **link público** para visualização.

---

## 📊 Metas de Referência por KPI

| KPI | Meta Recomendada | Referência de Mercado |
|---|---|---|
| Taxa de FCR | > 70% | Zendesk Benchmark 2024 |
| Tempo Médio de Resposta | < 4 horas | SLA padrão N1 |
| Tempo Médio de Resolução | < 24 horas | SLA padrão N1 |
| CSAT Médio | > 4,0 / 5,0 | HubSpot Support Benchmark |
| Tickets Pendentes | < 10% do volume total | Boas práticas operacionais |

---

## 🧩 Dicas Finais

- **Atualize os dados** substituindo o CSV e recarregando a fonte — nenhuma configuração de visual se perde.
- **Cores sugeridas:** use verde para metas atingidas, amarelo para atenção e vermelho para abaixo da meta (formatação condicional disponível em ambas as ferramentas).
- **Documentação:** consulte o `Dicionario_Dados.md` para entender o significado exato de cada coluna antes de criar medidas.
- Para evoluir o projeto, considere carregar o CSV em um banco **MySQL/PostgreSQL** e conectar diretamente pelo conector nativo do Power BI ou Looker.

---

*Desenvolvido por Joana Aghata Dias Cardoso — Suporte Técnico N1 | Cloud AWS | Análise de Dados*
