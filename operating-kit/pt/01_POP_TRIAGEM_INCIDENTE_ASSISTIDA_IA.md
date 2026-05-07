# POP - Triagem de Incidente Assistida por IA

## Objetivo
Definir um processo repetivel para triagem de incidentes com apoio de IA, mantendo responsabilidade humana.

## Escopo
- Operacao de plantao
- Triagem e primeira resposta a incidentes
- Incidentes em producao e staging

## Entradas
- Fonte do alerta e horario
- Servico e ambiente impactados
- Logs, metricas e traces iniciais
- Deploy atual e mudancas recentes

## Procedimento
1. Confirmar severidade e impacto no negocio.
2. Coletar evidencias nas fontes aprovadas.
3. Usar template de prompt aprovado para gerar hipoteses.
4. Validar hipoteses com evidencias reais.
5. Definir proxima acao e responsavel.
6. Registrar decisoes e justificativas na linha do tempo do incidente.

## Checklist de Revisao Humana
- Evidencias explicitas e com referencia.
- Suposicoes identificadas.
- Restricoes de seguranca e compliance respeitadas.
- Acoes propostas seguras e reversiveis.

## Saida
- Hipoteses priorizadas
- Plano de acao com responsavel e prazo
- Decisao de escalonamento, se necessario

## Historico de Revisao
- 2026-05-07: Template inicial.
