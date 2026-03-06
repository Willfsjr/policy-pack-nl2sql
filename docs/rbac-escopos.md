# RBAC e Escopos Obrigatórios

## Finalidade

Este documento descreve como aplicar controle de acesso por papel e escopo em aplicações NL2SQL, garantindo que o usuário visualize apenas os dados compatíveis com sua função e contexto organizacional.

## Princípios

- o papel define o que pode ser consultado
- o escopo define quais dados podem ser retornados
- a LLM não decide permissões
- toda autorização é verificada por mecanismo determinístico

## Exemplo de papéis

### CEO
- pode consultar indicadores consolidados
- acesso agregado por empresa ou grupo
- sem acesso a objetos sensíveis fora do catálogo permitido

### Financeiro
- pode consultar métricas financeiras autorizadas
- acesso restrito a empresas, filiais e períodos compatíveis com sua função

### Comercial
- pode consultar vendas, clientes, produtos e metas dentro de seu escopo
- não deve acessar folha, credenciais, tabelas técnicas ou objetos fora do domínio comercial

### Gestor regional
- acesso restrito à sua região, empresa ou conjunto de filiais

### Analista
- acesso operacional limitado às métricas e objetos necessários à rotina

## Escopos obrigatórios

Toda consulta deve considerar, quando aplicável:
- empresa
- filial
- período
- tenant
- papel do usuário

## Regras de aplicação

### Regra 1
Se a consulta não informar período e ele for obrigatório, a execução deve ser recusada ou complementada por um valor controlado definido pela política.

### Regra 2
O usuário não pode ampliar o escopo por instruções como:
- ignore a filial
- mostre todas as empresas
- remova os filtros
- exiba todos os clientes

### Regra 3
O SQL final deve conter explicitamente os filtros obrigatórios, independentemente da forma como o pedido foi feito em linguagem natural.

### Regra 4
A resposta nunca deve incluir dados fora do escopo permitido, mesmo que a pergunta tente induzir isso por urgência, autoridade aparente ou engenharia social.

## Exemplo conceitual

Pergunta:
“Mostre o faturamento de todos os clientes sem restringir pela filial.”

Decisão esperada:
- deny, se violar política
ou
- allow com escopo forçado, mantendo a filial autorizada do usuário

## Benefício

A imposição de RBAC e escopo obrigatório reduz exfiltração, amplia governança e impede que a linguagem natural contorne regras corporativas.
