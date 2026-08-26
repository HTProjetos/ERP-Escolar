# DOC 02 — ARQUITETURA CONSOLIDADA

## ERP Gestão Escolar

**Documento:** 02 — Arquitetura Consolidada
**Versão:** 1.0.0
**Estado:** Proposta consolidada para aprovação
**Data:** 26 de agosto de 2026
**Documento de referência:** DOC 01 — Project Charter v2.0.0
**Repositório:** GitHub — `HTProjetos/ERP-Escolar`

---

# 1. Objetivo do Documento

Este documento define a arquitetura tecnológica, estrutural e funcional de alto nível do **ERP Gestão Escolar**.

O objetivo é estabelecer uma arquitetura única e coerente antes do início da implementação da aplicação.

Este documento deverá servir como referência para:

* Desenvolvimento no Bolt.new.
* Configuração do Supabase.
* Organização do código no GitHub.
* Desenvolvimento das PWAs.
* Construção da base de dados.
* Implementação da segurança.
* Integração dos diferentes módulos.
* Evolução futura do sistema.

---

# 2. Visão Geral da Arquitetura

A arquitetura será baseada no seguinte modelo:

```text
                         UTILIZADORES
                              │
               ┌──────────────┴──────────────┐
               │                             │
          Aplicação Web                 PWAs
               │                             │
               └──────────────┬──────────────┘
                              │
                         FRONTEND
                    React + TypeScript
                              │
                         Supabase API
                              │
                ┌─────────────┴─────────────┐
                │                           │
           Supabase Auth              PostgreSQL
                │                           │
                │                           ├── CORE
                │                           ├── INVENTORY
                │                           ├── SALES
                │                           ├── PRINTSHOP
                │                           ├── LOCKERS
                │                           └── REPORTS
                │
          Supabase Storage
```

A segurança será aplicada através de autenticação, autorização, políticas RLS e regras de acesso aos ficheiros.

---

# 3. Princípios Arquiteturais

A arquitetura deverá respeitar os seguintes princípios:

1. **Separação entre apresentação, lógica e dados.**
2. PostgreSQL/Supabase como fonte de verdade.
3. Segurança aplicada no backend/base de dados.
4. RLS obrigatória nas tabelas que contenham dados protegidos.
5. Interfaces desacopladas da estrutura interna da base de dados.
6. Módulos independentes e reutilizáveis.
7. Configurações preferencialmente orientadas por dados.
8. Preparação para multi-escola.
9. Desenvolvimento incremental.
10. Versionamento permanente através do Git.
11. Evitar dependências desnecessárias de serviços pagos.
12. Prioridade à simplicidade e manutenção futura.
13. Nenhuma funcionalidade crítica deverá depender exclusivamente de lógica executada no navegador.

---

# 4. Stack Tecnológica

## 4.1 Frontend

Tecnologias principais:

* React
* TypeScript
* HTML5
* CSS
* Componentes UI compatíveis com PWA
* Progressive Web App

O frontend deverá ser responsivo e funcionar em:

* Computador.
* Tablet.
* Smartphone.

---

# 5. Backend

O backend será baseado em **Supabase**.

Componentes:

```text
Supabase
│
├── PostgreSQL
├── Auth
├── Storage
├── Row Level Security
└── API
```

O Supabase será responsável pela persistência, autenticação, armazenamento e exposição controlada dos dados.

---

# 6. Base de Dados

O PostgreSQL será a base central do sistema.

A estrutura será organizada por domínios funcionais.

Arquitetura conceptual:

```text
DATABASE
│
├── CORE
│   ├── schools
│   ├── users/profiles
│   ├── roles
│   ├── permissions
│   ├── settings
│   └── audit
│
├── INVENTORY
│   ├── products
│   ├── categories
│   ├── suppliers
│   ├── purchases
│   ├── stock movements
│   └── inventories
│
├── SALES
│   ├── sales
│   ├── sale items
│   ├── payments
│   ├── cash sessions
│   └── school card transactions
│
├── PRINTSHOP
│   ├── requests
│   ├── request files
│   ├── production
│   ├── delivery
│   ├── pricing
│   └── SLA
│
├── LOCKERS
│   ├── lockers
│   ├── students
│   ├── assignments
│   └── deposits
│
└── REPORTS
    └── reporting structures/views
```

A estrutura definitiva das tabelas será detalhada no documento de Modelo de Dados.

---

# 7. Arquitetura Multi-Escola

