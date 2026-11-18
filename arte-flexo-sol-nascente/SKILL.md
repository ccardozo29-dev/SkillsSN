---
name: "arte-flexo-sol-nascente"
description: "Use ao criar, revisar ou entregar artes de papel de seda em flexografia para a Sol Nascente Papéis: repetição que fecha no cilindro (330/420 mm), cores Pantone Solid Coated e PDF no padrão da empresa (ficha técnica + folha)."
---

# Artes flexo: Sol Nascente Papéis

## Como a gráfica trabalha
- Papel de seda (também kraft e barreira) impresso em flexografia com clichê, cores Pantone chapadas, sem retícula. A bobina tem corte aleatório: o tamanho da folha não define a arte, a estampa precisa ser contínua.
- Cilindros de 330 mm e 420 mm. No sentido da máquina, o passo da repetição tem que dividir o cilindro exatamente (330: 330, 165, 110, 82,5, 66, 55...; 420: 420, 210, 140, 105, 84, 70...). Na largura não há amarração.
- **Prioridade: fechamento em 330 mm** (regra do usuário, 18/11/2026). Comece sempre pelo 330: ajuste o passo ao divisor de 330 mais próximo do pedido, mexendo o mínimo no tamanho ou no espaçamento do motivo. Use o 420 só quando for necessário, ou seja, quando fechar em 330 deixaria o padrão visivelmente diferente do que foi pedido e o 420 fica bem mais perto. Como referência, um ajuste de até uns 5% no passo é aceitável no 330. Exemplo: um passo pedido de 140 mm cairia em 165 ou 110 no 330 (cerca de 18% a 21% de diferença), mas fecha exato em 420, então vai para o 420. Sempre que usar o 420, diga na entrega por que o 330 não serviu (o passo pedido e o passo que ficaria no 330).
- Corte individual (quando o pedido diz "corte individual"): todas as folhas saem exatamente iguais. Não existe repetição nem cilindro nessa arte, nem 330 nem 420: a arte é a própria folha, no formato pedido. Na ficha: `sentido=False` (sem "Sentido da repetição"), nenhuma cota de passo ou de repetição e Observações só "CORTE INDIVIDUAL SEM RETICULADO" (`obs=['CORTE INDIVIDUAL SEM RETICULADO']`). A margem de 1 mm vale igual. Supor cilindro num corte individual foi um erro corrigido pelo usuário na Steve Madden 70x30 (17/11/2026).
- O clichê é enrolado no cilindro e na emenda fica um pequeno espaço entre as artes. Isso é normal. Se o fundo for contínuo (grade, listras, chapado), avisar logo no início que a emenda vai aparecer, para a produção e o cliente saberem.
- Módulo de encaixe: quando pedirem, a emenda é "torta", passa só pelo fundo e fica a pelo menos 2,5 mm de qualquer desenho. Entregar só o módulo, para colar lado a lado no Corel (Etapa e repetição com deslocamento igual ao passo).
- Usam CorelDRAW 2019. O Corel 2019 abre o PDF gerado pelo `ficha.py` (testado com o usuário): PDF 1.7, textos em curvas (sem fontes), sem XObject, sem camadas, cores especiais com função tipo 2. PDF cru do reportlab dá "formato de arquivo inválido": nunca entregar PDF sem passar por `gerar()` ou `limpar_pdf()`.
- Não existe como gerar .CDR fora do Corel. Se pedirem CDR, mandar o PDF limpo (ou SVG) e o usuário salva como CDR no Corel.
- SVG também abre no Corel (bom para montagem): `pdf_para_svg()` gera um grupo por cor, com o nome da Pantone; as cores chegam em RGB e o usuário aplica a Pantone em cada grupo.

## Regras de arte
- Só cores especiais com o nome exato da cartela PANTONE Solid Coated ("PANTONE 7426 C"). Preto é preto puro e aparece na ficha como "Pantone PRETO". Nada de CMYK/RGB na arte; retícula vira chapado.
- Traço ou vazado abaixo de 0,15 mm e microtexto abaixo de uns 2 mm de altura são arriscados em papel seda: avisar.
- Conferir sempre: passo x cilindro (330 primeiro; se ficou em 420, a justificativa está na entrega), motivos cortados na borda aparecem pela metade dos dois lados (malha centralizada: corte de cima = corte de baixo, esquerda = direita), nada de desenho cortado na emenda. No corte individual não há passo nem emenda: conferir o formato e a margem.
- Margem de 1 mm na largura (regra do usuário, 17/11/2026). No formato final, dentro do quadro (C x L):
  - a arte fica centralizada no quadro e termina a 1 mm da borda de cima e a 1 mm da borda de baixo. O desenho que passa dessas linhas é cortado reto nelas (corte de verdade na curva: não some nem muda de lugar);
  - nas laterais (esquerda e direita, sentido da máquina) não tem margem: a arte vai até a borda e é cortada nela;
  - o fundo (cor chapada, ou o papel simulado) não tem margem: preenche 100% do quadro, inclusive essa faixa de 1 mm;
  - vale para a página 2, para a arte dentro da ficha e para o módulo do clichê. Os cortes internos da tela (ex.: as folhas 15x15 dentro do 30x30) não ganham margem;
  - referências do usuário, feitas no Corel: Van-Dal 30x70 v1 e Nacho Man 2026 30x30 e 15x15 v1. Nas duas, a tinta vai de 1,00 a L − 1,00 mm, encosta nas laterais, o fundo kraft cobre o quadro todo e o centro da tinta cai no meio da folha;
  - o `gerar()` e o `limpar_pdf()` fazem esse corte sozinhos (`margem=1.0`). O fundo é reconhecido por cobrir o quadro inteiro, então ele tem que ser um retângulo (ou imagem) do tamanho da arte.
- Logomarcas distribuídas (estampa com a logo do cliente repetida), na primeira versão de uma arte e quando o pedido não diz a orientação, tamanho ou espaçamento (regra do usuário, 18/11/2026):
  - **orientação:** a logo inclinada a **40°**, subindo da esquerda para a direita, para a estampa ficar na diagonal. Se o pedido ou o cliente disser a orientação (reta, vertical, outro ângulo), vale o que foi pedido;
  - **tamanho:** a logo com uns **40 mm** na maior dimensão (sem a inclinação);
  - **espaçamento:** uns **35 mm** de borda a borda entre uma logo e a próxima, um pouco menor que os ~40 mm usados até agora;
  - esses valores são o ponto de partida: tamanho e espaçamento podem mudar um pouco para o passo fechar em 330 mm (ver a prioridade do cilindro). Diga na entrega os valores que ficaram (ângulo, tamanho da logo, espaçamento e passo) e cote o tamanho da logo e o espaçamento na ficha;
  - a partir da v2, siga o que o cliente aprovou ou pediu na versão anterior.
- Arte distribuída no corte aleatório: centralizada e simétrica (regra do usuário, 18/11/2026). A estampa preenche a folha inteira, com a malha de motivos/logos centralizada no quadro (C x L):
  - se um motivo é cortado na borda de cima, o corte correspondente aparece na borda de baixo, na mesma medida; o mesmo vale para a esquerda e a direita (começo e fim da folha, sentido da máquina);
  - o centro da malha cai no centro da folha. Não começar a estampa encostada num canto, com motivos inteiros de um lado e cortados só do outro;
  - em cima e embaixo, o corte é na linha da margem de 1 mm (ver "Margem de 1 mm na largura"), então a simetria é contada a partir dessas linhas;
  - isso mostra ao cliente que, no corte aleatório, as logos são cortadas nas bordas. Na entrega, conferir visualmente a simetria (corte de cima = corte de baixo, esquerda = direita);
  - se o pedido definir outra posição para a estampa, vale o pedido.
- Arte a partir de imagem: se o motivo se repete na imagem, alinhar as cópias com precisão subpixel e tirar a média (fica mais nítido); separar as cores por desmistura linear (branco + tintas); contornar com marching squares e ajustar curvas Bézier (Schneider) com detecção de cantos. Conferir o desvio sobrepondo ao original e mostrar uma comparação ampliada.

## Padrão de entrega: PDF de 2 páginas
- Nome: `cliente_LxC_vN.pdf` (minúsculas, sem acento, com `_`). LxC em cm: L = largura (atravessa a máquina), C = comprimento (sentido da repetição). Ex.: `nico_e_pippa_30x70_v1.pdf`.
- Página 2: só a arte, 1:1, no tamanho C x L da folha. Folha curta (ex. 50x35) mostra duas folhas (700 mm) e a ficha ganha a linha de corte vermelha tracejada. Vetorial, só com as cores da arte. A arte termina a 1 mm das bordas de cima e de baixo; o fundo vai até a borda (ver Regras de arte).
- Tela com mais de uma folha na largura (ex.: folha 24x24 numa tela de 50x48, que é L 500 x C 480, ou seja, 2 x 2 folhas): marcar também os cortes na largura, com linhas vermelhas tracejadas de ponta a ponta da arte. As pistas ficam centradas e a sobra vira refile igual nas duas bordas: 500 - 2 x 240 = 20, então os cortes ficam em 10, 250 e 490 mm. À direita da arte entram as cotas verticais (36 pt, a 27,89 mm da borda): da borda de cima até o corte do meio (250,00 mm) e a pista de baixo (240,00 mm). No `gerar()`: `cortes=[240]` (comprimento) e `cortes_h=[10, 250, 490]` (largura); as cotas da direita saem sozinhas. Esquecer os cortes na largura foi um erro corrigido pelo usuário na VIDE 24x24 (10/11/2026).
- Página 1: ficha técnica, gerada pelo `ficha.py` abaixo, que reproduz o modelo do Corel com as medidas originais:
  - Largura da página max(800, C+200) mm (700x500 vira 1000) e altura L+300 mm. Bloco de 700 mm centralizado.
  - Cabeçalho 700x70 mm: logomarca oficial da Sol Nascente Papéis SEMPRE no quadrado da esquerda (célula de 130 x 70 mm; regra do usuário, 18/11/2026; vem embutida no `ficha.py` como `LOGO_SN`), "Arte: NOME LxC" (Arial Bold 48 pt), "Data: dd/mm/aaaa" e "Versão: 0N" (Arial 36 pt, à direita).
  - Arte 1:1 com o topo a 150 mm e moldura preta de 0,2 mm. Cotas em azul RGB(51,116,169): comprimento em cima e largura à esquerda (36 pt), o rótulo "Largura", "Sentido da repetição" com a seta, e cotas internas (tamanho do motivo, espaçamento ou passo; 12 a 24 pt).
  - Quadro inferior 700x70 mm: Substrato, Cores (arte) com amostras, Liso, Fundo (chapado) e Observações ("CILINDRO COM FECHAMENTO 330MM" ou "420MM" e "CORTE ALEATÓRIO SEM RETICULADO").
  - Cores especiais com alternativa Lab D50 e função tipo 2, como o Corel exporta; sem sobreimpressão.
  - Textos convertidos em curvas (contornos da Liberation Sans, que tem as mesmas medidas da Arial).

