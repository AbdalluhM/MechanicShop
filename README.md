# MechanicShop Workshop System

A comprehensive workshop management system built with .NET 9.0, featuring a clean architecture pattern with CQRS, domain-driven design, and a modern Blazor WebAssembly frontend.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Features](#features)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Docker Support](#docker-support)
- [Testing](#testing)
- [API Documentation](#api-documentation)

## 🎯 Overview

MechanicShop is a full-stack workshop management application designed to handle:
- Customer and vehicle management
- Work order scheduling and tracking
- Repair task management
- Labor and employee assignment
- Invoice generation and billing
- Real-time updates via SignalR
- Dashboard analytics

The system follows Clean Architecture principles with clear separation of concerns, making it maintainable, testable, and scalable.

## 🏗️ Architecture

The solution follows **Clean Architecture** with the following layers:

### Layer Structure

1. **Domain Layer** (`MechanicShop.Domain`)
   - Core business entities and domain logic
   - Domain events
   - Value objects and aggregates
   - No external dependencies

2. **Application Layer** (`MechanicShop.Application`)
   - Use cases and business logic
   - CQRS pattern with MediatR
   - FluentValidation for input validation
   - DTOs and mapping logic

3. **Infrastructure Layer** (`MechanicShop.Infrastructure`)
   - Data access (Entity Framework Core)
   - Identity management (ASP.NET Core Identity)
   - External service integrations
   - Background jobs
   - Real-time communication (SignalR)

4. **Presentation Layer** (`MechanicShop.Api`)
   - RESTful API controllers
   - Minimal API endpoints
   - API versioning
   - OpenAPI/Swagger documentation
   - Middleware configuration

5. **Client Layer** (`MechanicShop.Client`)
   - Blazor WebAssembly frontend
   - SignalR client for real-time updates
   - JWT authentication

6. **Contracts Layer** (`MechanicShop.Contracts`)
   - Shared request/response models
   - API contracts

## 🛠️ Technology Stack

### Backend
- **.NET 9.0** - Latest .NET framework
- **ASP.NET Core** - Web framework
- **Entity Framework Core 9.0.6** - ORM with SQL Server support
- **MediatR 12.5.0** - CQRS implementation
- **FluentValidation 12.0.0** - Input validation
- **ASP.NET Core Identity** - Authentication and authorization
- **JWT Bearer Authentication** - Token-based auth
- **SignalR** - Real-time communication
- **Serilog** - Structured logging with Seq integration
- **OpenTelemetry** - Observability and metrics
- **QuestPDF** - PDF generation for invoices
- **Swashbuckle/Scalar** - API documentation

### Frontend
- **Blazor WebAssembly** - Client-side web framework
- **Blazored.LocalStorage** - Browser storage
- **SignalR Client** - Real-time updates

### Infrastructure
- **Docker & Docker Compose** - Containerization
- **SQL Server 2022** - Database
- **Seq** - Log aggregation
- **Prometheus** - Metrics collection
- **Grafana** - Metrics visualization

### Testing
- **xUnit** - Unit testing framework
- **NSubstitute** - Mocking framework
- **Testcontainers** - Integration testing with containers
- **Coverlet** - Code coverage

## 📁 Project Structure

```
MechanicShop/
├── src/
│   ├── MechanicShop.Api/              # Web API (Presentation Layer)
│   │   ├── Controllers/               # REST API controllers
│   │   ├── Endpoints/                # Minimal API endpoints
│   │   ├── Components/               # Blazor server components
│   │   ├── Infrastructure/           # API-specific infrastructure
│   │   └── OpenApi/                  # OpenAPI transformers
│   ├── MechanicShop.Application/      # Application Layer (CQRS)
│   │   ├── Features/                 # Feature-based organization
│   │   │   ├── Customers/
│   │   │   ├── WorkOrders/
│   │   │   ├── RepairTasks/
│   │   │   └── ...
│   │   └── Common/                   # Shared application logic
│   ├── MechanicShop.Domain/          # Domain Layer
│   │   ├── Customers/
│   │   ├── Employees/
│   │   ├── Workorders/
│   │   ├── RepairTasks/
│   │   └── Identity/
│   ├── MechanicShop.Infrastructure/  # Infrastructure Layer
│   │   ├── Data/                     # EF Core DbContext
│   │   ├── Identity/                 # Identity configuration
│   │   ├── RealTime/                 # SignalR hubs
│   │   └── Services/                 # External services
│   ├── MechanicShop.Client/          # Blazor WebAssembly Client
│   │   ├── Components/               # UI components
│   │   ├── Pages/                    # Blazor pages
│   │   ├── Services/                 # Client services
│   │   └── Models/                   # Client models
│   └── MechanicShop.Contracts/       # Shared contracts
│       ├── Requests/                 # API request models
│       └── Responses/                # API response models
├── tests/
│   ├── MechanicShop.Api.IntegrationTests/
│   ├── MechanicShop.Application.SubcutaneousTests/
│   ├── MechanicShop.Application.UnitTests/
│   ├── MechanicShop.Domain.UnitTests/
│   └── MechanicShop.Tests.Common/    # Shared test utilities
├── containers/                        # Docker container configs
│   ├── seq/                          # Seq logging config
│   └── prometheus/                   # Prometheus config
├── docker-compose.yml                 # Docker Compose configuration
├── Dockerfile                         # Application Dockerfile
├── Directory.Build.props              # Shared build properties
└── Directory.Packages.props           # Centralized package management
```

## ✨ Features

### Core Functionality

1. **Customer Management**
   - Create, read, update, and delete customers
   - Vehicle management per customer
   - Customer search and filtering

2. **Work Order Management**
   - Create and schedule work orders
   - Assign labor and vehicles
   - Track work order states (Scheduled, InProgress, Completed, Cancelled)
   - Spot/bay assignment (4 available spots)
   - Work order relocation and rescheduling
   - Daily schedule view with timezone support

3. **Repair Task Management**
   - Define repair tasks with labor costs
   - Associate parts with repair tasks
   - Update repair tasks on work orders

4. **Billing & Invoicing**
   - Automatic invoice generation
   - Parts cost calculation
   - Labor cost calculation
   - Tax and discount support
   - PDF invoice generation

5. **Employee Management**
   - Role-based access control (Manager, Labor)
   - Employee assignment to work orders
   - Labor scheduling

6. **Dashboard & Analytics**
   - Daily schedule visualization
   - Work order statistics
   - Real-time updates

7. **Authentication & Authorization**
   - JWT-based authentication
   - Role-based authorization
   - Refresh token support
   - Policy-based access control

8. **Real-time Features**
   - SignalR hubs for live updates
   - Work order status changes broadcast

### Technical Features

- **API Versioning** - URL-based versioning (v1, v2, etc.)
- **Rate Limiting** - Sliding window rate limiting (100 requests/minute)
- **Output Caching** - Response caching for improved performance
- **Request Validation** - FluentValidation integration
- **Problem Details** - RFC 7807 compliant error responses
- **OpenTelemetry** - Distributed tracing and metrics
- **Structured Logging** - Serilog with Seq integration
- **CORS Support** - Configurable cross-origin resource sharing

## 🚀 Getting Started

### Prerequisites

- **.NET 9.0 SDK** - [Download](https://dotnet.microsoft.com/download/dotnet/9.0)
- **SQL Server 2022** (or use Docker)
- **Visual Studio 2022** or **VS Code** with C# extension
- **Docker Desktop** (optional, for containerized deployment)

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd MechanicShopWorkshop-master
   ```

2. **Restore dependencies**
   ```bash
   dotnet restore
   ```

3. **Update connection string**
   - Edit `src/MechanicShop.Api/appsettings.json`
   - Update the `ConnectionStrings:DefaultConnection` to point to your SQL Server instance

4. **Run database migrations**
   ```bash
   cd src/MechanicShop.Api
   dotnet ef database update
   ```
   Or the database will be automatically initialized on first run in Development mode.

5. **Run the application**
   ```bash
   dotnet run --project src/MechanicShop.Api/MechanicShop.Api.csproj
   ```

6. **Access the application**
   - API: `https://localhost:5001` or `http://localhost:5000`
   - Swagger UI: `https://localhost:5001/swagger`
   - Scalar API Docs: `https://localhost:5001/scalar/v1`

### Using Docker Compose

1. **Start all services**
   ```bash
   docker-compose up -d
   ```

2. **Access services**
   - API: `http://localhost:5001`
   - Seq Logs: `http://localhost:5341` or `http://localhost:8081`
   - Prometheus: `http://localhost:9090`
   - Grafana: `http://localhost:3000` (admin/YourStrongPwd)

3. **Stop services**
   ```bash
   docker-compose down
   ```

## ⚙️ Configuration

### Application Settings (`appsettings.json`)

Key configuration sections:

- **ConnectionStrings**: SQL Server connection string
- **AppSettings**:
  - `OpeningTime` / `ClosingTime`: Workshop operating hours
  - `MaxSpots`: Maximum number of work bays (default: 4)
  - `MinimumAppointmentDurationInMinutes`: Minimum booking duration
  - `CorsPolicyName`: CORS policy name
  - `AllowedOrigins`: Allowed CORS origins
- **JwtSettings**: JWT token configuration
- **Serilog**: Logging configuration with Seq integration

### Environment Variables

The application supports environment-specific configuration:
- `ASPNETCORE_ENVIRONMENT`: Set to `Development`, `Staging`, or `Production`
- `ConnectionStrings__DefaultConnection`: Override database connection
- `OTEL_EXPORTER_OTLP_ENDPOINT`: OpenTelemetry endpoint

## 🐳 Docker Support

### Dockerfile

Multi-stage build:
- **Build stage**: Compiles the application
- **Final stage**: Runtime image with minimal dependencies

### Docker Compose Services

- **mechanicshop-api**: Main application (port 5001)
- **sqlserver**: SQL Server 2022 (port 1433)
- **seq**: Log aggregation (ports 5341, 8081)
- **prometheus**: Metrics collection (port 9090)
- **grafana**: Metrics visualization (port 3000)

## 🧪 Testing

The solution includes comprehensive test coverage:

### Test Projects

1. **MechanicShop.Api.IntegrationTests**
   - API endpoint integration tests
   - Uses Testcontainers for database testing

2. **MechanicShop.Application.SubcutaneousTests**
   - Subcutaneous (thin integration) tests
   - Tests application layer with real dependencies

3. **MechanicShop.Application.UnitTests**
   - Unit tests for application logic
   - Behavior and mapper tests

4. **MechanicShop.Domain.UnitTests**
   - Domain model unit tests
   - Business rule validation tests

5. **MechanicShop.Tests.Common**
   - Shared test utilities and helpers
   - Test data builders

### Running Tests

```bash
# Run all tests
dotnet test

# Run specific test project
dotnet test tests/MechanicShop.Domain.UnitTests/

# Run with coverage
dotnet test /p:CollectCoverage=true
```

## 📚 API Documentation

### API Versioning

The API uses URL-based versioning:
- Base URL: `/api/v{version}/`
- Current version: `v1`
- Example: `/api/v1/customers`

### Available Endpoints

#### Customers
- `GET /api/v1/customers` - List all customers
- `GET /api/v1/customers/{id}` - Get customer by ID
- `POST /api/v1/customers` - Create customer (Manager only)
- `PUT /api/v1/customers/{id}` - Update customer (Manager only)
- `DELETE /api/v1/customers/{id}` - Delete customer (Manager only)

#### Work Orders
- `GET /api/v1/workorders` - List work orders (paginated, filterable)
- `GET /api/v1/workorders/{id}` - Get work order by ID
- `POST /api/v1/workorders` - Create work order (Manager only)
- `PUT /api/v1/workorders/{id}/relocation` - Relocate work order
- `PUT /api/v1/workorders/{id}/labor` - Assign labor
- `PUT /api/v1/workorders/{id}/state` - Update work order state
- `PUT /api/v1/workorders/{id}/repair-task` - Update repair tasks
- `DELETE /api/v1/workorders/{id}` - Delete work order (Manager only)
- `GET /api/v1/workorders/schedule/{date}` - Get daily schedule

#### Repair Tasks
- Full CRUD operations for repair tasks

#### Invoices
- Invoice generation and management

#### Identity
- `POST /api/v1/identity/register` - User registration
- `POST /api/v1/identity/login` - User login
- `POST /api/v1/identity/refresh` - Refresh token

### Authentication

All endpoints (except login/register) require JWT authentication:
```
Authorization: Bearer <token>
```

### API Documentation Tools

- **Swagger UI**: `/swagger` - Interactive API documentation
- **Scalar**: `/scalar/v1` - Modern API documentation interface
- **OpenAPI Spec**: `/openapi/v1.json` - OpenAPI specification

## 🔧 Solution File Fix

**Issue Fixed**: The `MechanicShop.Infrastructure` project was incorrectly nested under the `tests` folder in the solution file. This has been corrected to properly nest it under the `src` folder, ensuring proper organization in Visual Studio.

## 📝 Development Notes

### Code Style

- **StyleCop.Analyzers**: Code style enforcement
- **Nullable Reference Types**: Enabled throughout
- **Implicit Usings**: Enabled for cleaner code
- **Latest C# Language Version**: Using latest C# features

### Package Management

- **Central Package Management**: Using `Directory.Packages.props` for centralized version control
- **Shared Build Properties**: `Directory.Build.props` for common project settings

### Domain-Driven Design

- **Aggregates**: WorkOrder, Customer, RepairTask
- **Domain Events**: Dispatched via MediatR
- **Value Objects**: Used for domain concepts
- **Result Pattern**: Used for operation outcomes

## 🤝 Contributing

1. Follow the existing code style and architecture patterns
2. Write unit tests for new features
3. Update API documentation
4. Ensure all tests pass before submitting

## 📄 License

[Specify your license here]

## 🔗 Additional Resources

- [.NET 9.0 Documentation](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-9)
- [Blazor Documentation](https://learn.microsoft.com/en-us/aspnet/core/blazor/)
- [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/)
- [MediatR](https://github.com/jbogard/MediatR)
- [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

---

**Built with ❤️ using .NET 9.0 and Clean Architecture principles**
