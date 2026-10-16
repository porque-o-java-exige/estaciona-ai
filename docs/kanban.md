# Guia Prático e Templates de Kanban Semântico — Estaciona Aí (`estaciona-ai-front`)

Este manual reúne a arquitetura completa do **Kanban Semântico** para a organização `porque-o-java-exige` e o repositório `estaciona-ai-front`, incluindo a evolução prática da sua **Issue #3**, a comparação de etapas extras do fluxo, os templates estruturados prontos para uso e o passo a passo para treinar a equipe.

## Padronizar Títulos, Labels e Campos Personalizados Garante Leitura Imediata no Quadro

Em um Kanban semântico no GitHub, qualquer pessoa da equipe deve bater o olho em um cartão e entender **o que é a tarefa**, **qual a urgência** e **qual o tamanho do esforço** sem precisar abrir a Issue. Para isso, combinamos prefixos no título, Labels (etiquetas do repositório) e Custom Fields (campos exclusivos do GitHub Projects).

### Tabela de Prefixos e Labels Semânticas do Repositório

Configure as etiquetas abaixo na aba **Issues > Labels** do repositório `estaciona-ai-front`:

| Prefixo no Título | Nome da Label | Cor Hex | Quando usar no `estaciona-ai-front` | Exemplo real no projeto |
| :--- | :--- | :--- | :--- | :--- |
| `[FEAT]` | `feat: feature` | `#0E8A16` (Verde) | Criação de novas telas, páginas, fluxos ou variantes inéditas de componentes | `[FEAT] - criar variantes de ação no componente Button` |
| `[STYLE]` | `style: ui/css` | `#7057FF` (Roxo) | Ajustes visuais, estados `:hover`/`:active`, tokens SCSS, responsividade e fidelidade ao Figma | `[STYLE] - aplicar cursor pointer e estados hover/active em Button` |
| `[FIX]` | `fix: bug` | `#D73A4A` (Vermelho) | Correção de erros visuais, quebras de layout ou falhas de comportamento/integração | `[FIX] - corrigir quebra do modal de reserva em telas mobile` |
| `[REFACTOR]` | `refactor: code` | `#FBCA04` (Amarelo) | Melhoria na estrutura de pastas, componentização ou limpeza de código sem mudar o visual | `[REFACTOR] - extrair estilos repetidos de botões para mixin SCSS` |
| `[DOCS]` | `docs: documentation` | `#0075CA` (Azul) | Atualização do `README.md`, `CONTRIBUTING.md` ou documentação de componentes | `[DOCS] - adicionar guia de uso do Kanban semântico no repositório` |
| `[CHORE]` | `chore: config` | `#E4E669` (Cinza/Lima) | Configuração de bibliotecas, dependências, linter, rotas ou templates do GitHub | `[CHORE] - adicionar Issue Forms YAML na pasta .github` |

### Campos Personalizados (Custom Fields) no GitHub Projects

Enquanto as Labels classificam a **natureza técnica** da tarefa, os campos personalizados do GitHub Projects evitam a poluição de dezenas de etiquetas misturadas e permitem agrupar ou ordenar o quadro por prioridade e esforço:

| Nome do Campo | Tipo no GitHub Projects | Opções Recomendadas | Finalidade Prática para o Time |
| :--- | :--- | :--- | :--- |
| **Status** (Nativo) | Colunas do Quadro | `Backlog`, `Pronto para Fazer`, `Em Andamento`, `Em Revisão`, `Concluído` | Define em qual etapa do fluxo a tarefa está |
| **Prioridade** | `Single select` | `Alta (Bloqueante)`, `Média (Importante)`, `Baixa (Desejável)` | Indica qual cartão do `Pronto para Fazer` deve ser puxado primeiro |
| **Tamanho / Esforço** | `Single select` | `P (< 2h)`, `M (Meio turno a 1 dia)`, `G (2 a 3 dias)`, `GG (Quebrar em menores!)` | Ajuda a identificar tarefas grandes demais que precisam ser divididas |
| **Escopo / Área** | `Single select` | `Componente UI`, `Tela / Página`, `Integração API`, `Infra / Config` | Facilita filtrar apenas tarefas de componentes visuais ou páginas |