## Como gerar
1. Montar a arte como PDF de uma página em 1:1 (C x L mm) com cores especiais (reportlab `CMYKColorSep(..., spotName='PANTONE XXXX C')`). Fundo chapado: um retângulo do quadro inteiro. A estampa pode encostar nas bordas: o `gerar()` corta a arte a 1 mm em cima e embaixo e deixa o fundo inteiro.
2. A logomarca da Sol Nascente já está embutida no `ficha.py` (`LOGO_SN`, vetorial, cores da marca) e o `gerar()` a coloca sozinho no quadrado da esquerda do cabeçalho: não passar `modelo` e não pedir ficha nem logo ao usuário. Toda ficha entregue tem que sair com essa logo. `carregar_modelo('<ficha>.pdf')` só se o usuário pedir para usar a logo de uma ficha específica.
3. Salvar o código abaixo como `ficha.py` (usa `reportlab`, `pikepdf` e a fonte Liberation Sans, que tem as mesmas medidas da Arial) e rodar, por exemplo:
```python
import ficha
ficha.gerar('arte.pdf', ficha.nome_arquivo('Nico e Pippa', '30x70', 2),
            nome='Nico e Pippa', formato='30x70', versao=2,
            substrato='Papel Seda 20grm', cores=['PANTONE 7426 C', 'PANTONE 578 C'],
            cilindro=330,                       # 330 é a prioridade; 420 só se necessário. Logo entra sozinha
            cotas=[dict(p1=(20, 40), p2=(230, 40), off=12, size=18, eixo='h')])  # mm na arte
```
4. Cor Pantone nova: buscar o Lab D50 (Solid Coated) e passar `lab_extra={'PANTONE XXXX C': (L, a, b)}`. Sem isso a separação sai certa, mas a amostra na tela fica preta. Ao abrir o PDF, o Corel troca o Lab pelo da paleta dele: se o usuário devolver um PDF exportado do Corel, copiar o Lab da Separation para o `PANTONE_LAB` (foi assim com a 172 C).
5. Arte ou módulo avulso (sem ficha): `ficha.limpar_pdf('entrada.pdf', 'saida.pdf')` regrava na estrutura que o Corel abre e já corta a margem de 1 mm (`margem=0` para não cortar). SVG por cor: `ficha.pdf_para_svg('saida.pdf', 0, 'saida.svg')`.
6. Antes de entregar: renderizar as duas páginas e conferir medidas, nomes das cores, passo x cilindro e emenda; `pdffonts` deve sair vazio; `ficha.conferir_margem('saida.pdf')` tem que dar cima e baixo de 1,00 mm ou mais (`ok=True`); conferir na página 1 que a logomarca da Sol Nascente aparece no quadrado da esquerda do cabeçalho. Entregar o PDF (e o SVG por cor, se for montar no Corel).

