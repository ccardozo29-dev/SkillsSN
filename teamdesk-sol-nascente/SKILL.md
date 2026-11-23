---
name: teamdesk-sol-nascente
description: Use para consultar ou alterar o TeamDesk da Sol Nascente Papéis (banco 72265): pedidos, clientes, itens, OPs, clichês, faturas, estoque, compras e demais tabelas, pela API REST.
---

# TeamDesk: Sol Nascente Papéis

O TeamDesk é o sistema de gestão da Sol Nascente (pedidos, clientes, produção, estoque, compras, financeiro e RH). Banco **72265**. Acesso pela API REST v2 com o script `teamdesk.py` (abaixo).

## Credenciais e rede
- `TEAMDESK_TOKEN` (e `TEAMDESK_DB=72265`) no documento privado do Projeto `claude/credenciais.md` (ler com `project_read`), carregados nas variáveis de ambiente. Nunca gravar o token em arquivo da skill nem do repositório (o SkillsSN é público), nem repetir na resposta.
- Rede: `www.teamdesk.net` liberado pelo usuário (23/11/2026). Erro 403 no CONNECT = liberação não vale nesta conversa: avisar.
- O token (23/11/2026) tem escrita em todas as tabelas. O controle real é o perfil do token no TeamDesk; as regras abaixo valem mesmo assim.

## Regras
1. **Ler à vontade, escrever só com pedido explícito.** Criar, alterar ou apagar registro só quando o usuário pedir. Antes de gravar, mostrar a prévia (as funções de escrita sem `confirmar=True` só devolvem a prévia) e confirmar com o usuário quando a mudança não estiver clara ou afetar vários registros. Depois, dizer exatamente o que mudou (tabela, chave, colunas) e conferir os `erros` do retorno.
2. **Apagar:** só com pedido explícito, registro por registro listado ao usuário. Os registros vão para a lixeira do TeamDesk.
3. **RH é sensível:** Funcionários, Atestados, Exames, Banco de Horas, Documentos de Funcionários e Recebimentos de EPIs têm dados pessoais e de saúde (CPF, salário, dados bancários, atestados). Não consultar sem que o pedido precise disso, pedir só as colunas necessárias e nunca copiar esses dados para fora do TeamDesk (arquivos, GitHub, Dropbox, outros sistemas).
4. **Colunas calculadas** (lookup, fórmula, resumo e as automáticas) são só leitura: escrever na coluna de origem.
5. Responder ao usuário com o resultado em linguagem simples (tabela curta, totais), não com o JSON cru. Para relatórios grandes, gerar planilha.

## Mapa do banco
O mapa completo (39 tabelas, colunas, tipos, opções e referências) está no documento do Projeto `claude/teamdesk-mapa.md`. Consultar antes de montar filtros. Sem acesso ao Projeto, usar `td.tabelas()` e `td.colunas('<Tabela>')`.

Tabelas mais usadas (nome no singular para a API, alias, chave):
- **Pedido** (`t_680325`, chave `Pedido #`, ex. `I202611-23509`): Cliente, Empresa, Situação (NORMAL / SUSPENSO / CANCELADO), Etapa, Previsão, Total Pedido, Vendedor, Canal, Pagamento, Confirmado; arte: Referência da Arte, Arte (ARTE NOVA / REIMPRESSÃO / SEM ARTE), Arte Aprovada e Arte Vetorial (anexos), Fechamento, Cores, Corte (ALEATÓRIO / INDIVIDUAL), Largura, Clicheria, Clichê aprovado / recebido.
- **Item** (`t_680327`, chave `Id`): itens do pedido (Pedido, Produto, Formato, Qtd, Unid, Substrato, Cores, Valor, Impressora, Clichê a utilizar).
- **Cliente** (`t_680326`, chave `Código #`, nome `Empresa`): CNPJ / CPF, contato, endereço, Vendedor, Canal, Segmento, `MC_SubscriberID` (ligação com o ManyChat), resumos de compras e atrasos.
- **OP** (`t_694307`, chave `OP`) e **OPs x Itens** (`t_694308`): ordens de produção, máquina, prazos e conclusão.
- **Clichê** (`t_693021`): Identificação, Localização, Fechamento, Largura, Arte, Clicheria.
- **Fatura** (`t_727773`): Vencimento, Pago em, Valor, Situação, Cobrança.
- **Material**, **Estoque** (movimentos), **Compra** e **Compras x Itens**, **Fornecedor**, **Produto**, **Tabela de Preço**, **Oportunidade**.

