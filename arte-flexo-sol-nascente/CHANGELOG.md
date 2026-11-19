# Changelog: arte-flexo-sol-nascente

## v1.4.1 – 19/11/2026
- Conector Tiiny Host testado: ferramentas `list_sites`, `publish_site`, `update_site` e `delete_site` documentadas na skill.
- Antes de publicar, `list_sites` para checar nome repetido e o limite de projetos do plano.
- O conector não desativa downloads: o usuário liga "Disable PDF Downloads" no painel depois da publicação.

## v1.4.0 – 19/11/2026
- Nova etapa: publicar no tiiny.host só depois da aprovação do usuário, pelo conector Tiiny Host (sem chave de API na skill).
- Projeto com o PDF aprovado, downloads desativados e link no padrão `nomedaarte50x70v1` (título completo + formato + versão, minúsculas, sem espaços).
- Nova função `ficha.nome_link()` para montar o nome do link.

## v1.3.0 – 18/11/2026
- Nova regra para arte distribuída no corte aleatório: malha de motivos/logos centralizada na folha, com cortes simétricos (corte de cima = corte de baixo, esquerda = direita), mostrando ao cliente que as logos são cortadas nas bordas.
- Item de conferência atualizado para checar essa simetria.

## v1.2.0 – 18/11/2026
- Nova regra para logomarcas distribuídas na primeira versão, quando o pedido não informa: logo inclinada a 40° (diagonal), com ~40 mm na maior dimensão e ~35 mm de espaçamento entre logos (antes eram ~40 mm).
- Tamanho e espaçamento podem mudar um pouco para o passo fechar em 330 mm; os valores finais vão na entrega e nas cotas da ficha.

## v1.1.0 – 18/11/2026
- Fechamento em 330 mm passa a ser a prioridade: o passo é ajustado ao divisor de 330 mais próximo, mexendo o mínimo no motivo.
- O 420 mm só entra quando o 330 deixaria o padrão visivelmente diferente do pedido (referência: ajuste acima de ~5% no passo), com a justificativa na entrega.
- Exemplo de uso do `gerar()` trocado para `cilindro=330`.

## v1.0.0 – 18/11/2026
Primeira versão no repositório, com o estado atual da skill:
- Repetição que fecha nos cilindros de 330 e 420 mm; aviso de emenda em fundo contínuo; módulo de encaixe.
- Corte individual sem cilindro nem repetição (`sentido=False`, obs. "CORTE INDIVIDUAL SEM RETICULADO") – correção de 17/11/2026 (Steve Madden 70x30).
- Margem de 1 mm em cima e embaixo, fundo até a borda, cortado automaticamente por `gerar()` / `limpar_pdf()` – regra de 17/11/2026.
- Cortes na largura para telas com mais de uma folha (`cortes_h`, cotas à direita) – correção de 10/11/2026 (VIDE 24x24).
- Logomarca oficial da Sol Nascente embutida (`LOGO_SN`) e aplicada em toda ficha – regra de 18/11/2026.
- PDF de 2 páginas (ficha técnica + arte 1:1) compatível com CorelDRAW 2019; exportação SVG por cor.
