# Template de prompt - Analise de release (inteligencia de mudanca) V1

Versao do prompt: 1.0 | Data: 2026-05-14

## Quando usar

Antes do go-live, quando ja existir **lista de tasks**, **texto/resumo das tasks** e **ligacao a PRs ou lista de ficheiros alterados**, com ou sem snapshot de metricas Datadog.

## Como usar no Cursor

- Referenciar ficheiros do repo com `@` (diffs, migrations, configs).
- Colar abaixo apenas o bloco `text` na conversa (ou adaptar para no LLM no n8n).

## Prompt (copiar a partir da linha seguinte ate ao fim do bloco)

```text
Atue como analista DevOps senior. Vais avaliar riscos de PRODUCAO para uma release, cruzando intencao das tasks com alteracoes reais em codigo.

Contexto da release:
- release_id: <RELEASE_ID>
- janela_go_live: <DATA_OU_JANELA>
- ambiente_alvo: <AMBIENTE>
- servicos_impactados: <SERVICOS>
- lista_task_keys: <ABC-123, ABC-124, ...>

Texto ou resumo das tasks (sem PII; se faltar contexto, declara lacuna):
<TEXTO_TASKS_OU_LINKS>

Alteracoes de codigo (resumo ou lista de PRs/commits; podes citar ficheiros vistos no contexto):
<MUDANCAS_CODIGO_OU_PRS>

Contexto opcional Datadog (metricas, SLOs, links a dashboards — apenas o que for seguro partilhar):
<CONTEXTO_DATADOG_OPCIONAL>

Contexto opcional CI/CD (pipeline, artefacto, versao):
<CONTEXTO_CICD_OPCIONAL>

Tarefa:
1) Para CADA task_key da lista, produz uma secao com correlacao task -> mudanca real.
2) Para cada task, lista riscos de producao PLAUSIVEIS (nao inventes cenarios sem ligacao ao texto ou ao diff).
3) Para cada risco, indica: severidade estimada (baixa/media/alta), confianca (baixa/media/alta), evidencias a favor, evidencias contra, lacunas de dados.
4) Produz um "pacote de observabilidade" pos-release: metricas/logs/traces/sintomas a vigiar, queries ou descricoes de monitor sugeridos (texto), duracao sugerida da janela de atencao (ex.: 24h/72h), e o que validaria mitigacao ou rollback.
5) Lista explicita de lacunas: dados que faltam para reduzir incerteza.
6) Rollout: ordem sugerida (se aplicavel), feature flags, dependencias entre tasks.

Formato obrigatorio da resposta (usa estes titulos):
## Resumo executivo (go/no-go orientativo, sem decisao final)
## Mapa task -> mudanca
## Riscos por task
## Pacote de observabilidade (pos-go-live)
## Lacunas de dados
## Rollout e dependencias
## Itens para revisao humana obrigatoria

Regras:
- Nao executar acoes em infraestrutura; apenas recomendacoes.
- Nao expor nem pedir segredos, tokens ou PII.
- Quando nao houver evidencia suficiente, escreve "NAO DETERMINAVEL" em vez de adivinhar.
- Idioma da resposta: portugues (PT-BR).
```

## Criterio de qualidade (revisao humana)

- [ ] Cada risco alto/medio tem **evidencia** citando task ou ficheiro/area de codigo.
- [ ] Secao **Lacunas** nao esta vazia se o contexto foi parcial.
- [ ] Pacote de observabilidade e **acionavel** (sem apenas "monitorizar o sistema").

## Historico de versoes do prompt

| Versao | Data | Notas |
|--------|------|-------|
| 1.0 | 2026-05-14 | Versao inicial |
