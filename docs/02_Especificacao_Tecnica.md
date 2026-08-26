# DOC 03 — ESPECIFICAÇÃO TÉCNICA

**Projeto:** ERP Gestão Escolar  
**Versão:** 1.0.0  
**Estado:** Especificação Técnica  
**Data:** 26 de agosto de 2026

---

# 1. Objetivo

Este documento define a especificação técnica do ERP Gestão Escolar.

A especificação transforma a arquitetura definida no DOC 02 em requisitos técnicos concretos para implementação.

O documento servirá como referência para:

- desenvolvimento;
- configuração do Supabase;
- desenvolvimento frontend;
- criação da base de dados;
- segurança;
- testes;
- utilização do Bolt.new;
- manutenção;
- evolução futura.

---

# 2. Princípios Técnicos

O sistema deverá respeitar os seguintes princípios:

1. A base de dados é a fonte de verdade.
2. A lógica crítica deverá permanecer no backend.
3. O frontend não deverá conter regras críticas isoladas.
4. A segurança deverá ser aplicada no backend.
5. O Row Level Security será utilizado nas tabelas apropriadas.
6. O projeto deverá ser versionado através do GitHub.
7. A aplicação deverá ser modular.
8. O sistema deverá ser responsivo.
9. A aplicação deverá funcionar como PWA.
10. A arquitetura deverá permitir futura expansão para múltiplas escolas.
11. As operações críticas deverão ser transacionais.
12. A informação deverá possuir histórico sempre que necessário.
13. O sistema deverá evitar dependências desnecessárias de fornecedor.
14. As decisões arquiteturais deverão estar documentadas.

---

# 3. Stack Tecnológica

## 3.1 Backend

O backend será baseado em:

- Supabase;
- PostgreSQL;
- Supabase Auth;
- Supabase Storage;
- PostgreSQL Functions;
- PostgreSQL Views;
- PostgreSQL Triggers;
- Row Level Security.

---

## 3.2 Frontend

O frontend utilizará:

- React;
- TypeScript;
- Vite;
- Tailwind CSS;
- componentes reutilizáveis;
- Supabase Client;
- PWA.

---

## 3.3 Desenvolvimento

Ferramentas principais:

- GitHub;
- GitHub Desktop;
- Visual Studio Code;
- Bolt.new;
- ChatGPT.

---

# 4. Arquitetura Geral

A arquitetura será:

```text
UTILIZADOR
    │
    ▼
BOLT.NEW / PWA
    │
    ▼
SERVICES
    │
    ▼
SUPABASE
    │
    ├── AUTH
    ├── API
    ├── RPC
    └── STORAGE
    │
    ▼
POSTGRESQL
    │
    ├── RLS
    ├── FUNCTIONS
    ├── VIEWS
    ├── TRIGGERS
    └── AUDITORIA

# 11. Modelo de Dados

O modelo de dados será implementado em PostgreSQL através do Supabase.

A base de dados será organizada por módulos funcionais.

Estrutura conceptual:

```text
CORE
 ├── schools
 ├── profiles
 ├── roles
 ├── permissions
 ├── role_permissions
 ├── user_roles
 ├── school_settings
 └── audit_logs

INVENTORY
 ├── product_categories
 ├── suppliers
 ├── products
 ├── purchase_orders
 ├── purchase_order_items
 ├── stock_movements
 ├── inventories
 └── inventory_items

SALES
 ├── sales
 ├── sale_items
 ├── payments
 ├── cash_sessions
 └── cash_movements

PRINTSHOP
 ├── printshop_requests
 ├── printshop_items
 ├── printshop_files
 ├── printshop_estimates
 └── printshop_events

LOCKERS
 ├── lockers
 ├── students
 ├── locker_assignments
 ├── locker_assignment_students
 └── locker_deposits

REPORTS
 └── views / functions / report definitions

 # 55. Regras Gerais de Negócio

As regras de negócio representam comportamentos obrigatórios do ERP.

Estas regras deverão ser implementadas preferencialmente no backend e/ou base de dados sempre que possam afetar a integridade dos dados.

O frontend deverá apenas refletir e facilitar o cumprimento das regras.

---

# 56. Regra de Preços dos Produtos

Cada produto deverá possuir uma margem configurável individualmente.

Campos principais:

```text
cost_price
margin_percent
vat_rate
selling_price

# 124. Arquitetura Supabase

O Supabase será a infraestrutura principal do backend do ERP.

A arquitetura utilizará os seguintes componentes:

```text
Supabase
├── PostgreSQL
├── Authentication
├── Storage
├── Row Level Security
├── Database Functions
├── Database Triggers
└── REST API

