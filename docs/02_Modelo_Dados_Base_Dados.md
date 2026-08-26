# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 1. Objetivo

Este documento define o modelo de dados físico e lógico do ERP Gestão Escolar.

O objetivo é transformar a arquitetura definida no DOC 02 e a especificação técnica do DOC 03 numa estrutura concreta de base de dados PostgreSQL compatível com Supabase.

O documento será utilizado como referência para:

- Criação das tabelas.
- Criação das migrations.
- Definição das relações.
- Definição das constraints.
- Definição dos índices.
- Definição dos enums.
- Definição das funções PostgreSQL.
- Definição dos triggers.
- Definição das políticas RLS.
- Criação dos dados iniciais.
- Implementação dos services da aplicação.

---

## 2. Tecnologia

A base de dados será implementada em:

```text
PostgreSQL

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 53. Módulo CORE — Modelo de Dados

O CORE constitui a fundação da base de dados do ERP.

Todas as restantes áreas da aplicação dependerão das entidades e serviços definidos neste módulo.

O CORE será responsável por:

```text
Escolas
Utilizadores
Perfis
Roles
Permissões
Configurações
Auditoria

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 82. Módulo INVENTORY — Modelo de Dados

O módulo INVENTORY será responsável pela gestão dos produtos, fornecedores, compras, entradas e stock.

O modelo deverá garantir que o stock seja rastreável através de movimentos.

A arquitetura não deverá depender exclusivamente de um campo de stock alterado manualmente.

Modelo conceptual:

```text
Produto
   │
   ├── Compras
   │      │
   │      └── Entradas
   │
   └── Movimentos de Stock
             │
             └── Stock atual

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 119. Módulo SALES — Modelo de Dados

O módulo SALES será responsável pelo processo de venda de produtos.

O modelo deverá garantir a integração entre:

```text
Produto
   ↓
Venda
   ↓
Linhas de Venda
   ↓
Pagamento
   ↓
Caixa
   ↓
Stock

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 153. Módulo PRINTSHOP — Modelo de Dados

O módulo PRINTSHOP será responsável pela gestão dos pedidos de reprografia.

O modelo deverá suportar o fluxo:

```text
Professor
   ↓
Pedido
   ↓
Ficheiro
   ↓
Validação
   ↓
Orçamentação
   ↓
Produção
   ↓
Pronto
   ↓
Entrega

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 188. Módulo LOCKERS — Modelo de Dados

O módulo LOCKERS será responsável pela gestão dos cacifos escolares e das respetivas atribuições aos alunos.

O modelo deverá suportar:

```text
Escola
   ↓
Cacifos
   ↓
Atribuições
   ↓
Aluno principal
   ↓
Alunos partilhados

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 223. Módulo REPORTS — Modelo de Dados

O módulo REPORTS será responsável pela consulta, consolidação e geração de relatórios do ERP.

Os relatórios poderão utilizar dados provenientes de:

```text
CORE
INVENTORY
SALES
PRINTSHOP
LOCKERS

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 254. STORAGE — Modelo de Dados

O armazenamento de ficheiros será realizado através do **Supabase Storage**.

A base de dados PostgreSQL deverá guardar apenas os metadados necessários para identificar, controlar e relacionar os ficheiros.

Modelo:

```text
Aplicação
    ↓
Supabase Storage
    ↓
Ficheiro

PostgreSQL
    ↓
Metadados
    ↓
Relação com entidade

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 281. Segurança — Princípios Gerais

A segurança do ERP será implementada em várias camadas:

```text
Autenticação
    ↓
Autorização
    ↓
RLS
    ↓
Permissões por módulo
    ↓
Auditoria
    ↓
Storage Security

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 319. Estratégia de Integridade da Base de Dados

A base de dados deverá assumir responsabilidade direta pela integridade dos dados.

A aplicação não deverá depender exclusivamente de validações realizadas no frontend.

A estratégia será composta por:

```text
Constraints
    ↓
Foreign Keys
    ↓
Indexes
    ↓
Triggers
    ↓
Functions
    ↓
Views

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 365. Estratégia de Migrations

A implementação da base de dados PostgreSQL será realizada através de migrations.

Cada migration deverá representar uma alteração controlada e versionada da estrutura da base de dados.

Modelo:

```text
Migration 001
      ↓
Migration 002
      ↓
Migration 003
      ↓
...
      ↓
Estado atual da Base de Dados

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 408. Estrutura Geral do Repositório

O projeto deverá possuir uma estrutura organizada por responsabilidades.

Estrutura conceptual:

```text
erp-gestao-escolar/
│
├── docs/
├── src/
├── public/
├── supabase/
├── tests/
├── scripts/
├── .github/
│
├── .gitignore
├── README.md
├── package.json
└── outros ficheiros de configuração

# DOC 04 — MODELO DE DADOS E BASE DE DADOS

## 453. Revisão Final do Modelo de Dados

O modelo de dados foi concebido segundo uma arquitetura modular.

O princípio fundamental é:

```text
CORE
  ↓
Módulos de negócio
  ↓
Integrações
  ↓
Relatórios