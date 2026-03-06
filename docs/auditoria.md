# Auditoria e Evidência em NL2SQL

## Objetivo

Garantir que toda tentativa de consulta em linguagem natural gere trilha suficiente para auditoria, reprodutibilidade e investigação de incidentes.

## Logging estruturado

Cada tentativa deve registrar, no mínimo:
- identificador do usuário
- papel
- data e hora
- prompt original
- intenção estruturada produzida pela LLM
- SQL final gerado
- parâmetros aplicados
- filtros obrigatórios impostos
- decisão allow ou deny
- tempo de execução
- quantidade de linhas retornadas
- erro ou motivo da recusa

## Classificação de eventos

Os logs devem permitir classificar:
- consulta legítima permitida
- consulta legítima bloqueada indevidamente
- tentativa de DML ou DDL
- tentativa de ampliação de escopo
- tentativa de acesso a objeto não autorizado
- tentativa de DoS lógico
- tentativa de engenharia social
- erro de compilação ou validação

## Evidência na resposta

Além do log interno, a resposta ao usuário deve apresentar evidência mínima, como:
- SQL executado
- filtros efetivamente aplicados
- definição da métrica usada
- eventual recusa e justificativa resumida

## Benefícios da auditoria

A auditoria melhora:
- rastreabilidade
- transparência
- governança
- investigação de incidentes
- avaliação experimental do sistema

## Relação com métricas do TCC

A auditoria também sustenta métricas como:
- audit coverage
- evidence grounding

Essas métricas ajudam a verificar se a aplicação não apenas responde, mas responde com rastreabilidade e base reprodutível.