# 170. Modelo de Dados — Visão Geral

O modelo de dados será organizado por módulos funcionais.

Estrutura conceptual:

```text
CORE
 ├── schools
 ├── profiles
 ├── roles
 ├── permissions
 └── audit_logs

INVENTORY
 ├── categories
 ├── products
 ├── suppliers
 ├── purchases
 ├── purchase_items
 ├── stock_movements
 └── inventories

SALES
 ├── sales
 ├── sale_items
 ├── payments
 └── cash_sessions

PRINTSHOP
 ├── printshop_requests
 ├── printshop_files
 ├── printshop_items
 └── printshop_status_history

LOCKERS
 ├── students
 ├── lockers
 ├── locker_assignments
 ├── locker_shared_students
 └── locker_deposits

REPORTS
 └── dados obtidos através das restantes entidades

 # 226. Segurança — Princípio Geral

A segurança do ERP será implementada em várias camadas.

Arquitetura:

```text
Utilizador
    ↓
Autenticação
    ↓
Perfil
    ↓
Role
    ↓
Permission
    ↓
RLS
    ↓
Base de Dados

# 266. Arquitetura de Serviços

A aplicação deverá utilizar uma camada de serviços para concentrar a lógica de negócio.

Arquitetura conceptual:

```text
Interface / PWA
       ↓
Services
       ↓
Supabase
       ↓
PostgreSQL

# 332. Estrutura Física do Projeto

O projeto deverá possuir uma estrutura modular e previsível.

A organização deverá separar:

* Documentação.
* Frontend.
* Componentes.
* Módulos funcionais.
* Serviços.
* Tipos.
* Configurações.
* Integrações.
* Testes.

Estrutura conceptual:

```text
erp-gestao-escolar/
│
├── docs/
│
├── public/
│
├── src/
│   ├── components/
│   ├── layouts/
│   ├── pages/
│   ├── modules/
│   ├── services/
│   ├── lib/
│   ├── hooks/
│   ├── types/
│   ├── utils/
│   └── config/
│
├── supabase/
│   ├── migrations/
│   ├── functions/
│   └── seed/
│
├── tests/
│
├── .env.example
├── .gitignore
├── package.json
└── README.md

# 373. Arquitetura da Interface

A interface do ERP deverá ser desenvolvida como uma aplicação web responsiva e Progressive Web App (PWA).

A interface deverá funcionar em:

* Computador.
* Tablet.
* Smartphone.

A aplicação deverá adaptar a apresentação ao tamanho do ecrã sem duplicar a lógica de negócio.

---

# 374. Princípios de UX

A interface deverá privilegiar:

* Simplicidade.
* Clareza.
* Rapidez.
* Consistência.
* Baixa curva de aprendizagem.
* Redução de cliques.
* Feedback imediato.
* Prevenção de erros.

O utilizador deverá conseguir executar as operações frequentes com o menor número de passos possível.

---

# 375. Design System

A aplicação deverá utilizar um sistema visual consistente.

Deverão existir padrões para:

```text
Botões
Campos
Tabelas
Cards
Menus
Dialogos
Alertas
Badges
Formulários
Filtros
Paginação
Estados

# 428. Modelo de Dados

O modelo de dados será implementado em PostgreSQL através do Supabase.

A base de dados será considerada a fonte de verdade do sistema.

O modelo deverá privilegiar:

* Integridade referencial.
* Normalização adequada.
* Histórico.
* Auditoria.
* Segurança.
* Escalabilidade.
* Suporte multi-escola.
* Separação lógica entre módulos.

---

# 429. Princípios do Modelo de Dados

O modelo deverá seguir os seguintes princípios:

```text
Base de Dados
      ↓
Integridade
      ↓
Regras de Negócio
      ↓
Serviços
      ↓
Interface

# 486. Regras de Negócio

As regras de negócio representam o comportamento funcional obrigatório do ERP.

Estas regras deverão ser respeitadas independentemente da interface utilizada.

A validação deverá ocorrer preferencialmente no backend e na base de dados quando a regra tiver impacto na integridade dos dados.

---

# 487. Princípio Geral

Nenhuma operação crítica deverá depender exclusivamente da interface.

Fluxo:

