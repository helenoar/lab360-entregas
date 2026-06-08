# Comando: novo-pedido

Processa todos os clientes em `_inbox/` de ponta a ponta.

## Como funciona

Heleno cria no Finder:
```
_inbox/
  [Nome do Cliente]/
    [tipo-servico]/
      arquivo1.pdf
      arquivo2.pdf
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

## O que fazer — passo a passo

### 1. Escanear o inbox

```bash
ls "_inbox/"
```

Para cada pasta encontrada (= um cliente):

```bash
ls "_inbox/[Nome do Cliente]/"
```

O nome da subpasta é o `tipoServico`. Nenhum arquivo precisa ser lido nesta etapa.

Derivar:
- **clienteNome**: nome da pasta em `_inbox/` (ex: `Studio Alma`)
- **tipoServico**: nome da subpasta (ex: `identidade-visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem hífens
  - Regras: á→a, ã→a, â→a, é→e, ê→e, í→i, ó→o, ô→o, õ→o, ú→u, ç→c, remover espaços e hífens

Erros a verificar:
- Tipo não reconhecido → parar, reportar: "Tipo `[nome]` não reconhecido. Use: identidade-visual | logo | redes-sociais | gestao-de-redes-sociais"
- Inbox vazio → parar, reportar: "Nenhum cliente encontrado em `_inbox/`."

Mapa de configurações:
| tipoServico | pastaEntrega | labelCard | seçãoDS |
|-------------|-------------|-----------|---------|
| `identidade-visual` | `clientes/[slug]/identidade-visual/` | `Identidade Visual` | 8A |
| `logo` | `clientes/[slug]/logo/` | `Logo` | 8B |
| `redes-sociais` | `clientes/[slug]/redes-sociais/` | `Design para Redes Sociais` | 8C |
| `gestao-de-redes-sociais` | `clientes/[slug]/gestao-de-redes-sociais/[mes-ano]/` | `Calendário Editorial` | 8D |

### 2. Criar estrutura de pastas

Verificar se o cliente já existe:
```bash
ls "clientes/[clienteSlug]/" 2>/dev/null
```

**Para identidade-visual, logo, redes-sociais:**
```bash
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Formulario"
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Design"
mkdir -p "clientes/[clienteSlug]/[tipoServico]/Texto"
```

**Para gestao-de-redes-sociais:**

Identificar mês/ano a partir do formulário (ler o PDF para extrair período). Se não encontrar, perguntar ao Heleno.

Normalização: jan→janeiro, fev→fevereiro, mar→março, abr→abril, mai→maio, jun→junho, jul→julho, ago→agosto, set→setembro, out→outubro, nov→novembro, dez→dezembro

```bash
mkdir -p "clientes/[clienteSlug]/gestao-de-redes-sociais/[mes-ano]"
```

### 3. Mover arquivos da inbox

**Para identidade-visual, logo, redes-sociais:**

Identificar na pasta `_inbox/[Nome do Cliente]/[tipoServico]/`:
- **arquivoFormulario**: PDF do formulário (geralmente o menor, ou o que contém respostas do Tally)
- **arquivoDesign**: PDF do design do Canva (geralmente o maior)
- **arquivoTexto**: .txt ou .md para Notion (opcional)

Se houver ambiguidade entre dois PDFs, usar o nome para inferir qual é qual. Se não conseguir, perguntar.

```bash
mv "_inbox/[Nome do Cliente]/[tipoServico]/[arquivoFormulario]" "clientes/[clienteSlug]/[tipoServico]/Formulario/"
mv "_inbox/[Nome do Cliente]/[tipoServico]/[arquivoDesign]" "clientes/[clienteSlug]/[tipoServico]/Design/"
```

**Para gestao-de-redes-sociais:**

```bash
mv "_inbox/[Nome do Cliente]/gestao-de-redes-sociais/[arquivoFormulario]" "clientes/[clienteSlug]/gestao-de-redes-sociais/[mes-ano]/formulario.pdf"
```

### 4. Carregar especialista do tipo

