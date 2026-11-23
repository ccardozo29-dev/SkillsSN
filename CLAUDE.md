# Regras deste repositório

Este repositório guarda as skills da Sol Nascente Papéis. Sempre que uma skill for criada ou alterada:

1. Atualize o `SKILL.md` da pasta da skill (o conteúdo da skill é a fonte única; os scripts, como `ficha.py` e `teamdesk.py`, ficam embutidos nele e não são duplicados em outro arquivo).
2. Suba a versão em `<skill>/VERSION` seguindo o README (MAJOR / MINOR / PATCH).
3. Registre a mudança no topo de `<skill>/CHANGELOG.md`, com data (dd/mm/aaaa) e o que mudou em português.
4. Atualize a tabela do `README.md` se a descrição da skill mudou ou se entrou uma skill nova.
5. Faça commit com a mensagem `<skill>: vX.Y.Z – resumo` e envie para `main` (sem tags: o envio de tags é bloqueado nas sessões do Claude).
6. Nunca grave credenciais (tokens, chaves, segredos) em nenhum arquivo deste repositório: ele é público. As credenciais ficam no documento privado do Projeto no claude.ai.
