# Template - Rascunho de Postmortem

## Quando usar
Apos estabilizacao do incidente, para gerar rascunho de RCA com revisao humana.

## Prompt
```text
Crie um rascunho de postmortem tecnico e objetivo.

Entradas:
- incidente: <ID_INCIDENTE>
- servico/ambiente: <SERVICO_AMBIENTE>
- periodo do incidente: <INICIO_FIM>
- impacto observado: <IMPACTO>
- timeline de eventos: <TIMELINE>
- evidencias (logs/metricas/traces): <EVIDENCIAS>
- mudancas proximas ao incidente: <MUDANCAS>
- mitigacao aplicada: <MITIGACAO>

Formato obrigatorio:
1) Resumo executivo (5 bullets)
2) Impacto e escopo
3) Linha do tempo do incidente
4) Hipotese de causa raiz (e nivel de confianca)
5) Fatores contribuintes
6) O que funcionou bem
7) O que falhou
8) Acoes corretivas e preventivas (com prioridade)
9) Lacunas de observabilidade/dados

Regras:
- Se causa raiz nao for conclusiva, declarar explicitamente.
- Nao inventar evidencias nao fornecidas.
- Incluir somente recomendacoes manuais.
```