```text
Utilizador
    ↓
Interface
    ↓
Service
    ↓
Backend
    ↓
PostgreSQL

# 568. API e Camada de Serviços

A aplicação deverá utilizar uma camada de serviços para centralizar operações de negócio.

A interface não deverá executar diretamente operações complexas sobre a base de dados quando estas envolverem regras de negócio.

Arquitetura:

```text
UI
 ↓
Service
 ↓
Supabase
 ↓
PostgreSQL

# 626. Estrutura Física do Projeto

O projeto deverá possuir uma estrutura organizada por responsabilidades.

A estrutura deverá facilitar:

* Desenvolvimento.
* Manutenção.
* Testes.
* Evolução modular.
* Integração com Supabase.
* Desenvolvimento através do Bolt.new.
* Colaboração futura.

---

# 627. Estrutura Principal

A estrutura inicial será:

```text
ERP-Escolar/
│
├── docs/
├── public/
├── src/
├── supabase/
├── tests/
│
├── .env.example
├── .gitignore
├── README.md
├── package.json
├── tsconfig.json
├── vite.config.ts
└── outros ficheiros de configuração

# 698. Modelo de Dados

O ERP deverá utilizar PostgreSQL através do Supabase.

O modelo de dados deverá ser relacional, normalizado e preparado para evolução futura.

A estrutura deverá privilegiar:

* Integridade referencial.
* Segurança.
* Auditoria.
* Multi-escola.
* Consistência transacional.
* Histórico.
* Extensibilidade.

---

# 699. Princípio Multi-Escola

Todas as entidades que pertençam diretamente a uma escola deverão possuir referência à escola.

Modelo conceptual:

```text
schools
   │
   ├── users
   ├── products
   ├── suppliers
   ├── sales
   ├── print_requests
   ├── lockers
   ├── students
   └── reports

   # 768. API e Camada de Serviços

A aplicação deverá utilizar uma camada de serviços para comunicar com o backend Supabase.

A interface de utilizador não deverá concentrar regras de negócio críticas.

Modelo conceptual:

```text
Interface
    ↓
Componentes
    ↓
Serviços da aplicação
    ↓
Supabase
    ↓
PostgreSQL

# 808. Segurança da Aplicação

A segurança será um requisito transversal do ERP.

A arquitetura deverá aplicar o princípio:

```text
Nunca confiar no cliente.
Sempre validar no servidor.

# 867. Interface e Experiência de Utilização

A interface do ERP deverá ser moderna, simples, responsiva e orientada à utilização real numa escola.

O objetivo principal será permitir que um utilizador consiga executar tarefas frequentes com o menor número possível de passos.

---

# 868. Princípios de UX

A interface deverá seguir os princípios:

```text
Simplicidade
Clareza
Consistência
Rapidez
Acessibilidade
Responsividade
Feedback imediato

# 933. Estratégia de Testes

O ERP deverá possuir uma estratégia de testes desde o início do desenvolvimento.

Os testes deverão verificar:

```text
Funcionalidade
Segurança
Dados
Integrações
Interface
Responsividade
Permissões
Regras de negócio

# 996. Estratégia de Desenvolvimento

O desenvolvimento do ERP será realizado de forma incremental.

A implementação deverá seguir a seguinte sequência:

```text
Documentação
    ↓
Estrutura do projeto
    ↓
Configuração base
    ↓
Supabase
    ↓
CORE
    ↓
Módulos
    ↓
Integrações
    ↓
Testes
    ↓
Deploy

# 1064. Estrutura Final do Projeto

A estrutura do projeto deverá separar claramente:

```text
Frontend
Backend / Serviços
Base de Dados
Storage
Documentação
Testes
Configuração

# 1143. Modelo de Dados

A base de dados do ERP será implementada sobre PostgreSQL através do Supabase.

O modelo deverá privilegiar:

```text
Integridade
Consistência
Segurança
Auditabilidade
Escalabilidade
Normalização

# 1215. Regras de Negócio

As regras de negócio constituem a camada que define o comportamento funcional do ERP.

Estas regras deverão ser respeitadas independentemente da interface utilizada.

A mesma regra deverá aplicar-se quando uma operação é executada através de:

```text
Desktop
Tablet
Smartphone
PWA
Frontend
API
Serviço backend

# 1290. Arquitetura de API e Serviços

A comunicação entre a interface da aplicação e os dados deverá ser organizada através de uma camada de serviços.

Modelo:

```text
Frontend / PWA
      ↓
Services
      ↓
Supabase
      ↓
PostgreSQL

# 1364. Arquitetura do Frontend

O frontend será responsável pela apresentação da aplicação e pela interação com os utilizadores.

Deverá consumir os serviços definidos na arquitetura técnica.

Modelo:

Frontend
   ↓
Application Services
   ↓
Supabase

# 1466. Arquitetura PWA

O ERP deverá disponibilizar funcionalidades através de Progressive Web App (PWA).

A arquitetura PWA deverá permitir utilização através de:

```text
Desktop
Laptop
Tablet
Smartphone

# 1530. Modelo de Dados PostgreSQL

A base de dados do ERP será implementada em PostgreSQL através do Supabase.

O modelo deverá ser relacional, normalizado e preparado para crescimento futuro.

A base de dados deverá garantir:

```text
Integridade
Consistência
Rastreabilidade
Segurança
Escalabilidade
Manutenção

# 1613. Segurança da Base de Dados

A segurança dos dados será implementada em múltiplas camadas.

Arquitetura:

```text
Utilizador
   ↓
Supabase Auth
   ↓
Perfil / Role
   ↓
RLS
   ↓
Serviços / API
   ↓
PostgreSQL

# 1688. Arquitetura de API e Serviços

A comunicação entre a aplicação frontend e o backend deverá utilizar os mecanismos disponibilizados pelo Supabase, complementados por serviços de aplicação quando necessário.

Arquitetura:

```text
Frontend
   ↓
Serviços da Aplicação
   ↓
Supabase
   ├── Auth
   ├── PostgreSQL
   ├── Storage
   └── APIs

   # 1770. Estrutura Física do Frontend

O frontend será desenvolvido com uma arquitetura modular.

A estrutura deverá permitir:

```text
CORE
INVENTORY
SALES
CASH
PRINTSHOP
LOCKERS
REPORTS

# 1851. UI/UX e Design System

A interface do ERP deverá seguir um sistema visual consistente em toda a aplicação.

O objetivo é criar uma aplicação:

```text
Profissional
Simples
Rápida
Intuitiva
Responsiva
Acessível
Consistente

# 1938. Gestão de Ficheiros e Documentos

O sistema deverá possuir uma arquitetura centralizada para gestão de ficheiros.

A gestão deverá abranger:

```text
Upload
Armazenamento
Validação
Metadados
Acesso
Download
Retenção
Eliminação
Auditoria

# 2008. Autenticação e Segurança de Acessos

A autenticação e autorização da aplicação deverão ser centralizadas no módulo CORE.

A arquitetura deverá separar claramente:

```text
Autenticação
    ↓
Identidade
    ↓
Perfil
    ↓
Permissões
    ↓
Autorização
    ↓
Operação

# 2085. Modelo da Base de Dados

A base de dados do ERP deverá utilizar PostgreSQL através do Supabase.

O modelo deverá ser relacional e normalizado, evitando duplicação desnecessária de informação.

A base de dados deverá suportar:

```text
CORE
INVENTORY
SALES
CASH
PRINTSHOP
LOCKERS
REPORTS
STORAGE
AUDIT

# 2181. APIs e Serviços

A aplicação deverá utilizar uma camada de serviços responsável por intermediar a comunicação entre a interface, a lógica de negócio e a base de dados.

Arquitetura conceptual:

```text
Frontend
   ↓
Services
   ↓
Supabase
   ↓
PostgreSQL

# 2271. Frontend

O frontend será responsável pela interface de utilização do ERP.

A aplicação deverá ser:

- Responsiva.
- Modular.
- Acessível.
- Otimizada para utilização em computador.
- Otimizada para tablet.
- Compatível com smartphone.
- Preparada para PWA.

---

# 2272. Princípios de UI/UX

A interface deverá privilegiar:

```text
Simplicidade
Rapidez
Clareza
Consistência
Baixa curva de aprendizagem
Poucos passos por operação
Feedback imediato
Prevenção de erros

# 2351. Segurança

A segurança será um requisito transversal do ERP.

A arquitetura deverá aplicar segurança em múltiplas camadas:

```text
Frontend
   ↓
Autenticação
   ↓
Autorização
   ↓
Services
   ↓
RLS
   ↓
PostgreSQL
   ↓
Storage

# 2435. Estratégia de Desenvolvimento

O desenvolvimento do ERP será realizado de forma incremental.

A aplicação não deverá ser construída integralmente numa única operação.

Estratégia:

```text
Fundação
   ↓
CORE
   ↓
Base de Dados
   ↓
Segurança
   ↓
Módulos
   ↓
Integrações
   ↓
Testes
   ↓
Produção