# Resumo Executivo - POC IA para DevOps (1 pagina)

## Objetivo da POC
Validar, em 4 semanas, se o uso de IA no DevOps melhora a velocidade e a qualidade operacional sem aumentar risco.

Metas principais:
- reduzir tempo de triagem de incidentes;
- melhorar diagnostico de falhas de pipeline;
- criar previsibilidade semanal de risco por servico.

Regra inegociavel:
- IA **nao executa** mudancas de infraestrutura em producao.

---

## Escopo simples (o que entra)
### Fluxo 1 - Incidentes
- Datadog detecta alerta.
- IA ajuda a resumir contexto e sugerir hipoteses.
- Time valida e executa acao manual via runbook.

### Fluxo 2 - CI/CD
- Pipeline falha.
- IA analisa logs e sugere checks manuais.
- Release Manager aplica gate e pode acionar staging automaticamente.

### Camada comum - Previsibilidade
- Sinais dos 2 fluxos alimentam briefing semanal de risco.

---

## O que nao entra
- deploy automatico por IA;
- restart/scale/apply/delete automatico;
- uso de segredo/PII em prompts.

---

## Ferramentas e papel de cada uma
- **Datadog:** alerta, evidencias e validacao de recuperacao.
- **Bitbucket:** padrao de branch/PR (dev) e analise de falha de pipeline.
- **Release Manager:** gate de risco/aprovacao e trigger para staging.
- **Cursor:** suporte de analise e padronizacao de resposta.
- **n8n:** orquestracao dos fluxos e trilha de auditoria.

---

## Plano rapido (4 semanas)
- **Semana 1:** governanca, templates, configuracao das ferramentas, baseline.
- **Semana 2:** piloto em development (incidente + pipeline failure).
- **Semana 3:** calibracao e promocao para staging (se criterios atendidos).
- **Semana 4:** avaliacao final e decisao Go/No-Go para producao.

---

## Criterios de sucesso
- melhoria de triagem (meta sugerida: >= 15%);
- zero violacao critica de seguranca/politica;
- checklist humano aplicado em 100% dos casos piloto;
- ganho claro no diagnostico de falha de pipeline.

---

## KPIs da POC
- MTTA e MTTR;
- tempo de diagnostico de falha de pipeline;
- taxa de falso positivo preditivo;
- taxa de aderencia a templates;
- taxa de resolucao no primeiro ciclo de hipotese.

---

## Glossario rapido de siglas
- **POC**: Proof of Concept (prova de conceito)
- **KPI**: Key Performance Indicator (indicador-chave de desempenho)
- **MTTA**: Mean Time To Acknowledge (tempo medio para assumir/reconhecer um incidente)
- **MTTR**: Mean Time To Resolve (tempo medio para resolver um incidente)
- **CI/CD**: Continuous Integration / Continuous Delivery
- **PR**: Pull Request
- **ECR**: Elastic Container Registry (repositorio de imagens de container na AWS)
- **SLO**: Service Level Objective (meta interna de nivel de servico)
- **SLA**: Service Level Agreement (acordo formal de nivel de servico)
- **SLI**: Service Level Indicator (medida usada para acompanhar o nivel de servico)
- **RCA**: Root Cause Analysis (analise de causa raiz)

---

## Dependencias para iniciar
- dono da POC definido (DevOps Lead);
- app piloto em development com Datadog ativo;
- acesso configurado em Datadog, Bitbucket, Release Manager, Cursor e n8n;
- politica de uso de IA aprovada.

---

## Texto curto para descricao da Epic no Jira
Esta POC implementa IA assistiva no DevOps com dois fluxos independentes (Incidentes e CI/CD), mais uma camada de previsibilidade semanal. O rollout sera feito em development -> staging -> production, com gate humano obrigatorio e proibicao de automacao autonoma em infraestrutura. O objetivo e reduzir tempo de triagem e melhorar qualidade de decisao operacional com seguranca e auditoria.
