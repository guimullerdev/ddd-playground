<div align="center">

# 🏗️ ddd-playground

**A hands-on playground for exploring Domain-Driven Design patterns through focused Proof of Concepts**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-see_.nvmrc-339933?logo=node.js&logoColor=white)](.nvmrc)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](docker-compose.dev.yml)
[![Yarn](https://img.shields.io/badge/Yarn-package_manager-2C8EBB?logo=yarn&logoColor=white)](https://yarnpkg.com/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

</div>

---

## 📖 About

`ddd-playground` is a repository for validating **Domain-Driven Design** patterns and architectural decisions through isolated, focused POC implementations.

Each POC lives in its own directory under `pocs/`, models a real-world domain, and demonstrates how DDD tactical patterns translate into working code — with zero compromise on domain purity.

> **Goal:** Understand, experiment, and discuss DDD trade-offs *before* applying them in production systems.

---

## 📁 Repository Structure

```
ddd-playground/
└── pocs/
    └── oficina-api/        # 🔧 Vehicle workshop management domain
```

---

## 🧩 POCs

### 🔧 `oficina-api` — Vehicle Workshop API

A vehicle workshop management system that demonstrates DDD tactical patterns in a rich, rule-heavy domain.

**Domain highlights:**
- `Vehicle` — Aggregate Root tracking identity, owner, and mileage history
- `ServiceOrder` — Aggregate with a strict state machine (`Open → InProgress → Done → Invoiced`)
- `Part` — Entity managing stock inventory with low-stock domain events
- `Money` — Immutable Value Object with currency-safe arithmetic

**Key invariants enforced by the domain:**
- A `ServiceOrder` cannot close unless all services are marked complete
- A `Part` cannot be consumed beyond available stock
- Vehicle mileage can only ever *increase*
- An invoiced order is **immutable**

**→ [Go to oficina-api](./pocs/oficina-api/)**

---

## 🏛️ Architecture

All POCs follow the same layered architecture, keeping the domain model free of infrastructure concerns.

```
┌─────────────────────────────────────────┐
│   Interface       HTTP / CLI / Events   │
├─────────────────────────────────────────┤
│   Application     Use Cases             │
├─────────────────────────────────────────┤
│   Domain          Entities · VOs ·      │  ← Zero external dependencies
│                   Aggregates · Events   │
├─────────────────────────────────────────┤
│   Infrastructure  DB · Brokers · APIs   │
└─────────────────────────────────────────┘
         Dependencies point ▲ inward
```

| Layer | Responsibility |
|---|---|
| **Interface** | Translate external requests into application calls |
| **Application** | Orchestrate domain objects to fulfil a single user intent |
| **Domain** | Pure business logic — entities, value objects, events, repository *interfaces* |
| **Infrastructure** | Repository implementations, DB drivers, external services |

---

## 🔑 DDD Concepts Explored

| Pattern | Description |
|---|---|
| **Entity** | Object defined by identity, not attributes |
| **Value Object** | Immutable object defined entirely by its values |
| **Aggregate** | Cluster of objects with a single Root enforcing invariants |
| **Domain Event** | Immutable record of something meaningful that happened |
| **Repository** | Collection-like abstraction over persistence |
| **Domain Service** | Stateless operations that span multiple aggregates |

---

## 🚀 Getting Started

### Prerequisites

- [Docker](https://www.docker.com/) & Docker Compose
- [nvm](https://github.com/nvm-sh/nvm) — Node version is pinned in `.nvmrc`
- [Yarn](https://yarnpkg.com/)

### Running with Docker *(recommended)*

```bash
# Clone the repo
git clone https://github.com/guimullerdev/ddd-playground
cd ddd-playground/pocs/oficina-api

# Use the correct Node.js version
nvm use

# Setup environment variables
cp .env.example .env

# Start all services (app + database)
docker compose -f docker-compose.dev.yml up --build
```

### Running locally

```bash
cd ddd-playground/pocs/oficina-api

# Use the correct Node.js version
nvm use

# Install dependencies
yarn install

# Setup environment variables
cp .env.example .env

# Run database migrations
yarn prisma migrate dev

# Start dev server
yarn dev
```

---

## 🗺️ Roadmap

- [x] `oficina-api` — Vehicle workshop
- [ ] `banking-api` — Accounts, transfers, multi-currency Money VO
- [ ] `e-commerce-api` — Cart, orders, coupon policies
- [ ] `clinic-api` — Patients, appointments, ACL for legacy systems

---

## 📚 Resources

- 📕 **Domain-Driven Design** — Eric Evans (2003) — *The Blue Book*
- 📗 **Implementing Domain-Driven Design** — Vaughn Vernon (2013)
- 📘 **Learning Domain-Driven Design** — Vlad Khononov (2021)
- 🌐 [ddd-crew / ddd-starter-modelling](https://github.com/ddd-crew/ddd-starter-modelling-process)

---

<div align="center">

Made with ☕ by [@guimullerdev](https://github.com/guimullerdev)

</div>