# 04 — Automação de Testes End-to-End (E2E) com Cypress

> Especificação técnica da suíte de testes automatizados de interface e integração construída no módulo **Front-end (`estaciona-ai-front`)** sob responsabilidade de Quality Assurance (QA).

## 1. Visão Geral e Arquitetura de Automação

O framework **Cypress** é utilizado no **Estaciona Aí** para validar os fluxos críticos da Single Page Application (SPA) em Angular diretamente no navegador, garantindo que o contrato entre os componentes visuais (Atomic Design) e a API REST em Spring Boot permaneça íntegro a cada entrega de Sprint (`RNF07`).

### Estrutura de Diretórios no Repositório `estaciona-ai-front`

```text
estaciona-ai-front/
├── cypress/
│   ├── e2e/
│   │   ├── 01-auth.cy.ts            # Testes de Login, Cadastro e Proteção de Rotas JWT
│   │   ├── 02-ui-components.cy.ts   # Validação de Átomos e Moléculas (Button, Input, Chip)
│   │   ├── 03-vehicles.cy.ts        # CRUD de Veículos e validação de placa única (RN01)
│   │   ├── 04-garages-map.cy.ts     # Cadastro de Garagens, Upload Azure e Mapa Interativo
│   │   ├── 05-bookings.cy.ts        # Fluxo de Reserva (Hora/Diária), Conflito (RN04) e Tarifação
│   │   └── 06-chat-reviews.cy.ts    # Salas de Chat e Avaliação Mútua 1-5 estrelas (RN08)
│   ├── fixtures/
│   │   ├── user-driver.json         # Massa de dados da Persona 1 (Lucas Mendes - Motorista)
│   │   ├── user-owner.json          # Massa de dados da Persona 2 (Roberto Almeida - Locador)
│   │   └── garage-senai.json        # Massa de dados de garagem próxima ao SENAI Santo Amaro
│   └── support/
│       ├── commands.ts              # Comandos customizados (cy.loginAsDriver(), cy.loginAsOwner())
│       └── e2e.ts                   # Configurações globais e interceptadores HTTP
└── cypress.config.ts                # Configuração de baseUrl, timeouts e geração de vídeos/prints
```

## 2. Comandos Customizados e Boas Práticas Semânticas

Para evitar código duplicado e tornar os testes resilientes a mudanças de estilo CSS, a suíte adota seletores dedicados `data-cy` nos componentes Angular:

* **Seletores Semânticos no HTML Angular:** Uso padronizado de atributos `data-cy="input-email"`, `data-cy="btn-submit-login"`, `data-cy="chip-feature"`, `data-cy="map-marker-garage"`.
* **Interceptação e Espionagem de Rede (`cy.intercept`):** Monitoramento das chamadas REST para `POST /api/auth/login`, `GET /api/garages`, `POST /api/bookings` para validar códigos de status HTTP (`200`, `201`, `400`, `401`, `409`) e tempos de resposta inferiores a 2 segundos (`RNF05`).

### Exemplo de Especificação E2E (`01-auth.cy.ts` e `05-bookings.cy.ts`)

```typescript
describe('Fluxo Crítico: Autenticação e Reserva de Garagem (Estaciona Aí)', () => {
  beforeEach(() => {
    cy.intercept('GET', '**/api/garages*').as('getGarages');
    cy.intercept('POST', '**/api/bookings').as('createBooking');
  });

  it('Deve autenticar o motorista (Locatário), exibir garagens no mapa em < 2s e criar uma reserva', () => {
    cy.visit('/login');
    cy.get('[data-cy="input-email"]').type('lucas.mendes@aluno.senai.br');
    cy.get('[data-cy="input-password"]').type('SenhaForte@2026');
    cy.get('[data-cy="btn-login"]').click();

    cy.url().should('include', '/home');
    cy.wait('@getGarages').its('response.statusCode').should('eq', 200);

    cy.get('[data-cy="map-marker-garage"]').first().click();
    cy.get('[data-cy="select-booking-type"]').select('HOURLY');
    cy.get('[data-cy="select-vehicle"]').select('ABC1D23');
    cy.get('[data-cy="btn-confirm-booking"]').click();

    cy.wait('@createBooking').its('response.statusCode').should('eq', 201);
    cy.get('[data-cy="toast-notification"]').should('contain', 'Reserva solicitada com sucesso');
  });
});
```

## 3. Como Executar os Testes Localmente

```bash
# 1. Acessar o submódulo de front-end
cd estaciona-ai-front

# 2. Abrir a interface interativa do Cypress (Modo Desenvolvimento / Debug)
npx cypress open

# 3. Executar toda a bateria em modo Headless (Geração de Evidências para o TCC)
npx cypress run
```