## Avaliar Etapas Extras Ajuda a Decidir Entre o Fluxo de 5 Colunas e Processos Específicos

O fluxo base escolhido pela equipe (`Backlog → Pronto para Fazer → Em Andamento → Em Revisão → Concluído`) atende perfeitamente projetos ágeis de front-end. A tabela a seguir mostra exemplos práticos de como funcionam as etapas específicas adicionais para você decidir se alguma delas faz sentido no `estaciona-ai-front`:

| Etapa / Recurso Extra | Como funciona na prática (Exemplo no `estaciona-ai-front`) | Quando vale a pena aplicar | Recomendação para o seu projeto |
| :--- | :--- | :--- | :--- |
| **Coluna `Triagem / Refinamento`** (Antes do *Backlog*) | Alguém percebe durante uma reunião que "falta o botão de reservar vaga", abre uma Issue rápida e ela cai em *Triagem*. Ela só vai para *Pronto para Fazer* quando tiver o link do Figma e os critérios de CSS definidos. | Quando membros abrem ideias incompletas e os desenvolvedores começam a programar sem saber o que o Figma pede. | **Não precisa criar coluna extra**: o próprio uso da coluna `Backlog` (para ideias brutas) separada de `Pronto para Fazer` (para tarefas já detalhadas com Figma) já cumpre esse papel. |
| **Coluna `Design Review / QA`** (Entre *Em Revisão* e *Concluído*) | Após o código do botão ser aprovado no Pull Request, outra pessoa testa a tela rodando no navegador e confere se o `:hover` (`$clr-blue-1000`) e o `scale(0.90)` ficaram fiéis ao Figma antes de dar o merge. | Quando há uma pessoa responsável pelo Figma/Design ou quando bugs visuais passam batido na revisão de código. | **Opcional**: você pode manter apenas 5 colunas e incluir uma caixinha de verificação de UI/Figma dentro da própria etapa `Em Revisão` (no Pull Request). |
| **Coluna `Bloqueado / Impedido`** | Você está criando a tela de "recarregar saldo na carteira", mas a API do back-end ou o protótipo no Figma ainda não ficou pronto. | Quando tarefas de front-end dependem com frequência de entregas atrasadas de outra equipe (back-end/design). | **Evite criar coluna**: é mais limpo manter o cartão onde está e adicionar uma label vermelha `status: blocked` comentando o motivo do bloqueio. |
| **Campo `Iteration` (Sprints Semanais ou Quinzenais)** | Em vez de uma coluna, você cria um campo nativo `Iteration` (ex.: *Sprint 1: 16/Out a 30/Out*) e seleciona quais cartões do *Backlog* serão entregues naquela quinzena. | Quando o projeto tem entregas com datas fixas (apresentações acadêmicas ou marcos quinzenais). | **Altamente recomendado** se vocês tiverem prazos de entrega marcados: permite criar uma aba separada no Project mostrando apenas a meta da semana. |

## Refatorar a Issue #3 Demonstra Como Transformar Texto Livre em Tarefa Acionável

Analisando a sua **Issue #3 (`[FEAT] - funcionalidades que faltam em button`)**, vocês já mandaram muito bem em três pontos: usaram o prefixo `[FEAT]`, aplicaram a etiqueta `Feature` e colocaram trechos reais de SCSS (`$clr-blue-1000`, `transform: scale(0.90)`).

