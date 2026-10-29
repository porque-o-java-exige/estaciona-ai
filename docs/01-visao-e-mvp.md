# 01 — Visão do Produto, MVP e Estratégia de Negócio

> Documento estratégico de concepção do **Estaciona Aí — Sistema de Gestão Inteligente de Garagens**, desenvolvido como Trabalho de Conclusão de Curso (TCC) na **Escola SENAI Suíço-Brasileira "Paulo Ernesto Tolle"**.

## 1. Problema e Justificativa

No entorno de polos educacionais e comerciais intensos — como a região da **Escola SENAI Suíço-Brasileira em Santo Amaro (São Paulo/SP)** — motoristas, estudantes e profissionais perdem entre 20 e 35 minutos procurando vagas na via pública, ficando sujeitos a atrasos, multas, colisões e furtos. Em contrapartida, dezenas de imóveis residenciais e comerciais vizinhos mantêm vagas de garagem particulares 100% ociosas durante o horário de aula e expediente por falta de um canal digital seguro para locação sob demanda.

## 2. Proposta de Valor e Golden Circle (Círculo Dourado)

* **POR QUÊ? (Why — Propósito):** Eliminar o estresse, os atrasos e a insegurança patrimonial ao estacionar em centros urbanos, transformando espaços privados ociosos em mobilidade inteligente e geração de renda extra para a comunidade.
* **COMO? (How — Processo e Diferencial):** Conectando proprietários de garagens (**Locadores**) e motoristas (**Locatários**) em tempo real, com geolocalização precisa, identificação prévia de veículo/placa, comunicação direta via chat integrado, armazenamento em nuvem (Microsoft Azure) e sistema de reputação mútua.
* **O QUÊ? (What — O Produto):** Uma Single Page Application (SPA) Web Responsiva (Mobile-First) em **Angular** integrada a uma API RESTful em **Java 17 (Spring Boot)** para locação de garagens por hora (`HOURLY`) ou diária (`DAILY`).

## 3. Escopo do Produto Mínimo Viável (MVP) e Roadmap

| Fase | Escopo de Entrega | Status |
| :--- | :--- | :--- |
| **Fase 1 — MVP (Escopo do TCC)** | Autenticação JWT e perfis (`users`), cadastro de veículos com placa única (`vehicles`), anúncio de garagens com fotos no Azure e comodidades (`garages`, `garage_images`, `garage_features`), busca em mapa interativo, reservas por hora/diária com bloqueio de conflito (`bookings`), notificações (`#30`), chat entre locador e locatário (`chat_rooms`, `chat_messages`) e avaliações mútuas de 1 a 5 estrelas (`garage_reviews`, `driver_reviews`). | **Em Execução (Sprints 1–4)** |
| **Fase 2 — Expansão Comercial** | Liquidação financeira automática via gateway Pix/Stripe, credenciamento de estacionamentos comerciais e gestão de frotas corporativas. | **Roadmap Pós-TCC** |
| **Fase 3 — IoT e Mobilidade Elétrica** | Abertura automatizada de portões via trava digital IoT / QR Code dinâmico temporário e filtro dedicado para vagas com recarga de veículos elétricos. | **Roadmap Pós-TCC** |

## 4. Personas e Mapas da Empatia

### 4.1. Persona 1 — Lucas Mendes (Locatário / Motorista)

| Atributo | Descrição |
| :--- | :--- |
| **Perfil** | 21 anos, Auxiliar Técnico de Manutenção durante a semana e estudante aos sábados na Escola SENAI Suíço-Brasileira. Proprietário de um Chevrolet Onix 2019. |
| **Dores** | Atrasos para entrar em aula por falta de vagas na rua; medo de furtos ou avarias na via pública; estacionamentos comerciais fechados aos sábados ou com diárias elevadas. |
| **Necessidades** | Visualizar garagens próximas ao SENAI no mapa pelo celular, reservar por hora ou diária antes de sair de casa, cadastrar a placa do carro e combinar a entrada pelo chat. |

### 4.2. Persona 2 — Roberto Almeida (Locador / Proprietário da Garagem)

