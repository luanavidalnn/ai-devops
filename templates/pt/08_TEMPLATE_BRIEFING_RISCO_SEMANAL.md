# Template - Briefing de Risco Semanal

## Quando usar
Ritual semanal de previsibilidade por servico (camada preditiva).

## Prompt
```text
Gere um briefing semanal de risco operacional para o servico abaixo.

Contexto:
- servico: <SERVICO>
- ambiente: <AMBIENTE>
- janela semanal: <PERIODO>
- sinais de incidente (Datadog): <SINAIS_OBSERVABILIDADE>
- sinais de entrega (CI/CD): <SINAIS_PIPELINE_RELEASE>
- mudancas relevantes da semana: <MUDANCAS>

Formato obrigatorio:
1) Nivel de risco da semana (baixo/medio/alto)
2) Principais drivers de risco
3) Padroes de alerta antecipado detectados
4) Acoes preventivas manuais recomendadas
5) Confianca da analise
6) Lacunas de dados
7) Itens para monitorar na proxima semana

Regras:
- Saida orientativa, sem acao autonoma.
- Citar evidencias para cada driver de risco.
- Quando houver baixa confianca, explicitar o motivo.
```
