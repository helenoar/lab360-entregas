# LAB 360° — Instruções de Projeto

## 🔴 CRÍTICO: Design System LAB360° ≠ Identidade Visual do Cliente

### Design System LAB360° = ESTRUTURA do documento
- Arquivo: `_template/LAB360-design-system.md`
- É SEMPRE IGUAL para todos os clientes
- Define: moldura (fundo preto, nav dots, status bar, canvas rizoma)
- É a MOLDURA/APRESENTAÇÃO do documento

### Identidade Visual do Cliente = CONTEÚDO do documento
- Fonte: PDF do Canva em `clientes/[clienteslug]/[projetoslug]/Design/`
- MUDA a cada cliente
- Define: paleta, tipografia, logo DO CLIENTE
- É o que está DENTRO da moldura

**O HTML mostra a identidade visual do cliente, apresentada na estrutura LAB360°.**

---

**SEMPRE ler Design System antes de gerar HTML:**
```
_template/LAB360-design-system.md
```
Leitura é para entender a ESTRUTURA, não para replicar cores/tipografia no cliente.

## Estrutura de Pastas

```
clientes/[clienteslug]/[projetoslug]/
  Formulario/           ← brief do cliente (PDF ou .txt)
  Design/               ← PDF do Canva com a identidade visual
  Texto/                ← .txt/.md para Notion (opcional)
  identidade-visual/
    index.html          ← Claude gera e commita aqui
index.html              ← dashboard na raiz (lista todos os clientes)
```

### 🔴 Regra de nomenclatura de pastas (OBRIGATÓRIO)

Slugs **sempre** em minúsculas, sem espaços, sem acentos, sem hífens.

| Nome real | Slug correto |
|-----------|-------------|
| Quik Cia de Dança | `quikciadadanca` |
| MOVER CONSCIENTE | `moverconsciente` |
| Studio Alma | `studioalma` |

URL resultante: `lab360-entregas.vercel.app/clientes/[clienteslug]/[projetoslug]/identidade-visual/`

## Criar novo cliente

Usar o comando `/novo-cliente`:
```
/novo-cliente Nome do Cliente — Nome do Projeto
```
Cria as 4 pastas, adiciona card no `index.html` raiz e faz commit.

## Workflow de Geração de HTML

1. Ler `Formulario/` — brief do cliente em voz própria
2. Ler `Design/` — PDF do design visual
3. Ler `_template/LAB360-design-system.md` — estrutura LAB 360°
4. **Invocar skill `huashu-design`** para gerar o HTML
5. Salvar em `identidade-visual/index.html`
6. `git add` + `git commit` + `git push`
7. Vercel auto-deploya em ~30s
8. Entregar URL ao Heleno

### Seções do HTML (sempre as mesmas)

1. Hero — nome do projeto + eyebrow + tagline
2. Conceito — origem, intenção, contexto (do formulário)
3. Paleta de Cores — swatches com hex, nome, sensação
4. Tipografia — fontes com exemplos, hierarquia
5. Logo — variações, área de respiro, usos
6. Voz & Tom — como a marca fala, palavras-chave (do formulário)
7. Aplicações — exemplos visuais de aplicação

Se falta material em alguma seção, adaptar com o que existe — **nunca omitir a seção**.

## Hosting

- Projeto Vercel: `lab360-entregas.vercel.app`
- **Sem `vercel.json`** — Vercel detecta site estático automaticamente
- Git push → deploy automático em ~30s

## Notion

Usado apenas para documentos de texto puro (conteúdo de posts, revisões, etc.). Claude cria a página diretamente no Notion via MCP a partir do arquivo em `Texto/`.

## Git

Claude gerencia tudo. Heleno não toca em git.

Commits sempre com:
```
git commit -m "Gerar identidade visual: [Cliente] — [Projeto]"
```

## Comunicação

- Sempre português brasileiro
- Ao terminar, entregar a URL pronta para Heleno enviar ao cliente
- Tom direto, sem rodeios

## Contato

WhatsApp do Heleno (aprovações): (31) 99686-2968
