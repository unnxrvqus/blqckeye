# Architecture

## Overview

Infrastructure Intelligence Platform is designed as a hybrid system consisting of two main components:

1. Infrastructure Agent
2. Cloud Platform

The Agent is responsible for local project analysis.

The Cloud Platform provides advanced processing, storage and intelligence features.

The main principle:

> Analyze locally, process intelligently, scale globally.

---

# High Level Architecture

```
                    Cloud Platform

              ┌──────────────────┐
              │    API Gateway   │
              └────────┬─────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼

    AI Engine     Rules Engine    Database


                       ▲
                       │

              Infrastructure Agent


        ┌──────────────┼──────────────┐
        │              │              │

    Scanner       Parser        Analyzer


                       ▲

                 User Project
```

---

# Infrastructure Agent

Infrastructure Agent is a local application responsible for understanding a project.

The Agent performs:

- file discovery;
- technology detection;
- configuration parsing;
- dependency analysis;
- infrastructure modeling.

The Agent should work without requiring a cloud connection.

---

# Agent Components

## Scanner

Responsible for discovering project structure.

Example:

Input:

```
my-project/

├── package.json
├── docker-compose.yml
├── nginx.conf
├── Dockerfile
└── .env
```

Output:

```
Detected:

Node.js
Docker
Nginx
PostgreSQL
Redis
```

---

## Parser Layer

Parsers convert configuration files into structured data.

Examples:

Input:

```
docker-compose.yml
```

Output:

```json
{
  "services": {
    "backend": {
      "image": "node",
      "ports": ["3000"]
    },
    "postgres": {
      "image": "postgres"
    }
  }
}
```

Supported parsers:

- Dockerfile
- Docker Compose
- Nginx configuration
- package.json
- environment files

---

## Analyzer

Analyzer combines information from different parsers.

Responsibilities:

- detect relationships;
- find dependencies;
- generate warnings;
- build infrastructure model.

Example:

```
Nginx

↓

Backend

↓

PostgreSQL
```

---

# Infrastructure Model

The core of the system is an internal representation of the project.

Every component is an entity.

Example:

```
Service

{
    type: "backend",
    name: "api",
    technology: "Express",
    ports: [3000],
    dependencies: [
        "postgres",
        "redis"
    ]
}
```

Relationships:

```
Backend

uses

PostgreSQL
```

```
Nginx

proxies

Backend
```

---

# Cloud Platform

The Cloud Platform provides functionality that requires centralized processing.

Responsibilities:

- AI analysis;
- storing projects;
- rule updates;
- reports;
- team collaboration.

---

# Version 0.1 Architecture

The first version will intentionally avoid unnecessary complexity.

Target:

A local CLI application capable of scanning a backend project and creating an infrastructure model.

Architecture:

```
CLI

 |

Scanner

 |

Parser Layer

 |

Analyzer

 |

Infrastructure Model

 |

JSON Report
```

---

# Version 0.1 Components

## CLI

Example:

```
infra scan ./project
```

---

## Scanner

Finds:

- project files;
- configuration files;
- technologies.

---

## Parsers

Initial support:

- package.json
- Dockerfile
- docker-compose.yml
- nginx.conf

---

## Analyzer

Creates:

- services;
- dependencies;
- warnings.

---

## Output

First output format:

JSON.

Example:

```json
{
 "services": [
    {
      "name": "backend",
      "technology": "Node.js"
    },
    {
      "name": "database",
      "technology": "PostgreSQL"
    }
 ]
}
```

---

# Future Extensions

Possible future modules:

- Web Dashboard
- Desktop Application
- AI Assistant
- Security Scanner
- Log Analyzer
- GitHub Integration
- Kubernetes Support

---

# Design Principles

## 1. Modular Architecture

Every analyzer component should be independent.

New technologies should be added through plugins.

---

## 2. Local First

Sensitive project data should remain local whenever possible.

---

## 3. Extensible Data Model

The infrastructure model should support future features without redesign.

---

## 4. Simple First

The first working version is more valuable than a perfect architecture.