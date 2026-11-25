---
name: verificacao-pedidos-sol-nascente
description: Use para conferir pedidos do TeamDesk da Sol Nascente Papéis em busca de inconsistências. Verificação 1 (OC) compara a ordem de compra anexada (Anexar OC) com o pedido: prazo, itens, preços, unidades, quantidades, CNPJ, endereços, pagamento e frete CIF/FOB. A conferência de arte fica na skill arte-flexo-sol-nascente.
---

# Verificação de pedidos: Sol Nascente Papéis

Confere pedidos do TeamDesk (banco 72265) e aponta inconsistências antes que virem problema na produção, no faturamento ou na entrega. **Só lê**: nunca altera o pedido. Se o usuário pedir correção, use a skill `teamdesk-sol-nascente` (prévia e confirmação).

Fora do escopo: arte, clichê e arquivos gráficos. Isso continua na skill `arte-flexo-sol-nascente`.

Verificações disponíveis:

| Nº | Nome | O que compara |
|---|---|---|
| 1 | OC | Ordem de compra do cliente (campo `Anexar OC`) × pedido |

Quando o usuário pedir "verificar o pedido X" sem dizer qual verificação, rode todas as disponíveis.

## Preparação

1. **Acesso ao TeamDesk:** esta skill usa o `teamdesk.py` da skill `teamdesk-sol-nascente` (mesmas credenciais e regras). Salve o bloco `teamdesk.py` daquela skill no diretório de trabalho. `TEAMDESK_TOKEN` fica no documento privado do Projeto `claude/credenciais.md`. Nunca grave o token em arquivo nem o repita na resposta.
2. Salve o bloco `verifica_pedido.py` (no fim desta skill) no mesmo diretório.
3. Dependências: `pip install requests pdfplumber --break-system-packages`. Para OC escaneada também é preciso `pdftoppm` (poppler-utils).

## Verificação 1: OC

### Coleta
```python
import verifica_pedido as vp
d = vp.coletar('23538')        # número curto (fim da chave) ou chave completa 'I202611-23538'
print(vp.resumo(d))            # pedido, itens, cliente e texto da OC, prontos para comparar
```
`coletar` localiza o pedido, lê **todas** as colunas do Pedido, dos Itens e do Cliente (inclusive lookups e fórmulas), baixa o anexo `Anexar OC` para `./oc_<pedido>/` e extrai o texto. Quando a OC é escaneada (pouco texto), as páginas viram PNG em `d['oc']['imagens']`: leia as imagens com a ferramenta Read e transcreva os dados antes de comparar.

- Pedido sem OC anexada: informe "OC não anexada" e encerre a verificação 1. É uma pendência a relatar, não um erro.
- Mais de um pedido com o mesmo número curto: pergunte qual, mostrando as chaves completas.
- Anexo que não é PDF nem imagem (planilha, e-mail): abra com a ferramenta adequada (skill `xlsx` etc.) e siga.

### O que conferir

Leia a OC inteira antes de comparar. Para cada ponto, use um destes resultados:
- ✅ **OK**: confere.
- ⚠️ **Atenção**: provavelmente certo, mas exige olhar humano (conversão de unidade, arredondamento, descrição ambígua, campo do pedido vazio).
- ❌ **Divergente**: não confere.
- ➖ **Não consta na OC**: a OC não informa. Não é erro do pedido.

1. **Identificação**
   - CNPJ do cliente na OC × `CNPJ / CPF` do Cliente do pedido. Compare só os dígitos. A OC costuma ter dois CNPJs (comprador e fornecedor): use o do comprador. Filial diferente (mesma raiz de 8 dígitos, final diferente) é ❌, porque muda o faturamento.
   - Fornecedor da OC × `Empresa` do pedido (Sol Nascente ou Fibra). OC emitida para a outra empresa é ❌.
   - Número da OC: se o pedido tiver um campo com o número da OC do cliente, compare. Senão, só informe o número no cabeçalho.
2. **Prazo:** data de entrega da OC × `Prazo Cliente`.
   - Datas iguais: ✅. Diferentes: ❌, mostrando as duas datas e a diferença em dias.
   - OC com prazo em dias ("30 dias"): calcule a partir da data de emissão da OC e marque ⚠️ informando o cálculo.
   - Datas diferentes por item: compare cada uma. `Prazo Cliente` deve ser a mais cedo.
   - `Prazo Cliente` vazio com prazo na OC: ❌.