| Atributo | Descrição |
| :--- | :--- |
| **Perfil** | 48 anos, morador de imóvel residencial a duas quadras do SENAI Suíço-Brasileira, com uma vaga coberta e portão automático ociosa durante o dia e aos sábados. |
| **Dores** | Espaço parado gerando custo de IPTU sem retorno; insegurança de alugar informalmente para desconhecidos sem saber quem é o motorista ou qual veículo entrará no imóvel. |
| **Necessidades** | Anunciar sua vaga nos horários livres com fotos e regras claras, definir preço por hora/diária, exigir veículo identificado e avaliar o comportamento do motorista após o uso. |

## 5. Project Model Canvas (13 Blocos)

| Bloco | Definição Estratégica |
| :--- | :--- |
| **1. Justificativas** | Escassez de vagas no entorno do SENAI, insegurança na rua e alto índice de garagens residenciais ociosas. |
| **2. Objetivo SMART** | Entregar até 09/12/2026 a plataforma web responsiva Estaciona Aí (Angular + Spring Boot + MySQL + Azure) com busca em mapa inferior a 2 segundos e cobertura E2E no Cypress. |
| **3. Benefícios** | Pontualidade e segurança para alunos/motoristas; renda extra passiva para moradores; redução de veículos circulando em busca de vagas. |
| **4. Produto** | **Estaciona Aí** — Plataforma Web Responsiva (Mobile-First) de gestão e compartilhamento inteligente de garagens. |
| **5. Requisitos** | Auth JWT, CRUD de Usuários/Veículos/Garagens, Upload Azure, Mapa Interativo, Reservas (Hora/Diária), Chat em tempo real, Notificações e Avaliações Mútuas. |
| **6. Stakeholders** | Equipe de desenvolvimento, Profa. Orientadora Fiama Brenda Santos, Escola SENAI Suíço-Brasileira, Locatários (Motoristas), Locadores (Proprietários) e Provedores Cloud. |
| **7. Equipe** | Ryan Ferreira (TechLead), Rodrigo de Paula (Scrum Master), Vinícius Araújo (Product Owner), Francisco Alamino (Back-end), Gabriel Lima (Front-end) e Gabriel Vereda (QA). |
| **8. Premissas** | Java 17 + Spring Boot, Angular + Atomic Design, MySQL (UUID), Microsoft Azure Blob Storage, Cypress e GitHub Projects. |
| **9. Grupos de Entregas** | **E1:** Discovery, Requisitos, SQL e Figma; **E2:** Auth JWT, Usuários, Veículos e Azure; **E3:** Garagens, Mapa e Reservas; **E4:** Chat, Avaliações, Testes Cypress e TCC. |
| **10. Restrições** | Entrega final até 09/12/2026; execução concentrada em 4 Sprints; uso exclusivo de camadas gratuitas e estudantis. |
| **11. Riscos** | Sobrecarga de integração Front/Back, limites de cota em APIs externas e conflitos de merge (mitigados por revisão de PRs e WIP limit). |
| **12. Linha do Tempo** | Concepção: Abr–Out/2026; Sprint 1: 18/10–28/10; Sprint 2: 29/10–11/11; Sprint 3: 12/11–25/11; Sprint 4: 26/11–09/12/2026. |
| **13. Custos** | R$ 0,00 de custo financeiro direto (infraestrutura SENAI e GitHub/Azure Students) e **186 Dias de Esforço (Pessoa-Dia)** da equipe. |

## 6. Matriz SWOT (FOFA) e Stakeholders

* **Forças Internas:** Equipe multidisciplinar com QA dedicado, arquitetura desacoplada em submódulos (Angular + Spring Boot + Azure) e automação E2E em Cypress.
* **Fraquezas Internas:** Janela de execução compacta de 4 Sprints e ausência inicial de app nativo nas lojas (mitigada por SPA Mobile-First).
* **Oportunidades Externas:** Demanda real imediata no entorno do SENAI Santo Amaro, expansão da economia compartilhada e suporte docente institucional.
* **Ameaças Externas:** Limites de requisições gratuitas em APIs de terceiros e resistência cultural inicial de proprietários conservadores.