Ler `_especialistas/[tipoServico].md` — conhecimento específico do domínio.

### 5. Ler os arquivos de conteúdo

**Para identidade-visual, logo, redes-sociais:**
- Ler PDF em `Formulario/`
- Ler PDF em `Design/`
- Ler `_template/LAB360-design-system.md` — seção conforme tipoServico (8A, 8B ou 8C)

**Para gestao-de-redes-sociais:**
- Ler `formulario.pdf`
- Ler `_especialistas/gestao-de-redes-sociais.md`
- Ler `_template/LAB360-design-system.md` — seção 8D
- Gerar estratégia editorial internamente (sem precisar de Claude Desktop)

### 6. Adicionar card no index.html

O `index.html` agrupa projetos por cliente. Verificar se o cliente já existe buscando `data-cliente="[clienteSlug]"`.

URLs sempre absolutas do Vercel + `target="_blank"`.

**Se o cliente JÁ existe** — adicionar card dentro do `.projects-grid` e atualizar o contador:
```html
<a class="project-card" href="https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[tipoServico]/" target="_blank" rel="noopener">
  <span class="project-service">[labelCard]</span>
  <span class="project-name">[nomeProjeto]</span>
  <span class="card-action">Ver entrega</span>
</a>
```

Para `gestao-de-redes-sociais`, o path é `clientes/[clienteSlug]/gestao-de-redes-sociais/[mes-ano]/`.

**Se o cliente NÃO existe** — criar novo bloco antes do `</main>`:
```html
<div class="clients-sep"></div>
<div class="client-group" data-cliente="[clienteSlug]">
  <div class="client-header">
    <span class="client-name">[clienteNome]</span>
    <span class="client-count">1 projeto</span>
    <button class="copy-link" onclick="copyClientLink('[clienteSlug]', this)">Copiar link</button>
  </div>
  <div class="projects-grid">

    <a class="project-card" href="https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[tipoServico]/" target="_blank" rel="noopener">
      <span class="project-service">[labelCard]</span>
      <span class="project-name">[nomeProjeto]</span>
      <span class="card-action">Ver entrega</span>
    </a>

  </div>
</div>
```

O `nomeProjeto` vem do formulário. Se não houver nome de projeto específico, usar o nome da marca/cliente.

### 7. Gerar o HTML

Invocar o skill `huashu-design` para gerar o HTML de entrega.

Briefing base:
- Estrutura LAB360°: fundo preto, canvas rizoma, nav dots, status bar, cursor cyan
- Seção do design system: conforme tipoServico (8A, 8B, 8C ou 8D)
- Usar conhecimento do especialista carregado no passo 4

**Para gestao-de-redes-sociais:**
- Implementar grade mensal interativa (seção 8D)
- Cor de cada card = cor do pilar
- Cards clicáveis com detalhe completo

Salvar em:
- `identidade-visual/logo/redes-sociais`: `clientes/[clienteSlug]/[tipoServico]/index.html`
- `gestao-de-redes-sociais`: `clientes/[clienteSlug]/gestao-de-redes-sociais/[mes-ano]/index.html`

### 8. Commit + push imediato

```bash
git add "clientes/[clienteSlug]/" index.html
git commit -m "Gerar [tipoServico]: [clienteNome]"
git push
```

Vercel deploya em ~30 segundos.

### 9. Limpar inbox

```bash
rm -rf "_inbox/[Nome do Cliente]/"
```

### 10. Criar página no Notion (somente se Texto/ tiver arquivo)

Aplicável apenas para identidade-visual, logo, redes-sociais.
Ler o arquivo em Texto/, criar página no Notion via MCP.

### 11. Entregar URL ao Heleno

```
✓ [clienteNome] pronto

URL: https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[tipoServico]/
```

Para gestao-de-redes-sociais:
```
✓ [clienteNome] — Calendário [mes-ano] pronto

URL: https://lab360-entregas.vercel.app/clientes/[clienteSlug]/gestao-de-redes-sociais/[mes-ano]/
```

Se houver múltiplos clientes no inbox, processar todos em sequência e entregar todas as URLs ao final.
