# 05 — Arquitetura de Software, Diagramas UML e Banco de Dados

> Documentação arquitetural completa do **Estaciona Aí**, reunindo a organização em camadas, os Diagramas de Casos de Uso e de Classes (renderizados nativamente no GitHub via Mermaid), o Modelo Lógico Relacional corrigido (10 tabelas e 18 Foreign Keys) e o Fluxo de Telas.

## 1. Visão Arquitetural (Front-end, Back-end e Cloud)

```mermaid
flowchart LR
    subgraph Cliente ["SPA Front-end (Angular + Atomic Design)"]
        UI["Páginas & Componentes SCSS"]
        Guard["AuthGuard & JWT Interceptor"]
    end

    subgraph Nuvem ["Serviços Externos & Cloud"]
        Azure["Microsoft Azure Blob Storage
(Upload de Fotos #19)"]
        Geo["API de Geolocalização
& Mapas Interativos"]
    end

    subgraph Servidor ["API REST Back-end (Java 17 + Spring Boot)"]
        Sec["Spring Security (JWT + BCrypt)"]
        Ctrl["REST Controllers"]
        Svc["Services (Regras RN01..RN08 &
Notificações #30)"]
        Repo["Spring Data JPA Repositories"]
    end

    subgraph Dados ["Persistência Relacional"]
        DB[("MySQL Database
10 Tabelas UUID / 18 FKs")]
    end

    UI --> Guard
    Guard -->|HTTPS / JSON| Sec
    UI -->|Renderização < 2s| Geo
    Sec --> Ctrl
    Ctrl --> Svc
    Svc -->|SDK Cloud| Azure
    Svc --> Repo
    Repo --> DB
```

## 2. Diagrama de Casos de Uso (UML)

```mermaid
flowchart LR
    Driver(["👤 Locatário
(Motorista)"])
    Owner(["🏠 Locador
(Proprietário)"])
    Admin(["🛡️ Administrador
(ROLE_ADMIN)"])

    subgraph EstacionaAi ["Fronteira do Sistema — Estaciona Aí"]
        UC01(["UC01 - Cadastrar-se e Autenticar-se (JWT)"])
        UC02(["UC02 - Gerenciar Veículos (Placa Única)"])
        UC03(["UC03 - Buscar Vagas no Mapa Interativo"])
        UC04(["UC04 - Solicitar Reserva (Hora ou Diária)"])
        UC05(["UC05 - Realizar Checkout / Pagamento"])
        UC06(["UC06 - Conversar no Chat da Reserva"])
        UC07(["UC07 - Avaliar Garagem Utilizada (1 a 5★)"])
        UC08(["UC08 - Gerenciar Garagens, Fotos Azure e Comodidades"])
        UC09(["UC09 - Gerenciar Reservas Recebidas"])
        UC10(["UC10 - Avaliar Conduta do Motorista (1 a 5★)"])
        UC11(["UC11 - Moderar Contas e Status de Usuários"])
    end

    Driver --> UC01
    Driver --> UC02
    Driver --> UC03
    Driver --> UC04
    Driver --> UC05
    Driver --> UC06
    Driver --> UC07

    Owner --> UC01
    Owner --> UC06
    Owner --> UC08
    Owner --> UC09
    Owner --> UC10

    Admin --> UC01
    Admin --> UC11
```

## 3. Diagrama de Classes UML (Domínio Back-end Spring Boot)