Para deixá-la **100% semântica e prática para o Kanban**, aplicamos três melhorias:
1. **Substituir listas numeradas por Checklists interativas (`- [ ]`)**: no GitHub Projects, quando você usa `- [ ]` dentro da Issue, o cartão no quadro Kanban mostra automaticamente uma barra de progresso (ex.: `2 of 6 tasks completed`) conforme o desenvolvedor marca as caixinhas.
2. **Explicitar os Critérios de Aceite (Definition of Done)**: na Issue #3 original, a frase *"tente trocar a cor para um tom um pouco mais escuro"* abre margem para dúvida na hora da revisão. Em uma Issue semântica, definimos exatamente o resultado esperado.
3. **Avaliar se vale separar `[STYLE]` de `[FEAT]`**: os itens 1 e 2 da Issue #3 são ajustes visuais de interação no botão base (`[STYLE]`), enquanto o item 3 cria 4 novas variantes de negócio (`[FEAT]`). Você pode mantê-los juntos se forem feitos pela mesma pessoa em poucas horas, ou dividir em duas Issues menores para o cartão andar mais rápido no Kanban.

Veja abaixo como a sua **Issue #3** fica reescrita no padrão semântico ideal:

```markdown
### Contexto e Objetivo
O componente de botão atual não fornece feedback visual claro de interação (falta ponteiro de clique e estados de hover/active) e ainda não contempla todas as variantes mapeadas no protótipo do Figma para os fluxos de conversa, perfil, reserva e carteira.

**Referência no Figma:** [Inserir link direto do frame de Botões no Figma]
**Arquivos afetados:** `src/components/Button/...` (ou arquivo SCSS correspondente)

### Tarefas de Implementação (Subtarefas)
**1. Interação e Estados Visuais (Base)**
- [ ] Adicionar `cursor: pointer` na estilização base do botão para indicar clicabilidade
- [ ] Implementar estado `:hover` com escurecimento de tom (ex.: `background-color: $clr-blue-1000` na classe `.button-secondary:hover`)
- [ ] Implementar estado `:active` e `:focus` com redução sutil de escala (`transform: scale(0.90)`)

**2. Novas Variantes do Figma**
- [ ] Criar variante do botão **Sair da conversa**
- [ ] Criar variante do botão **Adicionar veículo e garagem** (Perfil do usuário)
- [ ] Criar variante do botão **Reservar vaga**
- [ ] Criar variante do botão **Recarregar o saldo na carteira**

### Notas Técnicas / Sugestão de Código
```scss
&.button-secondary:hover {
  /* Tom mais escuro para indicar foco do mouse sobre o botão */
  background-color: $clr-blue-1000;
}

button:active {
  transform: scale(0.90);
}
```

### Critérios de Aceite (Como testar na revisão)
- [ ] Todos os botões exibem `cursor: pointer` ao passar o mouse (exceto quando `disabled`)
- [ ] A transição de cor no `:hover` e o efeito `scale(0.90)` no clique funcionam sem quebrar o alinhamento do layout
- [ ] As 4 novas variantes estão disponíveis e seguem as cores, ícones e espaçamentos do Figma
```

## Adotar Issue Forms em YAML Substitui Textos Soltos por Formulários Guiados no GitHub

Quando vocês criam uma Issue pelo menu superior do GitHub usando um template comum em Markdown (`.md`), o GitHub abre apenas uma caixa de texto grande onde qualquer pessoa pode apagar as seções por engano ou esquecer de preencher informações importantes.

A forma **mais semântica e profissional** no GitHub é utilizar [Issue Forms escritos em YAML](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms) dentro da pasta `.github/ISSUE_TEMPLATE/` do repositório. Com o formato `.yml` baseado no [esquema oficial de formulários do GitHub](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-githubs-form-schema), o GitHub transforma a tela de criação de Issue em um **formulário interativo**:
* Preenche o prefixo `[FEAT] - ` automaticamente no campo de título.
* Adiciona a label correspondente automaticamente assim que a Issue é criada.
* Cria caixas de seleção (*dropdowns*) e campos obrigatórios (`required: true`), impedindo que alguém abra uma tarefa sem descrição ou sem critérios de aceite.

Abaixo estão os **3 arquivos prontos** para você criar no repositório `porque-o-java-exige/estaciona-ai-front`.

### 1. Template de Funcionalidade / Melhoria Visual (`.github/ISSUE_TEMPLATE/feature.yml`)

Crie o arquivo `.github/ISSUE_TEMPLATE/feature.yml` no repositório e cole o conteúdo abaixo:

