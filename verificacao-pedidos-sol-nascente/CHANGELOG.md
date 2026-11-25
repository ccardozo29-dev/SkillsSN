# Changelog: verificacao-pedidos-sol-nascente

## v1.0.0 – 25/11/2026
Primeira versão:
- Verificação 1 (OC): compara a ordem de compra anexada no campo `Anexar OC` com o pedido: CNPJ do cliente e empresa fornecedora, prazo (× `Prazo Cliente`), itens (casamento por conteúdo, quantidade, unidade de medida, preço unitário e total), endereços, condição de pagamento e frete CIF/FOB.
- Script `verifica_pedido.py` (somente leitura): localiza o pedido pelo número curto, lê todas as colunas de Pedido, Itens e Cliente, baixa a OC e extrai o texto (ou gera imagens das páginas quando a OC é escaneada).
- Resposta padronizada: veredito, tabela de conferência, tabela de itens e lista do que corrigir.
