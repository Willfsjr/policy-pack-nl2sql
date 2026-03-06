# Policy Pack para NL2SQL

Este repositório reúne políticas e controles de segurança para aplicações que utilizam LLMs para consultar bancos de dados por meio de linguagem natural.

## Objetivo
Definir um conjunto reutilizável de políticas para reduzir riscos em pipelines NL2SQL, com foco em controle de acesso, escopo, execução segura, auditoria e prevenção de ações indevidas.

## Problema tratado
Em aplicações NL2SQL, a LLM pode ser induzida por prompts maliciosos a:
- gerar instruções indevidas
- acessar dados fora do escopo permitido
- contornar filtros obrigatórios
- produzir consultas caras ou abusivas
- explorar caminhos semânticos não previstos

## Estrutura do Policy Pack
### 1. Autorização e escopo
- RBAC por papel
- escopo obrigatório por empresa, filial, período ou tenant
- menor privilégio no banco
- preferência por views controladas

### 2. Políticas estruturais
- permitir somente SELECT
- allowlist de tabelas, colunas e views
- restrição de joins de alto risco
- parametrização obrigatória
- bloqueio de padrões estruturais não previstos

### 3. Limites de execução
- timeout
- limite de linhas
- limite de custo, quando disponível
- proteção contra consultas abusivas

### 4. Auditoria e observabilidade
- logging estruturado
- trilha de decisão allow/deny
- registro de intenção, SQL final, tempo, linhas e erro
- cobertura de auditoria reprodutível

## Conteúdo previsto
- `docs/politicas.md`
- `docs/modelo-ameaca.md`
- `docs/rbac-escopos.md`
- `docs/allowlist.md`
- `docs/auditoria.md`
- `exemplos/`
- `schemas/`
- `checks/`

## Aplicação
Este material faz parte do meu TCC em Ciência da Computação sobre segurança em aplicações NL2SQL.

## Status
Em desenvolvimento.
