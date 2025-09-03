# DebtsDocumental
Gestion documental prueba tecnica  double v partners
# DebtSoft – Auth + Debts + Frontend

Repositorio documental para **dos microservicios .NET** (Autenticación y Deudas) y un **Frontend Next.js**.  
Incluye JWT, PostgreSQL por servicio, Redis (cache), exportación CSV, Swagger y colección Postman.

## ✅ Requisitos

- .NET SDK **8/9** (el repo usa `net9.0`)
- Node.js **18+** y pnpm/npm
- Docker (opcional, recomendado para DB/Redis)

## ⚙️ Variables de entorno (local)

### Auth.Api `appsettings.Development.json`
```json
{
  "Jwt": {
    "Issuer": "DebtSoft",
    "Audience": "DebtSoft-Clients",
    "Secret": "REPLACE_WITH_A_LONG_RANDOM_SECRET_64CHARS"
  },
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5433;Database=auth_db;Username=auth_user;Password=auth_pwd"
  },
  "AllowedHosts": "*"
}

{
  "Jwt": {
    "Issuer": "DebtSoft",
    "Audience": "DebtSoft-Clients",
    "Secret": "REPLACE_WITH_A_LONG_RANDOM_SECRET_64CHARS"
  },
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5434;Database=debts_db;Username=debts_user;Password=debts_pwd"
  },
  "Redis": {
    "ConnectionString": "localhost:6379"
  },
  "AllowedHosts": "*"
}
```json


### web/.env.local

NEXT_PUBLIC_AUTH_API=http://localhost:5267
NEXT_PUBLIC_DEBTS_API=http://localhost:5001

# docker-compose.yml (en la raíz)
services:
  pg-auth:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_USER: auth_user
      POSTGRES_PASSWORD: auth_pwd
      POSTGRES_DB: auth_db
    ports: [ "5433:5432" ]

  pg-debts:
    image: postgres:16
    restart: always
    environment:
      POSTGRES_USER: debts_user
      POSTGRES_PASSWORD: debts_pwd
      POSTGRES_DB: debts_db
    ports: [ "5434:5432" ]

  redis:
    image: redis:7
    ports: [ "6379:6379" ]
```
docker compose up -d

 Debts
dotnet ef database update --project Debts.Infrastructure --startup-project Debts.Api

 Auth
dotnet ef database update --project Auth.Infrastructure --startup-project Auth.Api

 Auth (Swagger: http://localhost:5267/swagger)
dotnet run --project Auth.Api

 Debts (Swagger: http://localhost:5001/swagger)
dotnet run --project Debts.Api

## Usuario de prueba en collection POSTAMN

{
  "email": "br.david@outloo.com",
  "password": "123469",
  "fullName": "Leonardo Burbano"
}


##  Frontend

cd web
pnpm i        # o npm i
pnpm dev      # o npm run dev
 http://localhost:3000

#Rutas principales:

/login – login

/register – registro

/debts – listado, filtros, ordenar, pagar

/debts/new – crear deuda

/debts/summary – agregados

#📜 Swagger

Auth: http://localhost:5267/swagger

Debts: http://localhost:5001/swagger

##📨 Postman

Importa postman/Auth+Debts.postman_collection.json.

Ejecuta Auth → Login (guarda {{accessToken}}/{{userId}}),

Prueba Debts (hereda Bearer).

## 🧪 Tests
dotnet test