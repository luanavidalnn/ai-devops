# Plano de implantacao - Inteligencia de mudanca por release (Ponto 1)

## Objetivo

Implantar um processo repetivel em que, para cada **release** com **lista de tasks**, o time DevOps (e parceiros de engenharia) use **IA assistida** para:

1. **Correlacionar** descricao/intencao da task com **alteracoes reais** em PRs (codigo, config, migrations, flags).
2. **Antecipar** problemas plausiveis em producao (com **evidencias** e **confianca** explicitas).
3. **Produzir um pacote de observabilidade** para o periodo pos-release (metricas, logs, sintomas, checks manuais) — sem confundir isso com execucao autonoma de infraestrutura.

Este processo **nao depende** de um fluxo CI/CD como motor; pipelines podem ser **fonte opcional** de contexto (status de build, artefato, versao).

## Principios (nao negociaveis)

- **IA recomenda; humano decide.** Nenhum deploy, restart, scale, alteracao de monitor ou mudanca de infra disparada automaticamente pela IA.
- **Sem segredos e sem PII** nos prompts. Usar IDs, trechos minimos de codigo e descricoes sanitizadas.
- **Toda conclusao critica** amarra a **evidencia** (trecho de task, ficheiro, diff) ou aparece como **lacuna de dados**.
- **Monitoramento continuo** e responsabilidade das ferramentas de observabilidade (ex.: Datadog); a IA **define o que observar** e o time **materializa** alertas/dashboards ou revisao ativa na janela pos-release.

## Escopo

### Dentro do escopo

- Definicao de **entrada minima** por release (tasks + ligacao a PRs/commits).
- **Template de analise** (prompt + formato de saida) e checklist humano pre e pos-go-live.
- Piloto com **N releases** (meta sugerida: 3 a 5) e retro para ajustar o template.
- **Handoff** para observabilidade: lista priorizada de sinais e queries sugeridas (texto), anexada ao registro da release.

### Fora do escopo (fase inicial)

- Automacao completa via n8n ou webhooks (pode ser **Ponto 2** depois de validar o ritual manual).
- Scoring automatico de risco em ferramenta externa sem calibracao humana.
- Acesso da IA a dados de producao nao necessarios a analise (logs completos com PII, credenciais).

## Pre-requisitos

| Area | Minimo necessario |
|------|-------------------|
| Rastreabilidade | Tasks da release referenciam PRs ou branches, ou existe mapeamento manual revisado |
| Ferramenta de IA | Cursor (ou equivalente) com acesso ao repositorio e politica de uso aprovada |
| Governanca | Politica do hub em `policies/pt/` lida e aplicada pelo time |
| Observabilidade | Servicos impactados ja tem dashboards/monitores base (mesmo que simples) para ancorar sugestoes |

## Fases de implantacao

### Fase 0 - Alinhamento (meio dia a 1 dia)

**Entregas**

- Dono do processo no time (nome + substituto).
- Definicao de **"release"** no teu contexto (ferramenta, etiqueta, branch, versao).
- Lista de **campos obrigatorios** na task (ex.: risco declarado, plano de validacao, link PR).

**Criterio de pronto**

- [ ] Time concorda com entradas minimas e com o que e "pacote de observabilidade".

---

### Fase 1 - Entrada e registro (2 a 3 dias)

**Entregas**

- **Checklist de coleta e configuracao** (`release_change_intelligence/artefatos/pt/01_CHECKLIST_COLETA_RELEASE.md`).
- Convencao de como colar/exportar texto das tasks sem vazar dados sensiveis.

**Criterio de pronto**

- [ ] Uma release exemplo pode ser montada em menos de 30 minutos por quem faz o papel de Release Manager.

---

### Fase 2 - Template de analise v1 (3 a 5 dias)

**Entregas**

- Prompt aprovado + **formato fixo de saida** (secao por task: riscos, evidencias, confianca, lacunas, o que monitorar).
- Versao inicial: `release_change_intelligence/artefatos/pt/02_PROMPT_ANALISE_RELEASE_V1.md` (ver indice em `artefatos/README.md`).

**Criterio de pronto**

- [ ] Duas pessoas diferentes geram saidas comparaveis na estrutura (mesmas secoes), para a mesma release de teste.

---

### Fase 3 - Piloto controlado (2 a 4 semanas)

**Entregas**

- **N releases reais** com analise concluida antes do go-live.
- **Ficha pos-release** (1 pagina) por release: `release_change_intelligence/artefatos/pt/03_FICHA_POS_RELEASE.md` — o que foi observado na janela vs o que a IA antecipou.

**Criterio de pronto**

- [ ] Pelo menos **1 lacuna de dados** por release foi endereçada (melhoria de ligacao task-PR ou descricao).
- [ ] Pelo menos **1 sugestao de monitoramento** foi adotada ou explicitamente recusada com motivo (para calibrar o processo).

---

### Fase 4 - Consolidacao e KPIs leves (1 semana)

**Entregas**

- Versao **v2** do template com ajustes do piloto.
- Indicadores simples: tempo medio para montar o pacote; taxa de "alerta util" pos-release; numero de lacunas criticas.

**Criterio de pronto**

- [ ] Retro escrita com **3 aprendizados** e **2 mudancas** no processo ou no prompt.

---

## Papéis sugeridos

| Papel | Responsabilidade |
|-------|------------------|
| Release Manager (ou equivalente) | Garante lista de tasks + mapeamento a PRs; anexa pacote ao registro da release |
| DevOps / SRE | Traduz "o que monitorar" em monitores/dashboards ou revisao ativa na janela acordada |
| Engenharia | Completa tasks com contexto suficiente e liga PRs |
| Security (consulta) | Valida prompt e fluxo de dados em releases com risco alto |

## Riscos do proprio processo

| Risco | Mitigacao |
|-------|-----------|
| Falsa confianca na IA | Formato obrigatorio com confianca e lacunas; revisao humana no go/no-go |
| Vazamento de dados | Checklist de sanitizacao; nunca colar credenciais |
| Sobrecarga | Limite de tamanho de contexto por release; analise por servico se necessario |

## Proximos passos apos este Ponto 1 (opcional, outro programa)

- Automatizar **coleta** de metadados (API Jira + API Git) com **gate** de revisao humana antes de enviar ao modelo.
- Orquestracao leve (n8n) apenas para **notificacao** e arquivo do pacote, sem acoes em infra.

## Historico de versoes do plano

| Versao | Data | Notas |
|--------|------|-------|
| 0.1 | 2026-05-14 | Plano inicial no hub |
