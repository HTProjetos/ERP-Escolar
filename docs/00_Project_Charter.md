# DOC 01 — PROJECT CHARTER

## ERP Gestão Escolar

**Documento:** 01 — Project Charter
**Versão:** 2.0.0
**Estado:** Aprovado para desenvolvimento
**Data de início do projeto:** 30 de julho de 2026
**Data de revisão:** 26 de agosto de 2026
**Plataforma:** ERP Gestão Escolar
**Repositório:** GitHub — `HTadeu73/erp-gestao-escolar`

---

# 1. Visão do Projeto

O **ERP Gestão Escolar** é uma plataforma modular destinada à gestão operacional de estabelecimentos de ensino, concebida inicialmente para escolas portuguesas e preparada para evolução futura para um modelo multi-escola.

O sistema pretende centralizar processos administrativos e operacionais que atualmente podem estar dispersos por folhas de cálculo, documentos e aplicações independentes.

A solução será desenvolvida com uma arquitetura moderna baseada em **Supabase/PostgreSQL**, aplicações web responsivas e **Progressive Web Apps (PWA)**, utilizando o **Bolt.new** como ferramenta de desenvolvimento da interface e aplicação.

O sistema será concebido de forma modular, permitindo que diferentes funcionalidades possam ser ativadas ou desativadas de acordo com as necessidades de cada escola.

---

# 2. Objetivos do Projeto

## 2.1 Objetivos principais

* Centralizar a gestão operacional da escola.
* Reduzir tarefas administrativas repetitivas.
* Automatizar cálculos e processos.
* Reduzir a utilização de folhas de cálculo dispersas.
* Disponibilizar informação centralizada e atualizada.
* Produzir relatórios automaticamente.
* Permitir utilização em computador, tablet e smartphone.
* Disponibilizar PWAs para diferentes perfis de utilizador.
* Criar uma arquitetura escalável e modular.
* Preparar o sistema para utilização futura por várias escolas.

## 2.2 Objetivo estratégico

Criar uma plataforma de gestão escolar modular, sustentável e de baixo custo, que possa começar por resolver necessidades concretas de uma escola e evoluir gradualmente para uma solução ERP escolar completa.

---

# 3. Princípios do Projeto

O desenvolvimento deverá respeitar os seguintes princípios:

1. **A base de dados é a fonte de verdade do sistema.**
2. A lógica crítica deve ser executada no backend sempre que possível.
3. A interface deverá poder ser alterada sem obrigar à alteração da estrutura de dados.
4. Todo o código será versionado no GitHub.
5. Toda a documentação técnica e funcional será mantida em Markdown no repositório.
6. A arquitetura deverá ser modular.
7. Os módulos deverão poder ser ativados por escola.
8. O sistema deverá suportar múltiplas escolas no futuro.
9. A segurança deverá ser implementada desde o início.
10. O desenvolvimento deverá ser incremental, começando por um MVP funcional e evoluindo posteriormente.
11. Sempre que possível, deverão ser utilizadas tecnologias gratuitas ou de baixo custo.
12. Os dados pessoais deverão ser tratados segundo princípios de segurança, minimização e retenção adequada.

---

# 4. Arquitetura Tecnológica

## 4.1 Backend

O backend principal será baseado em **Supabase**.

Componentes:

* PostgreSQL
* Supabase Auth
* Supabase Storage
* Row Level Security (RLS)
* APIs disponibilizadas pelo Supabase
* Funções e lógica de backend quando necessário

## 4.2 Frontend

A aplicação será construída com:

* Bolt.new
* React
* TypeScript
* Interface web responsiva
* Progressive Web Apps (PWA)

As PWAs permitirão utilizar determinadas funcionalidades em computadores, tablets e smartphones sem necessidade de desenvolver aplicações Android/iOS nativas.

## 4.3 Desenvolvimento e controlo de versões

Ferramentas principais:

* GitHub
* GitHub Desktop
* Visual Studio Code
* Bolt.new
* ChatGPT

O GitHub será utilizado como repositório oficial do código e documentação técnica.

---

# 5. Modelo Modular

O ERP será constituído por módulos independentes, com possibilidade de ativação por escola.

Arquitetura conceptual:

```text
ERP GESTÃO ESCOLAR
│
├── CORE
│
├── INVENTORY
│
├── SALES
│
├── PRINTSHOP
│
├── LOCKERS
│
└── REPORTS
```

