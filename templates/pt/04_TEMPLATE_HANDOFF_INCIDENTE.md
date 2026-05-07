# Template - Handoff de Incidente

## Quando usar
Na troca de turno, escalonamento ou transicao de ownership do incidente.

## Prompt
```text
Prepare um handoff objetivo de incidente para o proximo responsavel.

Dados de entrada:
- incidente: <ID_INCIDENTE>
- servico/ambiente: <SERVICO_AMBIENTE>
- status atual: <STATUS>
- impacto: <IMPACTO>
- timeline resumida: <TIMELINE>
- evidencias principais: <EVIDENCIAS>
- acoes ja executadas: <ACOES_EXECUTADAS>
- pendencias: <PENDENCIAS>

Formato obrigatorio:
1) Estado atual do incidente
2) O que ja foi validado
3) O que ainda nao foi validado
4) Riscos imediatos
5) Proximas 3 acoes manuais recomendadas
6) Dono sugerido por acao
7) Horario da proxima atualizacao

Regras:
- Linguagem objetiva em bullets.
- Nao incluir dados sensiveis.
- Nao propor automacao de infraestrutura.
```
