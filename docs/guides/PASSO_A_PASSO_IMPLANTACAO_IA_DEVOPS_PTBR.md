# Passo a Passo (Leigo) - Implantacao de IA no DevOps

## Para quem e este documento
Este guia e para quem **nunca implantou IA no time** e precisa de um roteiro simples, sem linguagem complicada.

Objetivo: colocar uma POC para funcionar com seguranca, em ordem:
1. Development
2. Staging
3. Production

Regra principal: **IA ajuda a analisar, mas nao executa mudancas de infraestrutura sozinha**.

---

## Visao simples do que voce vai montar
Voce vai ter 2 fluxos separados:

- **Fluxo A - Incidentes**
  - Datadog detecta problema
  - IA ajuda a analisar
  - Pessoa do time decide e executa manualmente

- **Fluxo B - CI/CD**
  - Pipeline falha
  - IA ajuda a encontrar causa
  - Release Manager decide e pode liberar staging automaticamente

Os dois fluxos alimentam uma camada de **previsibilidade** (risco semanal).

---

## Checklist rapido do que precisa antes
- [ ] Ter acesso admin (ou equivalente) em Datadog, Bitbucket, Release Manager e n8n
- [ ] Ter um cluster de **development** para testes controlados
- [ ] Ter pelo menos 1 app piloto com monitoramento no Datadog
- [ ] Definir um dono da POC (DevOps Lead)
- [ ] Definir um aprovador de seguranca (Security)

---

## Etapa 1 - Definir regras de seguranca e governanca (Dia 1)
### O que fazer
Criar regras claras de uso da IA.

### Onde configurar
- Documento interno (Confluence/Notion/Repo docs)

### O que precisa estar escrito
- IA nao executa deploy/restart/scale/apply/delete
- Nao pode enviar segredo/token/PII para prompt
- Toda recomendacao critica precisa de revisao humana
- Toda acao precisa de evidencias (logs/metricas/eventos)

### Como validar
- [ ] Politica aprovada por DevOps Lead e Security
- [ ] Time recebeu o documento

---

## Etapa 2 - Configurar Datadog (Dia 2 e 3)
### O que fazer
Preparar monitoramento para o fluxo de incidente.

### Onde configurar no Datadog
- **Monitors**: criar/ajustar alertas
- **Dashboards**: criar painel da POC
- **Logs/Traces**: garantir acesso por servico piloto

### Configuracoes minimas
- Alertas de erro (5xx)
- Alertas de latencia (p95/p99)
- Alertas de saturacao (CPU/memoria/pool)
- Alertas de backlog/retry

### Como validar
- [ ] Alertas disparam em simulacao
- [ ] Dashboard mostra dados dos apps piloto
- [ ] Links de logs e traces funcionam

---

## Etapa 3 - Configurar Bitbucket para CI/CD (Dia 3 e 4)
### O que fazer
Padronizar governanca de desenvolvimento e triagem de pipeline.

### Onde configurar no Bitbucket
- **Repository settings -> Branch restrictions**
- **Pull Request settings / templates**
- **Pipelines** (status e logs das execucoes)

### Configuracoes minimas
- Padrao de branch (exemplo: `feature/`, `fix/`, `hotfix/`)
- Padrao de PR (descricao, risco, plano de validacao)
- Guardar logs da pipeline para analise por IA

Importante:
- PR padrao e para **desenvolvimento**
- Nao usar PR como etapa obrigatoria para executar runbook de incidente

### Como validar
- [ ] Restricao de branch aplicada
- [ ] Template de PR funcionando
- [ ] Pipeline falha com log legivel para analise

---

## Etapa 4 - Configurar Release Manager (Dia 4)
### O que fazer
Criar gate de aprovacao e trigger automatico para staging.

### Onde configurar
- Fluxo de release no Release Manager

### Configuracoes minimas
- Gate com aprovacao humana
- Campo de risco (baixo/medio/alto)
- Confirmacao de rollback
- Integracao para trigger automatico em **staging** quando gate for aprovado

### Como validar
- [ ] Sem aprovacao, release nao avanca
- [ ] Com aprovacao, staging e acionado automaticamente
- [ ] Log de decisao go/no-go fica registrado

---

## Etapa 5 - Configurar Cursor (Dia 4 e 5)
### O que fazer
Padronizar como o time conversa com a IA.

### Onde configurar
- Pasta de documentacao com templates (no repositorio)
- Guia de prompts para o time

### Templates obrigatorios
- Triagem de incidente
- Hipoteses com confianca
- Handoff
- Postmortem
- Falha de pipeline
- Build de imagem/ECR na pipeline