A arquitetura deverá permitir acrescentar novos módulos no futuro sem necessidade de reconstruir o sistema.

---

# 6. Módulo CORE

O módulo CORE constitui a base transversal do ERP.

### Funcionalidades

* Gestão de escolas
* Utilizadores
* Perfis
* Permissões
* Configurações
* Ativação/desativação de módulos
* Auditoria
* Gestão de parâmetros gerais

O sistema deverá suportar diferentes níveis de acesso de acordo com o perfil do utilizador.

---

# 7. Módulo INVENTORY — Papelaria e Stock

Responsável pela gestão dos produtos e inventário.

### Funcionalidades previstas

* Produtos
* Categorias
* Fornecedores
* Compras
* Entradas de stock
* Saídas de stock
* Stock atual
* Inventários
* Ajustes de stock
* Histórico de movimentos

### Dados de produto

Sempre que aplicável, o produto poderá possuir:

* Código SIGE
* Código de barras
* Nome
* Categoria
* Fornecedor
* Stock atual
* Stock mínimo
* Preço de custo
* Margem
* IVA
* Preço de venda

### Cálculo de preço

A configuração inicialmente prevista contempla:

**Preço de venda = preço de custo + margem de 10% + IVA de 23%**

A arquitetura deverá permitir que estas regras sejam futuramente configuráveis por escola/produto quando necessário.

---

# 8. Módulo SALES — Vendas e Caixa

Responsável pelo registo das vendas e gestão financeira operacional da papelaria.

### Funcionalidades

* Registo de vendas
* Linhas de venda
* Métodos de pagamento
* Cartão escolar
* Movimentos de caixa
* Abertura de caixa
* Fecho de caixa
* Saldos
* Histórico de movimentos
* Relatórios de caixa

### Métodos de pagamento

O sistema deverá suportar diferentes métodos de pagamento, incluindo:

* Numerário
* Cartão
* Cartão escolar
* Outros métodos configuráveis

Os relatórios deverão apresentar apenas os métodos de pagamento efetivamente utilizados no período correspondente.

### Fecho de caixa

O fecho diário deverá:

* Registar automaticamente data e hora de abertura.
* Registar automaticamente data e hora de encerramento.
* Apresentar movimentos realizados.
* Apresentar valores por método de pagamento.
* Permitir conferência do saldo.
* Produzir relatório de fecho.

---

# 9. Módulo PRINTSHOP — Reprografia

O módulo PRINTSHOP será responsável pela gestão da reprografia.

Este módulo terá particular importância na utilização diária pelos professores.

## 9.1 PWA dos professores

Os professores poderão utilizar uma PWA para:

* Criar pedidos de impressão/reprodução.
* Anexar documentos.
* Indicar quantidade.
* Escolher formato.
* Escolher impressão a preto e branco ou cor.
* Indicar frente e verso.
* Selecionar acabamentos.
* Definir prioridade.
* Indicar data pretendida de entrega.
* Consultar o estado dos pedidos.

## 9.2 Gestão da reprografia

A área administrativa permitirá:

* Receber pedidos.
* Validar pedidos.
* Orçamentar.
* Colocar pedidos em produção.
* Alterar estado.
* Registar conclusão.
* Registar entrega.
* Consultar histórico.

---

# 10. Calculador / Estimador de Reprografia

Uma das funcionalidades diferenciadoras do projeto será um sistema de estimativa automática de custos.

O objetivo é permitir calcular o custo provável de um trabalho antes da produção.

O sistema deverá considerar, quando aplicável:

* Número de páginas.
* Formato A4/A3.
* Preto e branco.
* Cor.
* Frente.
* Frente e verso.
* Quantidade de exemplares.
* Acabamentos.
* Agrafagem.
* Outros serviços configuráveis.

A tabela de preços será configurável por escola.

Cada escola poderá definir preços por:

* Tipo de impressão.
* Formato.
* Cor/preto e branco.
* Frente/frente e verso.
* Acabamento.
* Outros serviços.

---

# 11. Agendamento e SLA

O módulo de Reprografia deverá incluir um sistema de prioridades e gestão de prazos.

O professor poderá indicar:

* Prioridade.
* Data pretendida.
* Observações.

A reprografia poderá visualizar os trabalhos num sistema tipo **Kanban**, organizado por estado e prioridade.

