# Arquitectura de Monolito Moderno

## Diagrama de Arquitectura

### 🎯 **Imagen Vectorial SVG** (Calidad perfecta para zoom)
![Arquitectura Monolito Moderno](./arquitectura-monolito.svg)

> **💡 Tip**: Haz clic derecho en la imagen SVG y selecciona "Abrir imagen en nueva pestaña" para verla en tamaño completo y hacer zoom sin perder calidad.

### 📸 **Imagen PNG de Alta Resolución** (Para presentaciones)
![Arquitectura Monolito Moderno PNG](./arquitectura-monolito.png)

### 📋 **Código Fuente Mermaid**
```mermaid
graph TB
    subgraph "Cliente"
        Browser[🌐 Navegador Web]
    end
    
    subgraph "Frontend Layer"
        Angular[📱 Angular 17+<br/>TypeScript<br/>RxJS<br/>Angular Material]
    end
    
    subgraph "Monolito Moderno - Java 21"
        subgraph "Presentation Layer"
            RestAPI[🔌 REST API<br/>Spring Boot 3.x<br/>Spring Web MVC]
            WebSockets[⚡ WebSockets<br/>Real-time Communication]
        end
        
        subgraph "Business Layer"
            Services[🏢 Business Services<br/>Spring Services<br/>Transaction Management]
            Security[🔒 Security Layer<br/>Spring Security<br/>JWT Authentication]
            Validation[✅ Validation Layer<br/>Bean Validation<br/>Custom Validators]
        end
        
        subgraph "Data Access Layer"
            JPA[🗂️ JPA/Hibernate<br/>Spring Data JPA<br/>Repository Pattern]
            Cache[⚡ Cache Layer<br/>Redis/Caffeine<br/>Spring Cache]
        end
        
        subgraph "Cross-Cutting Concerns"
            Logging[📝 Logging<br/>SLF4J + Logback<br/>Structured Logging]
            Monitoring[📊 Monitoring<br/>Micrometer<br/>Actuator]
            Config[⚙️ Configuration<br/>Spring Profiles<br/>External Config]
        end
    end
    
    subgraph "Database Layer"
        PostgreSQL[(🐘 PostgreSQL 15+<br/>ACID Transactions<br/>JSON Support<br/>Full-text Search)]
    end
    
    subgraph "External Services"
        Email[📧 Email Service<br/>SMTP]
        FileStorage[📁 File Storage<br/>S3/MinIO]
        ThirdPartyAPI[🔗 Third Party APIs<br/>REST/GraphQL]
    end
    
    %% Conexiones principales
    Browser --> Angular
    Angular --> RestAPI
    Angular --> WebSockets
    
    RestAPI --> Security
    Security --> Services
    Services --> Validation
    Validation --> JPA
    Services --> Cache
    JPA --> PostgreSQL
    Cache --> PostgreSQL
    
    %% Conexiones transversales
    Services --> Logging
    Services --> Monitoring
    Services --> Config
    
    %% Conexiones externas
    Services --> Email
    Services --> FileStorage
    Services --> ThirdPartyAPI
    
    %% Estilos
    classDef frontend fill:#61dafb,stroke:#333,stroke-width:2px
    classDef backend fill:#f9f,stroke:#333,stroke-width:2px
    classDef database fill:#336791,stroke:#fff,stroke-width:2px
    classDef external fill:#ff9,stroke:#333,stroke-width:2px
    
    class Angular frontend
    class RestAPI,WebSockets,Services,Security,Validation,JPA,Cache,Logging,Monitoring,Config backend
    class PostgreSQL database
    class Email,FileStorage,ThirdPartyAPI external
```

## Componentes Tecnológicos

### Frontend - Angular
- **Framework**: Angular 17+ con TypeScript
- **Estado**: NgRx o Akita para manejo de estado
- **UI**: Angular Material o PrimeNG
- **HTTP**: HttpClient con interceptors
- **Autenticación**: JWT tokens

### Backend - Java 21 Monolito
- **Framework**: Spring Boot 3.x
- **Seguridad**: Spring Security con JWT
- **Persistencia**: Spring Data JPA + Hibernate
- **Cache**: Redis o Caffeine
- **Monitoreo**: Spring Actuator + Micrometer

### Base de Datos
- **PostgreSQL 15+**: Base de datos principal
- **Características**: JSONB, Full-text search, Transacciones ACID

## Flujo de Datos

### 🎯 **Diagrama de Secuencia - Imagen Vectorial SVG**
![Flujo de Datos](./flujo-datos.svg)

### 📋 **Código Fuente del Diagrama de Secuencia**
```mermaid
sequenceDiagram
    participant U as Usuario
    participant A as Angular App
    participant R as REST API
    participant S as Services
    participant D as PostgreSQL
    
    U->>A: Interacción (click, form)
    A->>A: Validación Frontend
    A->>R: HTTP Request (GET/POST/PUT/DELETE)
    R->>R: Autenticación/Autorización
    R->>S: Lógica de Negocio
    S->>D: Query/Update
    D-->>S: Resultado
    S-->>R: Response Data
    R-->>A: JSON Response
    A->>A: Update UI
    A-->>U: Resultado Visual
```

## Patrones de Diseño Utilizados

- **MVC**: Separación clara de responsabilidades
- **Repository Pattern**: Abstracción de acceso a datos
- **Service Layer**: Lógica de negocio centralizada
- **DTO Pattern**: Transferencia de datos entre capas
- **Builder Pattern**: Construcción de objetos complejos
- **Observer Pattern**: Eventos y notificaciones