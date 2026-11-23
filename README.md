# SkillsSN

Skills do Claude da **Sol Nascente Papéis**, versionadas.

| Skill | Versão | Para que serve |
|---|---|---|
| [`teamdesk-sol-nascente`](teamdesk-sol-nascente/SKILL.md) | ver [`VERSION`](teamdesk-sol-nascente/VERSION) | Consultar e alterar o TeamDesk da empresa (banco 72265) pela API REST: pedidos, clientes, produção, estoque, compras e financeiro. |
| [`arte-flexo-sol-nascente`](arte-flexo-sol-nascente/SKILL.md) | ver [`VERSION`](arte-flexo-sol-nascente/VERSION) | Criar, revisar e entregar artes de papel de seda em flexografia: repetição que fecha no cilindro (330/420 mm), cores Pantone Solid Coated e PDF no padrão da empresa (ficha técnica + folha) que abre no CorelDRAW 2019. |

## Estrutura

```
<skill>/
  SKILL.md      # a skill (fonte única: os scripts ficam embutidos nela)
  VERSION       # versão atual (MAJOR.MINOR.PATCH)
  CHANGELOG.md  # o que mudou em cada versão
```

## Versionamento

- **MAJOR**: muda o padrão de entrega (layout da ficha, nome do arquivo, formato do PDF).
- **MINOR**: regra nova ou função nova no `ficha.py`.
- **PATCH**: correção, ajuste de texto, cor Pantone nova no `PANTONE_LAB`.

Cada versão é um commit em `main` com a mensagem `<skill>: vX.Y.Z – resumo`. Para achar uma versão antiga, procure essa mensagem no histórico (ou crie um *Release* no GitHub a partir do commit, se quiser um ZIP fixo).

## Instalar no Claude

Compacte a pasta da skill (`arte-flexo-sol-nascente/`) em ZIP e envie em *Configurações → Capacidades → Skills*, ou use *Code → Download ZIP* no GitHub e envie só a pasta da skill.
