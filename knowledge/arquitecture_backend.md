## PROJECT STRUCTURE (SCREAMING ARCHITECTURE)
El proyecto debe seguir estrictamente esta estructura de carpetas. Los nombres de archivos deben ser explícitos sobre su intención.

```text
📁 Solution
├── 📁 src
│   ├── 📁 Features  <-- (Aquí vive la lógica de negocio Vertical Slice)
│   │   ├── 📁 Customers
│   │   │   ├── 📁 Create
│   │   │   │   ├── ICreateCustomerService.cs
│   │   │   │   ├── CreateCustomerService.cs
│   │   │   │   ├── CreateCustomerEndpoint.cs  <-- (Single Action Controller)
│   │   │   │   ├── CreateCustomerValidator.cs
│   │   │   │   └── CreateCustomerDto.cs
│   │   │   └── CustomerDto.cs (Compartidos del slice padre)
│   ├── 📁 Infrastructure
│   │   ├── 📁 Data
│   │   │   ├── ApplicationDbContext.cs
│   │   │   └── Configurations (IEntityTypeConfiguration)
│   │   ├── 📁 Services
│   │   │   ├── 📁 External (Gateways, Emails, etc)
│   │   │   └── ServiceBase.cs
│   │   └── DependencyInjection.cs
│   ├── 📁 Domain
│   │   ├── 📁 Entities
│   │   └── 📁 Common (Result<T>, etc)
│   └── 📁 WebApi
│       ├── Program.cs
│       └── appsettings.json
└── 📁 tests
```