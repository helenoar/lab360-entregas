# Comando: novo-pedido

Gera HTML de entrega a partir de arquivos já colocados na estrutura de pastas.

## Como usar

Heleno coloca os arquivos diretamente na estrutura:

```
clientes/
  [clienteSlug]/
    [projetoSlug]/
      [tipo-servico]/
        Formulario/[formulario].pdf
        Design/[design].pdf
        Texto/[arquivo].txt (opcional)
      gestao-de-redes-sociais/
        [mes-ano]/
          formulario.pdf
```

Depois digita:

```
/novo-pedido [clienteSlug] [projetoSlug] [tipo-servico]
```

Exemplos:
```
/novo-pedido quikciadadanca mover-consciente identidade-visual
/novo-pedido studioalma branding-2025 logo
/novo-pedido caferaiz pack-janeiro gestao-de-redes-sociais
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

## O que fazer — passo a passo

### 1. Validar argumentos

Extrair:
- **clienteSlug**: primeiro argumento (ex: `quikciadadanca`)
- **projetoSlug**: segundo argumento (ex: `mover-consciente`)
- **tipoServico**: terceiro argumento (ex: `identidade-visual`)

Erros a verificar:
- Tipo não reconhecido → parar, reportar: "Tipo `[nome]` não reconhecido. Use: identidade-visual | logo | redes-sociais | gestao-de-redes-sociais"
- Pasta do cliente não existe → parar, reportar: "Cliente `[slug]` não encontrado em `clientes/`"
- Pasta do projeto não existe → parar, reportar: "Projeto `[slug]` não encontrado em `clientes/[clienteSlug]/`"

### 2. Validar estrutura de arquivos

**Para identidade-visual, logo, redes-sociais:**

Verificar se existem:
```bash
ls "clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/Formulario/"*.pdf
ls "clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/Design/"*.pdf
```

Se faltar formulário ou design → parar, reportar qual arquivo falta.

**Para gestao-de-redes-sociais:**

Identificar [mes-ano] dentro de `gestao-de-redes-sociais/`:
```bash
ls "clientes/[clienteSlug]/[projetoSlug]/gestao-de-redes-sociais/"
```

Se houver múltiplos meses, perguntar qual processar. Se nenhum, parar com "Nenhum calendário encontrado em gestao-de-redes-sociais/".

Verificar se existe:
```bash
ls "clientes/[clienteSlug]/[projetoSlug]/gestao-de-redes-sociais/[mes-ano]/formulario.pdf"
```

Se faltar → parar, reportar: "Formulário não encontrado em [mes-ano]/".

### 3. Carregar especialista do tipo

Ler `_especialistas/[tipoServico].md` — conhecimento específico do domínio.

### 4. Ler os arquivos de conteúdo

**Para identidade-visual, logo, redes-sociais:**
- Ler PDF em `Formulario/` (extrair brief em voz própria do cliente)
- Ler PDF em `Design/` (extrair visual, cores, elementos)
- Ler `_template/LAB360-design-system.md` — seção conforme tipoServico (8A, 8B ou 8C)

**Para gestao-de-redes-sociais:**
- Ler `formulario.pdf`
- Ler `_especialistas/gestao-de-redes-sociais.md`
- Ler `_template/LAB360-design-system.md` — seção 8D
- Gerar estratégia editorial internamente

### 5. Obter nome real do cliente e projeto (para HTML e URLs)

Procurar em `index.html` se o cliente já existe:
```bash
grep "data-cliente=\"[clienteSlug]\"" index.html
```

Se encontrar, extrair o `clienteNome` do HTML. Se não encontrar, pedir ao Heleno.

Mesmo para projeto (se houver padrão no index.html).

### 6. Gerar o HTML

Invocar skill `huashu-design` para gerar o HTML de entrega.

Briefing base:
- Estrutura LAB360°: fundo preto, canvas rizoma, nav dots, status bar, cursor cyan
- Seção do design system: conforme tipoServico (8A, 8B, 8C ou 8D)
- Usar conhecimento do especialista carregado no passo 3

**Para gestao-de-redes-sociais:**
- Implementar grade mensal interativa (seção 8D)
- Cor de cada card = cor do pilar
- Cards clicáveis com detalhe completo

Salvar em:
- identidade-visual/logo/redes-sociais: `clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/index.html`
- gestao-de-redes-sociais: `clientes/[clienteSlug]/[projetoSlug]/gestao-de-redes-sociais/[mes-ano]/index.html`

### 7. Adicionar ou atualizar card no index.html

O `index.html` agrupa projetos por cliente. URLs sempre absolutas do Vercel + `target="_blank"`.

**Se o cliente JÁ existe:**
```html
<a class="project-card" href="https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/" target="_blank" rel="noopener">
  <span class="project-service">[labelCard]</span>
  <span class="project-name">[nomeProjeto] — [tipoServicoLegível]</span>
  <span class="card-action">Ver entrega</span>
</a>
```

Para `gestao-de-redes-sociais`, o path é `clientes/[clienteSlug]/[projetoSlug]/gestao-de-redes-sociais/[mes-ano]/`.

**Se o cliente NÃO existe:**
```html
<div class="clients-sep"></div>
<div class="client-group" data-cliente="[clienteSlug]">
  <div class="client-header">
    <span class="client-name">[clienteNome]</span>
    <span class="client-count">1 projeto</span>
    <button class="copy-link" onclick="copyClientLink('[clienteSlug]', this)">Copiar link</button>
  </div>
  <div class="projects-grid">
    <a class="project-card" href="https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/" target="_blank" rel="noopener">
      <span class="project-service">[labelCard]</span>
      <span class="project-name">[nomeProjeto] — [tipoServicoLegível]</span>
      <span class="card-action">Ver entrega</span>
    </a>
  </div>
</div>
```

### 8. Commit + push imediato

```bash
git add "clientes/[clienteSlug]/[projetoSlug]/" index.html
git commit -m "Gerar [tipoServico]: [clienteNome] — [nomeProjeto]"
git push
```

Vercel deploya em ~30 segundos.

### 9. Criar página no Notion (somente se Texto/ tiver arquivo)

Aplicável apenas para identidade-visual, logo, redes-sociais.
Ler o arquivo em Texto/, criar página no Notion via MCP.

### 10. Entregar URL ao Heleno

**Para identidade-visual, logo, redes-sociais:**
```
✓ [clienteNome] — [nomeProjeto] pronto

URL: https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[projetoSlug]/[tipoServico]/
```

**Para gestao-de-redes-sociais:**
```
✓ [clienteNome] — [nomeProjeto] — Calendário [mes-ano] pronto

URL: https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[projetoSlug]/gestao-de-redes-sociais/[mes-ano]/
```

## Mapa de configurações

| tipoServico | labelCard | seçãoDS |
|-------------|-----------|---------|
| `identidade-visual` | `Identidade Visual` | 8A |
| `logo` | `Logo` | 8B |
| `redes-sociais` | `Design para Redes Sociais` | 8C |
| `gestao-de-redes-sociais` | `Calendário Editorial` | 8D |
