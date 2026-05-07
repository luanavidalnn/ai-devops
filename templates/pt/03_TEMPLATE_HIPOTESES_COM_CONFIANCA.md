# Template - Geracao de Hipoteses com Confianca

## Quando usar
Apos triagem inicial, para priorizar linhas de investigacao.

## Prompt
```text
Atue como analista DevOps para geracao de hipoteses.

Contexto do incidente:
<CONTEXTO_INCIDENTE>

Evidencias coletadas:
<EVIDENCIAS>

Tarefa:
Gerar hipoteses de causa raiz, ordenadas por probabilidade.

Formato obrigatorio de resposta:
- Hipotese 1:
  - descricao
  - confianca (alta/media/baixa)
  - evidencias a favor
  - evidencias contra
  - check manual de validacao
- Hipotese 2: (mesmo formato)
- Hipotese 3: (mesmo formato)
- Recomendacao de ordem de investigacao
- Condicoes de escalonamento

Regras:
- Explicitar suposicoes.
- Nao propor acao autonoma em producao.
- Se dados forem insuficientes, listar exatamente quais dados faltam.
```
