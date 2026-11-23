# Changelog: teamdesk-sol-nascente

## v1.0.0 – 23/11/2026
Primeira versão:
- Acesso ao TeamDesk da Sol Nascente (banco 72265) pela API REST v2 com o script `teamdesk.py`: consultar (com paginação acima de 500 linhas), contar, buscar registro pela chave, baixar anexo, listar tabelas e colunas.
- Escrita (criar, atualizar, upsert, apagar) com prévia por padrão e gravação só com `confirmar=True` e pedido explícito do usuário.
- Regras de uso: tabelas de RH tratadas como sensíveis; colunas calculadas só leitura; dicas de fórmula de filtro testadas no banco real (aspas duplas, `IsNull`, `ToDate` para data e hora, `Days()`).
- Leituras testadas no banco real; escritas não testadas (para não alterar dados sem pedido).