## ficha.py
```python
"""Gerador de PDF no padrão Sol Nascente Papéis.

Página 1: ficha técnica (cabeçalho com logo, arte em escala 1:1 com cotas em mm,
'Largura', 'Sentido da repetição' e quadro Substrato / Cores / Fundo / Observações).
Página 2: a arte inteira, 1:1, só com as cores da arte, a 1 mm das bordas de cima e de baixo
(o fundo vai até a borda).

Medidas do modelo tiradas das fichas originais (CorelDRAW 2019).
Coordenadas internas em mm, origem no canto superior esquerdo.

Saída no formato que o CorelDRAW 2019 abre (testado): PDF 1.7, textos em curvas
(sem fontes), sem XObject, sem camadas, sem /Trans, cores especiais Separation com
alternativa Lab D50 e função tipo 2. PDF direto do reportlab dá "formato de arquivo
inválido" no Corel 2019: sempre passar por gerar() ou limpar_pdf().
"""
import datetime, math, os, re, unicodedata
from decimal import Decimal
import pikepdf
from reportlab.pdfgen import canvas
from reportlab.pdfgen.canvas import FILL_EVEN_ODD, FILL_NON_ZERO
from reportlab.lib.units import mm
from reportlab.lib.colors import CMYKColorSep, Color
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont


FONT_DIR = '/usr/share/fonts/truetype/liberation'
pdfmetrics.registerFont(TTFont('Arial', os.path.join(FONT_DIR, 'LiberationSans-Regular.ttf')))
pdfmetrics.registerFont(TTFont('Arial-Bold', os.path.join(FONT_DIR, 'LiberationSans-Bold.ttf')))

from fontTools.ttLib import TTFont as _FTFont
from fontTools.pens.basePen import BasePen as _BasePen

TEXTO_EM_CURVAS = True     # o Corel importa sem depender de fonte
_FT = {'Arial': _FTFont(os.path.join(FONT_DIR, 'LiberationSans-Regular.ttf')),
       'Arial-Bold': _FTFont(os.path.join(FONT_DIR, 'LiberationSans-Bold.ttf'))}


class _PenRL(_BasePen):
    def __init__(self, gs, path, esc, ox):
        super().__init__(gs); self.p, self.e, self.ox = path, esc, ox
    def _t(self, q): return (self.ox + q[0] * self.e, q[1] * self.e)
    def _moveTo(self, q): self.p.moveTo(*self._t(q))
    def _lineTo(self, q): self.p.lineTo(*self._t(q))
    def _curveToOne(self, a, b, c): self.p.curveTo(*self._t(a), *self._t(b), *self._t(c))
    def _closePath(self): self.p.close()
    def _endPath(self): self.p.close()


AZUL = Color(0.2, 0.4549, 0.6627)          # cotas e rótulos
PRETO = Color(0, 0, 0)
VERMELHO = Color(1, 0, 0)                  # linha de corte
MARGEM_LARGURA = 1.0                       # mm sem tinta da arte em cima e embaixo do quadro (o fundo vai até a borda)

# Lab (D50) das cores Pantone Solid Coated usadas pela Sol Nascente (do Corel)
PANTONE_LAB = {
    'PANTONE 133 C': (38.824, 3.0, 41.0), 'PANTONE 172 C': (57.255, 60.0, 68.0),
    'PANTONE 1797 C': (45.49, 61.0, 41.0),
    'PANTONE 293 C': (32.549, 19.0, -68.0), 'PANTONE 302 C': (27.843, -10.0, -28.0),
    'PANTONE 330 C': (30.98, -25.0, -4.0), 'PANTONE 501 C': (75.294, 21.0, 5.0),
    'PANTONE 506 C': (30.98, 38.0, 8.0), 'PANTONE 578 C': (81.176, -14.0, 25.0),
    'PANTONE 7407 C': (69.02, 8.0, 50.0), 'PANTONE 7426 C': (39.608, 58.0, 17.0),
    'PANTONE 7477 C': (34.118, -14.0, -13.0), 'PANTONE 7524 C': (47.843, 29.0, 24.0),
    'PANTONE Hexachrome Orange C': (65.49, 42.0, 72.0),
}

SUBSTRATOS_A = ['Papel Seda 20grm', 'Papel Seda 30grm', 'Papel Seda 40grm', 'Papel Seda 50grm',
                'Papel Kraft Pardo 30grm', 'Papel Kraft Pardo 80grm']
SUBSTRATOS_B = ['Papel Seda Reciclado 20 ~ 26grm', 'Papel Barreira Tech 33grm', 'Papel Seda Translúcido 17grm']


# ---------------------------------------------------------------- modelo (logo + seta)
def _mul(a, b):
    a0, b0, c0, d0, e0, f0 = a
    a1, b1, c1, d1, e1, f1 = b
    return (a0 * a1 + b0 * c1, a0 * b1 + b0 * d1, c0 * a1 + d0 * c1, c0 * b1 + d0 * d1,
            e0 * a1 + f0 * c1 + e1, e0 * b1 + f0 * d1 + f1)


def carregar_modelo(pdf_ficha):
    """Extrai o logo e a seta do 'Sentido da repetição' de QUALQUER ficha Sol Nascente
    (página 1 do PDF exportado do Corel). Devolve {'logo': [...], 'seta': {...}}."""
    K = 25.4 / 72
    pdf = pikepdf.open(pdf_ficha)
    page = pdf.pages[0]
    H = float(page.MediaBox[3])
    csmap = {}
    for k, v in page.Resources.get('/ColorSpace', {}).items():
        if isinstance(v, pikepdf.Array) and str(v[0]) == '/Separation':
            csmap[str(k)] = ('spot', str(v[1])[1:])
        else:
            csmap[str(k)] = ('rgb', None)
    gs = dict(ctm=(1, 0, 0, 1, 0, 0), cs=None, col=None)
    stack, cur, sub, paths = [], [], [], []
    for operands, op in pikepdf.parse_content_stream(page):
        op = str(op)
        o = list(operands)
        if op == 'q': stack.append(dict(gs))
        elif op == 'Q': gs = stack.pop()
        elif op == 'cm': gs['ctm'] = _mul(tuple(float(x) for x in o), gs['ctm'])
        elif op == 'cs': gs['cs'] = csmap.get(str(o[0]), ('rgb', None))
        elif op == 'scn': gs['col'] = tuple(float(x) for x in o if not isinstance(x, pikepdf.Name))
        elif op == 'rg': gs['cs'], gs['col'] = ('rgb', None), tuple(float(x) for x in o)
        elif op == 'g': gs['cs'], gs['col'] = ('rgb', None), (float(o[0]),) * 3
        elif op in ('m', 'l', 'c', 're', 'h'):
            def P(i):
                a, b, c, d, e, f = gs['ctm']
                x, y = float(o[i]), float(o[i + 1])
                return [(a * x + c * y + e) * K, (H - (b * x + d * y + f)) * K]
            if op == 'm':
                if sub: cur.append(sub)
                sub = [['M', P(0)]]
            elif op == 'l': sub.append(['L', P(0)])
            elif op == 'c': sub.append(['C', P(0), P(2), P(4)])
            elif op == 'h': sub.append(['Z'])
            else:
                x, y, w, h = (float(t) for t in o)
                a, b, c, d, e, f = gs['ctm']
                pts = [((a * px + c * py + e) * K, (H - (b * px + d * py + f)) * K)
                       for px, py in ((x, y), (x + w, y), (x + w, y + h), (x, y + h))]
                sub = [['M', list(pts[0])], ['L', list(pts[1])], ['L', list(pts[2])], ['L', list(pts[3])], ['Z']]
        elif op in ('f', 'F', 'f*', 'S', 's', 'B', 'B*', 'b', 'b*', 'n'):
            if sub: cur.append(sub)
            if op != 'n' and cur:
                pts = [p for s_ in cur for seg in s_ for p in seg[1:]]
                xs = [p[0] for p in pts]; ys = [p[1] for p in pts]
                paths.append(dict(op=op, cs=gs['cs'], col=gs['col'], subs=cur,
                                  bbox=(min(xs), min(ys), max(xs), max(ys))))
            cur, sub = [], []
    boxes = [p for p in paths if p['op'] in ('s', 'S', 'b', 'b*', 'B')
             and abs(p['bbox'][2] - p['bbox'][0] - 700) < 1 and abs(p['bbox'][3] - p['bbox'][1] - 70) < 1]
    if not boxes:
        raise ValueError('cabeçalho 700x70 mm não encontrado: use a página 1 de uma ficha Sol Nascente')
    hb = min(boxes, key=lambda p: p['bbox'][1])
    BL, HT = hb['bbox'][0], hb['bbox'][1]

    def rel(subs, ox, oy):
        return [[[seg[0]] + [[q[0] - ox, q[1] - oy] for q in seg[1:]] for seg in s_] for s_ in subs]

    logo, seta = [], None
    for p in paths:
        x0, y0, x1, y1 = p['bbox']
        if p['op'] in ('f', 'f*') and x0 >= BL - 0.5 and x1 <= BL + 130 and y0 >= HT and y1 <= HT + 70:
            kind, name = p['cs'] or ('rgb', None)
            logo.append(dict(rule='evenodd' if p['op'] == 'f*' else 'nonzero',
                             color=dict(kind=kind, name=name, value=list(p['col'] or (0, 0, 0))),
                             subs=rel(p['subs'], BL, HT)))
        azul = p['col'] and len(p['col']) == 3 and abs(p['col'][0] - 0.2) < 0.02 and abs(p['col'][2] - 0.6627) < 0.02
        if p['op'] in ('f', 'f*') and azul and 40 < x1 - x0 < 55 and 6 < y1 - y0 < 11 and y0 > HT + 70:
            seta = dict(rule='evenodd' if p['op'] == 'f*' else 'nonzero', subs=rel(p['subs'], x0, y0))
    if len(logo) < 3 or seta is None:
        raise ValueError('logo/seta não encontrados na ficha de referência')
    return dict(logo=logo, seta=seta)


# ---------------------------------------------------------------- logo padrão da Sol Nascente Papéis
# Logomarca oficial (enviada pelo usuário em 18/11/2026), vetorizada e já posicionada na célula de
# 130 x 70 mm do cabeçalho (mm, origem no canto superior esquerdo da célula). Cores RGB da marca:
# laranja #F67D01, azul #1C4F7D, verde-petróleo #2B5762, preto (PAPÉIS). Vai em TODA ficha.
LOGO_SN = [["#F67D01","M18.9 23.57C19.69 23.95 22.7 28.28 23.45 29.29C23.88 29.86 24.73 30.64 24.83 31.32C24.63 31.44 24.37 31.67 24.12 31.64C23.78 31.59 22.48 30.09 22.16 29.77C21.2 28.85 20.29 27.88 19.35 26.94C19.1 26.69 18.43 26.21 18.39 25.84C18.31 25.17 18.62 24.14 18.9 23.57ZM23.4 24.14C24.07 25.18 24.47 26.48 24.95 27.62C25.4 28.68 26.02 29.73 26.29 30.84C25.3 31.43 25.11 30.4 24.67 29.65C24.27 28.97 22.57 26.41 22.5 25.84C22.44 25.3 23.01 24.34 23.4 24.14ZM27.81 30.93C27.67 30.92 27.32 31 27.22 30.84C27.01 30.53 25.87 26.72 25.91 26.43C25.97 25.98 26.54 25.34 26.86 25.06C27.21 25.68 27.95 29.92 27.92 30.72C27.88 31.42 27.93 30.28 27.81 30.93ZM14.82 25.3C15.77 25.73 16.61 26.56 17.44 27.2C18.52 28.04 19.58 28.88 20.66 29.71C21.55 30.4 22.79 31.11 23.41 32.04C23.25 32.22 22.97 32.58 22.69 32.57C22.22 32.54 20.57 31.35 20.06 31.05C18.79 30.32 17.53 29.55 16.25 28.81C15.88 28.6 15.13 28.35 14.9 27.98C14.63 27.55 14.71 25.77 14.82 25.3ZM29.12 31.23C28.99 31.21 28.71 31.27 28.63 31.08C28.51 30.81 28.37 27.14 28.46 26.91C28.6 26.55 29.27 26.12 29.6 26.1C29.64 27.33 29.48 30.09 29.12 31.23ZM29.96 31.66C29.87 31.52 29.78 31.66 29.74 31.44C29.67 31.06 30.29 28.19 30.5 27.86C30.76 27.48 31.05 27.5 31.39 27.49C31.31 28.3 30.48 31.15 29.96 31.66ZM111.84 31.56C111.15 30.98 109.81 29.16 109.41 28.34C109.73 28.19 112.37 28.05 112.51 28.34C112.62 28.59 112.18 31.14 112.02 31.44C111.87 31.72 112.07 31.28 111.84 31.56ZM30.67 32.01C30.67 31.21 30.49 32.6 30.59 31.8C30.63 31.42 31.6 29.36 31.86 29.11C32.18 28.82 32.54 28.81 32.9 28.82C32.64 29.65 31.38 31.54 30.67 32.01ZM11.6 29.08C13.09 29.34 14.58 30.2 16.01 30.74C17.3 31.22 18.56 31.79 19.83 32.33C20.52 32.62 21.26 32.83 21.91 33.23C21.84 33.48 21.61 34.12 21.26 34.2C20.81 34.3 18.73 33.58 18.16 33.43C16.68 33.04 15.18 32.7 13.71 32.27C13.28 32.15 12.47 32.12 12.14 31.8C11.83 31.49 11.6 29.52 11.6 29.08ZM22.86 36.68C23.27 34.54 27.32 32.14 29.12 34.06C29.32 34.28 28.66 35 28.53 35.17C27.19 36.98 27.86 36.78 25.43 36.77C24.58 36.77 23.68 36.83 22.86 36.68ZM9.93 33.9C11.97 33.87 14.42 34.44 16.49 34.7C17.79 34.86 19.34 34.87 20.54 35.31C20.51 35.53 20.15 36.63 19.95 36.73C19.58 36.92 18.37 36.79 17.92 36.79C16.21 36.78 14.5 36.78 12.79 36.78C12.3 36.78 10.92 36.93 10.53 36.71C10.2 36.53 9.92 34.28 9.93 33.9Z"],["#1C4F7D","M51.61 40.5C51.62 39.48 51.89 38.42 52.04 37.4C52.31 35.65 52.63 33.91 52.88 32.15C52.99 31.38 53.25 28.77 53.56 28.22C53.61 28.13 55.49 28.19 55.73 28.22C55.82 29.27 55.52 30.49 55.32 31.56C55.02 33.22 54.77 34.9 54.49 36.56C54.33 37.52 54.16 39.73 53.8 40.5C53.75 40.61 51.87 40.53 51.61 40.5ZM29.79 40.5C29.9 40.26 30.7 38.88 30.91 38.82C31.61 38.59 35.46 38.81 36.51 38.79C37.14 38.78 38.46 38.98 38.94 38.47C39.5 37.87 38.68 37.32 38.18 37.13C36.73 36.56 34.62 36.11 34.92 34.06C35.27 31.73 37.87 31.64 39.73 31.61C40.51 31.6 41.42 31.51 42.11 31.8C42.1 32.07 41.99 33.25 41.76 33.4C41.42 33.6 39.85 33.4 39.37 33.4C38.89 33.4 37.47 33.32 37.39 34.06C37.34 34.63 38.25 34.91 38.66 35.04C39.75 35.41 41.39 35.85 41.68 37.16C41.79 37.63 41.73 38.13 41.6 38.59C40.94 40.88 37.88 40.58 36.04 40.59C34.88 40.6 33.73 40.63 32.58 40.63C31.67 40.63 30.65 40.73 29.79 40.5ZM46.88 40.63C44.05 40.88 42.25 39.66 42.4 36.68C42.55 33.81 44.2 31.86 47.12 31.64C53.21 31.17 52.14 40.15 46.88 40.63ZM61.42 40.51C61.34 39.44 61.78 38.24 61.97 37.16C62.12 36.27 62.52 34.81 61.9 34.01C61.59 33.61 59.36 33.16 59.13 33.58C58.9 34 58.92 34.67 58.84 35.13C58.7 35.91 58.15 40.14 57.88 40.5C57.79 40.63 55.86 40.53 55.61 40.5C55.49 39.24 56 37.71 56.21 36.45C56.35 35.6 56.67 32.61 56.97 32.04C57.1 31.78 59.83 31.73 60.23 31.73C61.52 31.73 63.31 31.65 64.17 32.82C65.2 34.25 64.37 36.98 64.12 38.59C64.05 39.01 63.96 40.2 63.74 40.5C63.64 40.63 61.69 40.54 61.42 40.51ZM71.14 35.37C71.91 32.74 68.23 33.52 66.79 33.46C66.76 33.2 66.76 32.06 67.03 31.85C67.44 31.54 70.81 31.74 71.44 31.89C71.9 32 72.37 32.16 72.75 32.47C74.26 33.7 73.47 35.92 73.19 37.52C73.12 37.92 72.92 39.82 72.71 40.02C72.39 40.33 71.51 40.39 71.08 40.47C69.62 40.72 66.71 41 65.66 39.66C64.22 37.81 65.71 35.56 67.74 35.19C68.43 35.06 69.19 34.98 69.89 35.11C70.32 35.19 70.72 35.37 71.14 35.37ZM73.94 40.48C73.92 40.05 73.93 39.18 74.28 38.83C74.67 38.43 78.36 39.4 78.94 38.35C79.35 37.61 78.22 37.22 77.74 37.04C76.26 36.5 74.48 36.1 74.83 34.06C75.2 31.8 78 31.71 79.78 31.71C80.48 31.71 81.28 31.65 81.9 31.92C81.87 32.37 81.87 33.08 81.57 33.47C81.52 33.53 79.47 33.45 79.18 33.45C78.66 33.45 77.43 33.36 77.29 34.06C77.19 34.6 77.84 34.87 78.23 35.02C79.25 35.41 80.64 35.66 81.25 36.68C81.96 37.87 81.31 39.47 80.14 40.08C78.66 40.85 75.55 40.78 73.94 40.48ZM90.15 31.83C90.2 32.27 90.23 33.03 89.91 33.44C89.78 33.61 88.71 33.53 88.48 33.52C87.52 33.48 86.38 33.42 85.66 34.18C84.81 35.08 84.4 37.18 85.13 38.23C85.42 38.65 85.99 38.77 86.45 38.82C87.43 38.93 88.43 38.68 89.38 38.83C89.43 39.05 89.36 40.28 89.2 40.41C88.68 40.82 85.38 40.6 84.67 40.4C84.19 40.26 83.73 40.04 83.36 39.71C81.49 38.09 82.57 33.85 84.43 32.57C85.96 31.52 88.4 31.49 90.15 31.83ZM97.3 40.53C96.89 40.73 95.65 40.57 95.16 40.58C93.82 40.6 92.17 40.71 91.16 39.66C89.56 38 90.45 34.7 91.82 33.17C92.38 32.54 93.16 32.12 93.96 31.9C97.59 30.89 99.92 32.91 98.32 36.56C98.17 36.89 97.36 36.74 97.06 36.74C96.17 36.75 93.46 36.58 92.77 36.78C92.57 36.84 92.6 37.25 92.61 37.4C92.78 39.95 96.87 38.31 97.54 38.83C97.83 39.06 97.66 40.37 97.3 40.53ZM104.94 40.5C104.85 39.39 105.3 38.14 105.5 37.04C105.83 35.22 106.05 33.55 103.74 33.46C103.43 33.45 102.93 33.39 102.66 33.57C102.57 33.63 102.37 35.15 102.34 35.37C102.12 36.64 101.94 37.92 101.7 39.19C101.67 39.37 101.56 40.41 101.44 40.5C101.16 40.69 99.51 40.55 99.14 40.5C99.17 38.63 99.72 36.3 100.05 34.42C100.17 33.75 100.19 32.47 100.5 31.92C100.6 31.74 102.18 31.72 102.43 31.72C104.01 31.69 106.76 31.34 107.72 32.99C108.64 34.58 107.92 37.01 107.65 38.71C107.61 38.96 107.44 40.42 107.29 40.5C106.9 40.71 105.41 40.55 104.94 40.5ZM114.23 31.83C114.24 32.21 114.28 33.11 113.99 33.43C113.73 33.72 112.14 33.25 111.92 33.58C111.63 34.02 111.07 38.19 111.37 38.5C111.77 38.9 112.64 38.72 113.15 38.75C113.18 39.03 113.14 40.31 112.88 40.5C112.14 41.02 110.03 40.19 109.46 39.49C108.41 38.19 109.22 35.25 109.48 33.7C109.57 33.19 109.56 32.17 109.94 31.8C110.14 31.6 113.77 31.75 114.23 31.83ZM121.89 38.95C121.86 39.31 121.87 40.17 121.62 40.45C121.38 40.7 120.51 40.59 120.19 40.59C118.88 40.62 117.35 40.74 116.13 40.18C113.29 38.88 114.62 34.29 116.49 32.69C117.82 31.55 121.62 31.06 122.6 32.87C123.2 34 122.9 35.42 122.48 36.56C122.36 36.9 121.43 36.74 121.14 36.75C120.55 36.75 117.06 36.62 116.85 36.88C116.71 37.05 116.76 37.44 116.79 37.64C117 38.94 118.59 38.87 119.59 38.87C120.35 38.87 121.2 38.75 121.89 38.95ZM46.92 38.83C49.2 38.31 50.09 32.73 46.64 33.52C44.27 34.06 43.59 39.59 46.92 38.83ZM93.01 35.31C93.51 35.42 94.04 35.39 94.56 35.39C95.14 35.39 95.77 35.46 96.35 35.38C96.87 35.31 96.45 34.07 96.19 33.82C95.09 32.79 93.11 34.01 93.01 35.31ZM117.18 35.25C117.64 35.39 118.13 35.38 118.64 35.38C118.97 35.38 120.47 35.49 120.66 35.33C121.1 34.96 120.51 33.78 120.07 33.58C118.81 33.02 117.51 34.08 117.18 35.25ZM70.12 38.95C70.28 38.91 70.54 38.86 70.63 38.71C70.85 38.33 71.08 37.18 70.84 36.8C70.67 36.52 68.91 36.57 68.58 36.65C67.66 36.88 67.05 37.95 67.86 38.71C68.37 39.18 69.51 39.08 70.12 38.95Z"],["#2B5762","M7.57 40.5C7.74 40.11 8.59 38.69 8.98 38.52C9.42 38.33 12.78 38.48 13.51 38.48C16.81 38.48 20.1 38.48 23.4 38.48C24.89 38.48 26.46 38.62 27.93 38.49C28.43 38.44 29.63 36.46 29.96 35.99C31.17 34.22 32.42 32.46 33.69 30.72C34.16 30.09 34.74 28.91 35.32 28.44C36.05 27.87 38.41 28.17 39.37 28.17C42.15 28.17 44.94 28.19 47.72 28.19C48.89 28.19 50.72 27.96 51.77 28.29C51.75 28.59 51.6 30.03 51.41 30.2C51.12 30.45 47.83 30.3 47.24 30.3C44.46 30.3 41.68 30.28 38.9 30.28C37.98 30.28 36 30.09 35.2 30.29C34.62 30.44 33.44 32.43 33.06 32.99C31.95 34.65 30.7 36.22 29.6 37.89C29.14 38.59 28.37 40.03 27.69 40.47C27.27 40.74 23.23 40.6 22.45 40.6C19.31 40.61 16.17 40.56 13.03 40.58C11.26 40.6 9.3 40.83 7.57 40.5ZM57.25 30.2C57.25 29.69 57.28 28.66 57.61 28.23C57.67 28.16 59.95 28.18 60.23 28.18C62.97 28.18 65.72 28.18 68.46 28.18C78.43 28.18 88.4 28.18 98.37 28.17C100.52 28.17 102.66 28.14 104.81 28.15C105.56 28.16 106.47 28.03 107.19 28.21C107.44 28.27 108.46 29.84 108.62 30.13C108.43 30.21 108.62 30.24 108.51 30.26C107.47 30.46 105.36 30.28 104.21 30.28C100.88 30.28 97.54 30.28 94.2 30.28C85.9 30.28 77.59 30.27 69.29 30.27C66.55 30.27 63.81 30.27 61.07 30.27C59.84 30.27 58.44 30.43 57.25 30.2Z"],["#000000","M116.49 42.01C116.64 41.63 116.83 41.45 117.26 41.45C117.05 41.8 116.89 41.9 116.49 42.01ZM106.48 45.79C106.44 45.69 106.34 45.77 106.34 45.62C106.34 45.23 106.99 42.4 107.19 42.29C108.06 41.85 110.11 42.71 108.92 43.95C108.5 44.39 107.16 44.23 107.02 44.43C106.71 44.9 106.96 45.47 106.48 45.79ZM111.6 45.79C111.3 45.55 111.25 44.93 111.13 44.87C110.89 44.77 110.54 44.83 110.29 44.83C108.52 44.84 109.58 45.62 108.51 45.74C108.59 45.34 110.14 42.7 110.46 42.4C110.58 42.3 110.74 42.28 110.89 42.28C111.12 42.28 111.73 45.14 111.73 45.62C111.73 45.74 111.64 45.72 111.6 45.79ZM112.32 45.79C112.22 45.62 112.16 45.92 112.13 45.62C112.11 45.39 112.72 42.56 112.83 42.4C112.88 42.32 112.94 42.28 113.03 42.27C113.76 42.14 115.05 42.24 115 43.24C114.98 43.5 114.88 43.78 114.68 43.95C114.12 44.47 113.37 44.1 112.8 44.43C112.74 44.47 112.53 45.48 112.32 45.79ZM114.88 45.74C114.91 45.29 115.36 42.5 115.66 42.3C115.83 42.19 116.29 42.27 116.49 42.27C116.72 42.28 117.24 42.2 117.44 42.32C118.05 42.67 117 42.12 117.5 42.52C116.2 42.76 115.84 42.07 115.69 43.72C116.15 43.83 116.74 43.77 117.16 43.95C116.84 44.17 117.32 44 117.09 44.12C116.07 44.63 115.45 43.22 115.37 45.38C115.81 45.46 116.41 45.34 116.85 45.49C116.89 45.51 116.86 45.6 116.9 45.62C116.37 45.95 115.51 45.85 114.88 45.74ZM118.64 44.78C118.88 44.94 118.91 45.21 119.16 45.38C119.58 45.69 120.64 45.44 120.57 44.79C120.48 43.88 118.7 44.29 118.87 43.12C119.04 41.99 120.95 41.87 121.15 43.12C121.1 43.12 121.07 43.19 121.02 43.2C120.8 43.23 120.6 42.66 120.31 42.59C119.78 42.46 119.04 42.85 119.34 43.48C119.54 43.88 121.84 44.12 120.76 45.38C120.09 46.18 117.98 45.83 118.64 44.78ZM117.46 45.74C117.44 45.39 117.89 42.62 118.07 42.4C118.4 42 117.91 42.61 118.28 42.31C118.38 42.52 118.4 42.31 118.4 42.52C118.4 42.95 117.9 45.43 117.7 45.74C117.62 45.87 117.53 45.71 117.46 45.74ZM107.19 43.95C109.05 44.17 109.47 42.64 107.91 42.6C107.06 42.58 107.16 43.31 107.19 43.95ZM112.92 43.84C113.19 43.96 112.85 43.94 113.03 43.98C113.61 44.13 114.85 43.86 114.55 43C114.4 42.58 113.43 42.39 113.15 42.73C112.98 42.94 112.96 43.58 112.92 43.84ZM111.06 44.43C111.05 44.26 110.88 42.88 110.77 42.83C110.73 42.81 110.67 42.85 110.65 42.81C110.26 43.3 109.92 43.85 109.74 44.43C110.18 44.5 110.61 44.5 111.06 44.43Z"]]

SETA_PADRAO = dict(rule='nonzero', subs=[[['M', [0, 3]], ['L', [40, 3]], ['L', [40, 0]], ['L', [48, 4]],
                                         ['L', [40, 8]], ['L', [40, 5]], ['L', [0, 5]], ['Z']]])


def modelo_padrao():
    """Logo da Sol Nascente (LOGO_SN) + seta do 'Sentido da repetição', no formato de carregar_modelo()."""
    logo = []
    for cor, d in LOGO_SN:
        subs, sub = [], []
        for cmd, nums in re.findall(r'([MCZ])([^MCZ]*)', d):
            v = [float(x) for x in nums.split()]
            if cmd == 'M':
                sub = [['M', v[:2]]]
            elif cmd == 'C':
                sub.append(['C', v[0:2], v[2:4], v[4:6]])
            else:
                sub.append(['Z']); subs.append(sub); sub = []
        rgb = [int(cor[i:i + 2], 16) / 255 for i in (1, 3, 5)]
        logo.append(dict(rule='evenodd', color=dict(kind='rgb', name=None, value=rgb), subs=subs))
    return dict(logo=logo, seta=SETA_PADRAO)


# ---------------------------------------------------------------- utilidades
def lab_to_srgb(L, a, b):
    """Lab D50 -> sRGB (para a amostra/pré-visualização)."""
    fy = (L + 16) / 116; fx = fy + a / 500; fz = fy - b / 200
    def finv(t): return t ** 3 if t ** 3 > 0.008856 else (t - 16 / 116) / 7.787
    X, Y, Z = 0.9642 * finv(fx), 1.0 * finv(fy), 0.8249 * finv(fz)
    # Bradford D50 -> D65
    X, Y, Z = (0.9555766 * X - 0.0230393 * Y + 0.0631636 * Z,
               -0.0282895 * X + 1.0099416 * Y + 0.0210077 * Z,
               0.0122982 * X - 0.0204830 * Y + 1.3299098 * Z)
    r = 3.2404542 * X - 1.5371385 * Y - 0.4985314 * Z
    g = -0.9692660 * X + 1.8760108 * Y + 0.0415560 * Z
    bb = 0.0556434 * X - 0.2040259 * Y + 1.0572252 * Z
    def gam(c):
        c = max(0.0, min(1.0, c))
        return 12.92 * c if c <= 0.0031308 else 1.055 * c ** (1 / 2.4) - 0.055
    return gam(r), gam(g), gam(bb)


SPOT_CMYK = {}


def spot(name, lab=None):
    """Cor especial com nome exato (o Lab entra depois, no pós-processamento)."""
    lab = lab or PANTONE_LAB.get(name)
    if lab is None and name not in SPOT_CMYK:
        print(f'AVISO: sem Lab para {name}: a cor sai certa na separação, mas a amostra na tela fica preta. '
              f'Passe lab_extra={{"{name}": (L, a, b)}} (Lab D50 da Pantone Solid Coated).')
    if lab:
        r, g, b = lab_to_srgb(*lab)
        k = 1 - max(r, g, b)
        c, m, y = ((1 - r - k) / (1 - k), (1 - g - k) / (1 - k), (1 - b - k) / (1 - k)) if k < 1 else (0, 0, 0)
    else:
        c, m, y, k = 0, 0, 0, 1
    SPOT_CMYK[name] = (c, m, y, k)
    return CMYKColorSep(c, m, y, k, spotName=name, density=1)


def slug(s):
    s = unicodedata.normalize('NFKD', s).encode('ascii', 'ignore').decode()
    return re.sub(r'[^a-z0-9]+', '_', s.lower()).strip('_')


def nome_arquivo(nome, formato, versao):
    return f"{slug(nome)}_{formato.lower()}_v{int(versao)}.pdf"


def fmt_mm(v):
    return f"{v:.2f}".replace('.', ',') + ' mm'


# ---------------------------------------------------------------- desenho
class Pagina:
    def __init__(self, c, H):
        self.c, self.H = c, H

    def X(self, x): return x * mm
    def Y(self, y): return (self.H - y) * mm

    def rect(self, x, y, w, h, lw=0.2, stroke=PRETO, fill=None):
        c = self.c
        c.saveState()
        c.setLineWidth(lw * mm); c.setStrokeColor(stroke)
        if fill is not None: c.setFillColor(fill)
        c.rect(self.X(x), self.Y(y + h), w * mm, h * mm, stroke=1, fill=1 if fill is not None else 0)
        c.restoreState()

    def line(self, x0, y0, x1, y1, lw=0.076, color=AZUL, dash=None):
        c = self.c
        c.saveState(); c.setLineWidth(lw * mm); c.setStrokeColor(color)
        if dash: c.setDash([d * mm for d in dash])
        c.line(self.X(x0), self.Y(y0), self.X(x1), self.Y(y1))
        c.restoreState()

    def poly(self, pts, color=AZUL):
        c = self.c
        p = c.beginPath(); p.moveTo(self.X(pts[0][0]), self.Y(pts[0][1]))
        for x, y in pts[1:]: p.lineTo(self.X(x), self.Y(y))
        p.close()
        c.saveState(); c.setFillColor(color); c.drawPath(p, stroke=0, fill=1, fillMode=FILL_EVEN_ODD); c.restoreState()

    def text(self, s, x, y, size, font='Arial', color=PRETO, align='left', angle=0):
        c = self.c
        c.saveState(); c.setFillColor(color)
        c.translate(self.X(x), self.Y(y)); c.rotate(angle)
        w = pdfmetrics.stringWidth(s, font, size)
        x0 = 0 if align == 'left' else (-w if align == 'right' else -w / 2)
        if not TEXTO_EM_CURVAS:
            c.setFont(font, size); c.drawString(x0, 0, s)
        else:
            ft = _FT[font]; cmap = ft.getBestCmap(); gs = ft.getGlyphSet()
            esc = size / ft['head'].unitsPerEm
            p = c.beginPath(); ox = x0
            for ch in s:
                g = cmap.get(ord(ch), '.notdef')
                gs[g].draw(_PenRL(gs, p, esc, ox))
                ox += ft['hmtx'][g][0] * esc
            c.drawPath(p, stroke=0, fill=1, fillMode=FILL_NON_ZERO)
        c.restoreState()

    def width(self, s, size, font='Arial'):
        return pdfmetrics.stringWidth(s, font, size) / mm

    def vector(self, obj, ox, oy, color):
        c = self.c
        p = c.beginPath()
        for sub in obj['subs']:
            for seg in sub:
                t = seg[0]
                pts = [(self.X(ox + q[0]), self.Y(oy + q[1])) for q in seg[1:]]
                if t == 'M': p.moveTo(*pts[0])
                elif t == 'L': p.lineTo(*pts[0])
                elif t == 'C': p.curveTo(*pts[0], *pts[1], *pts[2])
                elif t == 'Z': p.close()
        c.saveState(); c.setFillColor(color)
        c.drawPath(p, stroke=0, fill=1, fillMode=FILL_EVEN_ODD if obj.get('rule') == 'evenodd' else FILL_NON_ZERO)
        c.restoreState()

    # cota no estilo Corel da Sol Nascente
    def cota(self, p1, p2, off, size=36, texto=None, gap=3.5, eixo=None):
        """off > 0: cota acima (horizontal) / à esquerda (vertical) / à esquerda do sentido p1->p2.
        eixo='h' ou 'v': cota alinhada ao eixo (mede só a distância em x ou em y)."""
        if eixo == 'h':
            yl = (min(p1[1], p2[1]) - off) if off >= 0 else (max(p1[1], p2[1]) - off)
            q1, q2 = (min(p1, p2, key=lambda p: p[0]), max(p1, p2, key=lambda p: p[0]))
            a1, a2 = (q1[0], yl), (q2[0], yl)
            s = -1 if off >= 0 else 1
            for (px, py) in (q1, q2):
                self.line(px, py + 2.5 * s, px, yl + 2.5 * s)
            L = a2[0] - a1[0]
        elif eixo == 'v':
            xl = (min(p1[0], p2[0]) - off) if off >= 0 else (max(p1[0], p2[0]) - off)
            q1, q2 = (max(p1, p2, key=lambda p: p[1]), min(p1, p2, key=lambda p: p[1]))   # de baixo p/ cima
            a1, a2 = (xl, q1[1]), (xl, q2[1])
            s = -1 if off >= 0 else 1
            for (px, py) in (q1, q2):
                self.line(px + 2.5 * s, py, xl + 2.5 * s, py)
            L = a1[1] - a2[1]
        else:
            x1, y1 = p1; x2, y2 = p2
            L = math.hypot(x2 - x1, y2 - y1)
            ux, uy = (x2 - x1) / L, (y2 - y1) / L
            nx, ny = uy, -ux
            s = 1 if off >= 0 else -1
            a1 = (x1 + nx * off, y1 + ny * off); a2 = (x2 + nx * off, y2 + ny * off)
            for (px, py) in (p1, p2):
                self.line(px + nx * 2.5 * s, py + ny * 2.5 * s, px + nx * (off + 2.5 * s), py + ny * (off + 2.5 * s))
        ux, uy = (a2[0] - a1[0]) / L, (a2[1] - a1[1]) / L
        nx, ny = uy, -ux
        texto = texto or fmt_mm(L)
        tw = self.width(texto, size)
        mx, my = (a1[0] + a2[0]) / 2, (a1[1] + a2[1]) / 2
        acima = False
        half = tw / 2 + gap
        if not L > 2 * half + 8:
            # cota curta: texto entre as setas, sem riscar o texto (diminui a letra até 16 pt se precisar)
            while size > 16 and tw + 2 * (3.73 + 1.0) > L:
                size -= 1; tw = self.width(texto, size)
            half = tw / 2 + 1.0
            if tw + 2 * (3.73 + 1.0) > L:
                acima = True              # não cabe nem assim: texto acima da linha
        if acima:
            self.line(a1[0], a1[1], a2[0], a2[1])
        else:
            self.line(a1[0], a1[1], mx - ux * half, my - uy * half)
            self.line(mx + ux * half, my + uy * half, a2[0], a2[1])
        cap = 0.716 * size * 25.4 / 72
        for (ax, ay), d in ((a1, 1), (a2, -1)):
            bx, by = ax + ux * 3.73 * d, ay + uy * 3.73 * d
            self.poly([(ax, ay), (bx + nx * 1.0, by + ny * 1.0), (bx - nx * 1.0, by - ny * 1.0)])
        ang = math.degrees(math.atan2(-uy, ux))       # ângulo no PDF (y para cima)
        if ang <= -90: ang += 180
        elif ang > 90: ang -= 180
        th = math.radians(ang)
        dx, dy = math.sin(th) * cap / 2, math.cos(th) * cap / 2
        if acima:                          # sobe o texto meia altura + 1,5 mm (lado de cima da leitura)
            dx -= math.sin(th) * (cap + 1.5); dy -= math.cos(th) * (cap + 1.5)
        self.text(texto, mx + dx, my + dy, size, color=AZUL, align='center', angle=ang)


# ---------------------------------------------------------------- ficha
def layout(art_w, art_h):
    W = max(800, art_w + 200)
    if art_w >= 700 and art_h >= 500:
        W = art_w + 300
    H = art_h + 300
    return W, H


def gerar(arte_pdf, saida, nome, formato, versao, data=None, substrato='Papel Seda 20grm',
          cores=(), fundo='Natural do substrato', cilindro=330, obs=None, cotas=(), cortes=(),
          cab_topo=40.0, tab_margem=20.0, arte_pagina=0, lab_extra=None, modelo=None, sentido=True,
          cortes_h=(), cotas_dir=None, margem=MARGEM_LARGURA):
    """arte_pdf: PDF de uma página com a arte em 1:1 (vira a página 2).
    cores: até 4 nomes ('PANTONE 1797 C' ou 'PRETO'); fundo: 'Natural do substrato' ou 'PANTONE ...'.
    cotas: dicts {p1:(x,y), p2:(x,y), off:mm, size:pt, eixo:'h'|'v'|None, texto:opcional},
    em mm relativos ao canto superior esquerdo da arte.
    cortes: posições x (mm, na arte) das linhas de corte quando a arte mostra mais de uma folha.
    cortes_h: posições y (mm, na arte) dos cortes na largura quando a tela tem mais de uma folha na largura
    (pistas centradas, refile igual nas duas bordas; ex.: 24x24 em tela 50x48 -> [10, 250, 490]).
    cotas_dir: pares (y1, y2) das cotas verticais à direita da arte. Padrão: da borda de cima até o corte
    do meio e daí até o último corte (ex.: [(0, 250), (250, 490)]), como na ficha corrigida pelo usuário.
    sentido=False: arte de corte individual (sem repetição) -> não desenha 'Sentido da repetição'.
    margem: mm livres de tinta da arte nas bordas de cima e de baixo (padrão 1 mm, regra da Sol Nascente).
    A arte é cortada nessas linhas e nas bordas laterais; o fundo (retângulo do quadro inteiro) não."""
    if modelo is None:
        modelo = modelo_padrao()          # logomarca oficial da Sol Nascente, sempre
    lab_extra = lab_extra or {}
    data = data or datetime.date.today().strftime('%d/%m/%Y')
    if obs is None:
        obs = [f'CILINDRO COM FECHAMENTO {int(cilindro)}MM', 'CORTE ALEATÓRIO SEM RETICULADO']
    src = pikepdf.open(arte_pdf)
    sp = src.pages[arte_pagina]
    info_margem = _aplicar_margem(src, sp, margem) if margem else None
    mb = [float(v) for v in sp.MediaBox]
    art_w, art_h = (mb[2] - mb[0]) * 25.4 / 72, (mb[3] - mb[1]) * 25.4 / 72
    art_w, art_h = round(art_w, 2), round(art_h, 2)
    W, H = layout(art_w, art_h)
    BL, HT = (W - 700) / 2, cab_topo
    AL, AT = (W - art_w) / 2, 150.0
    AR, AB = AL + art_w, AT + art_h
    TT = H - tab_margem - 70

    tmp = saida + '.tmp.pdf'
    c = canvas.Canvas(tmp, pagesize=(W * mm, H * mm), initialFontName='Arial', initialFontSize=12)
    c.setTitle(f"Arte: {nome.upper()} {formato}"); c.setAuthor('Sol Nascente Papéis')
    pg = Pagina(c, H)

    # -- cabeçalho
    pg.rect(BL, HT, 700, 70); pg.rect(BL, HT, 130, 70)
    for obj in modelo['logo']:
        col = obj['color']
        color = spot(col['name']) if col['kind'] == 'spot' else Color(*col['value'][:3])
        pg.vector(obj, BL, HT, color)
    titulo = f"Arte: {nome.upper()} {formato}"
    tsz = 48
    while pg.width(titulo, tsz, 'Arial-Bold') > 420 and tsz > 24:
        tsz -= 1
    pg.text(titulo, BL + 368.25, HT + 40.13, tsz, font='Arial-Bold', align='center')
    xr = BL + 597.48 + pg.width('Data: 00/00/0000', 36)
    pg.text(f"Data: {data}", xr, HT + 17.19, 36, align='right')
    pg.text(f"Versão: {int(versao):02d}", xr, HT + 32.59, 36, align='right')

    # -- moldura da arte (a arte entra por baixo, depois)
    pg.rect(AL, AT, art_w, art_h)

    # -- cotas gerais
    pg.cota((AL, AT), (AR, AT), 11.90, 36)                       # comprimento (em cima)
    pg.cota((AL, AB), (AL, AT), 20.37, 36)                       # largura (à esquerda)
    gap = 25.6 if AL < 200 else 42.0
    xl = max(10.0, AL - 20.37 - gap - pg.width('Largura', 36))
    pg.text('Largura', xl, (AT + AB) / 2 + 0.11, 36, color=AZUL)

    # -- linhas de corte entre folhas (quando a arte mostra mais de uma folha)
    if cortes:
        for xc in cortes:
            pg.line(AL + xc, AT, AL + xc, AB, lw=2.822, color=VERMELHO, dash=(5.64, 14.11))
        xs = [0.0] + list(cortes) + [art_w]
        for a, b in zip(xs[:-1], xs[1:]):
            pg.cota((AL + a, AB), (AL + b, AB), -23.68, 36)
        sx, sb = max(10.0, AL - 125.26), AB + 39.2
    else:
        sx, sb = BL + 1.74, AB + 28.0
    if sentido:
        pg.text('Sentido da repetição', sx, sb, 36, color=AZUL)
        pg.vector(modelo['seta'], sx + 121.37, sb - 7.74, AZUL)

    # -- cortes na largura (tela com mais de uma folha na largura) + cotas à direita da arte
    if cortes_h:
        ys = sorted(cortes_h)
        for yc in ys:
            pg.line(AL, AT + yc, AR, AT + yc, lw=2.822, color=VERMELHO, dash=(5.64, 14.11))
        if cotas_dir is None:
            pos = [0.0] + [y for y in ys if y > 20.0]          # o refile de cima não ganha cota própria
            if pos[-1] < art_h - 20.0:
                pos.append(art_h)
            cotas_dir = list(zip(pos[:-1], pos[1:]))
    for a, b in (cotas_dir or ()):
        pg.cota((AR, AT + a), (AR, AT + b), -27.89, 36, eixo='v')

    # -- cotas internas
    for d in cotas:
        p1 = (AL + d['p1'][0], AT + d['p1'][1]); p2 = (AL + d['p2'][0], AT + d['p2'][1])
        pg.cota(p1, p2, d.get('off', 10), d.get('size', 18), d.get('texto'), eixo=d.get('eixo'))

    # -- quadro inferior
    pg.rect(BL, TT, 700, 70); pg.rect(BL, TT, 200, 70); pg.rect(BL + 200, TT, 200, 70)
    pg.text('Substrato:', BL + 2.11, TT + 7.53, 12)
    pg.text('Cores (arte):', BL + 202.11, TT + 7.53, 12)
    pg.text('Fundo (chapado):', BL + 202.11, TT + 47.53, 12)
    pg.text('Observações:', BL + 402.21, TT + 7.53, 12, font='Arial-Bold')

    def item(x, base, label, fill=None):
        pg.rect(x - 7.68, base - 4.62, 5.82, 5.56, fill=fill)
        pg.text(label, x, base, 12)

    for i, s in enumerate(SUBSTRATOS_A):
        item(BL + 17.99, TT + 17.02 + 9 * i, s, PRETO if s == substrato else None)
    for i, s in enumerate(SUBSTRATOS_B):
        item(BL + 117.99, TT + (17.02, 26.02, 34.59)[i], s, PRETO if s == substrato else None)

    slots = [(BL + 217.99, TT + 17.02), (BL + 217.99, TT + 26.02), (BL + 217.99, TT + 35.02), (BL + 317.99, TT + 17.02)]
    cores = list(cores)
    for i, (x, b) in enumerate(slots):
        if i < len(cores):
            nm = cores[i]
            if nm.upper() in ('PRETO', 'PANTONE PRETO'):
                item(x, b, 'Pantone PRETO', PRETO)
            else:
                label = 'Pantone ' + nm.replace('PANTONE ', '')
                item(x, b, label, spot(nm, lab_extra.get(nm)))
        else:
            item(x, b, 'Pantone XXX')
    item(BL + 317.99, TT + 26.02, 'Liso (sem impressão)', PRETO if not cores else None)
    if fundo == 'Natural do substrato':
        item(BL + 217.99, TT + 57.02, 'Natural do substrato', PRETO)
        item(BL + 217.99, TT + 66.02, 'Pantone XXX')
    else:
        item(BL + 217.99, TT + 57.02, 'Natural do substrato')
        item(BL + 217.99, TT + 66.02, 'Pantone ' + fundo.replace('PANTONE ', ''), spot(fundo, lab_extra.get(fundo)))
    for i, s in enumerate(obs):
        pg.text(s, BL + 402.21, TT + 18.81 + 6.30 * i, 16, font='Arial-Bold')
    c.showPage(); c.save()

    # -- montagem final num PDF limpo (estrutura simples, como o Corel exporta)
    rl = pikepdf.open(tmp)
    out = pikepdf.new()
    ficha_pg = rl.pages[0]
    # página 1: arte (por baixo) + ficha
    res1 = pikepdf.Dictionary()
    arte_ops = _copiar_conteudo(src, sp, out, 'A', res1)
    ficha_ops = _copiar_conteudo(rl, ficha_pg, out, 'F', res1)
    x0, ybase = AL * 72 / 25.4 - mb[0], (H - AB) * 72 / 25.4 - mb[1]
    c1 = (f"q 1 0 0 1 {x0:.4f} {ybase:.4f} cm\n".encode() + arte_ops + b"\nQ\n" + ficha_ops)
    _nova_pagina(out, W * 72 / 25.4, H * 72 / 25.4, c1, res1)
    # página 2: só a arte
    res2 = pikepdf.Dictionary()
    arte2 = _copiar_conteudo(src, sp, out, 'A', res2)
    c2 = f"q 1 0 0 1 {-mb[0]:.4f} {-mb[1]:.4f} cm\n".encode() + arte2 + b"\nQ\n"
    _nova_pagina(out, art_w * 72 / 25.4, art_h * 72 / 25.4, c2, res2)
    fix_spots(out, lab_extra)
    out.docinfo['/Title'] = f"Arte: {nome.upper()} {formato}"
    out.docinfo['/Author'] = 'Sol Nascente Papéis'
    out.save(saida, min_version='1.7', compress_streams=True,
             stream_decode_level=pikepdf.StreamDecodeLevel.generalized,
             object_stream_mode=pikepdf.ObjectStreamMode.disable)
    rl.close()
    os.remove(tmp)
    return dict(W=W, H=H, art=(art_w, art_h), saida=saida, margem=info_margem)


def limpar_pdf(entrada, saida, lab_extra=None, titulo=None, margem=MARGEM_LARGURA):
    """Regrava um PDF (ex.: arte ou módulo gerado no reportlab) na estrutura que o
    CorelDRAW 2019 abre: PDF 1.7, sem XObject/camadas/transições, só Flate,
    cores especiais com função tipo 2. margem: corta a arte a 1 mm das bordas de cima e de baixo
    (o fundo não); margem=0 para regravar sem cortar."""
    src = pikepdf.open(entrada)
    out = pikepdf.new()
    for page in src.pages:
        mb = [float(v) for v in page.MediaBox]
        res = pikepdf.Dictionary()
        if margem:
            _aplicar_margem(src, page, margem)
        ops = _copiar_conteudo(src, page, out, 'A', res)
        c = f"q 1 0 0 1 {-mb[0]:.4f} {-mb[1]:.4f} cm\n".encode() + ops + b"\nQ\n"
        _nova_pagina(out, mb[2] - mb[0], mb[3] - mb[1], c, res)
    fix_spots(out, lab_extra)
    if titulo:
        out.docinfo['/Title'] = titulo
    out.save(saida, min_version='1.7', compress_streams=True,
             stream_decode_level=pikepdf.StreamDecodeLevel.generalized,
             object_stream_mode=pikepdf.ObjectStreamMode.disable)
    return saida


_DEVICE = {'/DeviceGray', '/DeviceRGB', '/DeviceCMYK', '/Pattern'}


def _copiar_conteudo(src_pdf, page, out, prefixo, res_dst):
    """Copia o conteúdo de uma página para `out`: renomeia os recursos (prefixo),
    troca as marcações de camada (BDC /OC) por BMC e não leva XObjects de camada."""
    page = pikepdf.Page(page) if not isinstance(page, pikepdf.Page) else page
    res = page.Resources
    nomes = {}
    sigla = {'/ColorSpace': 'CS', '/ExtGState': 'GS', '/Font': 'F', '/XObject': 'X', '/Pattern': 'P', '/Shading': 'SH'}
    for cat in ('/ColorSpace', '/ExtGState', '/Font', '/XObject', '/Pattern', '/Shading'):
        d = res.get(cat)
        if d is None or len(d) == 0:
            continue
        if cat not in res_dst:
            res_dst[cat] = pikepdf.Dictionary()
        for k, v in d.items():
            i = len(res_dst[cat]) + 1
            nk = f'/{prefixo}{sigla[cat]}{i}'
            while nk in res_dst[cat]:
                i += 1; nk = f'/{prefixo}{sigla[cat]}{i}'
            obj = v if v.is_indirect else src_pdf.make_indirect(v)
            res_dst[cat][nk] = out.copy_foreign(obj)
            nomes[(cat, str(k))] = nk
    novo = []
    for ins in pikepdf.parse_content_stream(page):
        if isinstance(ins, pikepdf.ContentStreamInlineImage):
            novo.append(ins); continue
        ops, op = list(ins.operands), str(ins.operator)

        def ren(i, cat):
            n = str(ops[i])
            if (cat, n) in nomes:
                ops[i] = pikepdf.Name(nomes[(cat, n)])
        if op in ('cs', 'CS') and str(ops[0]) not in _DEVICE:
            ren(0, '/ColorSpace')
        elif op in ('scn', 'SCN') and ops and isinstance(ops[-1], pikepdf.Name):
            ren(len(ops) - 1, '/Pattern')
        elif op == 'gs':
            ren(0, '/ExtGState')
        elif op == 'Do':
            ren(0, '/XObject')
        elif op == 'Tf':
            ren(0, '/Font')
        elif op == 'sh':
            ren(0, '/Shading')
        elif op == 'BDC':
            op, ops = 'BMC', ops[:1]
        elif op == 'DP':
            op, ops = 'MP', ops[:1]
        novo.append(pikepdf.ContentStreamInstruction(ops, pikepdf.Operator(op)))
    # tira blocos de texto vazios (o reportlab sempre abre um BT/Tf/ET sem texto) e fontes sem uso
    final, bloco = [], None
    for ins in novo:
        op = None if isinstance(ins, pikepdf.ContentStreamInlineImage) else str(ins.operator)
        if op == 'BT':
            bloco = [ins]; continue
        if bloco is not None:
            bloco.append(ins)
            if op == 'ET':
                if any(not isinstance(b, pikepdf.ContentStreamInlineImage) and str(b.operator) in ('Tj', 'TJ', "'", '"')
                       for b in bloco):
                    final.extend(bloco)
                bloco = None
            continue
        final.append(ins)
    usadas = {str(ins.operands[0]) for ins in final
              if not isinstance(ins, pikepdf.ContentStreamInlineImage) and str(ins.operator) == 'Tf'}
    if '/Font' in res_dst:
        for (cat, _), nk in nomes.items():
            if cat == '/Font' and nk not in usadas and nk in res_dst['/Font']:
                del res_dst['/Font'][nk]
        if len(res_dst['/Font']) == 0:
            del res_dst['/Font']
    return pikepdf.unparse_content_stream(final)


def _nova_pagina(out, w_pt, h_pt, conteudo, recursos):
    box = [0, 0, round(w_pt, 4), round(h_pt, 4)]
    pg = pikepdf.Dictionary(Type=pikepdf.Name.Page, MediaBox=box, CropBox=box, TrimBox=box,
                            Resources=recursos, Contents=out.make_stream(conteudo))
    out.pages.append(pikepdf.Page(pg))


def _ps_eval(code, x=1.0):
    """Avalia uma função PostScript calculadora simples (dup/exch/mul/add/sub/pop/números)."""
    st = [x]
    for tok in re.findall(r'[-+]?\d*\.?\d+|[a-z]+', code.decode('latin1') if isinstance(code, bytes) else code):
        if tok == 'dup': st.append(st[-1])
        elif tok == 'exch': st[-1], st[-2] = st[-2], st[-1]
        elif tok == 'pop': st.pop()
        elif tok == 'mul': b = st.pop(); st[-1] *= b
        elif tok == 'add': b = st.pop(); st[-1] += b
        elif tok == 'sub': b = st.pop(); st[-1] -= b
        else: st.append(float(tok))
    return st


def fix_spots(pdf, lab_extra=None):
    """Deixa todas as cores especiais como o Corel exporta: Separation com função tipo 2
    (alternativa Lab D50 quando o Lab é conhecido; senão CMYK)."""
    lab_extra = lab_extra or {}
    seen = set()

    def visit(res):
        if res is None or id(res) in seen: return
        seen.add(id(res))
        cs = res.get('/ColorSpace')
        if cs is not None:
            for k in list(cs.keys()):
                v = cs[k]
                if isinstance(v, pikepdf.Array) and str(v[0]) == '/Separation':
                    nm = str(v[1])[1:]
                    lab = lab_extra.get(nm) or PANTONE_LAB.get(nm)
                    if lab is not None:
                        fn = pikepdf.Dictionary(FunctionType=2, Domain=[0, 1], C0=[100.0, 0.0, 0.0],
                                                C1=[float(x) for x in lab], N=1)
                        alt = pikepdf.Array([pikepdf.Name.Lab, pikepdf.Dictionary(
                            WhitePoint=[0.9637, 1.0, 0.8241], Range=[-128, 127, -128, 127])])
                    else:   # sem Lab conhecido: alternativa CMYK, ainda com função tipo 2
                        cmyk = SPOT_CMYK.get(nm)
                        if cmyk is None and str(v[2]) == '/DeviceCMYK':
                            f = v[3]
                            try:
                                if int(f.get('/FunctionType')) == 2:
                                    cmyk = tuple(float(x) for x in f.C1)
                                elif int(f.get('/FunctionType')) == 4:
                                    cmyk = tuple(_ps_eval(f.read_bytes())[-4:])
                            except Exception:
                                cmyk = None
                        cmyk = cmyk or (0, 0, 0, 1)
                        fn = pikepdf.Dictionary(FunctionType=2, Domain=[0, 1], C0=[0.0, 0.0, 0.0, 0.0],
                                                C1=[float(x) for x in cmyk], N=1)
                        alt = pikepdf.Name.DeviceCMYK
                    cs[k] = pikepdf.Array([pikepdf.Name.Separation, pikepdf.Name('/' + nm), alt, fn])
        xo = res.get('/XObject')
        if xo is not None:
            for k, v in xo.items():
                visit(v.get('/Resources'))

    for p in pdf.pages:
        visit(p.Resources)


# ---------------------------------------------------------------- margem da arte (1 mm em cima e embaixo)
# Regra do usuário (17/11/2026): no quadro (C x L), a arte fica a 1 mm das bordas de cima e de baixo. O desenho
# que passa da linha é cortado nela (corte de verdade nas curvas, como nos arquivos do Corel). Nas laterais não
# tem margem: a arte vai até a borda. O fundo não é cortado e preenche 100% do quadro.
# Fundo = caminho preenchido com um subcaminho que cobre o quadro inteiro (ex.: retângulo do tamanho da arte).

class _Degenerado(Exception):
    pass


def _bz_pt(s, t):
    u = 1.0 - t
    a, b, c, d = u * u * u, 3 * u * u * t, 3 * u * t * t, t * t * t
    return (a * s[0][0] + b * s[1][0] + c * s[2][0] + d * s[3][0], a * s[0][1] + b * s[1][1] + c * s[2][1] + d * s[3][1])


def _bz_trecho(s, t0, t1):
    """pedaço [t0, t1] da cúbica s (de Casteljau)"""
    def div(s, t):
        (x0, y0), (x1, y1), (x2, y2), (x3, y3) = s
        a = (x0 + (x1 - x0) * t, y0 + (y1 - y0) * t); b = (x1 + (x2 - x1) * t, y1 + (y2 - y1) * t)
        c = (x2 + (x3 - x2) * t, y2 + (y3 - y2) * t); d = (a[0] + (b[0] - a[0]) * t, a[1] + (b[1] - a[1]) * t)
        e = (b[0] + (c[0] - b[0]) * t, b[1] + (c[1] - b[1]) * t); f = (d[0] + (e[0] - d[0]) * t, d[1] + (e[1] - d[1]) * t)
        return ((x0, y0), a, d, f), (f, e, c, (x3, y3))
    if t0 > 0.0:
        s = div(s, t0)[1]; t1 = (t1 - t0) / (1.0 - t0)
    return div(s, t1)[0] if t1 < 1.0 else s


def _reta(p, q):
    return (p, (p[0] + (q[0] - p[0]) / 3, p[1] + (q[1] - p[1]) / 3), (p[0] + 2 * (q[0] - p[0]) / 3, p[1] + 2 * (q[1] - p[1]) / 3), q)


def _cruza(c, v, eps=1e-9):
    """t em (0, 1) onde a cúbica (valores de controle c) troca de lado do valor v; encostar não conta.
    Ponta ou ponto crítico em cima da linha -> _Degenerado (aí a linha é deslocada um nada e tenta de novo)."""
    if min(c) > v + eps or max(c) < v - eps:
        return []
    if abs(c[0] - v) < eps or abs(c[3] - v) < eps:
        raise _Degenerado('ponta na linha')
    a, b, k, d = -c[0] + 3 * c[1] - 3 * c[2] + c[3], 3 * c[0] - 6 * c[1] + 3 * c[2], 3 * (c[1] - c[0]), c[0] - v

    def f(t):
        return ((a * t + b) * t + k) * t + d
    ts = [0.0, 1.0]
    if abs(a) > 1e-14:
        disc = 4 * b * b - 12 * a * k
        if disc > 0:
            ts += [t for t in ((-2 * b - math.sqrt(disc)) / (6 * a), (-2 * b + math.sqrt(disc)) / (6 * a)) if 0.0 < t < 1.0]
    elif abs(b) > 1e-14 and 0.0 < -k / (2 * b) < 1.0:
        ts.append(-k / (2 * b))
    ts.sort()
    out = []
    for t0, t1 in zip(ts[:-1], ts[1:]):
        f0, f1 = f(t0), f(t1)
        if t1 < 1.0 and abs(f1) < eps:
            raise _Degenerado('tangente na linha')
        if f0 * f1 < 0:
            for _ in range(60):
                tm = (t0 + t1) / 2; fm = f(tm)
                if f0 * fm <= 0: t1 = tm
                else: t0, f0 = tm, fm
            out.append((t0 + t1) / 2)
    return out


def _faixa_sub(sub, ax, lo, hi):
    """Parte do subcaminho fechado `sub` (lista de cúbicas) com lo < coordenada[ax] < hi, em subcaminhos
    fechados com a MESMA orientação (o resultado vale para preenchimento par-ímpar e não-zero)."""
    cr = []                                        # (segmento, t, linha, ponto)
    for i, s in enumerate(sub):
        for li, v in ((0, lo), (1, hi)):
            for t in _cruza([p[ax] for p in s], v):
                p = list(_bz_pt(s, t)); p[ax] = v
                cr.append((i, t, li, tuple(p)))
    if not cr:                                     # inteiro dentro ou inteiro fora
        p = max((_bz_pt(s, 0.5) for s in sub), key=lambda p: min(abs(p[ax] - lo), abs(p[ax] - hi)))
        return [sub] if lo < p[ax] < hi else []
    cr.sort(key=lambda c: (c[0], c[1]))
    n = len(cr)
    kept = {}                                      # trechos da curva dentro da faixa: início -> (fim, cúbicas)
    for j in range(n):
        (i0, t0), (i1, t1) = cr[j][:2], cr[(j + 1) % n][:2]
        if j + 1 < n and i1 == i0:
            if t1 - t0 < 1e-12:
                raise _Degenerado('cruzamentos coincidentes')
            segs = [_bz_trecho(sub[i0], t0, t1)]
        else:
            segs, i = [_bz_trecho(sub[i0], t0, 1.0)], (i0 + 1) % len(sub)
            while i != i1:
                segs.append(sub[i]); i = (i + 1) % len(sub)
            segs.append(_bz_trecho(sub[i1], 0.0, t1))
        if lo < _bz_pt(segs[len(segs) // 2], 0.5)[ax] < hi:
            kept[j] = ((j + 1) % n, segs)
    par = {}                                       # pedaços da linha dentro do desenho (par-ímpar ao longo da linha)
    for li in (0, 1):
        nos = sorted((j for j in range(n) if cr[j][2] == li), key=lambda j: cr[j][3][1 - ax])
        if len(nos) % 2:
            raise _Degenerado('cruzamentos ímpares')
        for a, b in zip(nos[0::2], nos[1::2]):
            par[a], par[b] = b, a
    out, usados = [], set()
    for ini in kept:
        laco, no = [], ini
        while ini not in usados or no != ini:
            if no not in kept or no in usados:
                raise _Degenerado('laço quebrado')
            usados.add(no)
            fim, segs = kept[no]
            laco += segs + [_reta(cr[fim][3], cr[par[fim]][3])]
            no = par[fim]
        if laco:
            out.append(laco)
    return out


def _recorta(subs, rect):
    """Corte exato de um preenchimento (subcaminhos de cúbicas, coordenadas da página) no retângulo (x0, y0, x1, y1)."""
    erro = None
    for j in (0.0, 1.3e-6, -1.7e-6, 2.9e-6, -3.1e-6):
        try:
            res = []
            for sub in subs:
                for s in _faixa_sub(sub, 1, rect[1] + j, rect[3] - j):
                    res += _faixa_sub(s, 0, rect[0] - j, rect[2] + j)
            return res
        except _Degenerado as e:
            erro = e
    raise erro


def _inv(m):
    a, b, c, d, e, f = m
    det = a * d - b * c
    return (d / det, -b / det, -c / det, a / det, (c * f - d * e) / det, (b * e - a * f) / det)


def _num(v, casas=4):
    s = f'{v:.{casas}f}'.rstrip('0').rstrip('.')
    return Decimal(s if s not in ('-0', '') else '0')


def _caixa(subs):
    pts = [p for s in subs for g in s for p in g]
    return (min(p[0] for p in pts), min(p[1] for p in pts), max(p[0] for p in pts), max(p[1] for p in pts)) if pts else None


def _eh_fundo(subs, mb, tol=0.5):
    return any(b[0] <= mb[0] + tol and b[1] <= mb[1] + tol and b[2] >= mb[2] - tol and b[3] >= mb[3] - tol
               for b in (_caixa([s]) for s in subs) if b)


_PINTA = ('f', 'F', 'f*', 'S', 's', 'B', 'B*', 'b', 'b*', 'n')


def _caminhos(page):
    """Percorre o conteúdo. Gera ('ins', instrução, estado) para o que não é caminho e
    ('path', instruções, operador, subcaminhos em coordenadas da página, estado, é_clip) para cada caminho.
    estado: ctm, lw e clip (caixa do recorte em vigor, coordenadas da página)."""
    gs = dict(ctm=(1, 0, 0, 1, 0, 0), lw=1.0, clip=None)
    pilha, pend, subs, sub, cur, ini, clip = [], [], [], [], None, None, False

    def P(x, y):
        a, b, c, d, e, f = gs['ctm']
        return (a * x + c * y + e, b * x + d * y + f)
    for ins in pikepdf.parse_content_stream(page):
        if isinstance(ins, pikepdf.ContentStreamInlineImage):
            yield ('ins', ins, dict(gs)); continue
        o, op = [float(v) if isinstance(v, (int, float, Decimal)) else v for v in ins.operands], str(ins.operator)
        if op not in ('m', 'l', 'c', 'v', 'y', 're', 'h', 'W', 'W*') + _PINTA:
            if op == 'q': pilha.append(dict(gs))
            elif op == 'Q': gs = pilha.pop() if pilha else gs
            elif op == 'cm': gs['ctm'] = _mul(tuple(float(x) for x in o), gs['ctm'])
            elif op == 'w': gs['lw'] = float(o[0])
            yield ('ins', ins, dict(gs)); continue
        pend.append(ins)
        if op == 'm':
            if sub: subs.append(sub)
            sub, cur = [], P(o[0], o[1]); ini = cur
        elif op == 'l':
            p = P(o[0], o[1]); sub.append(_reta(cur, p)); cur = p
        elif op in ('c', 'v', 'y'):
            q = [P(o[i], o[i + 1]) for i in range(0, len(o), 2)]
            g = (cur, *q) if op == 'c' else ((cur, cur, *q) if op == 'v' else (cur, q[0], q[1], q[1]))
            sub.append(g); cur = g[3]
        elif op == 're':
            if sub: subs.append(sub)
            x, y, w, h = o
            q = [P(x, y), P(x + w, y), P(x + w, y + h), P(x, y + h)]
            subs.append([_reta(q[i], q[(i + 1) % 4]) for i in range(4)]); sub, cur, ini = [], q[0], q[0]
        elif op == 'h':
            if sub and cur != ini: sub.append(_reta(cur, ini))
            if sub: subs.append(sub)
            sub, cur = [], ini
        elif op in ('W', 'W*'):
            clip = True
        else:                                      # pintura: fecha e entrega o caminho
            if sub:
                if cur != ini: sub.append(_reta(cur, ini))
                subs.append(sub)
            yield ('path', pend, op, subs, dict(gs), clip)
            if clip and subs:
                b, c0 = _caixa(subs), gs['clip']
                gs['clip'] = b if c0 is None else (max(b[0], c0[0]), max(b[1], c0[1]), min(b[2], c0[2]), min(b[3], c0[3]))
            pend, subs, sub, cur, ini, clip = [], [], [], None, None, False


def _aplicar_margem(pdf, page, margem=1.0):
    """Corta a arte da página nas linhas a `margem` mm das bordas de cima e de baixo (e nas laterais), com corte
    de verdade nas curvas; o fundo fica inteiro. Troca o conteúdo da página e devolve a contagem."""
    mb = [float(v) for v in page.MediaBox]
    m = margem * 72 / 25.4
    R = (mb[0], mb[1] + m, mb[2], mb[3] - m)       # área da arte (pontos, y para cima)
    tol = 1e-3
    st = dict(cortados=0, removidos=0, recorte_pdf=0, fundo=0)
    Ins, Op = pikepdf.ContentStreamInstruction, pikepdf.Operator
    xobj = page.Resources.get('/XObject', {})
    novo = []

    def recorte_pdf(instrs, ctm):                  # traço, imagem ou caminho que não deu para cortar
        st['recorte_pdf'] += 1
        return ([Ins([], Op('q')), Ins([_num(v, 8) for v in _inv(ctm)], Op('cm')),
                 Ins([_num(v) for v in (R[0], R[1], R[2] - R[0], R[3] - R[1])], Op('re')), Ins([], Op('W')),
                 Ins([], Op('n')), Ins([_num(v, 8) for v in ctm], Op('cm'))] + list(instrs) + [Ins([], Op('Q'))])

    for item in _caminhos(page):
        if item[0] == 'ins':
            ins, g = item[1], item[2]
            op = None if isinstance(ins, pikepdf.ContentStreamInlineImage) else str(ins.operator)
            if op in (None, 'Do', 'sh'):           # imagem (do quadro inteiro = fundo), formulário ou sombreado
                img = op is None or (op == 'Do' and str(xobj.get(str(ins.operands[0]), {}).get('/Subtype')) == '/Image')
                a, b, c, d, e, f = g['ctm']
                bb = _caixa([[((e, f), (a + e, b + f), (c + e, d + f), (a + c + e, b + d + f))]])
                if img and _eh_fundo([[(bb[:2], bb[2:], bb[2:], bb[2:])]], mb):
                    st['fundo'] += 1; novo.append(ins)
                elif img and bb[1] >= R[1] - tol and bb[3] <= R[3] + tol:
                    novo.append(ins)
                else:
                    novo += recorte_pdf([ins], g['ctm'])
            else:
                novo.append(ins)
            continue
        _, instrs, op, subs, g, eh_clip = item
        bb = _caixa(subs)
        if op == 'n' or eh_clip or bb is None:
            novo += instrs; continue
        traco = op in ('S', 's', 'B', 'B*', 'b', 'b*')
        if traco:
            e = g['lw'] * math.sqrt(abs(g['ctm'][0] * g['ctm'][3] - g['ctm'][1] * g['ctm'][2])) + tol
            bb = (bb[0] - e, bb[1] - e, bb[2] + e, bb[3] + e)
        elif _eh_fundo(subs, mb):
            st['fundo'] += 1; novo += instrs; continue
        if bb[0] >= R[0] - tol and bb[1] >= R[1] - tol and bb[2] <= R[2] + tol and bb[3] <= R[3] + tol:
            novo += instrs; continue               # já está dentro
        if bb[2] <= R[0] + tol or bb[0] >= R[2] - tol or bb[3] <= R[1] + tol or bb[1] >= R[3] - tol:
            st['removidos'] += 1; continue         # inteiro na margem ou fora da folha
        try:
            if traco:
                raise _Degenerado('traço')
            res = _recorta(subs, R)
        except _Degenerado:
            novo += recorte_pdf(instrs, g['ctm']); continue
        if not res:
            st['removidos'] += 1; continue
        st['cortados'] += 1
        iv = _inv(g['ctm'])

        def U(p):
            return (iv[0] * p[0] + iv[2] * p[1] + iv[4], iv[1] * p[0] + iv[3] * p[1] + iv[5])
        for s in res:
            novo.append(Ins([_num(v) for v in U(s[0][0])], Op('m')))
            for q in s:
                p0, p1, p2, p3 = (U(v) for v in q)
                if max(abs(p1[k] - (2 * p0[k] + p3[k]) / 3) + abs(p2[k] - (p0[k] + 2 * p3[k]) / 3) for k in (0, 1)) < 1e-6:
                    novo.append(Ins([_num(v) for v in p3], Op('l')))
                else:
                    novo.append(Ins([_num(v) for v in p1 + p2 + p3], Op('c')))
            novo.append(Ins([], Op('h')))
        novo.append(Ins([], Op(op)))
    page.Contents = pdf.make_stream(pikepdf.unparse_content_stream(novo))
    return st


def conferir_margem(pdf_path, pagina=-1, margem=MARGEM_LARGURA):
    """Distância (mm) da tinta da arte (sem o fundo) até as bordas do quadro numa página do PDF
    (padrão: a última, que é a arte). Devolve dict(cima, baixo, esquerda, direita, ok)."""
    pdf = pikepdf.open(pdf_path)
    page = pdf.pages[pagina]
    mb = [float(v) for v in page.MediaBox]
    lo, hi = [math.inf, math.inf], [-math.inf, -math.inf]
    for item in _caminhos(page):
        if item[0] != 'path' or item[2] == 'n' or item[5] or not item[3] or _eh_fundo(item[3], mb):
            continue
        g = item[4]
        traco = item[2] in ('S', 's', 'B', 'B*', 'b', 'b*')
        e = g['lw'] * math.sqrt(abs(g['ctm'][0] * g['ctm'][3] - g['ctm'][1] * g['ctm'][2])) / 2 if traco else 0.0
        c = g['clip'] or (-math.inf, -math.inf, math.inf, math.inf)
        for s in item[3]:
            for q in s:
                for i in range(33):
                    p = _bz_pt(q, i / 32)
                    for k in (0, 1):
                        lo[k] = min(lo[k], max(p[k] - e, c[k])); hi[k] = max(hi[k], min(p[k] + e, c[k + 2]))
    K = 25.4 / 72
    r = dict(cima=round((mb[3] - hi[1]) * K, 3) + 0.0, baixo=round((lo[1] - mb[1]) * K, 3) + 0.0,
             esquerda=round((lo[0] - mb[0]) * K, 3) + 0.0, direita=round((mb[2] - hi[0]) * K, 3) + 0.0)
    r['ok'] = r['cima'] >= margem - 0.05 and r['baixo'] >= margem - 0.05     # o Corel desloca a página uns 0,03 mm
    return r


# ---------------------------------------------------------------- PDF -> SVG (para o Corel)
def pdf_para_svg(pdf_path, pagina, svg_path):
    """Converte uma página do PDF gerado (só caminhos) em SVG em mm, com um grupo por cor.
    Grupos de cores especiais recebem o nome da Pantone (ex.: PANTONE_330_C)."""
    K = 25.4 / 72
    pdf = pikepdf.open(pdf_path)
    page = pdf.pages[pagina]
    mbx = [float(v) for v in page.MediaBox]
    Wm, Hm = (mbx[2] - mbx[0]) * K, (mbx[3] - mbx[1]) * K
    cs_def = {}
    for k, v in page.Resources.get('/ColorSpace', {}).items():
        if isinstance(v, pikepdf.Array) and str(v[0]) == '/Separation':
            nm = str(v[1])[1:]
            lab = None
            if isinstance(v[2], pikepdf.Array) and str(v[2][0]) == '/Lab':
                lab = [float(x) for x in v[3].C1]
            cs_def[str(k)] = ('spot', nm, lab)
        else:
            cs_def[str(k)] = ('rgb', None, None)

    def hexrgb(r, g, b):
        return '#%02X%02X%02X' % tuple(int(round(max(0, min(1, t)) * 255)) for t in (r, g, b))

    def cor(kind, val):
        if kind[0] == 'spot':
            nm, lab = kind[1], kind[2]
            t = val[0] if val else 1.0
            r, g, b = lab_to_srgb(*lab) if lab else (0, 0, 0)
            r, g, b = (1 - t + t * r, 1 - t + t * g, 1 - t + t * b)
            return hexrgb(r, g, b), nm.replace(' ', '_')
        if kind[0] == 'gray':
            return hexrgb(val[0], val[0], val[0]), 'FICHA'
        return hexrgb(*val[:3]), 'FICHA'

    gs = dict(ctm=(1, 0, 0, 1, 0, 0), fk=('gray',), fv=(0,), sk=('gray',), sv=(0,), lw=1.0, dash=None, clip=None)
    stack, d, grupos, ordem, clips = [], [], {}, [], []

    def P(x, y):
        a, b, c, dd, e, f = gs['ctm']
        X, Y = a * x + c * y + e, b * x + dd * y + f
        return ((X - mbx[0]) * K, Hm - (Y - mbx[1]) * K)

    def F(*ps):
        return ' '.join(f"{p[0]:.3f} {p[1]:.3f}" for p in ps)

    def emit(svg, grupo):
        if grupo not in grupos:
            grupos[grupo] = []; ordem.append(grupo)
        grupos[grupo].append(svg)

    pend_clip, cur, ini = None, (0.0, 0.0), (0.0, 0.0)
    for ins in pikepdf.parse_content_stream(page):
        if isinstance(ins, pikepdf.ContentStreamInlineImage):
            continue
        o, op = list(ins.operands), str(ins.operator)
        if op == 'q': stack.append(dict(gs))
        elif op == 'Q': gs = stack.pop()
        elif op == 'cm': gs['ctm'] = _mul(tuple(float(x) for x in o), gs['ctm'])
        elif op == 'w': gs['lw'] = float(o[0])
        elif op == 'd': gs['dash'] = [float(x) for x in o[0]] or None
        elif op == 'cs': gs['fk'] = cs_def.get(str(o[0]), ('rgb', None, None))
        elif op == 'CS': gs['sk'] = cs_def.get(str(o[0]), ('rgb', None, None))
        elif op in ('scn', 'sc'): gs['fv'] = tuple(float(x) for x in o if not isinstance(x, pikepdf.Name))
        elif op in ('SCN', 'SC'): gs['sv'] = tuple(float(x) for x in o if not isinstance(x, pikepdf.Name))
        elif op == 'rg': gs['fk'], gs['fv'] = ('rgb', None, None), tuple(float(x) for x in o)
        elif op == 'RG': gs['sk'], gs['sv'] = ('rgb', None, None), tuple(float(x) for x in o)
        elif op == 'g': gs['fk'], gs['fv'] = ('gray',), (float(o[0]),)
        elif op == 'G': gs['sk'], gs['sv'] = ('gray',), (float(o[0]),)
        elif op in ('m', 'l'):
            cur = P(float(o[0]), float(o[1]))
            if op == 'm': ini = cur
            d.append(('M ' if op == 'm' else 'L ') + F(cur))
        elif op == 'c':
            p1, p2, cur = (P(float(o[i]), float(o[i + 1])) for i in (0, 2, 4))
            d.append('C ' + F(p1, p2, cur))
        elif op == 'v':
            p2, p3 = (P(float(o[i]), float(o[i + 1])) for i in (0, 2))
            d.append('C ' + F(cur, p2, p3)); cur = p3
        elif op == 'y':
            p1, p3 = (P(float(o[i]), float(o[i + 1])) for i in (0, 2))
            d.append('C ' + F(p1, p3, p3)); cur = p3
        elif op == 'h':
            d.append('Z'); cur = ini
        elif op == 're':
            x, y, w, h = (float(t) for t in o)
            ini = cur = P(x, y)
            d.append('M ' + F(cur) + ' L ' + F(P(x + w, y)) + ' L ' + F(P(x + w, y + h)) + ' L ' + F(P(x, y + h)) + ' Z')
        elif op in ('W', 'W*'):
            pend_clip = ('evenodd' if op == 'W*' else 'nonzero')
        elif op in ('f', 'F', 'f*', 'S', 's', 'B', 'B*', 'b', 'b*', 'n'):
            path = ' '.join(d)
            if pend_clip and path:
                cid = f'c{len(clips) + 1}'
                clips.append(f'<clipPath id="{cid}" clipPathUnits="userSpaceOnUse"><path clip-rule="{pend_clip}" d="{path}"/></clipPath>')
                gs['clip'] = cid
                pend_clip = None
            if op != 'n' and path:
                a, b, c, dd, e, f = gs['ctm']
                esc = math.sqrt(abs(a * dd - b * c)) * K
                fill = op in ('f', 'F', 'f*', 'B', 'B*', 'b', 'b*')
                stroke = op in ('S', 's', 'B', 'B*', 'b', 'b*')
                if op in ('s', 'b', 'b*'): path += ' Z'
                attrs = []
                grupo = None
                if fill:
                    hx, grupo = cor(gs['fk'], gs['fv'])
                    attrs.append(f'fill="{hx}" fill-rule="{"evenodd" if op in ("f*", "B*", "b*") else "nonzero"}"')
                else:
                    attrs.append('fill="none"')
                if stroke:
                    hx, g2 = cor(gs['sk'], gs['sv'])
                    grupo = grupo or g2
                    attrs.append(f'stroke="{hx}" stroke-width="{gs["lw"] * esc:.3f}"')
                    if gs['dash']:
                        attrs.append('stroke-dasharray="' + ' '.join(f'{x * esc:.3f}' for x in gs['dash']) + '"')
                if gs['clip']:
                    attrs.append(f'clip-path="url(#{gs["clip"]})"')
                emit(f'<path {" ".join(attrs)} d="{path}"/>', grupo)
            d = []
    out = ['<?xml version="1.0" encoding="UTF-8"?>',
           f'<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="{Wm:.3f}mm" height="{Hm:.3f}mm" viewBox="0 0 {Wm:.3f} {Hm:.3f}">']
    if clips:
        out.append('<defs>' + ''.join(clips) + '</defs>')
    for g in ordem:
        out.append(f'<g id="{g}">'); out.extend(grupos[g]); out.append('</g>')
    out.append('</svg>')
    open(svg_path, 'w', encoding='utf-8').write('\n'.join(out) + '\n')
    return svg_path
```