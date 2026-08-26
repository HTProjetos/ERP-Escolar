# DOC 05 — PLANO DE IMPLEMENTAÇÃO

## 1. Objetivo

Este documento define o plano oficial para transformar a arquitetura, especificações funcionais e modelo de dados do projeto **ERP Gestão Escolar** numa aplicação funcional.

O documento estabelece:

* estratégia de desenvolvimento;
* sequência de implementação;
* ferramentas;
* ambientes;
* metodologia;
* critérios de validação;
* estratégia MVP;
* segurança;
* testes;
* deployment;
* utilização de ferramentas de Inteligência Artificial;
* evolução futura do sistema.

O objetivo é permitir que o projeto seja desenvolvido de forma incremental, controlada e sustentável.

---

# 2. Visão Geral da Implementação

A implementação seguirá o princípio:

```text
DOCUMENTAÇÃO
     ↓
CONFIGURAÇÃO
     ↓
BASE DE DADOS
     ↓
BACKEND
     ↓
CORE
     ↓
MÓDULOS
     ↓
INTERFACE
     ↓
PWA
     ↓
TESTES
     ↓
DEPLOY
```

Não será desenvolvido todo o sistema simultaneamente.

Cada módulo será implementado, testado e validado antes de avançar para o seguinte.

---

# 3. Princípios Fundamentais

O projeto deverá seguir os seguintes princípios:

1. Simplicidade.
2. Modularidade.
3. Segurança.
4. Reutilização.
5. Documentação.
6. Versionamento.
7. Testes incrementais.
8. Evolução gradual.
9. Minimização de custos.
10. Evitar dependências desnecessárias.
11. Não criar funcionalidades sem necessidade funcional.
12. Não introduzir complexidade prematuramente.

---

# 4. Estratégia de Desenvolvimento

O desenvolvimento seguirá ciclos curtos:

```text
Planeamento
    ↓
Implementação
    ↓
Teste
    ↓
Correção
    ↓
Validação
    ↓
Commit
    ↓
Próxima funcionalidade
```

Cada funcionalidade deverá ser considerada concluída apenas depois de validada.

---

# 5. MVP

O primeiro objetivo será criar um **MVP funcional**.

O MVP deverá permitir utilizar o sistema numa escola real em operações essenciais.

Funcionalidades prioritárias:

```text
Autenticação
Gestão da Escola
Utilizadores
Perfis
Permissões
Produtos
Categorias
Fornecedores
Stock
Vendas
Pagamentos
Caixa
Pedidos de Reprografia
Cacifos
Alunos
Relatórios básicos
```

Funcionalidades avançadas serão implementadas posteriormente.

---

# 6. Stack Tecnológica

A arquitetura utilizará:

| Área                      | Tecnologia                       |
| ------------------------- | -------------------------------- |
| Repositório               | GitHub                           |
| Versionamento             | Git                              |
| IDE                       | Visual Studio Code               |
| Desenvolvimento assistido | Bolt.new                         |
| Backend                   | Supabase                         |
| Base de dados             | PostgreSQL                       |
| Autenticação              | Supabase Auth                    |
| Storage                   | Supabase Storage                 |
| Segurança                 | Row Level Security               |
| API                       | Supabase                         |
| Aplicação Web             | Framework suportado pelo projeto |
| Aplicação móvel           | PWA                              |
| Relatórios                | PDF / CSV                        |
| Documentação              | Markdown                         |

A escolha final do framework frontend será confirmada durante a criação técnica da aplicação.

---

# 7. GitHub

O GitHub será a fonte oficial do código do projeto.

Repositório:

```text
ERP-Escolar
```

Fluxo:

```text
VS Code
   ↓
Git
   ↓
GitHub
```

Todo o código e documentação relevante deverão ser versionados.

---

# 8. Branch Principal

A branch principal será:

```text
main
```

A branch `main` deverá representar uma versão funcional e estável.