3. **Itens:** case cada item da OC com um item do pedido.
   - Os nomes **não** precisam bater: o sistema do cliente usa a descrição dele e o nosso usa a nossa. Case pelo conteúdo: tipo de produto (papel de seda, etc.), formato/medidas, gramatura, cores, referência da arte e código do cliente, se houver. Não aponte diferença só de nome. Se não for possível casar com segurança, marque ⚠️ e diga por quê.
   - **Quantidade:** igual na mesma unidade: ✅. Em unidades diferentes (milheiro × unidade, caixa × folha, kg × folha), converta só quando a relação for evidente (1 milheiro = 1.000 unidades) e marque ⚠️ mostrando a conta. Sem relação evidente: ❌.
   - **Unidade de medida:** unidade da OC × `Unid` do item. Sinônimos (UN/UND/PC/PÇ; MIL/MILH/MILHEIRO; FL/FOLHA) contam como iguais.
   - **Preço unitário:** compare na mesma unidade. Diferença de até R$ 0,01 por unidade (arredondamento) é ✅. Acima disso: ❌, com os dois valores e a diferença total no item (diferença × quantidade).
   - **Total:** some os itens da OC e compare com `Total Pedido`. Explique a diferença (frete, IPI, desconto) quando a OC deixar claro.
   - Item da OC que não está no pedido, ou item do pedido que não está na OC: ❌.
4. **Endereços**, quando a OC trouxer endereço de entrega ou de cobrança: compare com o endereço do Cliente e com qualquer endereço de entrega do pedido. Compare CEP pelos dígitos. Logradouro, número e cidade devem bater, aceitando abreviações (R./Rua, Av./Avenida) e acentos. Entrega em endereço diferente do cadastro sem esse endereço registrado no pedido: ❌ (o caminhão vai para o lugar errado).
5. **Condição de pagamento:** OC × `Pagamento` do pedido. Compare pelo conteúdo: mesmos prazos em dias (28/35/42 DDL = 28/35/42 dias), mesma forma (boleto, depósito, antecipado). Diferença de prazo ou de forma: ❌.
6. **Frete:** CIF ou FOB na OC × campo de frete do pedido. CIF = frete por conta do fornecedor (nós); FOB = por conta do cliente. Se a OC indicar transportadora, compare também. OC sem a informação: ➖.

Nomes de colunas: os mencionados acima são os conhecidos. Para os demais (frete, endereço de entrega, nº da OC), procure no `resumo` a coluna equivalente. Se o pedido não tiver campo para algo que a OC informa, diga isso em vez de marcar divergência.

### Resposta ao usuário
Linguagem simples, sem JSON:

1. Cabeçalho: pedido (chave completa), cliente, nº e data da OC.
2. **Veredito** em uma linha: "Sem divergências", "N pontos de atenção" ou "N divergências".
3. Tabela: Verificação | OC | Pedido | Resultado (identificação, prazo, total, endereços, pagamento, frete).
4. Tabela de itens: Item OC | Item pedido | Qtd/Unid OC × pedido | Preço OC × pedido | Resultado.
5. Lista curta do que corrigir ou confirmar com o cliente, começando pelos ❌.

Não corrija nada sozinho. Ofereça a correção pelo `teamdesk-sol-nascente`, quando couber.

