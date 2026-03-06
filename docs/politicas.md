# Policy Pack v1 para NL2SQL

## Objetivo

Este documento define um conjunto mínimo de políticas reutilizáveis para reduzir riscos em aplicações NL2SQL, com foco em impedir ações indevidas, restringir escopo, aumentar auditabilidade e manter utilidade operacional.

## 1. Políticas de autorização e escopo

### P1 — RBAC
Os papéis do usuário definem métricas, objetos e escopos permitidos. Exemplos de papéis:
- CEO
- financeiro
- comercial
- gestor regional
- analista

### P2 — Escopo obrigatório
Toda consulta deve respeitar filtros obrigatórios de contexto, como:
- empresa
- filial
- período
- tenant
- papel

Caso o período ou outro filtro essencial não seja informado, a consulta deve ser recusada ou complementada de forma controlada.

### P3 — Menor privilégio no banco
A conexão utilizada pela aplicação deve operar em modo somente leitura. Sempre que possível, a aplicação deve consultar views controladas de BI em vez de tabelas brutas.

## 2. Políticas estruturais

### P4 — Somente SELECT
A aplicação deve permitir apenas consultas de leitura, com bloqueio explícito de DML e DDL.

### P5 — Allowlist de objetos
Somente tabelas, colunas e views previamente autorizadas podem ser consultadas. Opcionalmente, também pode existir allowlist de joins permitidos.

### P6 — Bloqueio de padrões de alto risco
Devem ser bloqueados padrões como:
- CROSS JOIN sem chave
- UNION não previsto
- subqueries não autorizadas
- hints
- combinações estruturalmente fora do catálogo permitido

### P7 — Parametrização obrigatória
A geração de SQL deve utilizar bind variables. A concatenação de strings para montagem dinâmica de consulta não deve ser permitida.

### P8 — Limites de execução
Toda execução deve respeitar:
- limite de linhas
- timeout
- limite de custo ou plano, quando disponível

## 3. Políticas de auditoria e observabilidade

### P9 — Logging estruturado
Cada tentativa deve registrar:
- usuário
- prompt
- intenção estruturada
- SQL final
- decisão allow ou deny
- tempo
- quantidade de linhas
- erro, quando houver

### P10 — Detecção de tentativas
A aplicação deve contar e classificar recusas, permitindo identificar padrões repetitivos de abuso, tentativa de bypass ou comportamento anômalo.

### P11 — Evidência na resposta
A resposta deve anexar o SQL efetivamente executado, os filtros aplicados e a definição da métrica utilizada, aumentando transparência e reprodutibilidade.

## Resultado esperado

O Policy Pack não depende de “confiar” na LLM. Ele depende de impor controles determinísticos antes da execução do SQL.