Durante desenvolvimento poderão ser utilizadas branches adicionais quando necessário.

---

# 9. Commits

Os commits deverão possuir mensagens claras.

Exemplos:

```text
feat: add inventory module
feat: add sales module
fix: correct stock calculation
fix: correct cash closing
docs: update database model
test: add RLS tests
refactor: simplify product service
```

Os commits deverão representar alterações coerentes.

---

# 10. Supabase

O Supabase será o backend principal.

Responsabilidades:

```text
PostgreSQL
Authentication
Storage
API
RLS
Functions
Triggers
```

A aplicação não deverá depender de um servidor backend tradicional enquanto o Supabase satisfizer os requisitos.

---

# 11. Base de Dados

A base de dados será PostgreSQL.

A estrutura será criada através de migrations versionadas.

Local:

```text
database/migrations/
```

As migrations deverão ser executadas por ordem.

Exemplo:

```text
001_initial_schema.sql
002_core.sql
003_inventory.sql
004_sales.sql
005_cash.sql
...
```

---

# 12. Regra das Migrations

Depois de uma migration ter sido utilizada num ambiente partilhado, não deverá ser alterada silenciosamente.

Uma alteração deverá gerar uma nova migration.

Exemplo:

```text
016_add_product_barcode.sql
017_add_supplier_field.sql
018_add_printshop_priority.sql
```

---

# 13. Seed

Dados iniciais deverão ser mantidos em:

```text
database/seeds/
```

Poderão incluir:

```text
Roles
Permissions
Payment Methods
Estados
Configurações
Dados de demonstração
```

Dados reais da escola não deverão ser incluídos nos seeds de desenvolvimento.

---

# 14. Estrutura da Base de Dados

A organização prevista será:

```text
database/
│
├── backups/
├── migrations/
├── policies/
├── functions/
├── triggers/
├── views/
└── seeds/
```

Cada diretório terá responsabilidade específica.

---

# 15. CORE

O primeiro módulo funcional será o CORE.

O CORE será responsável por:

```text
Escola
Utilizadores
Autenticação
Perfis
Roles
Permissões
Configurações
Auditoria
```

O CORE será implementado antes dos módulos de negócio.

---

# 16. Autenticação

A autenticação utilizará Supabase Auth.

O sistema deverá suportar:

```text
Login
Logout
Sessão
Recuperação de acesso
Proteção de páginas
```

O acesso às funcionalidades dependerá do utilizador autenticado.

---

# 17. Perfis

Inicialmente serão considerados:

```text
Administrador
Assistente Operacional
Professor
```

Outros perfis poderão ser acrescentados posteriormente.

---

# 18. Permissões

As permissões deverão ser baseadas em funções/roles e permissões específicas.

Exemplo:

```text
Administrador
    ↓
Acesso completo

Assistente Operacional
    ↓
Acesso operacional limitado

Professor
    ↓
Acesso aos serviços destinados aos professores
```

As permissões deverão ser verificadas tanto na interface como no backend/base de dados.

---

# 19. Row Level Security

O RLS será obrigatório nas tabelas que contenham informação protegida.

O princípio será:

```text
Utilizador autenticado
        ↓
Verificar escola
        ↓
Verificar permissões
        ↓
Permitir / bloquear operação
```

A segurança não poderá depender exclusivamente da interface.

---

# 20. Isolamento por Escola

O sistema deverá ser preparado para suportar uma escola por instalação.

Todas as entidades relevantes deverão possuir referência à escola:

```text
school_id
```

Isto facilitará uma futura evolução para arquitetura multi-escola caso seja necessária.

---

# 21. Auditoria

Operações relevantes deverão poder ser auditadas.

Exemplos:

```text
Criação
Alteração
Eliminação
Venda
Cancelamento
Abertura de caixa
Fecho de caixa
Alteração de permissões
Alteração de stock
Atribuição de cacifo
```

---

