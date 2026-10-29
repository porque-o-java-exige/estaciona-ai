# Manual de Contribuição e Governança do Kanban Semântico — Estaciona Aí

> Guia oficial de padronização de branches, commits, Pull Requests e operação do quadro **GitHub Projects** para a equipe de desenvolvimento do **Estaciona Aí (SENAI Suíço-Brasileira)**.

## 1. Taxonomia Semântica de Issues e Pull Requests

Todos os cards criados no **GitHub Projects** e títulos de Pull Requests devem iniciar obrigatoriamente com uma tag semântica entre colchetes:

| Tag Semântica | Finalidade | Exemplo Real do Repositório |
| :--- | :--- | :--- |
| **`[FEAT]`** | Nova funcionalidade de produto ou endpoint de negócio | `[FEAT] implements photo system in azure (#19)` |
| **`[REFACTOR]`** | Melhoria de código ou componente sem alterar o comportamento externo | `[REFACTOR] - button component (#6)` |
| **`[CHORE]`** | Configuração de infraestrutura, dependências Maven/NPM ou arquitetura base | `[CHORE] add crud base structure (#4)` |
| **`[BUG]`** | Correção de defeito identificado em desenvolvimento, homologação ou quadro | `[BUG] issues sumindo (#29)` |
| **`[TEST]`** | Criação ou manutenção de testes unitários, de integração ou E2E no Cypress | `[TEST] Add Unit and Integration Tests for Notifications (#34)` |
| **`[DOCS]`** | Criação ou atualização de artefatos na pasta `/docs` ou no Caderno de TCC | `[DOCS] - estruturar central de documentação do projeto (#2)` |
| **`[DESIGN]`** | Prototipação de interfaces, fluxos de telas e guia de estilos no Figma | `[FEAT][DESIGN] - Prototipagem das page mobile no figma (#3)` |

## 2. Regras de Operação do Quadro Kanban (GitHub Projects)

O quadro oficial do projeto é dividido em 5 colunas com critérios rígidos de passagem (*Definition of Ready* e *Definition of Done*):

1. **`Backlog`:** Ideias, épicos e tarefas mapeadas para o projeto. Nenhum card sai do Backlog para a Sprint sem ter responsável (`Assignee`), `Sprint` selecionada, prioridade (`High`, `Medium`, `Low`) e estimativa em dias (`Estimate`) preenchida.
2. **`Ready`:** Tarefas priorizadas pelo Product Owner na *Sprint Planning*, com critérios de aceite claros e prontas para serem puxadas pelo desenvolvedor.
3. **`In progress`:** Tarefas em codificação ativa na branch da funcionalidade. Cada integrante deve manter no máximo 1 a 2 tarefas simultâneas em progresso.
4. **`In review (WIP Limit: Máximo 5 cards)`:** Pull Request aberto aguardando revisão técnica do **TechLead (Ryan)** e validação de **QA (Gabriel Vereda)**. Se a coluna atingir 5 cards, a equipe prioriza revisar PRs antes de abrir novas frentes.
5. **`Done`:** Código revisado, aprovado nos testes automatizados, mergeado na branch `develop`/`main` e validado pelo Product Owner.

## 3. Fluxo de Branches e Padrão de Commits (Git Flow Simplificado)

* **`main`:** Código estável e homologado ao final de cada Sprint Review.
* **`develop`:** Branch de integração contínua da Sprint vigente.
* **Branches de Trabalho:** Criadas a partir da `develop` seguindo o formato `tipo/numero-da-issue-descricao-curta`:
  * `feat/35-spring-security-jwt`
  * `feat/12-home-mapa-vagas`
  * `refactor/16-chip-component`
  * `test/34-unit-tests-notifications`

### Boas Práticas antes de abrir um Pull Request

1. Sincronize sua branch local com a `develop` (`git pull origin develop`) para evitar conflitos.
2. Verifique se o projeto compila sem erros (`mvn clean test` no back-end ou `npm run build` no front-end).
3. Referencie o número da Issue na descrição do PR (ex.: `Closes #35`) para que o GitHub Projects mova o card automaticamente para `Done` após o merge.