### Regra de resposta
Toda resposta da IA deve ter:
- situacao
- impacto
- evidencia
- confianca
- lacunas
- proximo passo manual

### Como validar
- [ ] Time usa o mesmo template
- [ ] Saida da IA vem no formato padrao

---

## Etapa 6 - Configurar n8n (Semana 1)
### O que fazer
Criar orquestracao dos fluxos sem automacao perigosa.

### Onde configurar no n8n
- Workflows
- Credentials
- Webhooks
- Logs/auditoria

### Workflows minimos
1. `incident-triage-orchestrator`
2. `cicd-failure-triage-orchestrator`
3. `release-risk-briefing-gate`
4. `weekly-predictive-briefing`

### Regras obrigatorias no n8n
- Credenciais separadas por ambiente (dev/stage/prod)
- Permissao read-only por padrao
- No de aprovacao humana antes de qualquer impacto
- Nao ter permissao para deploy/restart/scale/apply/delete em producao

### Como validar
- [ ] Fluxo roda em development
- [ ] Aprovacao humana bloqueia e libera corretamente
- [ ] Tudo fica auditado (quem aprovou, quando, por que)

---

## Etapa 7 - Rodar POC em Development (Semana 2)
### O que fazer
Executar os fluxos em ambiente controlado.

### Escopo da POC
- 1 servico critico
- 1 servico medio
- Incidente recorrente + falha de pipeline

### Como operar
- Incidente: Datadog -> IA analisa -> humano decide -> acao manual
- CI/CD: pipeline falha -> IA analisa -> Release Manager decide -> staging

### Como validar
- [ ] Nao houve violacao de politica
- [ ] Checklist humano foi preenchido em todos os casos
- [ ] Tempo de triagem melhorou

---

## Etapa 8 - Promocao para Staging (Semana 3)
### So promover se cumprir tudo:
- [ ] Fluxos estaveis por 1 semana em development
- [ ] Zero violacao de seguranca/dados
- [ ] Evidencia de ganho operacional

### O que fazer em staging
- Repetir testes com carga mais real
- Calibrar alertas e thresholds
- Validar briefing preditivo semanal

---

## Etapa 9 - Go/No-Go para Producao (Semana 4)
### Decisao conjunta
DevOps Lead + Release Manager + Security.

### Criterios minimos
- [ ] Melhora de triagem (meta sugerida: >= 15%)
- [ ] Zero violacao critica
- [ ] Processo padronizado e repetivel

Se aprovado:
- liberar rollout gradual em producao
- continuar com revisao quinzenal de qualidade

---

## O que monitorar toda semana (KPIs)
- MTTA
- MTTR
- tempo de diagnostico de falha de pipeline
- taxa de falso positivo preditivo
- taxa de aderencia ao template
- taxa de resolucao no primeiro ciclo de hipotese

---

## Glossario rapido de siglas
- **POC**: Proof of Concept (prova de conceito)
- **KPI**: Key Performance Indicator (indicador-chave de desempenho)
- **MTTA**: Mean Time To Acknowledge (tempo medio para assumir/reconhecer o incidente)
- **MTTR**: Mean Time To Resolve (tempo medio para resolver o incidente)
- **CI/CD**: Continuous Integration / Continuous Delivery
- **PR**: Pull Request
- **ECR**: Elastic Container Registry (repositorio de imagens na AWS)
- **SLO**: Service Level Objective (meta interna de nivel de servico)
- **SLA**: Service Level Agreement (acordo formal de nivel de servico)
- **SLI**: Service Level Indicator (medida usada para acompanhar o nivel de servico)
- **RCA**: Root Cause Analysis (analise de causa raiz)

---

## Erros comuns (evite)
- Querer automatizar producao cedo demais
- Usar prompt livre sem padrao
- Nao auditar aprovacoes humanas
- Misturar fluxo de incidente com fluxo de desenvolvimento
- Subir para staging/prod sem criterios de promocao

---

## Roteiro de 5 dias (resumo)
### Dia 1
- Politica e guardrails

### Dia 2
- Monitores e dashboard no Datadog

### Dia 3
- Padrao de branch/PR e logs de pipeline no Bitbucket

### Dia 4
- Gate no Release Manager + trigger para staging

### Dia 5
- Templates no Cursor + simulacao completa (game day)

---

## Resultado esperado
Ao final da POC, voce tera:
- fluxo de incidente mais rapido e padronizado
- fluxo de CI/CD com analise melhor de falhas
- previsibilidade semanal de risco
- governanca forte para escalar com seguranca