## Fórmulas de filtro (TeamDesk)
- Texto entre aspas duplas: `[Situação]="NORMAL"`; combinar com `and` / `or` / `not`.
- Vazio: `IsNull([Coluna])` e `not IsNull([Coluna])` (não existe `IsNotNull`).
- Datas: `Today()`, `Days(n)`; colunas de data e hora (ex. `Criado em`) precisam de `ToDate(...)` para comparar com datas: `ToDate([Criado em])>=Today()-Days(7)`.
- Ordem: `'Coluna//DESC'`. A API devolve no máximo 500 linhas por chamada; `consultar()` pagina sozinho.
- Datas voltam como texto ISO; checkbox como true/false; anexos como `arquivo.pdf;revisão;guid`.

## Uso
```python
import teamdesk as td                     # TEAMDESK_TOKEN no ambiente
td.consultar('Pedido', ['Pedido #', 'Empresa', 'Etapa', 'Previsão'],
             filtro='[Situação]="NORMAL" and [Arte]="ARTE NOVA"', ordem=['Criado em//DESC'], limite=20)
td.registro('Pedido', 'I202611-23509')
td.contar('Pedido', 'ToDate([Criado em])>=Today()-Days(7)')
td.anexo('Pedido', 'Arte Aprovada', 'I202611-23506', 'arte.pdf')     # baixa o arquivo
td.atualizar('Pedido', [{'Pedido #': 'I202611-23509', 'Referência da Arte': 'Van-Dal'}])   # só prévia
td.atualizar('Pedido', [{'Pedido #': 'I202611-23509', 'Referência da Arte': 'Van-Dal'}], confirmar=True)
```
Também: `criar()`, `criar_ou_atualizar(..., match='<coluna única>')`, `apagar()` (todas com prévia por padrão), `tabelas()`, `colunas()`.

## Ligação com a skill de artes (arte-flexo-sol-nascente)
- O pedido traz o que a arte precisa: Referência da Arte (nome da arte), Fechamento (cilindro), Cores, Corte, Largura e os itens (formato, substrato). Usar esses dados quando o usuário pedir a arte de um pedido.
- Anexar a arte aprovada no pedido (`Arte Aprovada`) ainda não está implementado no script (envio de anexo exige `multipart/related`): por enquanto, avisar o usuário.

