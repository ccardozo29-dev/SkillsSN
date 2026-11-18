# SkillsSN

Skills do Claude da **Sol Nascente Papéis**, versionadas.

| Skill | Versão | Para que serve |
|---|---|---|
| [`arte-flexo-sol-nascente`](arte-flexo-sol-nascente/SKILL.md) | ver [`VERSION`](arte-flexo-sol-nascente/VERSION) | Criar, revisar e entregar artes de papel de seda em flexografia: repetição que fecha no cilindro (330/420 mm), cores Pantone Solid Coated e PDF no padrão da empresa (ficha técnica + folha) que abre no CorelDRAW 2019. |

## Estrutura

```
<skill>/
  SKILL.md      # a skill (fonte única: o ficha.py está embutido nela)
  VERSION       # versão atual (MAJOR.MINOR.PATCH)
  CHANGELOG.md  # o que mudou em cada versão
```

## Versionamento

- **MAJOR**: muda o padrão de entrega (layout da ficha, nome do arquivo, formato do PDF).
- **MINOR**: regra nova ou função nova no `ficha.py`.
- **PATCH**: correção, ajuste de texto, cor Pantone nova no `PANTONE_LAB`.

Cada versão publicada recebe uma tag `<skill>/vX.Y.Z` (ex.: `arte-flexo-sol-nascente/v1.0.0`).

## Instalar no Claude

Compacte a pasta da skill (`arte-flexo-sol-nascente/`) em ZIP e envie em *Configurações → Capacidades → Skills*, ou baixe o ZIP da tag pelo GitHub.