O sistema deverá ser concebido desde o início como **multi-tenant**, mesmo que a primeira instalação seja utilizada por apenas uma escola.

O conceito principal será:

```text
ESCOLA
   │
   ├── Utilizadores
   ├── Produtos
   ├── Fornecedores
   ├── Vendas
   ├── Caixa
   ├── Pedidos
   ├── Cacifos
   └── Configurações
```

As entidades pertencentes a uma escola deverão possuir uma referência à respetiva escola, normalmente através de:

```text
school_id
```

As políticas RLS deverão garantir que um utilizador apenas consiga aceder aos dados da escola a que está autorizado.

---

# 8. Autenticação

A autenticação será efetuada através do **Supabase Auth**.

O utilizador deverá iniciar sessão através das credenciais disponibilizadas pela escola.

Após autenticação, o sistema deverá identificar:

* Utilizador.
* Escola.
* Perfil.
* Permissões.
* Estado da conta.

O acesso à aplicação deverá ser condicionado ao estado da conta.

---

# 9. Perfis e Permissões

O sistema deverá utilizar um modelo de autorização baseado em perfis e permissões.

Modelo conceptual:

```text
UTILIZADOR
    │
    └── PERFIL
          │
          └── PERMISSÕES
```

Exemplos de perfis:

* Administrador.
* Responsável de Papelaria.
* Responsável de Reprografia.
* Professor.
* Assistente Operacional.
* Outros perfis configuráveis.

A arquitetura deverá permitir acrescentar novos perfis sem alteração estrutural significativa.

---

# 10. Segurança e RLS

A segurança deverá ser implementada em várias camadas.

```text
Utilizador
    ↓
Supabase Auth
    ↓
Perfil / Permissões
    ↓
RLS
    ↓
Dados
```

A interface não deverá ser considerada mecanismo de segurança.

Por exemplo, esconder um botão "Eliminar" não é suficiente.

A operação deverá ser também impedida através das políticas do backend/base de dados.

---

# 11. Auditoria

Operações críticas deverão ser registadas.

A estrutura de auditoria deverá permitir identificar, quando aplicável:

* Utilizador.
* Escola.
* Data/hora.
* Operação.
* Entidade.
* Registo afetado.
* Valores anteriores.
* Valores posteriores.

Exemplos:

```text
CREATE
UPDATE
DELETE
LOGIN
LOGOUT
STOCK_ADJUSTMENT
CASH_CLOSE
PERMISSION_CHANGE
```

A implementação detalhada será definida na especificação técnica.

---

# 12. Storage

O Supabase Storage será utilizado para ficheiros que necessitem de armazenamento persistente.

Exemplos:

* Documentos de reprografia.
* Fotografias.
* Assinaturas digitais.
* Ficheiros associados a ocorrências, caso exista futura integração.
* Outros anexos.

O acesso aos ficheiros deverá ser controlado.

Os ficheiros não deverão ser armazenados diretamente em campos de texto da base de dados.

---

# 13. Organização Modular

O frontend deverá refletir a organização modular do backend.

Estrutura conceptual:

```text
src/
│
├── core/
├── modules/
│   ├── inventory/
│   ├── sales/
│   ├── printshop/
│   ├── lockers/
│   └── reports/
│
├── components/
├── layouts/
├── services/
├── hooks/
├── lib/
├── types/
└── utils/
```

A estrutura final poderá ser ajustada durante a implementação, desde que preserve a separação modular.

---

# 14. Módulo CORE

O CORE será o núcleo transversal da aplicação.

Responsabilidades:

* Autenticação.
* Sessão.
* Utilizadores.
* Perfis.
* Permissões.
* Escolas.
* Configurações.
* Auditoria.
* Navegação.
* Gestão dos módulos ativos.

Todos os restantes módulos dependerão dos serviços disponibilizados pelo CORE.

---

# 15. Módulo INVENTORY

O módulo INVENTORY será responsável pelo inventário e gestão de produtos.

Principais componentes:

```text
Produtos
Categorias
Fornecedores
Compras
Entradas
Movimentos
Stock
Inventários
```

O stock deverá ser tratado como resultado de movimentos registados.

Sempre que possível, alterações de stock deverão deixar histórico auditável.

---

# 16. Módulo SALES

O módulo SALES será responsável pelas vendas e caixa.

Fluxo conceptual:

```text
Produto
   ↓
Venda
   ↓
Linhas de Venda
   ↓
Pagamento
   ↓
Movimento de Caixa
   ↓
Stock
```

Uma venda poderá originar automaticamente movimentos de stock e movimentos financeiros.

As operações deverão ser transacionais quando necessário para garantir consistência dos dados.

---

# 17. Caixa

A gestão de caixa será integrada no módulo SALES.

Fluxo:

```text
Abertura
   ↓
Movimentos
   ↓
Vendas
   ↓
Pagamentos
   ↓
Fecho
```

O sistema deverá registar automaticamente:

* Data/hora de abertura.
* Data/hora de fecho.
* Utilizador.
* Movimentos.
* Totais por método de pagamento.
* Diferenças, quando aplicável.

---

# 18. Cartão Escolar

O sistema deverá suportar o método de pagamento **Cartão Escolar**.

A arquitetura deverá permitir associar movimentos ao cartão/conta escolar correspondente.

A implementação detalhada dependerá da forma como a escola disponibiliza o serviço e de eventuais integrações externas.

O sistema deverá inicialmente permitir o registo interno do pagamento sem depender obrigatoriamente de uma integração externa.

---

# 19. Módulo PRINTSHOP

O PRINTSHOP será responsável pela reprografia.

Fluxo principal:

```text
Professor
   ↓
Pedido
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
```

O pedido deverá possuir um estado controlado.

Exemplo:

```text
NOVO
VALIDADO
EM PRODUÇÃO
PRONTO
ENTREGUE
CANCELADO
```

---

# 20. PWA dos Professores

A PWA dos professores deverá permitir:

* Criar pedidos.
* Anexar ficheiros.
* Definir quantidades.
* Escolher opções de impressão.
* Definir prioridade.
* Indicar prazo pretendido.
* Consultar estado.
* Consultar histórico.

A interface deverá ser simplificada para utilização rápida em smartphone ou computador.

---

# 21. PWA / Interface de Reprografia

A área de reprografia deverá ser otimizada para utilização por um único operador ou equipa reduzida.

Funcionalidades:

* Lista de pedidos.
* Filtros.
* Prioridades.
* Kanban.
* Consulta de ficheiros.
* Produção.
* Conclusão.
* Entrega.

---

# 22. Estimador de Custos

O sistema deverá possuir um motor de cálculo configurável.

Entradas possíveis:

* Formato.
* Número de páginas.
* Quantidade.
* Cor/P&B.
* Frente/frente e verso.
* Acabamentos.
* Agrafagem.
* Outros serviços.

Saída:

```text
Custo estimado
```

As regras de cálculo deverão estar centralizadas e ser configuráveis.

O cálculo não deverá depender exclusivamente de código fixo no frontend.

---

# 23. Análise Automática de Documentos

Está prevista uma funcionalidade avançada para análise automática de documentos enviados para reprografia.

O sistema poderá futuramente analisar:

* Número de páginas.
* Formato.
* Conteúdo a cores.
* Densidade de tinta.
* Outras características relevantes.

Esta funcionalidade será considerada **fase posterior**, não sendo requisito obrigatório do primeiro MVP.

---

# 24. Prioridade e SLA

Os pedidos de reprografia poderão possuir:

* Prioridade.
* Data pretendida.
* SLA.
* Estado.
* Indicador de atraso.

A interface poderá apresentar os pedidos através de Kanban.

Exemplo:

```text
┌─────────┬───────────┬──────────────┬────────┐
│ NOVOS   │ VALIDADO  │ PRODUÇÃO     │ PRONTO │
├─────────┼───────────┼──────────────┼────────┤
│ Pedido  │ Pedido    │ Pedido       │ Pedido │
│ Pedido  │ Pedido    │ Pedido       │        │
└─────────┴───────────┴──────────────┴────────┘
```

---

# 25. QR Code

O sistema deverá estar preparado para utilização de QR Codes.

Possíveis utilizações:

* Identificação de pedido.
* Consulta rápida de pedido.
* Entrega.
* Identificação de equipamentos/cacifos.

A implementação será detalhada numa fase posterior.

---

# 26. Quotas de Impressão

A arquitetura deverá permitir implementar quotas.

Possíveis níveis:

```text
Professor
Departamento
Grupo
Período
Escola
```

As quotas deverão ser configuráveis e não ficar codificadas de forma rígida.

---

# 27. Retenção de Ficheiros

Os ficheiros de reprografia deverão estar sujeitos a políticas de retenção.

