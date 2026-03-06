# Modelo de Ameaça para NL2SQL

## Visão geral

Em aplicações NL2SQL, o usuário interage com o sistema por linguagem natural, e uma LLM interpreta essa entrada para gerar uma intenção de consulta. Esse modelo amplia a acessibilidade ao banco de dados, mas também aumenta a superfície de ataque, pois entradas maliciosas podem induzir o sistema a executar ações indevidas.

Neste trabalho, a LLM é tratada como componente não confiável. Ela não deve executar SQL livremente, mas apenas propor uma intenção estruturada sujeita a validação, políticas e execução controlada.

## Ativos protegidos

- dados corporativos consultados pelo sistema
- tabelas, views e colunas autorizadas
- escopos obrigatórios, como empresa, filial, período, tenant e papel do usuário
- disponibilidade do banco de dados
- trilha de auditoria da aplicação
- consistência das métricas e definições de negócio

## Agentes de ameaça

- usuário interno tentando acessar dados além do permitido
- usuário malicioso tentando induzir a LLM por prompt injection
- operador tentando remover filtros obrigatórios
- atacante tentando obter exfiltração indireta por agregações ou joins não previstos
- usuário tentando gerar consultas abusivas para degradar disponibilidade

## Classes de ação indevida

### A1 — Integridade
Tentativa de DML ou DDL, como:
- INSERT
- UPDATE
- DELETE
- CREATE
- DROP
- ALTER

### A2 — Confidencialidade
Acesso a tabelas, colunas ou views fora do allowlist definido.

### A3 — Escopo
Remoção, contorno ou ampliação indevida de filtros obrigatórios, como empresa, filial, período, tenant ou role.

### A4 — Disponibilidade
Execução de consultas abusivas com custo, tempo ou volume excessivos, caracterizando DoS lógico.

### A5 — Contorno semântico
Obtenção de dados fora do escopo por meio de agregações, combinações ou joins não previstos pelas regras.

## Premissas de segurança

- a LLM pode errar, alucinar ou ser influenciada por prompts maliciosos
- SQL livre gerado pela LLM não deve ser executado diretamente
- a autorização deve ser validada por mecanismo determinístico
- o banco deve operar com menor privilégio e modo somente leitura
- toda decisão de allow ou deny deve ser auditável

## Estratégia de mitigação

A mitigação proposta adota defesa em profundidade:
1. intenção estruturada em vez de SQL livre
2. validação por policy engine
3. compilação controlada por templates
4. execução em conexão read-only
5. limites de tempo, linhas e custo
6. logging estruturado e resposta com evidência
