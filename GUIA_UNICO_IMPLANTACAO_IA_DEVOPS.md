# Guia Unico de Implantacao - IA no DevOps (Incidentes + CI/CD)

## 1) Objetivo deste guia
Este documento descreve, de forma pratica, como implantar IA no time de DevOps desde o zero, com foco em:
- resposta a incidentes,
- suporte ao ciclo de CI/CD,
- previsibilidade operacional,
- governanca e seguranca.

Principio central: IA assistiva.  
IA nao executa mudancas de infraestrutura em producao.

---

## 2) Escopo da implantacao
## O que a IA pode fazer
- Resumir contexto de incidentes.
- Sugerir hipoteses com base em evidencias.
- Gerar rascunho de handoff e postmortem.
- Apoiar triagem de falhas de pipeline.
- Gerar briefing de risco pre-release e risco preditivo semanal.

## O que a IA nao pode fazer
- Deploy/restart/scale/apply/delete automatico.
- Mudancas diretas em IaC/politicas sem aprovacao humana.
- Uso de segredos/PII em prompts.

---

## 3) Modelo operacional com ferramentas
## Datadog
- Detecta alertas e fornece evidencias (metricas, logs, traces, eventos).
- Valida estabilizacao apos acao manual.

## Cursor (agente)
- Processa contexto e gera analises usando templates aprovados.
- Entrega: triagem, hipoteses, handoff, postmortem, briefing preditivo.

## Bitbucket
- Registra mudancas via PR auditavel.
- Nenhuma mudanca sugerida por IA entra sem review humano.

## Release Manager
- Faz gate de release (go/no-go).
- Exige plano de rollback e aprovacoes humanas.

---

## 4) Pre-requisitos de governanca (antes do piloto)
1. Aprovar politica de uso de IA.
2. Definir checklist de revisao humana obrigatoria.
3. Definir dono da POC (DevOps Lead) e sponsor executivo.
4. Definir servicos piloto e cenarios recorrentes.
5. Definir metricas baseline (antes da IA).
6. Definir estrategia de ambientes: development -> staging -> production.
7. Garantir cluster development controlado com apps piloto (alguns ja monitorados no Datadog).

---

## 5) Configuracao por ferramenta
## 5.1 Datadog - configuracao minima
- Definir monitores dos sinais criticos por servico:
  - erro (5xx),
  - latencia (p95/p99),
  - saturacao (CPU/memoria/pool),
  - backlog/retry.
- Padronizar pacote de evidencias por incidente:
  - janela temporal,
  - graficos principais,
  - eventos de deploy/config,
  - logs sanitizados.
- Criar dashboard de POC com:
  - MTTA, MTTR, tempo de triagem,
  - KPIs preditivos (recall, falso positivo, lead time).

## 5.2 Cursor - configuracao minima
- Criar pasta de templates oficiais no repositorio de docs.
- Definir formato fixo de saida para cada prompt:
  - situacao,
  - impacto,
  - evidencia,
  - lacunas,
  - proxima checagem manual.
- Definir regras de uso:
  - sem prompt com segredo/PII,
  - sem instrucoes de execucao automatica,
  - obrigatorio explicitar nivel de confianca.

## 5.3 Bitbucket - configuracao minima
- Ativar branch protection.
- Exigir PR para mudancas de pipeline/runbook/processo.
- Bloquear auto-merge para mudancas sugeridas por IA.
- Usar template de PR com:
  - contexto,
  - evidencia,
  - risco,
  - rollback,
  - validacao.

## 5.4 Release Manager - configuracao minima
- Gate obrigatorio de release com:
  - aprovacao humana,
  - risco declarado,
  - rollback validado,
  - plano de monitoramento Datadog.
- Registrar decisao go/no-go com justificativa.

---

## 6) Como se comunicar com o agente (Cursor) no dia a dia
## Regras praticas para prompts
- Sempre dar contexto minimo:
  - servico,
  - ambiente,
  - alerta,
  - periodo,
  - sintomas,
  - evidencias disponiveis.