Exemplo:

```text
NOVOS
   ↓
VALIDADOS
   ↓
EM PRODUÇÃO
   ↓
PRONTOS
   ↓
ENTREGUES
```

O sistema poderá utilizar regras de SLA para ajudar a identificar pedidos urgentes ou em risco de ultrapassar o prazo.

---

# 12. QR Code de Entrega

Está prevista a utilização de **QR Codes** como mecanismo de apoio à entrega de trabalhos.

O objetivo é permitir identificar rapidamente um pedido quando este estiver pronto para entrega.

A implementação concreta deverá ser definida na especificação técnica e funcional.

---

# 13. Quotas de Impressão

O sistema deverá estar preparado para suportar futuramente quotas de impressão.

As quotas poderão permitir estabelecer limites por:

* Professor.
* Departamento.
* Grupo.
* Período.
* Escola.

O modelo deverá ser configurável e preparado para diferentes políticas internas.

---

# 14. Gestão de Ficheiros e RGPD

Como os pedidos de reprografia poderão conter documentos enviados pelos utilizadores, o sistema deverá prever políticas de retenção e eliminação.

Deverá existir suporte para:

* Armazenamento controlado.
* Acesso limitado.
* Registo de utilização.
* Retenção configurável.
* Purga automática de ficheiros após determinado período.

A política de retenção deverá ser configurável de acordo com as necessidades da escola e os requisitos aplicáveis de proteção de dados.

---

# 15. Módulo LOCKERS — Cacifos

O módulo de Cacifos será integrado na arquitetura do ERP, mantendo as funcionalidades específicas do sistema de gestão de cacifos já definidas.

### Funcionalidades previstas

* Gestão de cacifos.
* Estados dos cacifos.
* Alunos.
* Atribuições.
* Partilha de cacifos.
* Aluno principal.
* Alunos partilhados.
* Cauções.
* Histórico de atribuições.

Estados possíveis dos cacifos:

* Livre
* Ocupado
* Avariado
* Inativo

O sistema deverá impedir operações incompatíveis com atribuições ativas.

---

# 16. Módulo REPORTS

O módulo REPORTS será responsável pela produção de relatórios.

### Relatórios previstos

* Diário
* Semanal
* Mensal
* Caixa
* Inventários
* Stock
* Vendas
* Reprografia
* Cacifos

Os relatórios deverão permitir filtros e, quando aplicável, exportação para PDF.

---

# 17. Modelo Multi-Escola

A arquitetura deverá ser preparada desde o início para suportar múltiplas escolas.

Cada escola deverá possuir os seus próprios:

* Utilizadores
* Configurações
* Produtos
* Fornecedores
* Preços
* Movimentos
* Pedidos
* Relatórios
* Cacifos
* Políticas de retenção
* Módulos ativos

O isolamento dos dados entre escolas deverá ser garantido através da arquitetura da base de dados e das políticas de segurança.

---

# 18. Segurança

A segurança será considerada um requisito estrutural.

Deverão ser utilizados:

* Supabase Auth
* Row Level Security
* Perfis e permissões
* Separação de dados por escola
* Auditoria
* Controlo de acesso ao Storage
* Validação de operações críticas

Nenhuma interface deverá ser considerada mecanismo suficiente de segurança.

As permissões deverão ser aplicadas também no backend/base de dados.

---

# 19. Auditoria

Operações relevantes deverão poder ser registadas.

Exemplos:

* Login.
* Alteração de utilizadores.
* Alteração de permissões.
* Criação/alteração/eliminação de produtos.
* Movimentos de stock.
* Movimentos de caixa.
* Alterações de pedidos.
* Alterações de configurações.
* Operações administrativas.

O nível de detalhe da auditoria será definido na Especificação Técnica.

---

# 20. Estratégia de Desenvolvimento

O projeto será desenvolvido de forma incremental.

Não será objetivo construir todos os módulos simultaneamente.

## Fase 1 — Fundação

* Repositório GitHub.
* Estrutura do projeto.
* Documentação.
* Arquitetura.
* Definição da base de dados.

## Fase 2 — Backend

* Projeto Supabase.
* PostgreSQL.
* Auth.
* Estrutura inicial das tabelas.
* RLS.
* Storage.
* Funções necessárias.

## Fase 3 — MVP

