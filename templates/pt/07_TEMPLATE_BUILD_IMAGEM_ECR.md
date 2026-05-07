# Template - Troubleshooting de Build de Imagem e ECR

## Quando usar
Quando a pipeline falhar em build/push/pull de imagem Docker no ECR.

## Prompt
```text
Atue como especialista de CI/CD para diagnostico de build de imagem e ECR.

Contexto:
- repositorio: <REPO>
- pipeline id: <PIPELINE_ID>
- etapa: <BUILD|PUSH|PULL>
- imagem/tag: <IMAGEM_TAG>
- registry/ECR: <ECR_URI>
- logs de erro: <LOGS_ERRO>
- ambiente: <AMBIENTE>

Tarefa:
Explicar causa provavel e listar verificacoes manuais.

Formato obrigatorio:
1) Diagnostico resumido
2) Possiveis causas (ordenadas)
3) Evidencias que suportam cada causa
4) Checklist manual de validacao:
   - autenticacao no ECR
   - permissao IAM
   - existencia de tag/repo
   - cache/camadas Docker
   - conectividade/rede
5) Correcao sugerida (manual)
6) Como prevenir recorrencia

Regras:
- Nao executar push/pull/delete automaticamente.
- Nao expor credenciais no output.
```