```yaml
name: "Nova Funcionalidade ou Melhoria de UI ([FEAT] / [STYLE])"
description: "Solicitar a criação de um componente, tela, variante do Figma ou estilização no Estaciona Aí."
title: "[FEAT] - "
labels: ["feat: feature"]
body:
  - type: markdown
    attributes:
      value: |
        **Bem-vindo(a) ao formulário semântico do Estaciona Aí!**
        Preencha os campos abaixo para que qualquer pessoa da equipe consiga desenvolver e revisar esta tarefa com clareza.

  - type: dropdown
    id: tipo_demanda
    attributes:
      label: "Categoria Semântica"
      description: "Qual o foco principal desta entrega? (Se escolher STYLE ou REFACTOR, ajuste o prefixo no título acima)."
      options:
        - "[FEAT] Nova funcionalidade, tela ou variante de componente"
        - "[STYLE] Estilização SCSS, estados (:hover/:active) ou ajuste visual"
        - "[REFACTOR] Refatoração ou melhoria de código sem mudança visual"
    validations:
      required: true

  - type: textarea
    id: contexto
    attributes:
      label: "Contexto e Descrição"
      description: "Explique de forma direta o que precisa ser feito e por que essa melhoria ou funcionalidade é necessária."
      placeholder: "Ex.: Atualmente os botões não possuem cursor pointer nem estados de hover/active, e faltam 4 variantes previstas no Figma..."
    validations:
      required: true

  - type: input
    id: figma_link
    attributes:
      label: "Link ou Referência do Figma"
      description: "Cole o link do frame no Figma ou escreva 'Não se aplica' se for apenas ajuste técnico."
      placeholder: "https://www.figma.com/file/..."
    validations:
      required: false

  - type: textarea
    id: subtarefas
    attributes:
      label: "Lista de Tarefas (Checkboxes)"
      description: "Quebre a entrega em pequenos passos usando '- [ ]'. Isso gera a barra de progresso automática no nosso Kanban!"
      value: |
        - [ ] Tarefa 1
        - [ ] Tarefa 2
        - [ ] Tarefa 3
    validations:
      required: true

  - type: textarea
    id: notas_tecnicas
    attributes:
      label: "Notas Técnicas ou Sugestão de Código (Opcional)"
      description: "Se houver variáveis SCSS específicas (ex.: $clr-blue-1000), propriedades CSS ou rotas envolvidas, deixe registrado aqui."
      placeholder: |
        ```scss
        &.button-secondary:hover {
          background-color: $clr-blue-1000;
        }
        ```
    validations:
      required: false

  - type: textarea
    id: criterios_aceite
    attributes:
      label: "Critérios de Aceite (Como revisar?)"
      description: "O que precisa estar funcionando perfeitamente para que o Pull Request seja aprovado?"
      value: |
        - [ ] O visual e os estados interativos estão fiéis ao Figma
        - [ ] O componente funciona sem quebrar o layout existente
    validations:
      required: true
```

### 2. Template de Correção de Bug (`.github/ISSUE_TEMPLATE/bug.yml`)

Crie o arquivo `.github/ISSUE_TEMPLATE/bug.yml` no repositório e cole o conteúdo abaixo:

```yaml
name: "Relato de Bug ou Quebra Visual ([FIX])"
description: "Reportar um erro de comportamento, falha no console ou quebra de layout no Estaciona Aí."
title: "[FIX] - "
labels: ["fix: bug"]
body:
  - type: textarea
    id: problema
    attributes:
      label: "Qual é o problema?"
      description: "Descreva de forma clara o erro visual ou funcional encontrado."
      placeholder: "Ex.: O botão de reservar vaga transborda para fora do card em telas menores que 375px."
    validations:
      required: true

  - type: textarea
    id: passos_reproducao
    attributes:
      label: "Passos para reproduzir"
      description: "Como outra pessoa do time consegue ver esse erro acontecendo?"
      value: |
        1. Acesse a página '...'
        2. Clique no botão '...'
        3. Observe o erro no elemento '...'
    validations:
      required: true

  - type: textarea
    id: comportamento_esperado
    attributes:
      label: "✅ Comportamento ou Visual Esperado"
      description: "Como deveria funcionar de acordo com o Figma ou regra do projeto?"
    validations:
      required: true

  - type: textarea
    id: evidencias
    attributes:
      label: "📸 Print da Tela ou Erro do Console (Opcional)"
      description: "Arraste e solte uma imagem do bug ou cole a mensagem de erro aqui."
    validations:
      required: false
```