- Sempre pedir resposta estruturada.
- Sempre pedir citacao de evidencias e lacunas.

## Prompt exemplo - triagem de incidente
"Voce e assistente de triagem DevOps. Resuma este incidente em bullets, com: (1) situacao atual, (2) impacto, (3) evidencias principais, (4) 3 hipoteses com confianca, (5) proximas validacoes manuais. Nao sugira automacao de infraestrutura. Se faltar dado, declarar insuficiente."

## Prompt exemplo - falha de CI
"Analise esta falha de pipeline e retorne: causa provavel, evidencias dos logs, checks manuais recomendados e sugestoes de teste para evitar recorrencia. Nao executar nenhuma acao automaticamente."

## Prompt exemplo - briefing preditivo semanal
"Gere briefing de risco semanal do servico X com: nivel de risco, drivers, padroes de alerta antecipado, acoes preventivas manuais, confianca e lacunas de dados."

## Boas praticas de comunicacao com o agente
- Use prompts curtos e objetivos.
- Nao misture multiplos objetivos no mesmo prompt.
- Reforce o formato de saida esperado.
- Evite perguntas vagas (ex.: "o que fazer?") sem contexto operacional.

---

## 7) Fluxo operacional padrao (incidente)
1. Datadog dispara alerta.
2. On-call coleta evidencias sanitizadas.
3. Cursor gera triagem e hipoteses com confianca.
4. On-call aplica checklist de revisao humana.
5. IC/on-call decide acao manual via runbook.
6. Datadog valida recuperacao.
7. Cursor gera rascunho de handoff/postmortem.
8. Time revisa e fecha incidente.

Regra: sem execucao automatica por IA em qualquer etapa.

---

## 8) Fluxo operacional padrao (CI/CD)
1. Pipeline falha ou release tem risco elevado.
2. Cursor resume causa provavel e checks manuais.
3. Ajustes vao para PR no Bitbucket.
4. Release Manager aplica gate (risco, rollback, aprovacoes).
5. Release segue decisao humana (go/no-go).
6. Datadog acompanha pos-release.

---

## 9) Plano de implantacao em 4 semanas (POC)
## Semana 1 - Fundacao
- Publicar politica e checklist.
- Configurar templates no Cursor.
- Definir baseline de metricas.
- Escolher 1 servico critico + 1 medio.
- Preparar ambiente development (cluster controlado) para testes ponta a ponta.

## Semana 2 - Piloto controlado
- Rodar triagem assistida em incidentes alvo no environment development.
- Rodar mini-piloto CI (falha de pipeline) no environment development.
- Registrar utilidade da IA por caso.
- Definir criterios de promocao para staging.

## Semana 3 - Calibracao
- Promover para staging somente se os criterios de development forem atendidos.
- Revisar prompts, thresholds e runbooks em staging.
- Iniciar briefing preditivo semanal em staging.
- Corrigir lacunas de dados antes de qualquer uso em producao.

## Semana 4 - Avaliacao e decisao
- Comparar baseline vs POC.
- Consolidar riscos e ganhos.
- Decidir go/no-go para producao com gate formal.

## Criterios de promocao entre ambientes
- Development -> Staging:
  - Fluxos n8n e prompts estaveis por pelo menos 1 semana
  - Zero violacao de politica de dados/seguranca
  - Checklists humanos preenchidos em 100% dos casos piloto
- Staging -> Production:
  - Evidencia de melhoria operacional (tempo de triagem/MTTA)
  - Falso positivo preditivo dentro do limite acordado
  - Aprovacao conjunta de DevOps Lead + Release Manager + Security

---

## 10) RACI simplificado
- DevOps Lead: dono do programa e governanca.
- Incident Commander: decisao operacional em incidente.
- On-call Engineer: validacao de recomendacoes e execucao manual.
- Release Manager: gate de release e risco.
- Security: revisao de compliance de dados e politica.

---

## 11) KPIs obrigatorios
## Eficiencia
- MTTA
- MTTR
- tempo para primeira hipotese validada

## Qualidade
- taxa de sugestoes uteis
- precisao de hipoteses
- retrabalho