# 22. INVENTORY

Depois do CORE será implementado o módulo de inventário.

Funcionalidades:

```text
Categorias
Produtos
Fornecedores
Entradas
Compras
Movimentos
Stock
Inventário
```

---

# 23. Produtos

Cada produto poderá possuir:

```text
SIGE Code
Barcode
Nome
Categoria
Fornecedor
Preço de custo
Margem
IVA
Preço de venda
Stock atual
Stock mínimo
Estado
```

O preço de venda deverá poder ser calculado automaticamente.

Regra inicial:

```text
Preço de venda sem IVA =
Preço de custo × 1,10
```

Depois:

```text
Preço final =
Preço sem IVA × 1,23
```

As regras fiscais deverão ser parametrizáveis.

---

# 24. Stock

O stock será baseado em movimentos.

Modelo:

```text
Stock inicial
+
Entradas
-
Saídas
+
Ajustamentos
=
Stock atual
```

As alterações de stock deverão ser auditáveis.

---

# 25. SALES

Depois do INVENTORY será implementado o módulo de vendas.

Funcionalidades:

```text
Criar venda
Adicionar produtos
Calcular total
Registar pagamento
Atualizar stock
Emitir comprovativo
Consultar histórico
Cancelar venda quando autorizado
```

---

# 26. Métodos de Pagamento

O sistema deverá suportar inicialmente:

```text
Dinheiro
Cartão
Cartão Escolar
Outros métodos configuráveis
```

Os métodos poderão ser parametrizados.

---

# 27. CASH

O módulo de caixa será implementado depois das vendas.

Funcionalidades:

```text
Abertura
Movimentos
Vendas
Entradas
Saídas
Fecho
Conferência
Diferenças
Relatórios
```

---

# 28. Fecho de Caixa

O fecho deverá registar:

```text
Data
Hora de abertura
Hora de fecho
Operador
Valor inicial
Movimentos
Valor esperado
Valor contado
Diferença
```

A diferença deverá ser calculada automaticamente.

---

# 29. Relatórios de Caixa

Deverão existir:

```text
Relatório diário
Relatório semanal
Relatório mensal
```

Quando aplicável, o sistema deverá determinar automaticamente o último dia útil.

Exemplo:

```text
Sexta-feira feriado
      ↓
Último dia útil = Quinta-feira
```

Os relatórios deverão mostrar apenas os métodos de pagamento efetivamente utilizados.

---

# 30. PRINTSHOP

O módulo de Reprografia será implementado depois do CORE e poderá ser desenvolvido em paralelo com funcionalidades do ERP.

Funcionalidades:

```text
Pedidos
Ficheiros
Estimativas
Preços
Prioridade
SLA
Kanban
Produção
Entrega
Histórico
```

---

# 31. PWA para Professores

Os professores deverão poder utilizar uma interface PWA.

Funcionalidades:

```text
Login
Novo pedido
Upload de ficheiro
Escolha de parâmetros
Estimativa
Prioridade
Data pretendida
Consulta do estado
```

---

# 32. Estimador de Reprografia

O sistema deverá permitir calcular automaticamente uma estimativa.

Possíveis parâmetros:

```text
Número de páginas
Formato
Preto e branco
Cor
Frente
Frente e verso
Agrafamento
Encadernação
Outros acabamentos
```

A análise automática de ficheiros poderá ser adicionada numa fase posterior.

---

# 33. Kanban de Reprografia

Os pedidos poderão ser apresentados em Kanban:

```text
Pendente
     ↓
Em análise
     ↓
Em produção
     ↓
Pronto
     ↓
Entregue
```

A prioridade deverá ser visível.

---

# 34. SLA

Cada pedido poderá possuir:

```text
Prioridade
Data pretendida
Prazo
Estado
```

O sistema deverá permitir identificar pedidos urgentes ou atrasados.

---

# 35. LOCKERS