### 3. Template de Pull Request (`.github/pull_request_template.md`)

Para que o Kanban funcione sozinho sem que ninguém esqueça cartões parados, crie também um [template de Pull Request](https://www.google.com/search?q=https%3A%2F%2Fdocs.github.com%2Fen%2Fcommunities%2Fusing-templates-to-encourage-useful-issues-and-pull-requests%2Fcreating-a-pull-request-template-for-your-repository) em `.github/pull_request_template.md`. Ele ensina o time a usar a palavra-chave `Closes #numero`, que [vincula o Pull Request à Issue](https://www.google.com/search?q=https%3A%2F%2Fdocs.github.com%2Fen%2Fissues%2Ftracking-your-work-with-issues%2Fusing-issues%2Flinking-a-pull-request-to-an-issue) e move o cartão automaticamente para `Concluído` quando o código recebe merge:

```markdown
## 📝 O que foi feito neste Pull Request?
<!-- Descreva resumidamente as alterações implementadas -->

## 🔗 Issue Relacionada
<!-- Substitua o número abaixo pelo número da Issue que este PR resolve. -->
<!-- IMPORTANTE: Manter a palavra "Closes" faz o cartão ir para "Concluído" sozinho no Kanban após o merge! -->
Closes #

## 📸 Capturas de Tela / Gravação (Se houve mudança visual)
<!-- Arraste um print ou GIF mostrando o componente/tela funcionando -->

## ✅ Checklist antes de pedir revisão
- [ ] Meu código segue os padrões de estilo e variáveis SCSS do projeto
- [ ] Comparei o resultado visual no navegador com o protótipo do Figma
- [ ] Marquei todas as caixinhas (`- [x]`) concluídas lá na Issue original
```

## Configurar Automações no GitHub Projects Elimina o Trabalho Manual de Arrastar Cartões

Siga este passo a passo para estruturar o quadro e ativar os [fluxos automatizados nativos do GitHub Projects](https://www.google.com/search?q=https%3A%2F%2Fdocs.github.com%2Fen%2Fissues%2Fplanning-and-tracking-with-projects%2Fautomating-your-project%2Fusing-the-built-in-automations):

1. **Criar o Project vinculado ao repositório**:
   * Acesse a página da organização `porque-o-java-exige` (ou a aba **Projects** dentro de `estaciona-ai-front`) e clique em **New project**.
   * Escolha o modelo **Board** (Quadro Kanban) e dê o nome `Estaciona Aí - Front-end Kanban`.
2. **Renomear e criar as 5 colunas semânticas**:
   * Edite as colunas existentes e adicione as novas na ordem exata:
     1. `📥 Backlog` *(Ideias e tarefas ainda em detalhamento)*
     2. `📋 Pronto para Fazer` *(Tarefas já detalhadas com Figma e checklist, prontas para alguém pegar)*
     3. `🚧 Em Andamento` *(Tarefas com desenvolvedor atribuído trabalhando no código)*
     4. `👀 Em Revisão` *(Pull Request aberto aguardando revisão de colegas)*
     5. `✅ Concluído` *(Pull Request aprovado e integrado na branch principal)*
3. **Ativar a entrada automática de Issues no Kanban**:
   * No canto superior direito do seu Project, clique no menu **`...` (três pontos)** > **Workflows** > [Auto-add to project](https://www.google.com/search?q=https%3A%2F%2Fdocs.github.com%2Fen%2Fissues%2Fplanning-and-tracking-with-projects%2Fautomating-your-project%2Fadding-items-automatically).
   * Clique em **Edit**, selecione o repositório `porque-o-java-exige/estaciona-ai-front`, use o filtro `is:issue is:open` e salve. Assim, toda Issue nova criada pelo time entra sozinha no quadro!
4. **Configurar as automações de movimentação de colunas**:
   * Ainda dentro de **Workflows**, configure estas 3 regras nativas:
     * **Item added to project**: defina `Set status to: 📥 Backlog` (toda Issue nova nasce no Backlog).
     * **Pull request linked to issue** (ou quando abrir PR): oriente o time a mover para `👀 Em Revisão` assim que abrir o PR vinculado.
     * **Item closed**: defina `Set status to: ✅ Concluído` (assim que o PR com `Closes #3` recebe merge, a Issue fecha sozinha e o cartão vai direto para Concluído).

## Compartilhar um Guia Rápido no Repositório Ensina a Equipe a Trabalhar no Mesmo Ritmo

Para ensinar seus colegas sem precisar dar uma aula longa toda vez, salve o guia abaixo em um arquivo chamado `CONTRIBUTING.md` na raiz do repositório `estaciona-ai-front` (ou fixe na descrição do próprio quadro). Ele resume o ritual diário em 5 passos práticos:

```markdown
# Como Trabalhar no Kanban Semântico do Estaciona Aí

Nosso quadro Kanban no GitHub Projects organiza tudo o que precisamos entregar no `estaciona-ai-front`. Siga estes 5 passos simples no dia a dia:

### 1. Como criar uma nova tarefa (Issue)
1. Vá na aba **Issues** > **New issue** e escolha um dos nossos formulários (`Nova Funcionalidade/UI` ou `Relato de Bug`).
2. Mantenha o prefixo semântico no título em letras maiúsculas:
   - `[FEAT] - ` para novas telas, fluxos ou variantes de componentes
   - `[STYLE] - ` para ajustes de CSS/SCSS, `:hover`, `:active` ou responsividade
   - `[FIX] - ` para correções de bugs
   - `[REFACTOR] - ` para melhorias internas de código
3. Quebre o que precisa ser feito em tópicos com caixinhas (`- [ ]`). **Regra de ouro:** se uma tarefa não tem link/referência do Figma ou critérios claros, ela fica em `📥 Backlog` até ser detalhada. Quando estiver completa e clara, mova para `📋 Pronto para Fazer`.

### 2. Como puxar uma tarefa para trabalhar
1. Olhe a coluna `Pronto para Fazer` (dando preferência aos cartões de maior prioridade).
2. Abra a Issue, clique em **Assignees** (na barra lateral direita) e coloque o **seu próprio usuário** (`Assign yourself`).
3. Arraste o cartão para a coluna `Em Andamento`. *Evite ter mais de 1 ou 2 cartões "Em Andamento" no seu nome ao mesmo tempo!*

### 3. Como criar a branch e fazer commits semânticos
1. Dentro da própria Issue, você pode clicar em **Create a branch** (na barra lateral direita, em *Development*) ou criar pelo terminal seguindo o padrão:
   - `feat/3-variantes-button` (tipo + número da issue + resumo curto)
   - `style/3-hover-focus-button`
   - `fix/12-modal-reserva`
2. Ao longo do desenvolvimento, vá marcando as caixinhas (`- [x]`) dentro da Issue para todo o time ver a barra de progresso avançar no Kanban!

### 4. Como abrir o Pull Request e pedir revisão
1. Terminou o código e testou no navegador? Abra um **Pull Request (PR)**.
2. No texto do PR, escreva **`Closes #NUMERO_DA_ISSUE`** (exemplo: `Closes #3`). Isso conecta o seu código ao cartão do Kanban!
3. Mova o cartão no Kanban para `Em Revisão` e marque pelo menos um colega em **Reviewers**.

### 5. Como concluir a tarefa
1. Quando o colega revisar, testar e aprovar o seu Pull Request, faça o **Merge pull request**.
2. Graças ao `Closes #numero`, o GitHub fechará a Issue e moverá o seu cartão para `Concluído` automaticamente! 
```