Fluxo conceptual:

```text
Upload
   ↓
Pedido
   ↓
Produção
   ↓
Entrega
   ↓
Período de retenção
   ↓
Eliminação
```

A eliminação automática será implementada quando estiver definida a política de retenção da escola.

---

# 28. Módulo LOCKERS

O módulo LOCKERS será responsável pela gestão de cacifos.

Entidades principais:

```text
Cacifo
Aluno
Atribuição
Caução
```

Estados do cacifo:

* Livre.
* Ocupado.
* Avariado.
* Inativo.

O sistema deverá impedir operações incompatíveis com atribuições ativas.

---

# 29. Partilha de Cacifos

Um cacifo poderá ser associado a:

* Um aluno principal.
* Um ou mais alunos partilhados.

A remoção de um aluno partilhado não deverá eliminar a atribuição do aluno principal.

O modelo de dados deverá representar explicitamente estas relações.

---

# 30. Módulo REPORTS

O módulo REPORTS deverá centralizar a produção de relatórios.

Fontes possíveis:

* INVENTORY.
* SALES.
* PRINTSHOP.
* LOCKERS.

Relatórios previstos:

* Diário.
* Semanal.
* Mensal.
* Caixa.
* Stock.
* Inventários.
* Reprografia.
* Cacifos.

---

# 31. Regras de Relatórios

Os relatórios deverão utilizar períodos definidos de forma consistente.

Exemplo:

**Relatório semanal**

Deverá poder utilizar automaticamente o último dia útil do período.

Caso determinado dia seja feriado, o sistema deverá considerar o dia útil anterior de acordo com o calendário escolar configurado.

Os relatórios de pagamento deverão mostrar apenas os métodos de pagamento que tenham movimentos no período.

---

# 32. Calendário e Dias Úteis

A arquitetura deverá permitir manter um calendário de:

* Dias úteis.
* Fins de semana.
* Feriados.
* Interrupções escolares.

Este calendário poderá ser utilizado por:

* Relatórios.
* SLA.
* Reprografia.
* Planeamento.
* Fechos de caixa.

---

# 33. Integração entre Módulos

Os módulos deverão comunicar através de serviços e entidades bem definidas.

Exemplo:

```text
INVENTORY
    │
    └──────► SALES
                │
                └──────► REPORTS
```

E:

```text
PRINTSHOP
    │
    ├──► REPORTS
    │
    └──► STORAGE
```

E:

```text
LOCKERS
    │
    └──────► REPORTS
```

Os módulos não deverão criar dependências circulares desnecessárias.

---

# 34. Ocorrências

O sistema de **Gestão de Ocorrências** existente deverá permanecer separado nesta fase.

Poderá existir futuramente uma integração com o ERP, caso seja considerada vantajosa.

A integração deverá ser feita através de interfaces/API ou estruturas de dados bem definidas, evitando a fusão prematura dos dois projetos.

---

# 35. Aplicação Responsiva

A interface deverá adaptar-se a diferentes tamanhos de ecrã.

### Desktop

Destinado principalmente a:

* Administração.
* Papelaria.
* Reprografia.
* Relatórios.

### Tablet

Destinado a:

* Administração.
* Operações.
* Inventário.
* Cacifos.

### Smartphone

Destinado principalmente a:

* Professores.
* Operações.
* Consultas rápidas.
* Scanners/QR Code.

---

# 36. PWA

A aplicação deverá possuir características PWA.

Requisitos:

* Instalação no dispositivo.
* Interface responsiva.
* Ícone da aplicação.
* Manifest.
* Service Worker quando aplicável.
* Experiência semelhante a aplicação instalada.

O suporte offline deverá ser introduzido apenas quando existir uma necessidade concreta.

A arquitetura não deverá adicionar complexidade offline ao MVP sem necessidade.

---

# 37. Estrutura de Navegação

A aplicação deverá possuir navegação modular.

Exemplo:

```text
ERP
│
├── Início
├── Papelaria
│   ├── Produtos
│   ├── Stock
│   ├── Fornecedores
│   └── Compras
│
├── Vendas
│   ├── Nova Venda
│   ├── Histórico
│   └── Caixa
│
├── Reprografia
│   ├── Pedidos
│   ├── Produção
│   ├── Kanban
│   └── Entregas
│
├── Cacifos
│   ├── Cacifos
│   ├── Alunos
│   └── Atribuições
│
├── Relatórios
│
└── Administração
```

