# 03 — Roteiro de Testes, Plano de QA e Evidências

> Governança de Garantia de Qualidade (Quality Assurance — QA) conduzida para validação funcional, de regras de negócio e de regressão do sistema **Estaciona Aí**.

## 1. Estratégia de Testes em Camadas (Pirâmide de Testes)

A validação de qualidade do **Estaciona Aí** está estruturada em três níveis complementares:

1. **Testes Unitários e de Integração no Back-end (`estaciona-ai-back`):** Validação isolada das regras de negócio nas camadas `Service` e `Controller` com JUnit 5 e Spring Boot Test (ex.: Issue `#34 [TEST] Add Unit and Integration Tests for Notifications`, validação de cálculo tarifário e bloqueio de conflito de reservas).
2. **Testes de Componentes UI no Front-end (`estaciona-ai-front`):** Inspeção de estados, propriedades de entrada (`@Input`), eventos de saída (`@Output`) e acessibilidade dos átomos e moléculas do Atomic Design (`button #3/#6`, `input #5`, `chip #16`).
3. **Testes Automatizados End-to-End (E2E) com Cypress:** Simulação completa da jornada do Locatário e do Locador diretamente no navegador, validando a integração entre a SPA Angular, a API Spring Boot e o banco de dados MySQL.

## 2. Caderno de Casos de Teste (Roteiro de Homologação)

| ID do Teste | Caso de Teste | Requisito / Regra | Pré-Condição | Passos de Execução | Resultado Esperado | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: |
| **CT01** | Cadastro de usuário com e-mail válido e único | `RF01`, `RN01`, `RNF01` | E-mail não existente na tabela `users`. | 1. Acessar tela de Cadastro;<br>2. Preencher nome, e-mail, telefone e senha;<br>3. Submeter formulário. | Usuário criado com status `ACTIVE`, senha salva em hash BCrypt e retorno HTTP `201 Created`. | **Aprovado (Sprint 1)** |
| **CT02** | Bloqueio de cadastro com e-mail duplicado | `RN01` | E-mail já registrado no banco. | 1. Tentar cadastrar novo usuário com o mesmo e-mail. | API rejeita a operação com HTTP `409 Conflict` / `400 Bad Request` e exibe alerta amigável na UI. | **Aprovado (Sprint 1)** |
| **CT03** | Autenticação JWT e bloqueio de rota privada | `RF02`, `RNF01`, `RNF02` | Usuário cadastrado e ativo. | 1. Efetuar login com credenciais válidas;<br>2. Acessar rota protegida com o token Bearer JWT. | Token JWT emitido com sucesso; requisições sem token recebem HTTP `401 Unauthorized`. | **Em Execução (`#35`)** |
| **CT04** | Cadastro de veículo com validação de placa única | `RF03`, `RN01` | Usuário autenticado. | 1. Acessar Meus Veículos;<br>2. Informar marca, modelo, cor, ano e placa;<br>3. Salvar e tentar repetir a mesma placa. | Primeiro cadastro retorna `201 Created`; segundo retorna erro pela constraint `uk_vehicles_license_plate`. | **Aprovado (`#7`)** |
| **CT05** | Upload de imagem de garagem/perfil no Microsoft Azure | `RF05` | Conta de armazenamento Azure configurada. | 1. Enviar arquivo de imagem válido (`.jpg`/`.png`) no endpoint de fotos. | Arquivo salvo no container do Azure Blob Storage e URL pública persistida no banco (`#19`). | **Aprovado (`#19`)** |
| **CT06** | Cadastro de garagem e exibição no mapa interativo | `RF04`, `RF06`, `RF07`, `RNF05` | Locador autenticado. | 1. Cadastrar garagem com endereço válido, comodidades e preços;<br>2. Abrir a Home com Mapa. | Endereço convertido em Lat/Long e pino renderizado no mapa em tempo `< 2 segundos`. | **Planejado (Sprint 2)** |
| **CT07** | Bloqueio de edição de endereço em garagem com reserva ativa | `RN03` | Garagem com reserva em status `PENDING` ou `CONFIRMED`. | 1. O locador tenta alterar o endereço ou excluir a garagem. | Sistema bloqueia a alteração direta do endereço e exibe mensagem de segurança ao locador. | **Planejado (Sprint 2)** |
| **CT08** | Reserva de vaga por hora (`HOURLY`) e diária (`DAILY`) | `RF08`, `RF09`, `RN05`, `RN06` | Garagem disponível e veículo cadastrado. | 1. Selecionar vaga no mapa;<br>2. Escolher veículo, modalidade e horário;<br>3. Confirmar reserva. | Cálculo exato de `total_amount`, criação do registro em `bookings` e disparo de notificação (`#30`). | **Planejado (Sprint 3)** |
| **CT09** | Prevenção de conflito de horário na mesma vaga | `RN04` | Vaga já reservada das 08h às 12h no sábado. | 1. Outro motorista tenta reservar a mesma vaga das 10h às 14h no mesmo sábado. | Sistema impede a sobreposição de horário e informa indisponibilidade no período. | **Planejado (Sprint 3)** |
| **CT10** | Troca de mensagens no Chat e Avaliação Mútua única | `RF11`, `RF12`, `RN07`, `RN08` | Reserva existente / concluída (`COMPLETED`). | 1. Enviar mensagem na sala da reserva;<br>2. Registrar avaliação de 1 a 5 estrelas;<br>3. Tentar avaliar novamente a mesma reserva. | Mensagem gravada em `chat_messages`; primeira avaliação aceita e segunda bloqueada por `UNIQUE (booking_id)`. | **Planejado (Sprint 4)** |

## 3. Registro de Defeitos e Lições Aprendidas (Bug Tracking)

| Issue | Descrição do Incidente | Ação Corretiva Aplicada | Sprint |
| :--- | :--- | :--- | :---: |
| `back #6` | Necessidade de ajuste estrutural após o primeiro merge do CRUD de usuários (`[REVERT 5] feat user crud`). | Execução de revert controlado, refatoração sobre a estrutura base (`#4`) e reintegração limpa junto ao CRUD de veículos (`#7`). | **Sprint 1** |
| `back #28 / #29` | Inconsistência na visualização e filtros de Issues no quadro (`[BUG] issues sumindo`). | Revisão das regras de filtro e vinculação dos submódulos ao painel central do GitHub Projects pelo Scrum Master. | **Sprint 1** |
