# Changelog: arte-flexo-sol-nascente

## v1.9.1 – 24/11/2026
- `repeticoes()`: novo parâmetro `limiar` (use 20 na tinta branca, que a prova simula em cinza claro). Roteiro explica como comparar à mão repetições giradas (logos a 180° no corte individual).

## v1.9.0 – 24/11/2026
- Verificação da prova ganha o passo 3b: todas as repetições de cada elemento têm que ser iguais entre si, na cor e no desenho. Um elemento que destoa dos outros (ex. faceta do diamante em dourado em vez de vermelho no RUBI 2026, pedido 23311) é falha: REPROVAR. A comparação com a nossa arte não pegava esse erro.
- `verificar_prova.py`: nova função `repeticoes()` (separa as cores da tinta, agrupa os elementos iguais pela forma, monta o padrão de cada grupo e aponta as repetições que fogem dele, com imagem padrão | repetição para olhar).

## v1.8.0 – 24/11/2026
- Verificação da prova passa a conferir também a `Arte Aprovada` (o que o cliente aprovou, em geral uma captura de tela da ficha no tiiny.host): a cadeia aprovada = vetorial = clicheria tem que bater; nossa arte diferente da aprovada (versão, elemento, medida ou cor) = REPROVAR.
- `verificar_prova.py`: novas funções `aprovada()` (captura em PDF/imagem ou PDF do padrão), `quadro_aprovada()` (acha o quadro da arte na captura pela proporção), `mascara_aprovada()`, `comparar_aprovada()` e `lado_a_lado()`.

## v1.7.6 – 24/11/2026
- Link publicado no tiiny.host passa a ser entregue sempre num bloco de código (só o link), com botão de copiar, para repassar ao vendedor.

## v1.7.5 – 24/11/2026
- Corrigido: a página 2 (só a arte) podia sair com cores diferentes da arte na ficha (página 1). O `fix_spots` pulava os recursos da página 2 porque usava `id()` do Python para não repetir; agora usa o objgen do PDF. Visto na Growfit 2026 30x40.
- Novo `conferir_cores()`: cada Pantone tem que ter a mesma definição em todas as páginas; o `gerar()` e o `limpar_pdf()` param com erro se divergir. Regra registrada no padrão de entrega e na conferência final.

## v1.7.4 – 24/11/2026
- E-mails da verificação de provas passam a ir sempre com cópia para nelly.medeiros@solnascentepapeis.com.

## v1.7.3 – 24/11/2026
- Rotina de verificação: um e-mail por pedido pelo Gmail para claudio@solnascentepapeis.com.br, com assunto "Arte: <referência> Sugestão: APROVAR/REPROVAR"; o e-mail automático da plataforma foi desligado.

## v1.7.2 – 24/11/2026
- Verificação da prova: sugestão passa a ser só APROVAR ou REPROVAR (confirmações vão na explicação) e cada pedido sai no formato "Pedido / Arte / Sugestão" seguido da explicação, também no e-mail da rotina.

## v1.7.1 – 23/11/2026
- Rotina agendada de verificação passa a rodar de hora em hora (8h às 18h, seg a sex): o mínimo das tarefas agendadas é 1 hora.

## v1.7.0 – 23/11/2026
- Novo roteiro "Verificar a prova da clicheria": compara a prova da clicheria (Arte Clicheria) com a nossa arte (Arte Vetorial, PDF ou CDR) e diz se pode aprovar o clichê (tamanho, cores, elementos, espelhamento, peso de traço e fonte, margem, emenda, quadro de informações).
- Novo script `verificar_prova.py` (baixa do TeamDesk, converte CDR, acha o quadro da prova, sobrepõe e mede). Validado nos pedidos 23496 (ok), 23492 (ok, com campos a confirmar) e 23337 (fonte refeita, não aprovar).
- Rotina agendada (seg a sex, 8h–18h, a cada 30 min) e registro das verificações no documento do Projeto `claude/verificacoes-clicheria.md`.

## v1.6.0 – 19/11/2026
- Toda arte gerada é gravada no Dropbox em `/SolNascente/Artes Clientes/<Nome da Arte>/` (pasta só com o nome da arte; criada se não existir), pelo novo script `dropbox_artes.py` (API HTTP do Dropbox; o conector não envia binários). Testado: criar pasta, gravar, usar pasta existente e bloquear substituição sem confirmação.
- Credenciais (tiiny.host e Dropbox) reunidas no documento privado do Projeto `claude/credenciais.md`.

## v1.5.0 – 19/11/2026
- Publicação no tiiny.host passa a ser pela API v3 com o novo script `publicar_tiiny.py` (embutido na skill): o PDF vai direto do disco, sem base64 na conversa. Testado com publicar, atualizar e apagar.
- Downloads desativados automaticamente (`disableExportPdf: true`), conferido após a publicação.
- O script checa nome repetido, limite de projetos e tamanho do arquivo antes de enviar.
- Chave de API fora do repositório: documento privado do Projeto "Artes SN" (`claude/tiiny-api-key.md`), lida para a variável `TIINY_API_KEY`.

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
