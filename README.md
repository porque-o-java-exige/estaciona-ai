# Estaciona Aí — Sistema de Gestão Inteligente de Garagens

> Plataforma integrada para transformar garagens ociosas em vagas inteligentes, seguras e compartilháveis em tempo real.

[Angular](https://www.google.com/search?q=https%3A%2F%2Fangular.dev%2F)
[TypeScript](https://www.google.com/search?q=https%3A%2F%2Fwww.typescriptlang.org%2F)
[Java](https://www.google.com/search?q=https%3A%2F%2Fwww.oracle.com%2Fjava%2F)
[Spring Boot](https://www.google.com/search?q=https%3A%2F%2Fspring.io%2Fprojects%2Fspring-boot)
[Cypress](https://www.google.com/search?q=https%3A%2F%2Fwww.cypress.io%2F)
[GitHub Projects](https://www.google.com/search?q=https%3A%2F%2Fgithub.com%2Forgs%2Fporque-o-java-exige%2Fprojects)

---

## Proposta do Projeto

O **Estaciona Aí** é uma solução de mobilidade urbana desenvolvida no **SENAI** que conecta proprietários de garagens ociosas, motoristas em busca de vagas e operadores de mobilidade em uma única plataforma digital.

Por meio da integração entre software em nuvem, controle de acesso seguro e dispositivos conectados (IoT), o sistema elimina o tempo perdido na procura por vagas, reduz o tráfego urbano e gera renda extra para donos de imóveis.

---

## Pilares da Solução

* **Prático e Real:** Opera sobre tecnologias acessíveis e consolidadas (smartphones, QR Codes, sensores de presença e travas digitais), sem exigir equipamentos industriais de alto custo.
* **Relevante:** Resolve dores reais dos centros urbanos — escassez de estacionamento, insegurança nas ruas e desperdício de garagens vazias durante o horário comercial.
* **Inovador e Escalável:** Centraliza a gestão de vagas particulares e comerciais em um único mapa, preparando o terreno para algoritmos de precificação dinâmica e rotas otimizadas.
* **Segurança Ponta a Ponta:** Garante controle de acesso físico rigoroso (validação de entrada via QR Code e chave digital) e proteção de dados com autenticação segura e criptografia.

---

## Mecanismos de Acesso e Segurança

1. **Validação de Entrada e Saída:** Geração de QR Code temporário e dinâmico no aplicativo do motorista para liberação do acesso.
2. **Monitoramento e Ocupação:** Registro em tempo real de ocupação de cada vaga cadastrada.
3. **Auditoria e Bloqueio Automático:** Notificações instantâneas de uso indevido ou permanência além do tempo contratado com bloqueio preventivo de novos acessos.

---

## Roadmap de Evolução

* **Fase 1 (MVP Atual):** Cadastro de usuários (motoristas e proprietários), mapeamento de vagas particulares, reserva em tempo real e checkout digital.
* **Fase 2:** Integração com estacionamentos comerciais, credenciamento corporativo e gestão de frotas de logística.
* **Fase 3:** Suporte a pontos de recarga para veículos elétricos e vagas reservadas para modais de micromobilidade (bicicletas e patinetes).

---

## Estrutura dos Repositórios (Arquitetura)

Este repositório (`estaciona-ai`) funciona como o portal central e agregador do projeto, conectando os módulos de front-end e back-end por meio de submódulos Git:

```text
estaciona-ai/ (Repositório Principal)
├── docs/                      # Central de documentação e engenharia de requisitos
├── .github/                   # Automações de projetos e templates
├── estaciona-ai-front/        # [Submodule] SPA em Angular, SCSS e Atomic Design
└── estaciona-ai-back/         # [Submodule] REST API em Java com Spring Boot e Maven
```

* **[Repositório Front-end (estaciona-ai-front)](https://github.com/porque-o-java-exige/estaciona-ai-front):** Interface SPA desenvolvida em Angular, TypeScript, SCSS com arquitetura Atomic Design e testes em Cypress.
* **[Repositório Back-end (estaciona-ai-back)](https://github.com/porque-o-java-exige/estaciona-ai-back):** API RESTful desenvolvida em Java 17+ com Spring Boot, Maven, validações de negócio e persistência de dados.

---

## Central de Documentação do Projeto

Todas as especificações técnicas e de gestão ágil estão organizadas na pasta [docs/](https://www.google.com/search?q=docs%2F):

* [Visão do Produto, MVP e Justificativas](https://www.google.com/search?q=docs%2F01-visao-e-mvp.md)
* [Requisitos Funcionais e Não Funcionais](https://www.google.com/search?q=docs%2F02-requisitos.md)
* [Roteiro de Testes e Evidências](https://www.google.com/search?q=docs%2F03-roteiro-e-evidencias-testes.md)
* [Automação de Testes com Cypress](https://www.google.com/search?q=docs%2F04-testes-cypress.md)
* [Manual de Contribuição e Kanban Semântico](https://www.google.com/search?q=docs%2FCONTRIBUTING.md)

---

## Equipe de Desenvolvimento (SENAI)

| Integrante | Papel na Equipe | Atribuições Principais | 
| ----- | ----- | ----- | 
| **Ryan** | **TechLead** | Arquitetura técnica, padrões de código e supervisão de integrações | 
| **Rodrigo** | **Scrum Master** | Facilitação ágil, gestão do Kanban Projects e governança de documentação | 
| **Vinicius** | **Product Owner** | Visão do produto, validação de regras de negócio e priorização do Backlog | 
| **Francisco** | **Developer Backend** | Desenvolvimento da REST API em Java com Spring Boot e banco de dados | 
| **Gabriel Lima** | **Developer Frontend** | Desenvolvimento da SPA em Angular com SCSS e Atomic Design | 
| **Gabriel Vereda** | **Quality Assurance** | Planejamento de cenários de teste, automação em Cypress e garantia de qualidade | 

---

## Licença

Projeto acadêmico desenvolvido para o Trabalho de Conclusão de Curso do **SENAI**. Uso restrito para fins didáticos e avaliativos.