O módulo de Cacifos será integrado no ERP como módulo independente.

Funcionalidades:

```text
Alunos
Cacifos
Atribuições
Partilhas
Cauções
Histórico
```

---

# 36. Alunos

Os alunos poderão possuir:

```text
Nome
Número
Ano
Turma
Estado
```

---

# 37. Cacifos

Cada cacifo poderá possuir:

```text
Bloco
Número
Estado
```

Estados iniciais:

```text
Livre
Ocupado
Avariado
Inativo
```

---

# 38. Atribuições

Será possível associar:

```text
Aluno principal
Cacifo
Data
Estado
```

Também será possível adicionar alunos partilhados.

A remoção de um aluno partilhado não deverá eliminar o aluno principal nem a atribuição inteira.

---

# 39. CALENDAR

O calendário escolar será utilizado por funcionalidades que dependam de dias úteis.

Deverá permitir:

```text
Ano letivo
Feriados
Interrupções
Dias especiais
Dias úteis
```

---

# 40. REPORTS

O módulo de relatórios será implementado progressivamente.

Relatórios previstos:

```text
Vendas
Stock
Caixa
Reprografia
Cacifos
Auditoria
```

Formatos:

```text
PDF
CSV
```

---

# 41. NOTIFICATIONS

O módulo de notificações poderá ser implementado inicialmente de forma interna.

Exemplos:

```text
Pedido de reprografia pronto
Pedido atrasado
Stock baixo
Aviso de caixa
Alteração de estado
```

Push notifications e email poderão ser adicionados posteriormente.

---

# 42. Storage

O Supabase Storage será utilizado para ficheiros.

Possíveis buckets:

```text
printshop
reports
signatures
```

Os buckets deverão ser privados sempre que contenham informação da escola.

---

# 43. Ficheiros de Reprografia

Os ficheiros enviados pelos professores deverão:

```text
Ser associados ao pedido
Possuir identificação
Possuir data
Respeitar permissões
Ser armazenados de forma segura
```

---

# 44. Relatórios PDF

Os relatórios poderão incluir:

```text
Cabeçalho
Escola
Data
Filtros
Dados
Totais
Assinatura digital quando aplicável
Fotografias quando aplicável
```

---

# 45. PWA

As PWAs deverão ser responsivas e instaláveis em dispositivos compatíveis.

Objetivos:

```text
Android
PC
Tablet
```

A interface deverá adaptar-se ao tamanho do dispositivo.

---

# 46. Aplicação Administrativa

A aplicação administrativa será orientada para utilização em PC.

Deverá permitir:

```text
Gestão
Configuração
Consultas
Relatórios
Operações administrativas
```

---

# 47. PWA Operacional

Funcionalidades que beneficiem de mobilidade poderão ser disponibilizadas através de PWA.

Exemplos:

```text
Scanner de códigos de barras
Consulta de stock
Pedidos de reprografia
Gestão operacional
Consulta de cacifos
```

---

# 48. Responsividade

A interface deverá funcionar corretamente em:

```text
Desktop
Tablet
Smartphone
```

A experiência poderá variar por dispositivo, mas a funcionalidade essencial deverá permanecer disponível.

---

# 49. UX

A interface deverá privilegiar:

```text
Simplicidade
Poucos cliques
Boa legibilidade
Feedback imediato
Erros compreensíveis
Navegação consistente
```

---

# 50. Validação de Dados

Os dados deverão ser validados em múltiplas camadas:

```text
Interface
   ↓
Aplicação
   ↓
Database
```

A base de dados deverá possuir constraints sempre que apropriado.

---

# 51. Tratamento de Erros

Os erros deverão ser apresentados de forma compreensível.

Exemplo:

Em vez de:

```text
Foreign key violation
```

apresentar:

```text
Não é possível eliminar este produto porque existem vendas associadas.
```

---

# 52. Testes

Os testes serão realizados progressivamente.

Tipos:

