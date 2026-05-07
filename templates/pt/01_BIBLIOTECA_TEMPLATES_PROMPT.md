# Biblioteca de Templates de Prompt (Aprovados)

## Objetivo
Disponibilizar templates padronizados para respostas consistentes em Incidentes e CI/CD.

## Guardrails obrigatorios (todos os prompts)
- IA assistiva apenas. Nao executar mudancas de infraestrutura.
- Nao usar segredo, token, credencial ou PII no prompt.
- Sempre declarar confianca e lacunas de dados.
- Sempre propor proximas checagens manuais.

## Biblioteca oficial
- `02_TEMPLATE_TRIAGEM_INCIDENTE.md`
- `03_TEMPLATE_HIPOTESES_COM_CONFIANCA.md`
- `04_TEMPLATE_HANDOFF_INCIDENTE.md`
- `05_TEMPLATE_POSTMORTEM_RASCUNHO.md`
- `06_TEMPLATE_FALHA_PIPELINE_CICD.md`
- `07_TEMPLATE_BUILD_IMAGEM_ECR.md`
- `08_TEMPLATE_BRIEFING_RISCO_SEMANAL.md`

## Contrato minimo de saida
Toda resposta deve trazer, nesta ordem:
1. Situacao atual
2. Impacto
3. Evidencias
4. Hipoteses/causa provavel (com confianca)
5. Lacunas de dados
6. Proximas checagens manuais
7. Risco da recomendacao

## Fluxo de aprovacao
1. Rascunho pelo time de plataforma.
2. Revisao de seguranca e operacoes.
3. Publicacao na biblioteca oficial.
4. Revalidacao quinzenal com ajuste de qualidade.

## Historico de revisao
- 2026-05-07: Biblioteca preenchida com templates operacionais.
