# 02 — Engenharia de Requisitos e Regras de Negócio

> Especificação semântica rastreável entre os atores do domínio, as Regras de Negócio (`RN`), os Requisitos Funcionais (`RF`), os Requisitos Não Funcionais (`RNF`) e a modelagem de dados SQL do **Estaciona Aí**.

## 1. Padronização Semântica dos Atores do Domínio

Para garantir precisão técnica e jurídica em toda a arquitetura e documentação:

* **Locador (Proprietário / `owner_id`):** Usuário autenticado (`ROLE_USER`) proprietário ou responsável por um imóvel que cadastra e disponibiliza sua garagem ociosa para locação.
* **Locatário (Motorista / `driver_id`):** Usuário autenticado (`ROLE_USER`) que cadastra seus veículos, busca vagas disponíveis no mapa e realiza reservas por hora ou diária.
* **Administrador (`ROLE_ADMIN`):** Perfil de governança responsável por moderar o status de contas (`ACTIVE`, `PENDING`, `INACTIVE`, `SUSPENDED`, `BANNED`) e assegurar a conformidade da plataforma.

## 2. Regras de Negócio (RN)

| Código | Regra de Negócio | Implementação / Constraint no Banco (SQL) |
| :--- | :--- | :--- |
| **RN01** | **Unicidade de E-mail e Placa:** Não é permitido o cadastro de mais de um usuário com o mesmo endereço de e-mail, nem o registro de mais de um veículo com a mesma placa (`license_plate`). | Constraints `uk_users_email` e `uk_vehicles_license_plate` |
| **RN02** | **Controle de Status de Conta:** Apenas usuários com status `ACTIVE` podem anunciar garagens ou efetuar reservas; contas `PENDING`, `INACTIVE`, `SUSPENDED` ou `BANNED` ficam bloqueadas para transações. | Coluna `users.status` (`ENUM`) e filtro no Spring Security |
| **RN03** | **Bloqueio de Edição Crítica de Garagem:** O sistema não permite alterar o endereço/geolocalização nem excluir uma garagem que possua reservas com status `PENDING` ou `CONFIRMED`. | Validação de integridade em `GarageService` e `fk_bookings_garage_id_garages` |
| **RN04** | **Prevenção de Conflito de Horários:** Uma garagem não pode receber duas reservas sobrepostas no mesmo intervalo (`start_date_time` a `end_date_time`), e o locatário mantém apenas uma reserva ativa por vez. | Verificação temporal em `BookingRepository` e flag `garages.available` |
| **RN05** | **Vínculo Obrigatório de Veículo:** Toda solicitação de reserva exige que o locatário selecione um veículo previamente cadastrado em seu perfil para identificação de entrada pelo locador. | Chave estrangeira obrigatória `bookings.vehicle_id` (`NOT NULL`) |
| **RN06** | **Cálculo Tarifário Automático:** O valor total da reserva (`total_amount`) é calculado automaticamente conforme a modalidade escolhida (`HOURLY` ou `DAILY`) multiplicada pelo período selecionado. | `bookings.booking_type` × `garages.price_per_hour` / `price_per_day` |
| **RN07** | **Privacidade de Comunicação:** Salas de chat (`chat_rooms`) são abertas exclusivamente entre o locador proprietário da garagem (`owner_id`) e o locatário vinculado (`driver_id`). | Chaves estrangeiras `owner_id`, `driver_id` e `garage_id` em `chat_rooms` |
| **RN08** | **Avaliação Mútua Única por Reserva:** Cada reserva concluída (`COMPLETED`) permite exatamente uma avaliação da garagem pelo motorista e uma avaliação do motorista pelo proprietário, com nota inteira de 1 a 5. | Constraints `UNIQUE` `uk_garage_reviews_booking_id` e `uk_driver_reviews_booking_id` |

## 3. Requisitos Funcionais (RF)

> Descritos sob o padrão **"O sistema deve permitir..."** e vinculados às operações da API REST (`POST`, `GET`, `PUT`, `PATCH`, `DELETE`).