## Controle
- compliance de checklist humano
- violacoes de politica (meta: zero)

## Preditivo
- recall de alertas antecipados
- taxa de falso positivo
- lead time do alerta

---

## 12) Criterios de sucesso da POC
- Melhorar tempo de triagem em pelo menos 15%.
- Manter zero violacao critica de politica.
- Ter adesao do time no uso dos templates.
- Evidenciar ganho operacional sem aumento de risco.

---

## 13) Riscos comuns e mitigacao
- Risco: confiar cegamente na IA.
  - Mitigacao: checklist humano obrigatorio.
- Risco: prompt sem contexto gera baixa qualidade.
  - Mitigacao: templates padronizados.
- Risco: vazamento de dados sensiveis.
  - Mitigacao: sanitizacao obrigatoria + auditoria.
- Risco: excesso de falso positivo preditivo.
  - Mitigacao: calibracao quinzenal de thresholds.

---

## 14) Checklist de inicio rapido (primeiros 5 dias)
## Dia 1
- Nomear dono da POC e sponsor.
- Aprovar regra "IA nao executa infra".

## Dia 2
- Publicar politica e checklist.
- Definir servicos e cenarios piloto.

## Dia 3
- Configurar dashboards/monitores no Datadog.
- Publicar templates de prompt no Cursor.
- Confirmar que apps piloto no cluster development estao com observabilidade minima no Datadog.

## Dia 4
- Ativar template de PR no Bitbucket.
- Configurar gate no Release Manager.
- Configurar gates de promocao development -> staging -> production.

## Dia 5
- Rodar simulacao (game day) e ajustar processo.

---

## 15) Entregaveis minimos esperados
- Politica de IA (1 pagina).
- Checklist de revisao humana.
- Biblioteca de prompts aprovados.
- 2 runbooks com fluxo assistido por IA.
- Dashboard de KPIs da POC.
- Relatorio final com decisao go/no-go.

---

## 16) Trilhas de melhoria de processo (incrementais na POC)
Estas trilhas aproveitam padroes praticos de DevOps com IA, sem permitir execucao autonoma de infraestrutura.

## 16.1 Trilha Menos Retrabalho
- Padronizar templates de troubleshooting por dominio:
  - Docker
  - Kubernetes
  - Terraform (analise de plan/drift somente leitura)
  - CI/CD (falha de pipeline)
- Tornar obrigatorio que toda resposta da IA traga:
  - evidencias, confianca, lacunas e proximo passo manual
- Medir resolucao no primeiro ciclo de hipotese validada

## 16.2 Trilha Mais Padrao
- Um template oficial por tipo de cenario
- Contrato de resposta padrao para incidente e CI/CD
- Revisao quinzenal de qualidade de prompts e outputs
- Branch/PR padronizados apenas no fluxo de desenvolvimento

## 16.3 Trilha de Troubleshooting de Plataforma
- Docker e ECR: diagnostico de build de imagem na pipeline, validacao de tags/versoes e troubleshooting de push/pull no ECR
- Kubernetes: correlacao de eventos/logs e troubleshooting de health checks
- Terraform: explicacao de `plan` e identificacao de risco de drift
- Restricao: sem apply/mutacao automatica por IA

## 16.4 Trilha de Performance de CI/CD
- Bitbucket AI apenas para analise de falhas e tendencia de pipeline
- Incluir checks especificos de etapas de build de imagem (cache Docker, camadas, autenticacao no ECR e publicacao da imagem)
- Release Manager para gatilho automatico em staging apos gate aprovado
- Datadog para validacao de comportamento em staging
- Medir tempo de diagnostico de falha de pipeline

## 16.5 Trilha de Capacitacao do Time (60 min)
- 20 min: politica e guardrails
- 20 min: dois cenarios reais com templates
- 20 min: checklist humano e qualidade de decisao

## 16.6 KPIs adicionais da POC
- taxa de retrabalho evitado
- taxa de resolucao no primeiro ciclo de hipotese
- aderencia aos templates oficiais
- lead time de diagnostico de falha de pipeline