```text
Unitários
Integração
Segurança
End-to-End
Testes manuais
```

---

# 53. Unit Tests

Deverão testar regras isoladas.

Exemplos:

```text
Cálculo de preço
Cálculo de IVA
Cálculo de margem
Cálculo de stock
Cálculo de caixa
Cálculo de SLA
```

---

# 54. Integration Tests

Deverão testar integrações.

Exemplos:

```text
Venda → Stock
Venda → Caixa
Pedido → Storage
Cacifo → Aluno
Relatório → Dados
```

---

# 55. Security Tests

Deverão verificar:

```text
Login
Logout
RLS
Permissões
Isolamento de dados
Storage
Acesso à API
```

---

# 56. E2E Tests

Fluxos completos deverão ser testados.

Exemplo:

```text
Login
 ↓
Criar produto
 ↓
Adicionar stock
 ↓
Efetuar venda
 ↓
Registar pagamento
 ↓
Consultar caixa
 ↓
Gerar relatório
```

---

# 57. Dados de Teste

Durante desenvolvimento deverão ser utilizados dados fictícios.

Exemplo:

```text
Escola Demo
Aluno Demo
Professor Demo
Produto Demo
Fornecedor Demo
Cacifo Demo
Pedido Demo
```

---

# 58. Segurança

Nunca deverão ser armazenados no GitHub:

```text
Passwords
Tokens secretos
Service Role Keys
Credenciais
Segredos
```

O ficheiro `.env` deverá permanecer excluído através do `.gitignore`.

---

# 59. Variáveis de Ambiente

Configurações poderão utilizar:

```text
SUPABASE_URL
SUPABASE_ANON_KEY
```

Chaves privilegiadas deverão permanecer apenas em ambientes seguros.

A `SERVICE_ROLE_KEY` nunca deverá ser exposta ao frontend.

---

# 60. Backups

Antes de alterações importantes na base de dados deverá existir backup adequado.

No ambiente de produção deverá existir uma política de backup.

---

# 61. Desenvolvimento com Bolt.new

O Bolt.new será utilizado como ferramenta de desenvolvimento assistido.

O processo deverá ser:

```text
Documentação
     ↓
Prompt
     ↓
Bolt.new
     ↓
Código
     ↓
GitHub
     ↓
Teste
```

---

# 62. Regra de Utilização da IA

A IA deverá implementar aquilo que foi definido.

Não deverá alterar automaticamente:

```text
Arquitetura
Modelo de dados
Regras de negócio
Segurança
Fluxos críticos
```

sem validação.

---

# 63. Prompts

Os prompts utilizados no Bolt.new deverão ser guardados em:

```text
prompts/Bolt.new/
```

Deverão existir prompts específicos por módulo ou tarefa.

Exemplo:

```text
01_setup.md
02_core.md
03_inventory.md
04_sales.md
05_cash.md
06_printshop.md
07_lockers.md
```

---

# 64. Desenvolvimento Incremental com Bolt

Não deverá ser enviado um único prompt gigantesco para criar todo o ERP.

A estratégia será:

```text
Setup
 ↓
CORE
 ↓
Testar
 ↓
Inventory
 ↓
Testar
 ↓
Sales
 ↓
Testar
 ↓
Cash
 ↓
...
```

---

# 65. Controlo do Código Gerado

Todo código gerado por IA deverá ser analisado e testado.

O facto de o Bolt.new gerar código não significa que o código esteja automaticamente correto.

---

# 66. Git como Ponto de Segurança

Antes de alterações grandes deverá existir um commit.

Modelo:

```text
Commit estável
      ↓
Alteração
      ↓
Testes
      ↓
Novo commit
```

Se uma alteração causar problemas, será possível regressar a uma versão anterior.

---

# 67. Ordem de Implementação

A ordem inicial será:

```text
1. Preparação
2. Supabase
3. Projeto frontend
4. CORE
5. Segurança
6. INVENTORY
7. SALES
8. CASH
9. PRINTSHOP
10. LOCKERS
11. CALENDAR
12. REPORTS
13. NOTIFICATIONS
14. Testes
15. Integrações
16. PWA
17. Deploy
```

---

# 68. Fase 1 — Preparação

Objetivos:

```text
GitHub
VS Code
Node/ambiente necessário
Supabase
Bolt.new
Estrutura do projeto
```

Resultado esperado:

```text
Ambiente pronto para desenvolvimento.
```

---

# 69. Fase 2 — Base de Dados

Objetivos:

```text
Criar projeto Supabase
Configurar PostgreSQL
Criar migrations
Criar CORE
Criar RLS inicial
```

Resultado:

```text
Base de dados funcional.
```

---

# 70. Fase 3 — CORE

Objetivos:

```text
Login
Utilizadores
Roles
Permissões
Escola
Configurações
Auditoria
```

Resultado:

```text
Sistema base funcional.
```

---

# 71. Fase 4 — INVENTORY

Objetivos:

```text
Produtos
Categorias
Fornecedores
Stock
Compras
Movimentos
```

Resultado:

```text
Gestão de stock funcional.
```

---

# 72. Fase 5 — SALES

Objetivos:

```text
Vendas
Linhas
Pagamentos
Atualização de stock
Histórico
```

Resultado:

```text
Venda funcional.
```

---

# 73. Fase 6 — CASH

Objetivos:

```text
Abertura
Movimentos
Fecho
Conferência
Relatórios
```

Resultado:

```text
Caixa funcional.
```

---

# 74. Fase 7 — PRINTSHOP

Objetivos:

```text
Pedidos
Upload
Estimativas
Prioridade
Kanban
SLA
Entrega
```

Resultado:

```text
Reprografia funcional.
```

---

# 75. Fase 8 — LOCKERS

Objetivos:

```text
Alunos
Cacifos
Atribuições
Partilhas
Cauções
```

Resultado:

```text
Gestão de cacifos funcional.
```

---

# 76. Fase 9 — CALENDAR

Objetivos:

```text
Calendário escolar
Feriados
Dias úteis
```

Resultado:

```text
Regras temporais disponíveis para os módulos.
```

---

# 77. Fase 10 — REPORTS

Objetivos:

```text
Relatórios
Filtros
PDF
CSV
```

Resultado:

```text
Sistema de reporting funcional.
```

---

# 78. Fase 11 — NOTIFICATIONS

Objetivos:

```text
Notificações internas
Eventos
Estados
```

Resultado:

```text
Comunicação interna funcional.
```

---

# 79. Fase 12 — Testes

Serão realizados testes:

```text
Unitários
Integração
Segurança
E2E
Manuais
```

Problemas identificados deverão ser corrigidos antes do deployment.

---

# 80. Fase 13 — PWA

Depois de o núcleo funcional estar estável:

```text
Manifest
Service Worker
Instalação
Responsividade
Offline quando aplicável
```

---

# 81. Fase 14 — Deploy

O deployment será realizado apenas depois de:

```text
Testes
Segurança
Backup
Configuração
Validação
```

---

# 82. Critérios de Conclusão de um Módulo

Um módulo só será considerado concluído quando possuir:

```text
Database
Backend
Interface
Regras de negócio
Validações
Segurança
RLS
Testes
Documentação
```

---

# 83. Definition of Done

Uma tarefa será considerada concluída quando:

```text
[ ] Funcionalidade implementada
[ ] Interface validada
[ ] Database validada
[ ] Segurança validada
[ ] Testes realizados
[ ] Erros corrigidos
[ ] Documentação atualizada
[ ] Commit realizado
```

---

# 84. Gestão de Bugs

Os problemas deverão ser classificados:

```text
CRITICAL
HIGH
MEDIUM
LOW
```

