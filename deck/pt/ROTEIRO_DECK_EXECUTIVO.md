# Roteiro de Deck Executivo (8-10 Slides) - IA em DevOps

## Slide 1 - Titulo e Decisao Solicitada
- Transformacao DevOps Assistida por IA (Incidentes + CI/CD)
- Decisao solicitada: aprovar rollout em fases com operacao sob controle humano

## Slide 2 - Por que Agora
- Pressao de incidentes e complexidade de releases em crescimento
- Oportunidade de reduzir MTTA/MTTR e elevar qualidade de release
- Necessidade de padronizacao, governanca e resultado mensuravel

## Slide 3 - Objetivo Estrategico e Guardrails
- Objetivo de 90 dias: resposta a incidentes mais rapida e releases mais confiaveis
- Regra inegociavel: sem controle autonomo de infraestrutura por IA
- Papel da IA: analisar, resumir e recomendar; humanos aprovam e executam

## Slide 4 - Modelo Operacional com Ferramentas
- Datadog: deteccao, evidencias e validacao
- Cursor: triagem, hipoteses, handoff e rascunho de postmortem
- Bitbucket: fluxo auditavel de PR para qualquer mudanca
- Release Manager: governanca de release e gates de aprovacao

## Slide 5 - Fluxo Fim a Fim de Incidente
- Alerta -> coleta de contexto -> triagem por IA -> checklist humano
- Acao manual por runbook -> validacao -> postmortem
- Beneficio esperado: ciclo de decisao mais rapido e consistente

## Slide 6 - Plano de Acao de IA em CI/CD
- Assistencia na triagem de falhas de pipeline
- Briefing de risco antes de go/no-go de release
- Verificacao de prontidao de rollback
- Guardrails: sem auto-merge e sem deploy em producao acionado por IA

## Slide 7 - Programa de Risco Preditivo
- Score de risco por servico (baixo/medio/alto)
- Gatilhos de alerta antecipado por tendencia (erro, latencia, saturacao, backlog)
- Briefing semanal para acoes preventivas manuais

## Slide 8 - Governanca, Risco e Compliance
- Checklist obrigatorio de revisao humana
- Recomendacoes de alto impacto devem conter evidencias
- Auditoria de prompts/outputs e politica de sanitizacao de dados
- Processo de escalonamento para desvios de politica

## Slide 9 - Framework de KPIs e Metas
- Eficiencia: MTTA, MTTR, tempo ate primeira hipotese validada
- Qualidade: utilidade e precisao das sugestoes de IA
- Controle: violacoes de politica (meta: zero)
- Preditivo: recall, falso positivo e lead time

## Slide 10 - Roadmap e Pedido de Aprovacao
- Semanas 1-2: fundamentos e treinamento
- Semanas 3-5: piloto
- Semanas 6-8: integracao ao fluxo
- Semanas 9-12: escala com calibracao
- Pedido: confirmar donos, aprovar escopo do piloto e iniciar semana 1
