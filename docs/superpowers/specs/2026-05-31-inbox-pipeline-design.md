# Spec: Pipeline _inbox/ — LAB360°

**Data:** 2026-05-31  
**Status:** Aprovado pelo usuário

## Problema

O fluxo atual para processar um novo cliente exige vários passos manuais: rodar `/novo-cliente`, mover arquivos manualmente para as pastas certas, pedir para gerar o HTML, pedir para criar a página no Notion. São passos demais para uma operação que deveria ser simples.

## Solução

Criar a pasta `_inbox/` na raiz do projeto. Heleno joga os arquivos do cliente lá com prefixo de identificação, roda um único comando, e recebe as URLs ao final.

---

## Convenção de nomes na `_inbox/`

| Prefixo | Tipo | Exemplo |
|---------|------|---------|
| `form-*` | Formulário Tally (PDF) | `form-studioalma.pdf` |
| `design-*` | Design Canva (PDF) | `design-studioalma.pdf` |
| `texto-*` | Texto para Notion (opcional) | `texto-studioalma.txt` |

- Sempre um cliente por vez na inbox
- Após processamento, inbox fica vazia

---

## Comando

```
/novo-pedido Nome do Cliente — tipo-de-servico
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais`

Exemplos:
```
/novo-pedido Studio Alma — identidade-visual
/novo-pedido Café Raiz — logo
/novo-pedido Maria Souza — redes-sociais
```

---

## Pipeline completo (10 passos)

1. **Ler `_inbox/`** — identificar arquivos por prefixo (`form-*`, `design-*`, `texto-*`)
2. **Criar pastas** — `clientes/[slug]/[tipo]/Formulario/`, `Design/`, `Texto/`
3. **Mover arquivos** — cada um para a subpasta correta
4. **Ler os PDFs** — formulário (brief do cliente) + design (identidade visual feita por Heleno)
5. **Gerar HTML** — invocar `huashu-design` com as seções do tipo de serviço (conforme design system seção 8A/8B/8C)
6. **Pausar para revisão** — mostrar resumo do HTML gerado (seções, cores, tipografia detectadas)
7. **Aguardar aprovação de Heleno**
8. **Commit + push** — `git add`, `git commit -m "Gerar [tipo]: [Cliente]"`, `git push` → Vercel deploya em ~30s
9. **Criar página no Notion** (somente se existir `texto-*`) — via MCP, retornar URL
10. **Entregar URLs:**
    - URL Vercel: `https://lab360-entregas.vercel.app/clientes/[slug]/[tipo]/`
    - URL Notion: `https://notion.so/...` (se houver texto)

---

## Tratamento de erros

| Situação | Comportamento |
|----------|--------------|
| Nenhum `form-*` na inbox | Parar e avisar: "Não encontrei o formulário em `_inbox/`" |
| Nenhum `design-*` na inbox | Parar e avisar: "Não encontrei o design em `_inbox/`" |
| Dois `form-*` na inbox | Listar arquivos e perguntar qual usar |
| Dois `design-*` na inbox | Listar arquivos e perguntar qual usar |
| Cliente já tem esse serviço | Avisar e perguntar se deve sobrescrever |

---

## Mudanças no projeto

### Criar
- `_inbox/` com `.gitkeep`
- `.gitignore` com entradas para `_inbox/*.pdf`, `_inbox/*.txt`, `_inbox/*.md`
- `.claude/commands/novo-pedido.md` — o novo comando principal

### Manter
- `.claude/commands/novo-cliente.md` — continua funcionando para criar estrutura sem inbox

### Atualizar
- `CLAUDE.md` — adicionar seção do novo fluxo como fluxo principal
- `_template/LAB360-design-system.md` — já está atualizado com 8A/8B/8C

---

## Estrutura de pastas resultante

```
_inbox/                          ← arquivos temporários, ignorado pelo git
  .gitkeep
clientes/
  studioalma/
    identidade-visual/
      Formulario/
        form-studioalma.pdf
      Design/
        design-studioalma.pdf
      Texto/
        texto-studioalma.txt     ← somente se existir
      index.html                 ← gerado por Claude
.gitignore
```

---

## Fluxo Heleno — do pedido à entrega

```
Cliente envia formulário Tally
        ↓
Heleno exporta: form-*.pdf + design-*.pdf (+ texto-*.txt se tiver)
        ↓
Heleno joga tudo em _inbox/
        ↓
Heleno abre Claude Code e digita:
  /novo-pedido Nome — tipo
        ↓
Claude processa, gera HTML, mostra resumo
        ↓
Heleno aprova
        ↓
Claude commita + push + Notion (se tiver texto)
        ↓
Heleno recebe as URLs e repassa ao cliente
```