Problemas críticos deverão ser corrigidos antes de continuar para funcionalidades dependentes.

---

# 85. Gestão de Alterações

Uma alteração que afete arquitetura ou base de dados deverá ser documentada.

Exemplos:

```text
Nova tabela
Novo módulo
Alteração de relacionamento
Alteração de segurança
Alteração de regra financeira
```

---

# 86. Decisões Arquiteturais

Decisões importantes deverão ser registadas em:

```text
docs/08_Decisions/
```

Exemplo:

```text
ADR-001.md
ADR-002.md
```

---

# 87. Documentação

A documentação deverá acompanhar o código.

Quando uma funcionalidade mudar significativamente:

```text
Código
+
Documentação
```

deverão ser atualizados.

---

# 88. Performance

A aplicação deverá evitar:

```text
Queries desnecessárias
Downloads excessivos
Processamento duplicado
Consultas sem índices
```

Índices deverão ser criados quando justificados.

---

# 89. Escalabilidade

A arquitetura deverá permitir evolução futura.

Possíveis módulos:

```text
Gestão de Ativos
Recursos Humanos
Compras
Manutenção
Documentos
Inventário avançado
```

Não serão implementados antes de existir necessidade.

---

# 90. Custos

O projeto deverá privilegiar ferramentas gratuitas ou de baixo custo.

Prioridade:

```text
Open Source
Free Tier
Serviços gratuitos
Ferramentas já disponíveis
```

Serviços pagos só deverão ser adotados quando existir uma necessidade concreta.

---

# 91. Dependências Externas

Cada dependência externa deverá ser avaliada segundo:

```text
Custo
Licença
Segurança
Manutenção
Necessidade
Alternativas
```

Não deverão ser adicionadas dependências apenas por conveniência.

---

# 92. Backup e Recuperação

Deverá existir uma estratégia para:

```text
Backup
Restauro
Recuperação
Versionamento
```

O processo deverá ser testado antes da entrada em produção.

---

# 93. Produção

Antes da utilização real:

```text
Base de dados
Segurança
Utilizadores
Backups
Storage
Relatórios
PWA
Testes
```

deverão estar validados.

---

# 94. Dados Reais

Os dados reais da escola só deverão ser introduzidos depois da validação do MVP.

Durante desenvolvimento deverão ser utilizados dados fictícios.

---

# 95. Migração de Dados

Caso seja necessário importar dados existentes:

```text
Dados existentes
      ↓
Limpeza
      ↓
Mapeamento
      ↓
Importação
      ↓
Validação
```

A importação deverá ser documentada.

---

# 96. Estratégia de Evolução

Após o MVP:

```text
MVP
 ↓
Utilização
 ↓
Feedback
 ↓
Correções
 ↓
Melhorias
 ↓
Novas funcionalidades
```

O sistema deverá evoluir de acordo com necessidades reais.

---

# 97. Funcionalidades Avançadas Futuras

Depois do MVP poderão ser implementadas:

```text
Análise automática de PDFs
Estimativa avançada de impressão
Notificações Push
Email
Dashboards
Estatísticas
Automatizações
Integrações externas
```

---

# 98. Dashboard

O dashboard não será prioridade inicial.

Poderá ser implementado depois dos módulos principais estarem estáveis.

O dashboard poderá apresentar:

```text
Vendas
Stock
Caixa
Pedidos
Cacifos
Alertas
```

---

# 99. Integrações Futuras

A arquitetura deverá permitir futuras integrações com:

```text
Sistemas escolares
Serviços de email
Serviços de impressão
Outros sistemas administrativos
```

As integrações serão analisadas individualmente.

---

# 100. Critério de MVP Concluído

O MVP será considerado concluído quando:

```text
[ ] Login funcional
[ ] Utilizadores funcionais
[ ] Roles funcionais
[ ] RLS funcional
[ ] Produtos funcionais
[ ] Stock funcional
[ ] Vendas funcionais
[ ] Caixa funcional
[ ] Reprografia funcional
[ ] Cacifos funcionais
[ ] Relatórios básicos funcionais
[ ] Testes realizados
[ ] Backups definidos
[ ] Documentação atualizada
```

