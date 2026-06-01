# Pipeline _inbox/ — Plano de Implementação

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar o comando `/novo-pedido` e a pasta `_inbox/` para que Heleno jogue os arquivos do cliente lá e receba as URLs do Vercel e Notion com um único comando.

**Architecture:** Sistema baseado em comandos Claude (`.claude/commands/`). O comando `/novo-pedido` lê `_inbox/`, cria a estrutura de pastas, move os arquivos, delega a geração do HTML ao `huashu-design`, faz commit/push e cria a página no Notion se houver texto. Nenhum código executável — tudo são instruções em markdown que o Claude interpreta.

**Tech Stack:** Claude Code commands (markdown), Bash (mkdir/mv), git, Vercel (auto-deploy via push), Notion MCP

---

## Arquivos a criar/modificar

| Ação | Arquivo | Responsabilidade |
|------|---------|-----------------|
| Criar | `_inbox/.gitkeep` | Manter pasta rastreada pelo git |
| Modificar | `.gitignore` | Ignorar PDFs/txts na inbox |
| Criar | `.claude/commands/novo-pedido.md` | Comando principal — o pipeline completo |
| Modificar | `CLAUDE.md` | Documentar novo fluxo como fluxo principal |

---

## Task 1: Criar `_inbox/` e atualizar `.gitignore`

**Files:**
- Criar: `_inbox/.gitkeep`
- Modificar: `.gitignore`

- [ ] **Passo 1: Criar a pasta `_inbox/` com `.gitkeep`**

```bash
mkdir -p "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/_inbox"
touch "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/_inbox/.gitkeep"
```

- [ ] **Passo 2: Adicionar entradas no `.gitignore`**

Abrir `.gitignore` e adicionar ao final:

```
# Inbox — arquivos temporários de clientes (não commitar)
_inbox/*.pdf
_inbox/*.txt
_inbox/*.md
_inbox/*.png
_inbox/*.jpg
_inbox/*.jpeg
```

- [ ] **Passo 3: Verificar que o `.gitkeep` será commitado mas os PDFs não**

```bash
cd "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°"
git status
```

Esperado: `_inbox/.gitkeep` aparece como untracked. Se criar um `_inbox/teste.pdf` temporariamente, ele não deve aparecer no `git status`.

- [ ] **Passo 4: Commit**

```bash
git add _inbox/.gitkeep .gitignore
git commit -m "chore: criar _inbox/ e atualizar .gitignore"
```

---

## Task 2: Criar o comando `/novo-pedido`

**Files:**
- Criar: `.claude/commands/novo-pedido.md`

Este arquivo é o coração do sistema. Ele define em linguagem natural o que o Claude deve fazer quando Heleno escrever `/novo-pedido`. Precisa ser detalhado o suficiente para que o Claude execute corretamente sem contexto adicional.

- [ ] **Passo 1: Criar `.claude/commands/novo-pedido.md` com o conteúdo completo**

Conteúdo exato do arquivo:

````markdown
# Comando: novo-pedido

Processa um novo pedido de cliente do LAB 360° de ponta a ponta: lê os arquivos da `_inbox/`, cria estrutura de pastas, gera o HTML de entrega, commita + push e cria a página no Notion se houver texto.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Maria Souza — redes-sociais`

## O que fazer — passo a passo

### 1. Derivar slug e tipo de serviço

A partir de `$ARGUMENTS`, extrair:
- **clienteNome**: parte antes do `—` (ex: `Studio Alma`)
- **tipoServico**: parte depois do `—` (ex: `identidade-visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem hífens (ex: `studioalma`)

Regras de slug: á→a, ã→a, â→a, é→e, ê→e, í→i, ó→o, ô→o, õ→o, ú→u, ç→c, remover espaços.

Mapa de label por serviço para o card do index.html:
| tipoServico | labelCard | seçãoDesignSystem |
|-------------|-----------|-------------------|
| `identidade-visual` | `Ver Identidade Visual` | 8A (12 seções) |
| `logo` | `Ver Logo` | 8B (9 seções) |
| `redes-sociais` | `Ver Redes Sociais` | 8C (8 seções) |

### 2. Ler a `_inbox/`

Raiz do projeto: `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/`

Listar arquivos em `_inbox/` e identificar:
- **arquivoFormulario**: arquivo com prefixo `form-` (ex: `form-studioalma.pdf`)
- **arquivoDesign**: arquivo com prefixo `design-` (ex: `design-studioalma.pdf`)
- **arquivoTexto**: arquivo com prefixo `texto-` (ex: `texto-studioalma.txt`) — opcional

Erros a verificar antes de continuar:
- Se não encontrar nenhum `form-*`: parar e dizer "Não encontrei o formulário na `_inbox/`. Adicione um arquivo com prefixo `form-` e tente novamente."
- Se não encontrar nenhum `design-*`: parar e dizer "Não encontrei o design na `_inbox/`. Adicione um arquivo com prefixo `design-` e tente novamente."
- Se encontrar dois ou mais `form-*`: listar os arquivos e perguntar qual usar.
- Se encontrar dois ou mais `design-*`: listar os arquivos e perguntar qual usar.
- Se `clientes/[clienteSlug]/[tipoServico]/` já existir: avisar e perguntar se deve sobrescrever antes de continuar.

### 3. Criar estrutura de pastas

```bash
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Formulario"
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Design"
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Texto"
```

### 4. Mover arquivos da `_inbox/` para as subpastas