## teamdesk.py
```python
"""Acesso ao TeamDesk da Sol Nascente Papéis (banco 72265) pela API REST v2.

Credenciais nas variáveis de ambiente TEAMDESK_TOKEN e TEAMDESK_DB (padrão 72265); nunca gravar
o token em arquivo do repositório, que é público.

Leitura:
    import teamdesk as td
    td.consultar('Pedido', ['Pedido #', 'Empresa', 'Etapa'],
                 filtro='[Situação]="NORMAL" and [Arte]="ARTE NOVA"',
                 ordem=['Criado em//DESC'], limite=20)
    td.registro('Pedido', 'I202611-23509')            # um registro pela chave
    td.contar('Pedido', '[Situação]="NORMAL"')
Escrita (só com pedido explícito do usuário; sem confirmar=True só mostra o que faria):
    td.atualizar('Pedido', [{'Pedido #': 'I202611-23509', 'Referência da Arte': 'Van-Dal v2'}], confirmar=True)
"""
import json, os, urllib.parse
import requests

API = 'https://www.teamdesk.net/secure/api/v2'
LOTE = 500                 # máximo de linhas por select e de registros por delete na API


class ErroTeamDesk(Exception):
    pass


def _base():
    tok = os.environ.get('TEAMDESK_TOKEN', '').strip()
    if not tok:
        raise ErroTeamDesk('TEAMDESK_TOKEN não definida: leia as credenciais do documento do Projeto.')
    db = os.environ.get('TEAMDESK_DB', '72265').strip()
    return f'{API}/{db}', {'Authorization': f'Bearer {tok}'}


def _url(tabela, metodo):
    base, _ = _base()
    return f"{base}/{urllib.parse.quote(tabela, safe='')}/{metodo}.json"


def _resp(r):
    try:
        d = r.json()
    except ValueError:
        raise ErroTeamDesk(f'HTTP {r.status_code}: {r.text[:300]}')
    if r.status_code >= 400:
        raise ErroTeamDesk(f'HTTP {r.status_code}: {d}')
    return d


def _get(tabela, metodo, params):
    _, h = _base()
    return _resp(requests.get(_url(tabela, metodo), headers=h, params=params, timeout=120))


# ---------------------------------------------------------------- estrutura
def tabelas():
    """Lista das tabelas: [(nome no plural, nome no singular, alias)]."""
    base, h = _base()
    d = _resp(requests.get(f'{base}/describe.json', headers=h, timeout=60))
    return [(t['recordsName'], t['recordName'], t['alias']) for t in d['tables']]


def colunas(tabela):
    """Colunas da tabela: [(nome, tipo, kind, somente_leitura, referência)]."""
    d = _get(tabela, 'describe', {})
    return [(c['name'], c['type'], c['kind'], c['readOnly'], (c.get('reference') or {}).get('table'))
            for c in d['columns']]


# ---------------------------------------------------------------- leitura
def consultar(tabela, colunas=None, filtro=None, ordem=None, limite=None, pular=0):
    """Linhas da tabela (nome no singular, ex. 'Pedido', ou alias 't_680325').
    colunas: lista de nomes (None = todas as editáveis; '*' junto de outras soma as editáveis).
    filtro: fórmula do TeamDesk que devolve verdadeiro/falso, ex. '[Situação]="NORMAL"' ou
            'ToDate([Criado em])>=Today()-Days(7)'. ordem: ['Coluna//DESC', ...]. limite: None = todas (pagina de 500 em 500)."""
    linhas, pulo = [], pular
    while True:
        top = LOTE if limite is None else min(LOTE, limite - len(linhas))
        if top <= 0:
            break
        p = [('top', top), ('skip', pulo)]
        p += [('column', c) for c in (colunas or [])]
        p += [('sort', s) for s in (ordem or [])]
        if filtro:
            p.append(('filter', filtro))
        lote = _get(tabela, 'select', p)
        linhas += lote
        pulo += len(lote)
        if len(lote) < top:
            break
    return linhas


def contar(tabela, filtro=None):
    """Quantidade de registros que atendem ao filtro (lê só a coluna chave)."""
    chave = _get(tabela, 'describe', {})['key']
    return len(consultar(tabela, [chave], filtro))


def registro(tabela, chave, colunas=None):
    """Um registro pela chave (ex. 'I202611-23509') ou None."""
    p = [('key', chave)] + [('column', c) for c in (colunas or [])]
    d = _get(tabela, 'retrieve', p)
    return d[0] if d else None


def anexo(tabela, coluna, chave, destino):
    """Baixa o arquivo de uma coluna de anexo (ex. 'Arte Aprovada' do pedido) para `destino`."""
    base, h = _base()
    q = lambda x: urllib.parse.quote(x, safe='')
    r = requests.get(f'{base}/{q(tabela)}/{q(coluna)}/attachment', headers=h, params={'key': chave}, timeout=300)
    if r.status_code != 200:
        raise ErroTeamDesk(f'anexo: HTTP {r.status_code} {r.text[:200]}')
    open(destino, 'wb').write(r.content)
    return destino


# ---------------------------------------------------------------- escrita (só com pedido explícito)
def _escrever(metodo, tabela, linhas, confirmar, match=None, workflow=True):
    if not confirmar:
        return dict(previa=True, metodo=metodo, tabela=tabela, match=match, linhas=linhas)
    _, h = _base()
    params = {}
    if match:
        params['match'] = match
    if not workflow:
        params['workflow'] = 0
    r = requests.post(_url(tabela, metodo), headers={**h, 'Content-Type': 'application/json'},
                      params=params, data=json.dumps(linhas, ensure_ascii=False).encode(), timeout=300)
    d = _resp(r)
    erros = [x for x in d if x.get('status', 200) >= 400]
    return dict(previa=False, resultado=d, erros=erros)


def criar(tabela, linhas, confirmar=False):
    """Cria registros. linhas: [{coluna: valor}]. Sem confirmar=True só devolve a prévia."""
    return _escrever('create', tabela, linhas, confirmar)


def atualizar(tabela, linhas, confirmar=False, match=None):
    """Atualiza registros identificados pela chave (ou por '@row.id', ou pela coluna única `match`)."""
    return _escrever('update', tabela, linhas, confirmar, match)


def criar_ou_atualizar(tabela, linhas, match, confirmar=False):
    """Upsert: atualiza quem já existe pela coluna única `match` e cria os demais."""
    return _escrever('upsert', tabela, linhas, confirmar, match)


def apagar(tabela, chaves, confirmar=False):
    """Apaga registros pela chave (vão para a lixeira do TeamDesk). Sem confirmar=True só devolve a prévia."""
    if not confirmar:
        return dict(previa=True, metodo='delete', tabela=tabela, chaves=list(chaves))
    res = []
    chaves = list(chaves)
    for i in range(0, len(chaves), LOTE):
        res += _get(tabela, 'delete', [('key', k) for k in chaves[i:i + LOTE]])
    return dict(previa=False, resultado=res, erros=[x for x in res if x.get('status', 200) >= 400])
```