| Código | Requisito Funcional | Entidade / Módulo Relacionado |
| :--- | :--- | :--- |
| **RF01** | O sistema deve permitir o cadastro, consulta, atualização e inativação de usuários (Locadores, Locatários e Administradores). | `users` (`#5`) |
| **RF02** | O sistema deve permitir a autenticação de usuários via e-mail e senha, gerenciando o controle de sessão por token JWT. | `users` / Auth (`#35`) |
| **RF03** | O sistema deve permitir que o locatário cadastre, edite, consulte e remova seus veículos (marca, modelo, cor, ano e placa). | `vehicles` (`#7`) |
| **RF04** | O sistema deve permitir que o locador cadastre, edite, consulte e altere a disponibilidade de suas garagens (nome, descrição, endereço e preços por hora/dia). | `garages` |
| **RF05** | O sistema deve permitir que o locador associe comodidades específicas (ex.: vaga coberta, câmera, portão automático) e faça upload de fotos da garagem no Microsoft Azure. | `garage_features`, `garage_images` (`#19`) |
| **RF06** | O sistema deve validar e converter o endereço da garagem em coordenadas geográficas (latitude e longitude) via serviço de geolocalização. | `garages` (Mapa) |
| **RF07** | O sistema deve exibir as garagens disponíveis em um mapa interativo na Home, permitindo visualizar preços, fotos e detalhes da vaga. | `garages` (`#12`) |
| **RF08** | O sistema deve permitir que o locatário realize a reserva de uma garagem disponível informando o veículo, a modalidade (`HOURLY` ou `DAILY`) e o período de entrada e saída. | `bookings` |
| **RF09** | O sistema deve calcular o valor total estimado da locação (`total_amount`) e processar o fluxo de confirmação e pagamento da reserva. | `bookings` / Payment (`#21`) |
| **RF10** | O sistema deve permitir gerenciar o ciclo de vida da reserva (`PENDING`, `CONFIRMED`, `COMPLETED` e `CANCELLED`) e consultar o histórico de locações. | `bookings` |
| **RF11** | O sistema deve disponibilizar salas de chat entre locador e locatário para alinhamento de acesso e suporte, armazenando o histórico de mensagens. | `chat_rooms`, `chat_messages` |
| **RF12** | O sistema deve permitir que o locatário avalie a garagem utilizada e que o locador avalie a conduta do motorista (nota 1 a 5 e comentário) após o término da reserva. | `garage_reviews`, `driver_reviews` |
| **RF13** | O sistema deve emitir notificações e alertas sobre eventos de reserva, pagamentos e segurança. | Notification System (`#30`) |

## 4. Requisitos Não Funcionais (RNF)

> Especificados segundo as categorias de qualidade de software da norma **ISO/IEC 25010**.

| Código | Categoria | Requisito Não Funcional |
| :--- | :--- | :--- |
| **RNF01** | **Segurança** | O sistema deve armazenar senhas criptografadas utilizando algoritmo BCrypt e proteger os endpoints da API REST via autenticação stateless com tokens JWT. |
| **RNF02** | **Controle de Acesso** | O sistema deve implementar autorização baseada em papéis (`ROLE_USER` e `ROLE_ADMIN`), garantindo que apenas o proprietário manipule seus próprios recursos. |
| **RNF03** | **Arquitetura e Padrões** | O front-end deve ser estruturado como uma Single Page Application (SPA) em Angular seguindo a metodologia Atomic Design, consumindo uma API RESTful em Java 17 (Spring Boot). |
| **RNF04** | **Portabilidade e Responsividade** | A interface web deve ser responsiva (Mobile-First), adaptando-se a navegadores em smartphones, tablets e desktops sem exigir instalação nativa. |
| **RNF05** | **Desempenho e Eficiência** | As consultas de disponibilidade e renderização de marcadores no mapa devem responder em tempo inferior a 2 segundos, realizando requisições sob demanda para poupar rede e bateria. |
| **RNF06** | **Integridade de Dados** | O banco de dados relacional deve utilizar identificadores universais únicos (`UUID / CHAR(36)`) como chaves primárias e garantir integridade referencial por meio de 18 chaves estrangeiras. |
| **RNF07** | **Testabilidade e Qualidade** | Os fluxos críticos de interface e regras de negócio devem contar com cobertura de testes unitários/integração no back-end e testes End-to-End (E2E) no framework Cypress. |
