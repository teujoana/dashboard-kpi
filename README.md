# 📊 Dashboard de KPIs de Suporte Técnico (Zendesk/CRM)

Projeto demonstra a aplicação de **Análise de Dados** para otimização de operações de **Suporte Técnico N1**. Utilizando um dataset simulado baseado em métricas reais de mercado (Zendesk, HubSpot), o objetivo é transformar dados brutos em insights acionáveis para gestão de equipes e melhoria da experiência do cliente.

## 🎯 Objetivos do Projeto

- Monitorar o volume de chamados por categoria e canal.
- Analisar a eficiência do suporte através do **FCR (First Contact Resolution)**.
- Acompanhar o **SLA (Service Level Agreement)** através dos tempos de resposta e resolução.
- Avaliar a satisfação do cliente (CSAT).

## 📂 Estrutura do Projeto

- `tickets_suporte_kpi.csv`: Dataset com 500 registros de tickets simulados.
- `Dicionario_Dados.md`: Explicação detalhada de cada métrica e coluna.
- `Guia_Implementacao.md`: Passo a passo para criar o dashboard no Power BI ou Google Looker.

## 📈 Principais KPIs Analisados

1. **Volume Total de Tickets**: Quantidade de chamados abertos no período.
2. **Taxa de FCR (First Contact Resolution)**: Percentual de tickets resolvidos no primeiro atendimento (Meta: >70%).
3. **Tempo Médio de Resposta (TMR)**: Média de horas para o primeiro retorno ao cliente.
4. **Tempo Médio de Resolução (TMT)**: Média de horas para o fechamento total do ticket.
5. **CSAT (Customer Satisfaction Score)**: Média de avaliação dada pelos usuários (1 a 5).

## 🛠️ Ferramentas Sugeridas

- **Processamento**: Python (Pandas) ou Excel.
- **Visualização**: Power BI, Google Looker Studio ou Tableau.
- **Banco de Dados**: MySQL/PostgreSQL (opcional para carga de dados).

---

## 🚀 Guia de Implementação (Power BI / Looker)

### 1. Importação dos Dados
- Carregue o arquivo `tickets_suporte_kpi.csv`.
- Verifique se as colunas de data (`Data_Criacao`) foram reconhecidas corretamente.

### 2. Criação de Medidas (DAX no Power BI)
- **Total de Tickets**: `COUNT(Ticket_ID)`
- **% FCR**: `DIVIDE(CALCULATE(COUNT(Ticket_ID), FCR="Sim"), COUNT(Ticket_ID))`
- **Média de Satisfação**: `AVERAGE(Satisfacao_Cliente)`

### 3. Visualizações Recomendadas
- **Cartões**: Exiba o Total de Tickets, % FCR e Média de Satisfação no topo.
- **Gráfico de Linhas**: Volume de tickets ao longo do tempo (por dia ou semana).
- **Gráfico de Barras**: Tickets por Categoria (ajuda a identificar problemas recorrentes).
- **Gráfico de Pizza/Rosca**: Distribuição por Canal (Chat, E-mail, etc).
- **Tabela de Detalhes**: Lista dos últimos tickets com status "Pendente".

---
**Desenvolvido por Joana Aghata Dias Cardoso**  
*Suporte Técnico N1 | Cloud AWS | Análise de Dados*
