# EShopMicroservices

Application e-commerce construite sur une architecture microservices en **.NET 8**. Elle met en œuvre DDD, CQRS, la Vertical Slice Architecture et la Clean Architecture, avec une communication orientée événements.

## Architecture

| Service | Rôle | Stockage / technologie |
|---|---|---|
| **Catalog.API** | Gestion du catalogue produits | PostgreSQL (Marten, document DB) |
| **Basket.API** | Gestion du panier | PostgreSQL + cache Redis |
| **Discount.Grpc** | Calcul des remises, exposé en gRPC | SQLite |
| **Ordering.API** | Gestion des commandes (DDD, Clean Architecture) | SQL Server (EF Core) |
| **YarpApiGateway** | Point d'entrée unique, routage et rate limiting | YARP |
| **Shopping.Web** | Front-end client | ASP.NET Core Razor Pages |

Basket et Ordering communiquent de façon asynchrone via **RabbitMQ** (MassTransit) : un checkout du panier publie un événement consommé par le service de commandes.

## Patterns et librairies

CQRS avec MediatR, validation avec FluentValidation, endpoints minimalistes avec Carter, mapping avec Mapster, health checks, pipeline behaviors (logging, validation) et gestion globale des exceptions.

## Lancer le projet

Prérequis : Docker Desktop et le SDK .NET 8.

```bash
cd src
docker compose -f docker-compose.yml -f docker-compose.override.yml up -d
```

| Service | URL |
|---|---|
| Catalog.API | http://localhost:6000 |
| Basket.API | http://localhost:6001 |
| Discount.Grpc | http://localhost:6002 |
| Ordering.API | http://localhost:6003 |
| API Gateway | http://localhost:6004 |
| Shopping.Web | http://localhost:6005 |
| RabbitMQ (management) | http://localhost:15672 |

## Auteur

**Borice Noumedem** — développeur Full Stack

Projet basé sur un code open source publié sous licence MIT (voir `LICENSE`).
