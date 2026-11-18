# Changelog: arte-flexo-sol-nascente

## v1.0.0 – 18/11/2026
Primeira versão no repositório, com o estado atual da skill:
- Repetição que fecha nos cilindros de 330 e 420 mm; aviso de emenda em fundo contínuo; módulo de encaixe.
- Corte individual sem cilindro nem repetição (`sentido=False`, obs. "CORTE INDIVIDUAL SEM RETICULADO") – correção de 17/11/2026 (Steve Madden 70x30).
- Margem de 1 mm em cima e embaixo, fundo até a borda, cortado automaticamente por `gerar()` / `limpar_pdf()` – regra de 17/11/2026.
- Cortes na largura para telas com mais de uma folha (`cortes_h`, cotas à direita) – correção de 10/11/2026 (VIDE 24x24).
- Logomarca oficial da Sol Nascente embutida (`LOGO_SN`) e aplicada em toda ficha – regra de 18/11/2026.
- PDF de 2 páginas (ficha técnica + arte 1:1) compatível com CorelDRAW 2019; exportação SVG por cor.