Os itens apresentados deverão depender das permissões do utilizador e dos módulos ativos.

---

# 38. Configuração de Módulos

A escola deverá poder ter módulos ativos ou inativos.

Exemplo:

```text
Papelaria       → ATIVO
Caixa           → ATIVO
Reprografia     → ATIVO
Cacifos         → ATIVO
Outro módulo    → INATIVO
```

A aplicação deverá evitar apresentar funcionalidades de módulos que não estejam disponíveis para aquela escola/utilizador.

---

# 39. API e Serviços

O frontend deverá comunicar com o backend através dos mecanismos disponibilizados pelo Supabase.

A lógica de negócio crítica deverá ser centralizada em:

* PostgreSQL.
* Funções SQL.
* Supabase Edge Functions, quando justificável.
* Serviços específicos do backend.

O frontend deverá evitar implementar regras críticas exclusivamente em JavaScript.

---

# 40. Transações

Operações que envolvam múltiplas alterações relacionadas deverão ser tratadas de forma transacional quando necessário.

Exemplo de venda:

```text
Criar Venda
     +
Criar Linhas
     +
Registar Pagamento
     +
Atualizar Stock
     +
Registar Caixa
```

A operação deverá ser consistente: ou todas as alterações necessárias são concluídas, ou a operação é revertida.

---

# 41. Estratégia de Implementação

O desenvolvimento será incremental.

### Fase 1 — Fundação

* Documentação.
* Arquitetura.
* GitHub.
* Estrutura do projeto.

### Fase 2 — Supabase

* Projeto.
* Base de dados.
* Auth.
* RLS.
* Storage.

### Fase 3 — CORE

* Login.
* Utilizadores.
* Perfis.
* Permissões.
* Escola.
* Configurações.

### Fase 4 — INVENTORY

* Produtos.
* Categorias.
* Fornecedores.
* Stock.
* Movimentos.

### Fase 5 — SALES

* Vendas.
* Pagamentos.
* Caixa.
* Cartão Escolar.

### Fase 6 — PRINTSHOP

* Pedidos.
* Upload.
* Produção.
* Estimador.
* Kanban.
* Entregas.

### Fase 7 — LOCKERS

* Cacifos.
* Alunos.
* Atribuições.
* Cauções.

### Fase 8 — REPORTS

* Relatórios.
* PDF.
* Filtros.
* Fechos.

### Fase 9 — Funcionalidades avançadas

* QR Code.
* Quotas.
* Análise automática.
* Automatizações.
* Integrações.

---

# 42. MVP

O primeiro MVP não deverá implementar toda a arquitetura funcional.

O objetivo será criar rapidamente uma versão funcional com:

```text
CORE
  ↓
INVENTORY
  ↓
SALES
  ↓
CAIXA
```

O MVP deverá demonstrar:

* Login.
* Perfis.
* Produtos.
* Stock.
* Vendas.
* Pagamentos.
* Caixa.
* Relatórios básicos.

Depois de validado, os restantes módulos serão adicionados gradualmente.

---

# 43. Desenvolvimento no Bolt.new

O Bolt.new será utilizado para acelerar a construção do frontend e da aplicação.

O Bolt.new não deverá ser utilizado como fonte de verdade da arquitetura.

A arquitetura e documentação oficial estarão no:

```text
GitHub
```

O Bolt.new deverá trabalhar sobre o projeto versionado.

Fluxo:

```text
Documentação
      ↓
GitHub
      ↓
Bolt.new
      ↓
Código
      ↓
GitHub
      ↓
Supabase
```

Cada etapa relevante deverá ser versionada através de commits.

---

# 44. GitHub

O GitHub será o repositório oficial do projeto.

Repositório:

```text
HTProjetos/ERP-Escolar
```

Responsabilidades:

* Código.
* Documentação.
* Scripts.
* Testes.
* Diagramas.
* Histórico de alterações.

---

# 45. Estrutura de Diretórios

A estrutura inicial deverá evoluir para algo semelhante a:

```text
ERP-Escolar/
│
├── src/
│   ├── core/
│   ├── modules/
│   │   ├── inventory/
│   │   ├── sales/
│   │   ├── printshop/
│   │   ├── lockers/
│   │   └── reports/
│   ├── components/
│   ├── layouts/
│   ├── services/
│   ├── hooks/
│   ├── lib/
│   ├── types/
│   └── utils/
│
├── database/
│   ├── migrations/
│   ├── seeds/
│   └── functions/
│
├── docs/
│
├── prompts/
│
├── tests/
│
├── assets/
│
├── diagrams/
│
├── scripts/
│
├── public/
│
├── README.md
├── CHANGELOG.md
└── .gitignore
```

