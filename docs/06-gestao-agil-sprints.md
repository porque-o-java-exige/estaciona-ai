# 06 — Gestão Ágil Scrum, Métricas em Dias e Relatórios de Sprints

> Governança de acompanhamento das 4 Sprints do projeto **Estaciona Aí** utilizando o framework Scrum aliado à métrica de **Estimativa em Dias de Esforço (Pessoa-Dia)** em execução paralela no **GitHub Projects**.

## 1. Dinâmica de Estimativa em Dias com Equipe em Paralelo

Como os **6 integrantes** atuam simultaneamente em frentes complementares (`TechLead`, `Back-end`, `Front-end`, `QA`, `Product Owner` e `Scrum Master`), cada dia útil do calendário comporta até **6 Dias de Esforço combinados** da equipe:

* **Capacidade Individual Segura:** Máximo de **7 a 8 dias** de tarefas na Sprint 1 (8 dias úteis) e **8 a 9 dias** nas Sprints 2, 3 e 4 (10 dias úteis cada), reservando margem para Daily Scrum, revisão de Pull Requests, Sprint Review e Retrospectiva.
* **Regra de Granularidade para o Burndown:** Nenhuma Issue individual deve ultrapassar **5 dias de estimativa**, garantindo entregas contínuas na coluna `Done` ao longo da Sprint.

## 2. Alocação Consolidada de Capacidade por Integrante (Sprints 1 a 4)

| Integrante | Papel na Equipe | Sprint 1 *(18/10–28/10)* | Sprint 2 *(29/10–11/11)* | Sprint 3 *(12/11–25/11)* | Sprint 4 *(26/11–09/12)* | Total Acumulado |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **Ryan Ferreira da Silva** | **TechLead** | 7 dias | 9 dias | 9 dias | 8 dias | **33 dias** |
| **Francisco Alamino Neto** | **Developer Back-end** | 8 dias | 9 dias | 9 dias | 8 dias | **34 dias** |
| **Gabriel de Lima Silva** | **Developer Front-end** | 6 dias | 8 dias | 9 dias | 8 dias | **31 dias** |
| **Gabriel Vereda dos Santos** | **Quality Assurance (QA)** | 6 dias | 8 dias | 8 dias | 8 dias | **30 dias** |
| **Vinícius Araújo da Silva** | **Product Owner (PO)** | 8 dias | 7 dias | 8 dias | 7 dias | **30 dias** |
| **Rodrigo J. de Paula Silva** | **Scrum Master (SM)** | 7 dias | 7 dias | 7 dias | 7 dias | **28 dias** |
| **Total da Equipe (Eixo Y do Burndown)** | **6 Integrantes** | **42 dias** | **48 dias** | **50 dias** | **46 dias** | **186 dias** |

## 3. Relatório Detalhado da Sprint 1 (18/10/2026 a 28/10/2026)

* **Objetivo da Sprint (Sprint Goal):** Estruturar a base arquitetural de submódulos, modelagem SQL, CRUD de Usuários e Veículos, upload de fotos no Microsoft Azure, componentes base em Atomic Design, autenticação JWT e motor de notificações.
* **Resumo de Dailies, Review e Retrospectiva:** A equipe tratou rapidamente o incidente de visualização de cards no quadro (`#28` e `#29 [BUG] issues sumindo`) e executou um rollback preventivo no primeiro PR de usuários (`#6 [REVERT 5] feat user crud`) para consolidar a estrutura base (`#4`) antes de entregar os CRUDs definitivos (`#5` e `#7`) e a integração com o Azure (`#19`).

| Issue / PR no GitHub | Entrega Realizada na Sprint 1 | Frente / Responsável | Estimativa (Dias) |
| :--- | :--- | :--- | :---: |
| `estaciona-ai-back #1` e `#2` | Configuração de dependências e nova estrutura arquitetural | TechLead (Ryan) | **2 dias** |
| `estaciona-ai-back #4` | `[CHORE] add crud base structure` | Back-end / TechLead | **2 dias** |
| `estaciona-ai-back #5, #6, #7` | `[FEAT] add user CRUD` (com ajuste `#6`) & `Develop feat vehicle crud` | Back-end (Francisco) | **4 dias** |
| `estaciona-ai-back #19` | `[FEAT] implements photo system in azure` | Back-end / TechLead | **5 dias** |
| `estaciona-ai-back #28` e `#29` | `[BUG] issues sumindo` & `Problemas de Issue` | Scrum Master (Rodrigo) | **2 dias** |
| `estaciona-ai-front #3` e `#6` | `[FEAT / REFACTOR] button component` | Front-end (Gabriel L.) | **3 dias** |
| `estaciona-ai-front #5` | `[FEAT] funcionalidades no componente input` | Front-end (Gabriel L.) | **2 dias** |
| `estaciona-ai-front #16` | `[REFACTOR] chip component` | Front-end / QA | **1 dia** |
| `estaciona-ai #1` e `#2` | `[DOCS] motivos do projeto e central de documentação` | SM (Rodrigo) / PO | **4 dias** |
| `estaciona-ai #3` | `[FEAT][DESIGN] Prototipagem das páginas mobile no Figma` | PO (Vinícius) / Front | **4 dias** |
| `estaciona-ai-back #30` | `[FEAT] Notification System for Booking & Payment Events` | Back-end (Francisco) | **5 dias** |
| `estaciona-ai-back #35` | `[FEAT] Authentication and Authorization with Spring Security and JWT` | TechLead (Ryan) | **5 dias** |
| `estaciona-ai-back #34` | `[TEST] Add Unit and Integration Tests for Notifications` | QA (Gabriel Vereda) | **3 dias** |

## 4. Planejamento das Sprints 2, 3 e 4

* **Sprint 2 (29/10 a 11/11/2026 — 48 Dias):** Páginas de Login/Cadastro no Front-end (`Draft`), Home com Mapa de Vagas e Detalhes da Garagem (`#12`), Telas de Perfil/Veículos/Garagens, Endpoints CRUD de Garagens + Geocodificação e suíte Cypress inicial.
* **Sprint 3 (12/11 a 25/11/2026 — 50 Dias):** Fluxo completo de Reserva de Vagas por Hora/Diária e Histórico, Módulo de Pagamento/Checkout (`#21`), Salas de Chat em Tempo Real (`chat_rooms` / `chat_messages`), Landing Page e testes E2E de reservas no Cypress.
* **Sprint 4 (26/11 a 09/12/2026 — 46 Dias):** Avaliações Mútuas de 1 a 5 estrelas (`garage_reviews` e `driver_reviews`), integração visual de Alertas (`#30`), refinamento de responsividade Mobile-First, bateria final de regressão Cypress e entrega do Caderno de TCC.
