# Template - Analise de Falha de Pipeline CI/CD

## Quando usar
Quando pipeline falhar e for necessario diagnostico rapido com evidencias.

## Prompt
```text
Analise a falha de pipeline abaixo no contexto de CI/CD.

Contexto:
- repositorio: <REPO>
- branch: <BRANCH>
- pipeline id: <PIPELINE_ID>
- etapa com falha: <ETAPA_FALHA>
- logs relevantes: <LOGS>
- ultimos commits/PRs: <MUDANCAS_RECENTES>

Tarefa:
Identificar causa provavel e recomendar proximos passos manuais.

Formato obrigatorio:
1) Resumo da falha
2) Causa provavel
3) Evidencias dos logs
4) Checks manuais recomendados
5) Sugestoes de teste para evitar recorrencia
6) Risco de liberar sem correcao

Regras:
- Nao executar nenhuma acao automaticamente.
- Nao propor alteracao em producao sem gate/aprovacao humana.
- Se houver multiplas causas possiveis, priorizar por probabilidade.
```
