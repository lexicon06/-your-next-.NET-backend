# .NET Backend Blueprint
A comprehensive .NET 9 backend template using .NET Aspire for local development and deployment to Azure. This template provides a modern, cloud-ready API with authentication, authorization and database integration.

## Overview

This template includes:

- **.NET 9 Web API** with Entity Framework Core and PostgreSQL
- **Vertical Slice Architecture** with feature-based organization
- **Keycloak authentication** for JWT-based security
- **Global error handling** for consistent API responses
- **.NET Aspire** orchestration for local development
- **Azure Container Apps** deployment ready
- **CI/CD pipeline** for GitHub Actions

## Prerequisites

Before getting started, ensure you have the following tools installed:

- **[.NET 9 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)** - The latest .NET SDK
- **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** - For containerized services (PostgreSQL, Keycloak)
- **[.NET Aspire CLI](https://learn.microsoft.com/dotnet/aspire/cli/install)** - To run the application locally
- **[Azure Developer CLI (azd)](https://learn.microsoft.com/en-us/azure/developer/azure-developer-cli/install-azd)** - For Azure deployment

## Getting Started

### Running the Project Locally

1. **Navigate to the root directory** of the project
2. **Run the application** using .NET Aspire:
   ```bash
   dotnet run --project src/TemplateApp.AppHost
   ```
   Or alternatively:
   ```bash
   aspire run
   ```

3. **Access the Aspire Dashboard** using the URL provided in the terminal output

### Testing the API

Once the application is running, you can test the API using:

- **Swagger UI**: Navigate to http://localhost:5082/swagger to explore and test the API endpoints interactively
- **Aspire Dashboard**: Monitor application health, logs, and metrics through the dashboard

### Authentication with Keycloak

The template uses Keycloak for authentication. Once the application is running:

#### Testing API with Swagger UI

1. **Navigate to Swagger UI** at http://localhost:5082/swagger
2. **Click the "Authorize" button** in the Swagger UI interface
3. **Complete the OAuth2 flow**:
   - You'll be redirected to the Keycloak login page
   - **Login with**: `devuser` / `Password123!`
   - After successful authentication, you'll be redirected back to Swagger UI
4. **Test API endpoints**: You can now use all API endpoints in Swagger UI with the obtained access token

#### Managing Keycloak (Admin Access)

If you need to manage Keycloak realm, clients, scopes, or users:

1. **Access Keycloak Admin Console** from the Aspire Dashboard by clicking on the Keycloak service endpoint
2. **Login with admin credentials**:
   - **Username**: `admin`
   - **Password**: `admin`


## Azure Deployment

### Deploy with Azure Developer CLI

1. **Login to Azure**:
   ```bash
   azd auth login
   ```

2. **Initialize the project** (if not already done):
   ```bash
   azd init
   ```

3. **Deploy to Azure**:
   ```bash
   azd up
   ```

4. Follow the prompts to:
   - Select your Azure subscription
   - Choose a deployment region
   - Provide environment-specific configuration

The deployment will provision:
- Azure Container Apps Environment
- PostgreSQL Flexible Server
- Container Registry
- Managed Identity
- All necessary networking and security configurations

## CI/CD Pipelines

### GitHub Actions

To set up GitHub Actions CI/CD:

1. **Run the pipeline configuration command**:
   ```bash
   azd pipeline config --provider github
   ```

2. **Follow the interactive prompts** to configure:
   - GitHub repository connection
   - Azure authentication (Federated Identity recommended)
   - Deployment settings and environments

3. **Commit and push** your changes to trigger the pipeline

## Project Structure

```
├── src/
│   ├── TemplateApp.Api/              # Main Web API project
│   │   ├── Features/                 # Feature-based organization (Vertical Slice Architecture)
│   │   ├── Data/                     # Entity Framework context and configurations
│   │   ├── Models/                   # Domain models
│   │   └── Shared/                   # Shared components (auth, CORS, error handling, etc.)
│   ├── TemplateApp.AppHost/          # .NET Aspire orchestration
│   └── TemplateApp.ServiceDefaults/  # Shared service configurations
├── .github/workflows/                # GitHub Actions workflows
├── .devcontainer/                    # Dev container configuration
└── azure.yaml                       # Azure Developer CLI configuration
```

## Architecture

This template follows **Vertical Slice Architecture** principles, organizing code by features rather than technical layers. The architecture is structured as follows:

### Feature Organization

Each feature (like `Items` or `Categories`) in the `Features/` folder contains:

- **Feature Endpoints** - A main endpoints class that groups and maps all related routes
- **Individual Operations** - Each operation (GetItems, CreateItem, UpdateItem, etc.) has its own folder containing:
  - **Endpoint** - The minimal API endpoint definition with all business logic inline
  - **DTOs** - Request/response models specific to that operation

### Example Structure

```
Features/
├── Items/
│   ├── ItemsEndpoints.cs           # Groups all item-related endpoints
│   ├── Constants/                  # Shared constants for the feature
│   ├── CreateItem/
│   │   ├── CreateItemEndpoint.cs   # POST endpoint with business logic
│   │   └── CreateItemDtos.cs       # Request/response DTOs
│   ├── GetItems/
│   │   ├── GetItemsEndpoint.cs     # GET collection endpoint
│   │   └── GetItemsDtos.cs         # DTOs for pagination and filtering
│   └── GetItem/
│       ├── GetItemEndpoint.cs      # GET single item endpoint
│       └── GetItemDtos.cs          # Single item response DTOs
```

### Key Principles

This approach promotes:
- **Self-contained operations** - Each endpoint contains its complete logic flow
- **Feature cohesion** - All related operations are grouped together
- **Minimal dependencies** - Each operation only depends on what it needs
- **Easy testing** - Individual operations can be tested in isolation
- **Simple maintenance** - Changes to one operation don't affect others

## Installing the Template

To use this template for creating new projects:

1. **Navigate to the template root directory**
2. **Install the template**:
   ```bash
   dotnet new install .\
   ```

### Using the Template

Once installed, create a new project using the template:

```bash
# Create a new project in the current directory
dotnet new backend-blueprint

# Create a new project with a specific name
dotnet new backend-blueprint -n MyAwesomeBackend
```

### Template Parameters

The template supports the following parameters:

- `-n|--name`: Name of the project (default: current directory name)


## Configuration

### Local Development

The template uses Keycloak for local authentication. Configuration is handled automatically through .NET Aspire service discovery.

**Key features enabled for local development:**
- **Global error handling** - Consistent error responses across all endpoints
- **CORS configuration** - Properly configured for local development
- **Health checks** - Built-in health monitoring endpoints
- **Logging and telemetry** - Integrated with .NET Aspire dashboard

### Production

In production (Azure), the following are automatically configured:
- Managed Identity for secure service-to-service communication
- Azure Database for PostgreSQL
- Container Apps for scalable hosting


## License

This template is provided as-is for educational and development purposes.