```mermaid
classDiagram
    direction TB

    class User {
        -UUID id
        -String name
        -String email
        -String password
        -String phoneNumber
        -String photoUrl
        -Role roles
        -UserStatus status
        -LocalDateTime createdAt
        -LocalDateTime updatedAt
        +cadastrar()
        +autenticarJWT()
        +atualizarPerfil()
    }

    class Vehicle {
        -UUID id
        -UUID userId
        -String brand
        -String model
        -String color
        -Integer year
        -String licensePlate
        -LocalDateTime createdAt
        +validarPlacaUnica()
    }

    class Garage {
        -UUID id
        -UUID userId
        -String name
        -String description
        -String address
        -Double latitude
        -Double longitude
        -BigDecimal pricePerHour
        -BigDecimal pricePerDay
        -Boolean available
        -String coverImageUrl
        -List~String~ features
        -List~String~ imageUrls
        +validarReservaAtiva()
        +uploadFotoAzure()
    }

    class Booking {
        -UUID id
        -UUID driverId
        -UUID garageId
        -UUID vehicleId
        -BookingType bookingType
        -BookingStatus status
        -LocalDateTime startDateTime
        -LocalDateTime endDateTime
        -BigDecimal totalAmount
        +calcularValorTotal()
        +confirmarReserva()
        +cancelarReserva()
        +concluirReserva()
    }

    class ChatRoom {
        -UUID id
        -UUID bookingId
        -UUID garageId
        -UUID ownerId
        -UUID driverId
        -LocalDateTime createdAt
    }

    class ChatMessage {
        -UUID id
        -UUID roomId
        -UUID senderId
        -String content
        -LocalDateTime createdAt
        +enviarMensagem()
    }

    class GarageReview {
        -UUID id
        -UUID bookingId
        -UUID garageId
        -UUID driverId
        -Integer rating
        -String comment
        -LocalDateTime createdAt
    }

    class DriverReview {
        -UUID id
        -UUID bookingId
        -UUID ownerId
        -UUID driverId
        -Integer rating
        -String comment
        -LocalDateTime createdAt
    }

    User "1" --> "0..*" Vehicle : possui (Locatário)
    User "1" --> "0..*" Garage : anuncia (Locador)
    User "1" --> "0..*" Booking : solicita (driver)
    Garage "1" --> "0..*" Booking : recebe
    Vehicle "1" --> "0..*" Booking : utilizado em
    Booking "0..1" --> "0..1" ChatRoom : origina
    ChatRoom "1" --> "0..*" ChatMessage : contém
    Booking "1" --> "0..1" GarageReview : avalia garagem
    Booking "1" --> "0..1" DriverReview : avalia motorista
```

## 4. Modelo Lógico Relacional (DER — 10 Tabelas e 18 Foreign Keys)

```mermaid
erDiagram
    USERS ||--o{ VEHICLES : "possui (fk_vehicles_user_id)"
    USERS ||--o{ GARAGES : "anuncia (fk_garages_user_id)"
    USERS ||--o{ BOOKINGS : "reserva como motorista (fk_bookings_driver_id)"
    VEHICLES ||--o{ BOOKINGS : "identifica (fk_bookings_vehicle_id)"
    GARAGES ||--o{ BOOKINGS : "aloca (fk_bookings_garage_id)"
    GARAGES ||--o{ GARAGE_FEATURES : "possui comodidades (CASCADE)"
    GARAGES ||--o{ GARAGE_IMAGES : "possui fotos Azure (CASCADE)"
    GARAGES ||--o{ CHAT_ROOMS : "contexto da sala"
    BOOKINGS |o--o| CHAT_ROOMS : "vincula conversa"
    USERS ||--o{ CHAT_ROOMS : "participa (owner_id / driver_id)"
    CHAT_ROOMS ||--o{ CHAT_MESSAGES : "contém mensagens (CASCADE)"
    USERS ||--o{ CHAT_MESSAGES : "envia (sender_id)"
    BOOKINGS ||--o| GARAGE_REVIEWS : "gera avaliação única da vaga"
    BOOKINGS ||--o| DRIVER_REVIEWS : "gera avaliação única do motorista"
```

## 5. Fluxo de Navegação de Telas (User Flow — SPA Angular)

```mermaid
flowchart LR
    LP["Landing Page"] --> Auth["Login / Cadastro JWT"]
    Auth -->|Perfil Locatário| Home["Home: Mapa Interativo de Vagas"]
    Home --> Veh["Meus Veículos (Placa Única)"]
    Home --> Det["Detalhes da Garagem + Fotos Azure"]
    Det --> Book["Agendar Reserva (Hora/Diária) + Checkout"]
    Book --> ChatD["Chat com Proprietário & Avaliação 1-5★"]

    Auth -->|Perfil Locador| Dash["Painel: Minhas Garagens"]
    Dash --> NewG["Cadastrar/Editar Garagem + Upload Azure"]
    Dash --> Req["Gerenciar Reservas Recebidas"]
    Req --> ChatO["Chat com Motorista & Avaliação 1-5★"]
```