A primeira versão funcional deverá concentrar-se nos componentes essenciais:

```text
CORE
  ↓
INVENTORY
  ↓
SALES
  ↓
CAIXA
```

O objetivo será obter rapidamente uma versão funcional e utilizável.

## Fase 4 — Reprografia

* Pedidos.
* PWA dos professores.
* Gestão de produção.
* Preços.
* Estimador.
* Entregas.
* Prioridades.
* SLA/Kanban.

## Fase 5 — Cacifos

* Cacifos.
* Alunos.
* Atribuições.
* Cauções.
* Relatórios.

## Fase 6 — Funcionalidades avançadas

* QR Code.
* Quotas.
* Políticas de retenção.
* Automatizações.
* Melhorias de relatórios.
* Novos módulos.

---

# 21. Documentação do Projeto

Toda a documentação técnica e funcional deverá ser mantida em Markdown no GitHub.

Estrutura prevista:

```text
docs/
├── 00_Project_Charter.md
├── 01_Arquitetura_Consolidada.md
├── 02_Especificacao_Tecnica.md
├── 03_Especificacao_Funcional.md
├── 04_Modelo_de_Dados.md
├── 05_Regras_de_Negocio.md
├── 06_Seguranca_e_RLS.md
├── 07_Plano_de_Testes.md
└── 08_Plano_de_Implementacao.md
```

O Google Drive será utilizado como **arquivo documental complementar**, mantendo cópias dos documentos oficiais para consulta.

O GitHub continuará a ser a referência documental associada ao código e ao desenvolvimento.

---

# 22. Repositório Atual

O projeto encontra-se atualmente no repositório:

```text
HTadeu73/erp-gestao-escolar
```

Estrutura inicial:

```text
erp-gestao-escolar/
│
├── assets/
├── database/
├── diagrams/
├── docs/
├── prompts/
├── scripts/
├── tests/
│
├── .gitignore
├── .gitkeep
├── CHANGELOG.md
├── LICENSE
└── README.md
```

O projeto encontra-se na fase de fundação/documentação, antes da implementação da aplicação.

---

# 23. Estado do Projeto

**Estado atual:**

🟢 Repositório GitHub — concluído
🟢 Estrutura inicial — concluída
🟢 Project Charter — consolidado
🟡 Arquitetura consolidada — próxima etapa
🟡 Especificação técnica — por concluir
🟡 Especificação funcional — por concluir
🟡 Modelo de dados — por concluir
🔴 Supabase — implementação por iniciar
🔴 Aplicação — implementação por iniciar
🔴 MVP — implementação por iniciar

---

# 24. Critérios Gerais de Sucesso

O projeto será considerado bem-sucedido quando:

1. O ERP possuir uma arquitetura modular.
2. Os dados estiverem centralizados no Supabase.
3. A segurança estiver implementada no backend.
4. A primeira versão funcional permitir gerir produtos, stock, vendas e caixa.
5. A reprografia permitir pedidos digitais dos professores.
6. O sistema permitir cálculo de custos de reprografia.
7. Existir gestão de prioridades e prazos.
8. Existirem relatórios operacionais.
9. A aplicação funcionar em computador, tablet e smartphone.
10. A arquitetura estiver preparada para múltiplas escolas.
11. O código e documentação estiverem versionados no GitHub.
12. O sistema puder evoluir modularmente sem reconstrução da plataforma.

---

# 25. Próximo Marco

O próximo marco do projeto será a elaboração do:

**DOC 02 — Arquitetura Consolidada**

Este documento deverá transformar as decisões deste Project Charter numa arquitetura técnica concreta, incluindo:

* Componentes do sistema.
* Frontend.
* Backend.
* Supabase.
* Estrutura de módulos.
* Fluxos de dados.
* Autenticação.
* Segurança.
* RLS.
* Storage.
* Integração entre módulos.
* Estrutura do projeto.
* Estratégia de desenvolvimento no Bolt.new.
* Integração GitHub ↔ Bolt.new ↔ Supabase.

---

# 26. Aprovação

Este Project Charter constitui a referência de alto nível para o desenvolvimento do ERP Gestão Escolar.

Alterações significativas à arquitetura, objetivos, módulos ou princípios do projeto deverão ser documentadas e versionadas.

**Versão aprovada:** 2.0.0

**Estado:** Aprovado para continuação do projeto.