## verifica_pedido.py
```python
"""Coleta de dados para a verificação de pedidos da Sol Nascente (somente leitura).

Requer teamdesk.py (skill teamdesk-sol-nascente) no mesmo diretório e TEAMDESK_TOKEN no ambiente.

    import verifica_pedido as vp
    d = vp.coletar('23538')
    print(vp.resumo(d))
"""
import os, re, json, shutil, subprocess
import teamdesk as td

COL_OC = 'Anexar OC'
_cache_cols = {}


def todas_colunas(tabela):
    """Nomes de todas as colunas da tabela (inclusive lookups e fórmulas)."""
    if tabela not in _cache_cols:
        _cache_cols[tabela] = [c[0] for c in td.colunas(tabela)]
    return _cache_cols[tabela]


def achar_pedido(numero):
    """Chave completa do pedido a partir do número curto ('23538') ou da própria chave."""
    numero = str(numero).strip()
    if '-' in numero:
        return [numero] if td.registro('Pedido', numero, ['Pedido #']) else []
    n = re.sub(r'\D', '', numero)
    linhas = td.consultar('Pedido', ['Pedido #'], filtro=f'Right([Pedido #],{len(n)})="{n}"')
    return [l['Pedido #'] for l in linhas if re.sub(r'\D', '', l['Pedido #'].split('-')[-1]) == n]


def _ler(tabela, filtro=None, chave=None):
    cols = todas_colunas(tabela)
    if chave is not None:
        return td.registro(tabela, chave, cols)
    return td.consultar(tabela, cols, filtro=filtro)


def _cliente(pedido):
    """Registro do cliente do pedido: tenta a referência 'Cliente' como chave e, se falhar, pelo nome."""
    ref = pedido.get('Cliente')
    if ref in (None, ''):
        return None
    try:
        r = _ler('Cliente', chave=ref)
        if r:
            return r
    except td.ErroTeamDesk:
        pass
    nome = str(ref).replace('"', '""')
    r = _ler('Cliente', filtro=f'[Empresa]="{nome}"')
    return r[0] if len(r) == 1 else None


def _texto_pdf(caminho):
    try:
        import pdfplumber
        with pdfplumber.open(caminho) as pdf:
            return '\n\n'.join(f'--- página {i+1} ---\n' + (p.extract_text() or '')
                               for i, p in enumerate(pdf.pages))
    except Exception as e:
        return f'[erro ao extrair texto: {e}]'


def _imagens_pdf(caminho, pasta):
    if not shutil.which('pdftoppm'):
        return []
    base = os.path.join(pasta, 'oc_pagina')
    subprocess.run(['pdftoppm', '-r', '150', '-png', caminho, base], check=False)
    return sorted(os.path.join(pasta, f) for f in os.listdir(pasta) if f.startswith('oc_pagina'))


def baixar_oc(chave, valor, pasta):
    """Baixa o anexo da OC e extrai o texto; páginas viram PNG quando o PDF não tem texto."""
    if not valor:
        return None
    nome = str(valor).split(';')[0] or 'oc'
    nome = re.sub(r'[^\w.\- ]', '_', nome)
    os.makedirs(pasta, exist_ok=True)
    caminho = td.anexo('Pedido', COL_OC, chave, os.path.join(pasta, nome))
    ext = os.path.splitext(nome)[1].lower()
    oc = dict(arquivo=caminho, texto='', imagens=[])
    if ext == '.pdf':
        oc['texto'] = _texto_pdf(caminho)
        if len(re.sub(r'\s|--- página \d+ ---', '', oc['texto'])) < 200:
            oc['imagens'] = _imagens_pdf(caminho, pasta)
    elif ext in ('.png', '.jpg', '.jpeg', '.gif', '.bmp', '.tif', '.tiff', '.webp'):
        oc['imagens'] = [caminho]
    else:
        oc['texto'] = f'[anexo {ext}: abrir com a ferramenta adequada]'
    return oc


def coletar(numero, pasta=None):
    """Pedido, itens, cliente e OC de um pedido. Erro se não achar ou se houver mais de um."""
    chaves = achar_pedido(numero)
    if not chaves:
        raise td.ErroTeamDesk(f'pedido {numero} não encontrado')
    if len(chaves) > 1:
        raise td.ErroTeamDesk(f'mais de um pedido para {numero}: {chaves}')
    chave = chaves[0]
    pedido = _ler('Pedido', chave=chave)
    itens = _ler('Item', filtro='[Pedido]="%s"' % chave.replace('"', '""'))
    cliente = _cliente(pedido)
    pasta = pasta or f'oc_{chave}'
    oc = baixar_oc(chave, pedido.get(COL_OC), pasta)
    return dict(chave=chave, pedido=pedido, itens=itens, cliente=cliente, oc=oc)


def _limpo(reg):
    """Tira colunas vazias e de sistema para o resumo ficar legível."""
    fora = ('@row.id', 'Modificado', 'Criado por', 'Modificado por', 'Modified', 'Created By', 'Last Modified By')
    return {k: v for k, v in (reg or {}).items() if v not in (None, '', [], False) and not k.startswith(fora)}


def resumo(d):
    """Texto com tudo o que a comparação precisa."""
    partes = [f"PEDIDO {d['chave']}", json.dumps(_limpo(d['pedido']), ensure_ascii=False, indent=1),
              f"ITENS ({len(d['itens'])})"]
    partes += [json.dumps(_limpo(i), ensure_ascii=False, indent=1) for i in d['itens']]
    partes += ['CLIENTE', json.dumps(_limpo(d['cliente']), ensure_ascii=False, indent=1) if d['cliente']
               else '(cliente não localizado: conferir manualmente)']
    oc = d['oc']
    if not oc:
        partes.append('OC: não anexada')
    else:
        partes.append(f"OC: {oc['arquivo']}")
        if oc['imagens']:
            partes.append('Imagens para leitura visual: ' + ', '.join(oc['imagens']))
        partes.append(oc['texto'])
    return '\n\n'.join(partes)
```
