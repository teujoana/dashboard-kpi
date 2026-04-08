# 📖 Dicionário de Dados - Dataset de Tickets de Suporte

Este documento descreve cada coluna presente no arquivo `tickets_suporte_kpi.csv` para facilitar a análise e criação do dashboard.

| Coluna | Descrição | Tipo de Dado | Exemplo |
| :--- | :--- | :--- | :--- |
| **Ticket_ID** | Identificador único do chamado no sistema. | Texto | TKT-1001 |
| **Data_Criacao** | Data e hora em que o ticket foi aberto pelo cliente. | Data/Hora | 2024-01-15 14:30:00 |
| **Categoria** | Classificação do problema relatado pelo usuário. | Texto | Acesso, Financeiro, Erro de Sistema |
| **Prioridade** | Nível de urgência definido para o atendimento. | Texto | Baixa, Média, Alta, Urgente |
| **Canal** | Meio de comunicação utilizado pelo cliente. | Texto | Chat, WhatsApp, E-mail |
| **Agente** | Nome do profissional responsável pelo atendimento. | Texto | Joana Cardoso, Agente 2 |
| **Status** | Situação atual do chamado no fluxo de trabalho. | Texto | Resolvido, Fechado, Pendente |
| **FCR** | Indica se o ticket foi resolvido no primeiro contato. | Texto (Sim/Não) | Sim |
| **Tempo_Resposta_Horas** | Tempo decorrido até a primeira resposta do agente. | Numérico (Horas) | 1.5 |
| **Tempo_Resolucao_Horas** | Tempo total decorrido até a resolução do chamado. | Numérico (Horas) | 4.2 |
| **Satisfacao_Cliente** | Nota de avaliação dada pelo cliente (CSAT). | Inteiro (1 a 5) | 5 |

---
**Desenvolvido por Joana Aghata Dias Cardoso**  
*Suporte Técnico N1 | Cloud AWS | Análise de Dados*