A estrutura poderá ser adaptada durante o desenvolvimento, mantendo os princípios de separação definidos neste documento.

---

# 46. Gestão de Alterações

Alterações arquiteturais relevantes deverão ser:

1. Identificadas.
2. Avaliadas.
3. Documentadas.
4. Implementadas.
5. Versionadas no GitHub.

Alterações significativas deverão originar uma nova versão do documento de arquitetura.

---

# 47. Ambiente de Desenvolvimento

Ambiente principal:

```text
Windows
   │
   ├── Visual Studio Code
   ├── Git
   ├── GitHub Desktop
   └── Navegador
```

Ferramentas externas:

```text
GitHub
Supabase
Bolt.new
ChatGPT
```

---

# 48. Estratégia de Custos

O projeto deverá privilegiar ferramentas gratuitas ou de baixo custo.

A arquitetura deverá evitar dependências obrigatórias de serviços pagos.

Quando uma funcionalidade avançada exigir um serviço pago, deverá existir sempre que possível uma alternativa ou modo básico sem esse serviço.

---

# 49. Escalabilidade

A arquitetura deverá permitir crescimento progressivo.

Possíveis evoluções:

```text
1 Escola
   ↓
Vários módulos
   ↓
Vários utilizadores
   ↓
Várias escolas
   ↓
Integrações externas
```

A primeira versão não deverá introduzir complexidade de infraestrutura desnecessária para uma escola.

---

# 50. Manutenção

O código deverá privilegiar:

* Componentes reutilizáveis.
* Serviços separados.
* Tipagem TypeScript.
* Funções pequenas.
* Nomenclatura consistente.
* Documentação.
* Testes.
* Commits frequentes.

A manutenção deverá ser possível mesmo quando o projeto crescer.

---

# 51. Testes

Os testes serão introduzidos progressivamente.

Categorias:

* Testes de lógica.
* Testes de componentes.
* Testes de integração.
* Testes de permissões.
* Testes de RLS.
* Testes de fluxos críticos.

Fluxos prioritários:

* Login.
* Criação de produto.
* Movimento de stock.
* Venda.
* Pagamento.
* Fecho de caixa.
* Pedido de reprografia.
* Atribuição de cacifo.

---

# 52. Critérios Arquiteturais de Aceitação

A arquitetura será considerada adequada quando:

* A aplicação estiver separada por módulos.
* O Supabase estiver corretamente integrado.
* Os dados estiverem protegidos por RLS.
* O sistema suportar diferentes perfis.
* O modelo suportar múltiplas escolas.
* O frontend puder evoluir independentemente da base de dados.
* O sistema puder ser utilizado como PWA.
* Os módulos puderem evoluir individualmente.
* O código estiver versionado no GitHub.
* A documentação estiver atualizada.

---

# 53. Documentos Relacionados

Este documento deverá ser complementado por:

```text
DOC 01 — Project Charter
DOC 03 — Especificação Técnica
DOC 04 — Especificação Funcional
DOC 05 — Modelo de Dados
DOC 06 — Regras de Negócio
DOC 07 — Segurança e RLS
DOC 08 — Plano de Testes
DOC 09 — Plano de Implementação
```

A numeração poderá ser ajustada à medida que a documentação for consolidada.

---

# 54. Estado da Arquitetura

**Estado:** Proposta consolidada para aprovação.

A arquitetura encontra-se suficientemente definida para servir de base à elaboração da Especificação Técnica e do Modelo de Dados.

A implementação da aplicação deverá começar apenas depois de os requisitos técnicos fundamentais estarem documentados.

---

# 55. Próximo Passo

O próximo documento será:

**DOC 03 — Especificação Técnica**

Este documento deverá detalhar:

* Estrutura concreta do projeto.
* Tecnologias.
* Dependências.
* Supabase.
* PostgreSQL.
* Tabelas.
* Relações.
* Auth.
* RLS.
* Storage.
* APIs.
* Serviços.
* Componentes.
* Estados.
* Fluxos.
* Estratégia de erros.
* Logging.
* Testes.
* Deploy.
* Integração com Bolt.new.

O DOC 03 será utilizado posteriormente como referência técnica para a implementação.
