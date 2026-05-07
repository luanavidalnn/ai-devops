# Runbook de Triagem de Incidente

## Gatilho
Use este runbook quando um incidente de producao ou staging for aberto.

## Passo a Passo
1. Confirmar escopo, severidade e partes interessadas.
2. Coletar evidencias: logs, metricas, traces e deploys recentes.
3. Usar template de prompt aprovado para gerar hipoteses.
4. Validar as principais hipoteses com checagens diretas.
5. Aplicar mitigacao segura ou escalar.
6. Comunicar status e proximo horario de atualizacao.

## Pontos de Decisao
- O impacto ao usuario continua?
- Existe evidencia suficiente para agir?
- Rollback e mais seguro do que corrigir adiante?

## Escalonamento
Escalar para lider de plataforma e dono do servico quando o impacto for alto ou houver incerteza.

## Encerramento
- Confirmar estabilidade do servico.
- Registrar resumo da causa raiz.
- Abrir acoes de follow-up.

## Historico de Revisao
- 2026-05-07: Template inicial de runbook.
