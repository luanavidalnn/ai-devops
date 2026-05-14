# Release Change Intelligence (programa)

Este diretorio concentra **tudo o que for desenvolvido** para o processo de **inteligencia de mudanca por release**: lista de tasks, texto das tasks, alteracoes em PRs, analise de risco para producao e **pacote de observabilidade** (o que monitorar apos o go-live).

## Posicao no plano macro de IA no DevOps

Este programa e o **Ponto 1** da implantacao: comeca **sem orquestracao de pipeline** como premissa; o foco e **qualidade e previsibilidade da mudanca** antes e depois da liberacao.

O guia geral do hub referencia este ponto no inicio do passo a passo: `docs/guides/PASSO_A_PASSO_IMPLANTACAO_IA_DEVOPS_PTBR.md`.

## Conteudo

| Caminho | Descricao |
|--------|-----------|
| `pt/01_PLANO_IMPLANTACAO_INTELIGENCIA_MUDANCA_RELEASE.md` | Plano de implantacao (fonte principal PT) |
| `en/01_RELEASE_CHANGE_INTELLIGENCE_IMPLEMENTATION_PLAN.md` | Plano em ingles (espelho) |
| `docs/en/CONFLUENCE_RELEASE_CHANGE_INTELLIGENCE_POC.md` | Pagina EN para colar/publicar no Confluence |
| `artefatos/README.md` | Indice dos artefatos operacionais |
| `artefatos/pt/01_CHECKLIST_COLETA_RELEASE.md` | Checklist configuracao + coleta por release |
| `artefatos/pt/02_PROMPT_ANALISE_RELEASE_V1.md` | Prompt de analise de release (V1) |
| `artefatos/en/02_PROMPT_RELEASE_ANALYSIS_V1.md` | Prompt EN |
| `artefatos/pt/03_FICHA_POS_RELEASE.md` | Ficha pos-release (piloto) |
| `artefatos/en/03_POST_RELEASE_PILOT_NOTE.md` | Post-release note EN |
| `../data/jira/jira_release_change_intelligence_poc.csv` | Importacao Jira (Epic + issues POC) |

Nota: o caminho do CSV e relativo a esta pasta; no repositorio o ficheiro esta em `data/jira/jira_release_change_intelligence_poc.csv`.

- Documentacao formal do programa: subpastas `pt/` e `en/`.
- Artefatos operacionais (prompts, checklists): preferir `artefatos/pt/` e `artefatos/en/` quando houver traducao.

## Relacao com outras pastas do hub

- **Governanca**: `policies/` (sem PII/segredos em prompts; revisao humana).
- **Templates genericos** (reutilizar ou derivar): `templates/pt/` (briefing de risco, hipoteses com confianca).
- **Diagramas**: quando existir fluxo desenhado, preferir `diagrams/` com link a partir daqui.