```bash
mv "_inbox/[arquivoFormulario]" "clientes/[clienteSlug]/[tipoServico]/Formulario/"
mv "_inbox/[arquivoDesign]" "clientes/[clienteSlug]/[tipoServico]/Design/"
```

Se existir `arquivoTexto`:
```bash
mv "_inbox/[arquivoTexto]" "clientes/[clienteSlug]/[tipoServico]/Texto/"
```

### 5. Ler os arquivos de conteúdo

- Ler o PDF em `clientes/[clienteSlug]/[tipoServico]/Formulario/` — brief do cliente
- Ler o PDF em `clientes/[clienteSlug]/[tipoServico]/Design/` — identidade visual feita por Heleno
- Ler `_template/LAB360-design-system.md` — estrutura LAB 360°, usar a seção correspondente ao tipoServico (8A, 8B ou 8C)

### 6. Adicionar card no index.html

Abrir `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/index.html` e adicionar um novo `<a class="project-card">` dentro de `.projects-grid`, logo após o último card existente:

```html
<a class="project-card" href="./clientes/[clienteSlug]/[tipoServico]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[nomeServicoLegível]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

Nomes legíveis por tipo:
- `identidade-visual` → `Identidade Visual Completa`
- `logo` → `Logo`
- `redes-sociais` → `Design para Redes Sociais`

### 7. Gerar o HTML

Invocar o skill `huashu-design` para gerar o HTML de entrega.

Briefing para o huashu-design:
- Tipo de serviço: [tipoServico] (usar seção [seçãoDesignSystem] do design system)
- Estrutura LAB360°: fundo preto, canvas rizoma, nav dots, status bar, cursor cyan
- Conteúdo: extraído do formulário (voz do cliente) + design PDF (cores, tipografia, logo)
- Salvar em: `clientes/[clienteSlug]/[tipoServico]/index.html`

### 8. Pausar e mostrar resumo para revisão

Após gerar o HTML, mostrar ao Heleno:
- Seções geradas (com nome de cada uma)
- Cores detectadas (hexes)
- Tipografia usada
- Caminho do arquivo: `clientes/[clienteSlug]/[tipoServico]/index.html`

Aguardar aprovação antes de continuar. Não commitar ainda.

### 9. Após aprovação: commit + push

```bash
git add clientes/[clienteSlug]/ index.html
git commit -m "Gerar [tipoServico]: [clienteNome]"
git push
```

Vercel deploya automaticamente em ~30 segundos.

### 10. Criar página no Notion (somente se existir arquivo em Texto/)

Se o arquivo de texto foi movido para `clientes/[clienteSlug]/[tipoServico]/Texto/`:
- Ler o conteúdo do arquivo
- Criar página no Notion via MCP com o conteúdo
- Retornar a URL da página criada

### 11. Entregar as URLs ao Heleno

Mostrar:
```
✓ Entrega pronta para [clienteNome]

URL Vercel:
https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[tipoServico]/

[Se houver Notion:]
URL Notion:
[url retornada pelo MCP]

_inbox/ está vazia e pronta para o próximo cliente.
```
````

- [ ] **Passo 2: Verificar que o arquivo foi criado corretamente**

```bash
cat "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/.claude/commands/novo-pedido.md" | head -20
```

Esperado: primeiras linhas do arquivo aparecem corretamente.

- [ ] **Passo 3: Commit**

```bash
git add .claude/commands/novo-pedido.md
git commit -m "feat: criar comando /novo-pedido com pipeline completo de inbox"
```

---

## Task 3: Atualizar `CLAUDE.md` com o novo fluxo principal

**Files:**
- Modificar: `CLAUDE.md`

- [ ] **Passo 1: Adicionar seção "Fluxo Principal — Novo Pedido" no CLAUDE.md**

Localizar a seção `## Workflow de Geração de HTML` e adicionar ANTES dela uma nova seção:

```markdown
## Fluxo Principal — Novo Pedido (use este)

Quando Heleno traz um novo cliente/pedido, o fluxo é:

1. Heleno nomeia os arquivos com prefixo e joga em `_inbox/`:
   - `form-[slug].pdf` — formulário exportado do Tally
   - `design-[slug].pdf` — design exportado do Canva
   - `texto-[slug].txt` — texto para Notion (opcional)
2. Heleno abre Claude Code e digita:
   ```
   /novo-pedido Nome do Cliente — tipo-de-servico
   ```
3. Claude faz tudo: cria pastas, move arquivos, gera HTML, pausa para revisão
4. Heleno aprova → Claude commita, faz push, cria Notion se tiver texto
5. Heleno recebe as URLs e repassa ao cliente

**Tipos de serviço:** `identidade-visual` | `logo` | `redes-sociais`
```

- [ ] **Passo 2: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: documentar /novo-pedido como fluxo principal no CLAUDE.md"
```

---

## Task 4: Verificação final

- [ ] **Passo 1: Confirmar estrutura de arquivos**

```bash
ls "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/_inbox/"
ls "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/.claude/commands/"
```

Esperado:
- `_inbox/` contém apenas `.gitkeep`
- `.claude/commands/` contém `novo-cliente.md` e `novo-pedido.md`

- [ ] **Passo 2: Confirmar que .gitignore ignora arquivos da inbox**

```bash
cd "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°"
touch _inbox/teste.pdf
git status | grep inbox
rm _inbox/teste.pdf
```

Esperado: `_inbox/teste.pdf` NÃO aparece em `git status` (está ignorado). O `.gitkeep` também não aparece (já está commitado).

- [ ] **Passo 3: Confirmar commits recentes**

```bash
git log --oneline -5
```

Esperado: os 3 commits desta implementação aparecem no topo.