---

# 101. Critério de Produção

O sistema só deverá entrar em produção quando:

```text
Segurança validada
Base de dados estável
Módulos principais testados
Backups definidos
Utilizadores configurados
Dados validados
Relatórios validados
PWA validada
```

---

# 102. Workflow Diário

Durante o desenvolvimento, o fluxo recomendado será:

```text
1. Escolher uma tarefa
2. Consultar documentação
3. Criar prompt
4. Implementar
5. Testar
6. Corrigir
7. Validar
8. Fazer commit
9. Atualizar documentação
10. Passar à próxima tarefa
```

---

# 103. Regra Contra Complexidade

Sempre que existir mais de uma solução possível, deverá ser privilegiada a solução:

```text
Mais simples
+
Segura
+
Manutenível
+
Suficiente para o requisito
```

Não será procurada complexidade apenas para demonstrar capacidade técnica.

---

# 104. Regra Contra Desenvolvimento Prematuro

Não serão implementados módulos ou funcionalidades apenas porque poderão ser úteis no futuro.

A prioridade será:

```text
Necessidade atual
   ↓
Implementação
   ↓
Validação
   ↓
Evolução
```

---

# 105. Gestão de Créditos de IA

Quando uma ferramenta de IA possuir créditos limitados, os prompts deverão ser preparados previamente.

Objetivo:

```text
Planeamento
   ↓
Prompt preciso
   ↓
Execução
   ↓
Teste
```

Evitar ciclos desnecessários de tentativa e erro.

---

# 106. Prompts Incrementais

Os prompts do Bolt.new deverão ser específicos.

Exemplo:

```text
Criar módulo de produtos
```

em vez de:

```text
Criar todo o ERP.
```

Cada prompt deverá indicar:

```text
Contexto
Objetivo
Ficheiros
Regras
Database
UI
Segurança
Critérios de aceitação
```

---

# 107. Critérios de Aceitação

Cada funcionalidade importante deverá possuir critérios verificáveis.

Exemplo:

```text
Produto criado
   ↓
Stock inicial registado
   ↓
Produto aparece na pesquisa
   ↓
Preço calculado corretamente
   ↓
Produto pode ser vendido
```

---

# 108. Não Regressão

Uma nova funcionalidade não deverá quebrar funcionalidades existentes.

Depois de alterações importantes deverá ser realizado um teste de regressão.

---

# 109. Estado Atual do Projeto

Documentação disponível:

```text
✓ Project Charter
✓ Arquitetura Consolidada
✓ Especificação Técnica
✓ Modelo de Dados e Base de Dados
✓ Plano de Implementação
```

Infraestrutura:

```text
✓ Repositório GitHub
✓ Git
✓ VS Code
✓ Estrutura inicial
```

---

# 110. Próximo Passo

Após a conclusão deste documento será iniciada a implementação técnica.

A sequência será:

```text
DOC 05
   ↓
Verificação do ambiente
   ↓
Supabase
   ↓
Estrutura inicial da aplicação
   ↓
Primeira migration
   ↓
CORE
   ↓
Primeiro módulo funcional
```

---

# 111. Regra de Transição

A partir deste ponto o projeto deixa a fase predominantemente documental e entra progressivamente na fase de implementação.

A documentação continuará a ser atualizada sempre que forem tomadas decisões técnicas relevantes.

---

# 112. Estado do Documento

**DOC 05 — PLANO DE IMPLEMENTAÇÃO**

Estado:

**CONCLUÍDO**

Objetivo:

Estabelecer o plano oficial para implementação incremental do ERP Gestão Escolar.

Próxima grande etapa:

**IMPLEMENTAÇÃO TÉCNICA**
