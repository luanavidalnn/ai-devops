# Checklist - Configuracao do ambiente e coleta por release

Versao: 0.1 | Data: 2026-05-14

Este ficheiro junta **duas coisas**: (A) o que configurar **uma vez** antes de operar o fluxo com n8n/Cursor/Datadog/Jira/Release Manager; (B) o que repetir **por release** para montar o pacote de entrada da analise.

---

## Parte A - Configuracao do ambiente (uma vez, ou por ambiente dev/stage)

### A.1 Governanca e dados

- [ ] Politica de uso de IA e dados lida (`policies/pt/` no hub).
- [ ] Definido o que **nao** entra em prompts (PII, tokens, payloads completos de clientes, segredos).
- [ ] Definido **quem aprova** criacao/alteracao de monitores ou dashboards em Datadog (nome + substituto).
- [ ] Definido **TTL ou revisao** para artefactos temporarios de release (tags, monitores “janela pos-release”).

### A.2 Jira (ou ferramenta de tasks)

- [ ] Conta de servico ou usuario bot com permissao de leitura nas issues do projeto (e escrita apenas se for estritamente necessario).
- [ ] Campos minimos acordados nas tasks: link para PR/branch, **servico** impactado, **risco** declarado, criterios de aceite.
- [ ] Convencao de etiquetas ou “fix version” / campo **Release** para filtrar a lista de tasks por release.

### A.3 Release Manager (ou equivalente)

- [ ] Identificado o **evento** que marca “release pronta para analise” (webhook, API, export manual).
- [ ] Se existir API/webhook: URL, metodo, payload de exemplo e **segredo** de assinatura guardado no cofre (nao no repo).
- [ ] Definido onde o **pacote de analise** fica arquivado (campo custom, anexo, Confluence, ticket filho).

### A.4 Git / Bitbucket (ou GitHub/GitLab)

- [ ] Token com leitura de **repos**, **PRs** e **diffs** nos repositorios da release.
- [ ] Convencao de como tasks ligam a PRs (campo URL, parser de “ABC-123” no titulo do PR, etc.).

### A.5 Datadog

- [ ] API Key e Application Key com **scopes minimos** para o piloto.
- [ ] Se o fluxo for criar monitores/dashboards via API: permissoes `monitors_write` e/ou `dashboards_write` **apenas** na conta correta; preferir subconjunto via permissao de role.
- [ ] Lista de **servicos** (tags `service:` ou equivalente) alinhada com Jira.
- [ ] URLs dos **dashboards base** por servico (para o prompt e para humanos validarem sugestoes).

### A.6 LLM e n8n (automacao opcional; ritual manual pode comecar sem isto)

- [ ] Decisao: **Modo manual** (coleta no n8n ou export + analise no Cursor) vs **Modo orquestrado** (n8n chama LLM).
- [ ] Se LLM no n8n: credencial (OpenAI, Azure OpenAI, etc.) com **limite de custo** e modelo acordado.
- [ ] Se n8n: instancia, credenciais por no, **registo de execucoes** e politica de retencao de logs (sem dados sensiveis).
- [ ] Workflow base criado (mesmo que esqueleto): trigger -> Jira -> Git -> opcional Datadog snapshot -> **no de aprovacao humana** antes de qualquer `write` em Datadog.

### A.7 Cursor (desenvolvimento e revisao do prompt)

- [ ] Repositorio de documentacao/prompts acessivel na workspace (este hub ou repo interno).
- [ ] Se usar MCP Jira no Cursor: configurado com as mesmas restricoes de dados da Parte A.1.
- [ ] Template de saida acordado: usar `artefatos/pt/02_PROMPT_ANALISE_RELEASE_V1.md`.

### A.8 Validacao “release exemplo” (criterio da Fase 1 do plano)

- [ ] Uma release ficticia ou antiga foi montada **em menos de 30 minutos** seguindo apenas a Parte B deste checklist.
- [ ] Registado **o que falhou** (campos em falta, APIs lentas, mapping task-PR) e acao corretiva.

---

## Parte B - Coleta por release (operacional, antes da analise IA)

Preencher para cada release. Substituir `<...>` por valores reais (sanitizados).

### B.1 Identidade da release

- [ ] Nome / versao da release: `<RELEASE_ID>`
- [ ] Data ou janela de go-live: `<JANELA>`
- [ ] Ambiente alvo: `<prod|stage|...>`
- [ ] Dono da release (contacto): `<NOME>`

### B.2 Lista de tasks

- [ ] Lista de keys (ex.: `ABC-123`, `ABC-124`): `<LISTA>`
- [ ] Export ou resumo **sem PII** das descricoes/criterios (link para Confluence/Jira export): `<ORIGEM>`
- [ ] Confirmacao: nenhum anexo/token foi colado no prompt bruto.

### B.3 Ligacao a mudancas de codigo

- [ ] Para cada task, PR(s) ou commits associados listados (ou “sem PR” justificado).
- [ ] Branch ou tag de release no Git: `<BRANCH_OR_TAG>`
- [ ] Diff agregado ou lista de ficheiros tocados disponivel para o analista (link Bitbucket/GitHub ou export).

### B.4 Contexto de pipeline / artefacto (opcional mas util)

- [ ] IDs ou links de pipeline da release: `<PIPELINE>`
- [ ] Versao de artefacto/imagem: `<ARTEFACTO>`
- [ ] Estado: verde / falhas conhecidas e waivers documentados.

### B.5 Contexto Datadog (opcional; ancora sugestoes de monitorizacao)

- [ ] Servicos impactados: `<SERVICOS>`
- [ ] Link para dashboard(s) atuais: `<URLS>`
- [ ] Monitores ou SLOs ja existentes que devem **nao** ser duplicados: `<NOTAS>`

### B.6 Entrega para analise

- [ ] Pacote reunido num unico sitio (ficheiro markdown interno, ticket “Release intel”, etc.).
- [ ] Executada analise com o prompt `02_PROMPT_ANALISE_RELEASE_V1.md` (Cursor e/ou n8n+LLM).
- [ ] Saida revista por humano antes de go/no-go.

---

## Parte C - Apos a analise (gate e observabilidade)

- [ ] **Go/No-Go** registado com nome e data.
- [ ] Itens “criar monitor/dashboard” foram **aprovados** ou recusados com motivo.
- [ ] Se aplicavel: IDs dos novos monitores/dashboards Datadog: `<IDS>`
- [ ] Pacote final anexado ao Release Manager / Jira conforme convencao A.3.

---

## Historico de revisoes do checklist

| Versao | Data | Autor | Notas |
|--------|------|-------|-------|
| 0.1 | 2026-05-14 | Hub ai-devops | Versao inicial (config + coleta + pos) |
