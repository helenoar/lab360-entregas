# Sistema de Entregas LAB 360° — Expansão Multi-Serviço

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Expandir o sistema de entregas para suportar qualquer tipo de serviço (começando por calendário editorial), com estrutura de pastas invertida (serviço → cliente) e configs autodescritivas por tipo.

**Architecture:** Pastas raiz por serviço (`identidade-visual/`, `gestao-de-redes-sociais/`), configs em `_servicos/[tipo]/config.md`, knowledge base em `_servicos/[tipo]/knowledge-base.md`. Pipeline via `/novo-pedido` unificado com detecção de tipo por argumento e arquivos de inbox.

**Tech Stack:** HTML estático, Vercel, Git, markdown, Claude Code commands.

**Spec:** `docs/superpowers/specs/2026-06-08-sistema-entregas-lab360-design.md`

---

## Parte 1 — Infraestrutura e Migração

### Task 1: Criar pastas raiz e `_servicos/`

**Files:**
- Create: `identidade-visual/.gitkeep`
- Create: `gestao-de-redes-sociais/.gitkeep`
- Create: `_servicos/identidade-visual/config.md`
- Create: `_servicos/logo/config.md`
- Create: `_servicos/redes-sociais/config.md`
- Create: `_servicos/gestao-de-redes-sociais/calendario-editorial/config.md`

- [ ] **Criar pasta `identidade-visual/`**

```bash
mkdir -p "identidade-visual"
touch "identidade-visual/.gitkeep"
```

- [ ] **Criar pasta `gestao-de-redes-sociais/`**

```bash
mkdir -p "gestao-de-redes-sociais"
touch "gestao-de-redes-sociais/.gitkeep"
```

- [ ] **Criar config de identidade-visual**

Criar `_servicos/identidade-visual/config.md`:

```markdown
# Config — Identidade Visual Completa

## Identificação
- **Slug:** identidade-visual
- **Pasta de saída:** `identidade-visual/[clienteSlug]/`
- **Variante:** nenhuma (uma entrega por cliente)

## Input em `_inbox/`
- `form-[slug].pdf` — formulário Tally
- `design-[slug].pdf` — PDF do Canva com o design

## Design System
- Seção: **8A** (12 seções)
- Seções: Hero → Sobre a Marca → Posicionamento → Público → Personalidade → Logo → Paleta → Tipografia → Elementos Visuais → Voz & Tom → Aplicações → Guia Rápido

## Label no dashboard
- Card: `Ver Identidade Visual`
- Nome legível: `Identidade Visual Completa`
```

- [ ] **Criar config de logo**

Criar `_servicos/logo/config.md`:

```markdown
# Config — Logo

## Identificação
- **Slug:** logo
- **Pasta de saída:** `logo/[clienteSlug]/`
- **Variante:** nenhuma (uma entrega por cliente)

## Input em `_inbox/`
- `form-[slug].pdf` — formulário Tally
- `design-[slug].pdf` — PDF do Canva com o design

## Design System
- Seção: **8B** (9 seções)
- Seções: Hero → Conceito → Versões do Logo → Paleta → Tipografia → Sobre Fundos → Área de Respiro → Usos Corretos & Incorretos → Aplicações

## Label no dashboard
- Card: `Ver Logo`
- Nome legível: `Logo`
```

- [ ] **Criar config de redes-sociais**

Criar `_servicos/redes-sociais/config.md`:

```markdown
# Config — Design para Redes Sociais

## Identificação
- **Slug:** redes-sociais
- **Pasta de saída:** `redes-sociais/[clienteSlug]/`
- **Variante:** nenhuma (uma entrega por cliente)

## Input em `_inbox/`
- `form-[slug].pdf` — formulário Tally
- `design-[slug].pdf` — PDF do Canva com as peças

## Design System
- Seção: **8C** (8 seções)
- Seções: Hero → Estratégia → Galeria das Peças → Detalhamento por Peça → Tipografia → Paleta → Especificações Técnicas → Como Usar

## Label no dashboard
- Card: `Ver Redes Sociais`
- Nome legível: `Design para Redes Sociais`
```

- [ ] **Criar config de calendario-editorial**

Criar `_servicos/gestao-de-redes-sociais/calendario-editorial/config.md`:

```markdown
# Config — Calendário Editorial

## Identificação
- **Slug:** calendario-editorial
- **Categoria:** gestao-de-redes-sociais
- **Pasta de saída:** `gestao-de-redes-sociais/[clienteSlug]/[mes-ano]/`
- **Variante:** mês de referência (ex: `junho-2025`)

## Input em `_inbox/`
- `calendario-[slug]-[mes-ano].md` — estratégia gerada pelo agente externo
  - Ex: `calendario-studioalma-jun-2025.md`
  - O mês é extraído do nome do arquivo (parte após o segundo `-`)

## Design System
- Seção: **8D** (6 seções)
- Seções: Hero → Estratégia do Mês → Pilares de Conteúdo → Calendário Visual → Detalhamento dos Posts → O que esperar

## Knowledge Base
- Arquivo: `knowledge-base.md` (mesmo diretório deste config)
- Usar para: estratégia de redes, pilares de conteúdo, boas práticas por plataforma

## Label no dashboard
- Card: `Ver Calendário`
- Nome legível: `Calendário Editorial`
```

- [ ] **Verificar estrutura criada**

```bash
find _servicos -type f | sort
```

Saída esperada:
```
_servicos/gestao-de-redes-sociais/calendario-editorial/config.md
_servicos/identidade-visual/config.md
_servicos/logo/config.md
_servicos/redes-sociais/config.md
```

- [ ] **Commit**

```bash
git add identidade-visual/ gestao-de-redes-sociais/ _servicos/
git commit -m "feat: criar pastas raiz por serviço e configs em _servicos/"
```

---

### Task 2: Migrar cliente existente

**Files:**
- Move: `clientes/quikciadadanca/moverconsciente/` → `identidade-visual/quikciadadanca/`
- Delete: `clientes/`
- Modify: `index.html` (raiz) — atualizar href do card

- [ ] **Criar pasta de destino e mover arquivos**

```bash
mkdir -p "identidade-visual/quikciadadanca"
cp -r "clientes/quikciadadanca/moverconsciente/Design" "identidade-visual/quikciadadanca/Design"
cp -r "clientes/quikciadadanca/moverconsciente/Formulario" "identidade-visual/quikciadadanca/Formulario"
cp -r "clientes/quikciadadanca/moverconsciente/Texto" "identidade-visual/quikciadadanca/Texto"
cp "clientes/quikciadadanca/moverconsciente/identidade-visual/index.html" "identidade-visual/quikciadadanca/index.html"
```

- [ ] **Verificar que o index.html chegou no lugar certo**

```bash
ls "identidade-visual/quikciadadanca/"
```

Saída esperada: `Design  Formulario  Texto  index.html`

- [ ] **Remover pasta `clientes/` antiga**

```bash
rm -rf "clientes/"
```

- [ ] **Atualizar href no `index.html` raiz**

Abrir `index.html` e localizar:
```html
href="./clientes/quikciadadanca/moverconsciente/identidade-visual/"
```

Substituir por:
```html
href="./identidade-visual/quikciadadanca/"
```

- [ ] **Verificar que o index.html raiz não tem mais referências a `clientes/`**

```bash
grep -n "clientes/" index.html
```

Saída esperada: nenhuma linha.

- [ ] **Commit**

```bash
git add identidade-visual/ index.html
git rm -r clientes/
git commit -m "feat: migrar para estrutura serviço/cliente — mover quikciadadanca"
```

---

### Task 3: Adicionar `.superpowers/` ao `.gitignore`

**Files:**
- Modify: `.gitignore`

- [ ] **Adicionar ao `.gitignore`**

Abrir `.gitignore` e adicionar ao final:

```
# Brainstorming e specs temporários
.superpowers/
docs/superpowers/
```

- [ ] **Verificar**

```bash
git check-ignore -v .superpowers/
```

Saída esperada: `.gitignore:N:.superpowers/	.superpowers/`

- [ ] **Commit**

```bash
git add .gitignore
git commit -m "chore: ignorar .superpowers/ e docs/superpowers/"
```

---

## Parte 2 — Documentação

### Task 4: Adicionar seção 8D ao design system

**Files:**
- Modify: `_template/LAB360-design-system.md`

- [ ] **Ler o arquivo para encontrar o ponto de inserção**

Abrir `_template/LAB360-design-system.md` e localizar o final da seção `8C. DESIGN PARA REDES SOCIAIS` (termina após a tabela de 8 seções e antes da `## 9. Meta Tags Obrigatórias`).

- [ ] **Inserir seção 8D após o `---` que fecha a 8C**

Adicionar após o último `---` da seção 8C, antes da `## 9. Meta Tags Obrigatórias`:

```markdown
### 8D. CALENDÁRIO EDITORIAL
*Apresentação do planejamento mensal de conteúdo. Entrega recorrente (mensal). 6 seções.*

Fonte dos dados: `_inbox/calendario-[slug]-[mes-ano].md` — arquivo gerado pelo agente de estratégia.
Antes de gerar, ler `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md` para referência estratégica.

| # | Seção | Conteúdo | Fonte no arquivo .md |
|---|-------|----------|----------------------|
| 1 | **Hero** | Nome da marca, mês de referência, plataformas cobertas | `META.cliente`, `META.mes`, `META.plataformas` |
| 2 | **Estratégia do Mês** | Objetivo central, tema do mês, tom de voz, público | `ESTRATÉGIA` completo |
| 3 | **Pilares de Conteúdo** | Cards com nome, cor hex, percentual e descrição de cada pilar | `PILARES` completo |
| 4 | **Calendário Visual** | Grade mensal interativa — cada dia com card clicável: pilar (cor do card), formato, tema. Clique expande detalhe. Dias sem post ficam vazios. | `POSTS` — iterar por data |
| 5 | **Detalhamento dos Posts** | Lista de todos os posts: data formatada, pilar, formato, tema e legenda completa | `POSTS` — todos os campos |
| 6 | **O que esperar** | Lista de indicadores de foco do mês, sem prometer números | `EXPECTATIVAS` |

**Componente do Calendário Visual (seção 4):**
- Grade CSS de 7 colunas (dom–sáb) com cabeçalho dos dias da semana
- Dias sem post: célula vazia com número do dia em `rgba(255,255,255,0.15)`
- Dias com post: card clicável com cor de fundo do pilar (opacidade 0.15), borda esquerda sólida na cor do pilar, número do dia, ícone do formato e título curto
- Clique no card: expande modal ou seção âncora com legenda completa
- Legenda do pilar: lista colorida abaixo do calendário como key
- Mobile: grade de 3 colunas ou lista por semana

**Formato do arquivo de entrada (`calendario-[slug]-[mes-ano].md`):**
```
## META
cliente: [nome]
slug: [slug]
mes: [junho-2025]
plataformas: [Instagram, TikTok]
objetivo_do_mes: [frase]

## ESTRATÉGIA
tom_de_voz: [adjetivos]
tema_central: [frase]
publico: [descrição]

## PILARES
- nome: [nome]
  cor: [#hex]
  percentual: [N%]
  descricao: [texto]

## POSTS
- data: [YYYY-MM-DD]
  pilar: [nome do pilar]
  formato: [Reels|Carrossel|Feed|Stories]
  tema: [título curto]
  legenda: |
    [legenda completa]

## EXPECTATIVAS
- [indicador 1]
- [indicador 2]
```

---
```

- [ ] **Verificar que a seção foi adicionada**

```bash
grep -n "8D" "_template/LAB360-design-system.md"
```

Saída esperada: linha com `### 8D. CALENDÁRIO EDITORIAL`

- [ ] **Commit**

```bash
git add "_template/LAB360-design-system.md"
git commit -m "feat: adicionar seção 8D (Calendário Editorial) ao design system"
```

---

### Task 5: Criar `knowledge-base.md`

**Files:**
- Create: `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md`

- [ ] **Criar o arquivo com conteúdo completo**

Criar `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md`:

```markdown
# Knowledge Base — Gestão de Redes Sociais

Referência estratégica para geração de calendários editoriais. Atualizada para 2024–2025.

---

## Framework de Pilares de Conteúdo

Todo calendário precisa de 3–5 pilares. Pilares são categorias temáticas que organizam o que a marca publica.

| Pilar | Objetivo principal | Melhor formato | KPI de foco |
|---|---|---|---|
| **Educativo** | Autoridade e salvamentos | Carrossel, Reels explicativo | Salvamentos, compartilhamentos |
| **Institucional** | Confiança e humanização | Feed estático, Stories | Comentários, DMs |
| **Produto/Serviço** | Conversão e leads | Reels, Stories com link | Cliques, DMs, vendas |
| **Engajamento** | Interações ativas | Stories (enquete/quiz), Reels resposta | Votos, respostas, comentários |
| **Entretenimento** | Alcance e novos seguidores | Reels (trends adaptadas) | Alcance, novos seguidores |

### Mix por objetivo

| Objetivo | Educativo | Produto | Institucional | Engajamento | Entret. |
|---|---|---|---|---|---|
| Crescimento de seguidores | 25% | 15% | 15% | 20% | 25% |
| Engajamento da comunidade | 35% | 20% | 20% | 25% | 0% |
| Conversão/vendas | 25% | 40% | 15% | 15% | 5% |
| Reconhecimento de marca | 20% | 20% | 30% | 15% | 15% |

---

## Instagram — Boas Práticas 2024–2025

### Algoritmo e formatos

- **Reels**: maior distribuição orgânica. Priorizado para contas em crescimento. Ideal: 3–5x/semana.
- **Carrossel**: maior taxa de salvamento. O algoritmo reexibe carrosséis não vistos completamente. Ideal para educativo.
- **Feed estático**: menor alcance orgânico, mas mantém consistência de grid. Use com moderação.
- **Stories**: não ampliam alcance mas mantêm relacionamento com seguidores existentes. Use diariamente.

### Frequência por estágio da conta

| Fase | Seguidores | Feed/Reels | Stories |
|---|---|---|---|
| Crescimento | < 5k | 5–7x/semana | Diário |
| Consolidação | 5k–50k | 3–5x/semana | 3–5x/semana |
| Manutenção | > 50k | 3–4x/semana | Diário |

### Horários com maior engajamento (Brasil)

- **Melhor:** terça a quinta, 11h–13h e 18h–21h
- **Bom:** segunda e sexta, 12h–14h
- **Evitar:** sábado de manhã, domingo antes das 18h

### Captions — Estrutura eficiente

1. **Hook** (primeiras 125 caracteres — aparecem antes do "mais"): pergunta, dado surpreendente ou afirmação provocativa
2. **Corpo**: parágrafos de 1–2 linhas, quebras generosas, emojis como marcadores visuais
3. **CTA claro no final**: uma ação — "salva", "comenta X", "compartilha com quem precisa"

Evitar: CTA no meio do texto, parágrafos longos, múltiplos CTAs.

### Hashtags

| Tipo | Seguidores do hashtag | Quantidade |
|---|---|---|
| Nicho específico | < 500k | 2–3 |
| Médio porte | 500k–2M | 1–2 |
| Alto alcance | > 5M | máx 1 |

Total: 3–5 hashtags específicas superam 30 genéricas.

---

## TikTok — Boas Práticas 2024–2025

### Algoritmo

- Baseado em **conclusão do vídeo** e **compartilhamento** — não em seguidores
- Conta nova tem a mesma chance de viralizar que conta grande
- Conteúdo filmado no app nativo tem distribuição maior que upload externo
- Trending sounds aumentam distribuição em ~30%

### Frequência

- Crescimento ativo: 1x/dia ou mínimo 5x/semana
- Manutenção: 3–4x/semana
- Abaixo de 3x/semana: o algoritmo desacelera a distribuição da conta

### Hook nos primeiros 2 segundos — decisivo

- Pergunta direta: "Você sabe por que sua pele resseca mesmo usando hidratante?"
- Dado surpreendente: "95% das pessoas lavam o rosto do jeito errado"
- Spoiler do resultado: mostrar o "antes e depois" no início

### Formatos que performam no TikTok

- Tutoriais rápidos (15–30s)
- "3 erros que [público] comete"
- Reação/opinião honesta sobre trend do nicho
- POV + narração em off
- Resposta a comentário com vídeo

---

## Frequência por Plataforma — Resumo

| Plataforma | Mínimo | Ideal | Máximo útil |
|---|---|---|---|
| Instagram Feed/Reels | 3x/semana | 5x/semana | 7x/semana |
| Instagram Stories | 3x/semana | Diário | 7–10 stories/dia |
| TikTok | 3x/semana | Diário | 2x/dia |
| LinkedIn | 2x/semana | 4x/semana | Diário |

---

## Volume mensal por frequência semanal

| Posts/semana | Posts/mês (aprox.) | Perfil |
|---|---|---|
| 3x | 12–14 | Manutenção |
| 4x | 16–18 | Crescimento moderado |
| 5x | 20–22 | Crescimento ativo |
| 7x | 28–30 | Crescimento agressivo |

---

## Diretrizes de distribuição semanal

- Nunca dois posts do mesmo pilar consecutivos
- Alternar: post de valor (educativo/produto) → post relacional (institucional/engajamento)
- Distribuir posts nos dias de maior engajamento da semana (terça a quinta como prioridade)
- Verificar datas comemorativas do mês e do nicho — posts temáticos têm alcance maior

---

## Formatos e quando usar

| Formato | Quando usar | Plataforma |
|---|---|---|
| Reels | Alcance, entretenimento, tutoriais rápidos | Instagram, TikTok |
| Carrossel | Educativo, passo a passo, comparativos | Instagram |
| Feed estático | Lançamento, quote, data comemorativa | Instagram |
| Stories | Cotidiano, enquete, contagem regressiva, bastidores | Instagram |
| Vídeo longo | Tutorial completo, entrevista | YouTube (quando aplicável) |
```

- [ ] **Verificar**

```bash
wc -l "_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md"
```

Saída esperada: acima de 100 linhas.

- [ ] **Commit**

```bash
git add "_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md"
git commit -m "feat: criar knowledge-base de estratégia de redes sociais 2024-2025"
```

---

### Task 6: Atualizar CLAUDE.md

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Substituir a seção "Estrutura de Pastas"**

Localizar o bloco que começa com `## Estrutura de Pastas` e termina antes de `## Criar estrutura manualmente`. Substituir pelo conteúdo abaixo:

```markdown
## Estrutura de Pastas

O serviço é a pasta raiz. Dentro do serviço ficam os clientes. Dentro do cliente ficam os arquivos e o `index.html`.

```
[servico]/
  [clienteSlug]/
    Formulario/    ← brief do cliente (PDF ou .txt)
    Design/        ← PDF do Canva com o design
    Texto/         ← .txt/.md para Notion (opcional)
    index.html     ← Claude gera e commita aqui

gestao-de-redes-sociais/
  [clienteSlug]/
    [mes-ano]/     ← ex: junho-2025
      index.html
      estrategia.md  ← cópia do arquivo de inbox após processamento
```

Serviços disponíveis e suas pastas raiz:
| Serviço | Pasta raiz | URL base |
|---------|-----------|----------|
| Identidade Visual | `identidade-visual/` | `lab360-entregas.vercel.app/identidade-visual/[slug]/` |
| Logo | `logo/` | `lab360-entregas.vercel.app/logo/[slug]/` |
| Design Redes Sociais | `redes-sociais/` | `lab360-entregas.vercel.app/redes-sociais/[slug]/` |
| Calendário Editorial | `gestao-de-redes-sociais/[slug]/[mes-ano]/` | `lab360-entregas.vercel.app/gestao-de-redes-sociais/[slug]/[mes-ano]/` |

### Regra de nomenclatura de pastas (OBRIGATÓRIO)

Slugs sempre em minúsculas, sem espaços, sem acentos, sem hífens.

| Nome real | Slug correto |
|-----------|-------------|
| Quik Cia de Dança | `quikciadadanca` |
| Studio Alma | `studioalma` |
| Café Raiz | `caferaiz` |

Mês/ano: `junho-2025`, `julho-2025`, etc. (por extenso, com hífen).
```

- [ ] **Atualizar a seção "Fluxo Principal"**

Localizar `## Fluxo Principal — Novo Pedido` e atualizar os tipos aceitos e o exemplo de inbox:

```markdown
## Fluxo Principal — Novo Pedido (use este)

Quando Heleno traz um novo cliente/pedido, o fluxo é:

**Para identidade visual, logo e redes sociais:**
1. Heleno nomeia os arquivos com prefixo e joga em `_inbox/`:
   - `form-[slug].pdf` — formulário exportado do Tally
   - `design-[slug].pdf` — design exportado do Canva
   - `texto-[slug].txt` — texto para Notion (opcional)
2. Heleno digita: `/novo-pedido Nome do Cliente — tipo-de-servico`

**Para calendário editorial:**
1. Heleno gera a estratégia com o agente externo (Claude Project)
2. Salva o arquivo `.md` em `_inbox/` com o nome: `calendario-[slug]-[mes-ano].md`
   - Ex: `calendario-studioalma-jun-2025.md`
3. Heleno digita: `/novo-pedido Studio Alma — calendario-editorial`

**Continuação (todos os tipos):**
3. Claude faz tudo: cria pastas, move arquivos, gera HTML, pausa para revisão
4. Heleno aprova → Claude commita, faz push, cria Notion se tiver texto
5. Heleno recebe as URLs e repassa ao cliente

**Tipos aceitos:** `identidade-visual` | `logo` | `redes-sociais` | `calendario-editorial`
```

- [ ] **Atualizar a seção "Seções por serviço"**

Localizar o bloco "Resumo:" dentro do "Workflow de Geração de HTML" e adicionar a linha do 8D:

```
- **8D — Calendário Editorial** (6 seções): Hero → Estratégia do Mês → Pilares de Conteúdo → Calendário Visual → Detalhamento dos Posts → O que esperar
```

- [ ] **Atualizar commit pattern**

Na seção `## Git`, adicionar exemplo para calendário editorial:

```
- `Gerar calendário editorial: Studio Alma — Junho 2025`
```

- [ ] **Commit**

```bash
git add CLAUDE.md
git commit -m "feat: atualizar CLAUDE.md para estrutura serviço/cliente e calendário editorial"
```

---

## Parte 3 — Comandos

### Task 7: Atualizar `/novo-pedido`

**Files:**
- Modify: `.claude/commands/novo-pedido.md`

- [ ] **Reescrever o arquivo completo**

Substituir todo o conteúdo de `.claude/commands/novo-pedido.md` por:

```markdown
# Comando: novo-pedido

Processa um novo pedido de cliente do LAB 360° de ponta a ponta.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `calendario-editorial`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Maria Souza — redes-sociais`
- `Studio Alma — calendario-editorial`

## O que fazer — passo a passo

### 1. Derivar slug e tipo de serviço

A partir de `$ARGUMENTS`, extrair:
- **clienteNome**: parte antes do `—` (ex: `Studio Alma`)
- **tipoServico**: parte depois do `—` (ex: `identidade-visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem hífens (ex: `studioalma`)

Regras de slug: á→a, ã→a, â→a, é→e, ê→e, í→i, ó→o, ô→o, õ→o, ú→u, ç→c, remover espaços e hífens.

Ler o config do serviço em `_servicos/[tipoServico]/config.md` (ou `_servicos/gestao-de-redes-sociais/calendario-editorial/config.md` para calendário editorial).

Mapa de configurações:
| tipoServico | pastaRaiz | labelCard | seçãoDS | tipoInbox |
|-------------|-----------|-----------|---------|-----------|
| `identidade-visual` | `identidade-visual/` | `Ver Identidade Visual` | 8A | form + design PDF |
| `logo` | `logo/` | `Ver Logo` | 8B | form + design PDF |
| `redes-sociais` | `redes-sociais/` | `Ver Redes Sociais` | 8C | form + design PDF |
| `calendario-editorial` | `gestao-de-redes-sociais/` | `Ver Calendário` | 8D | calendario .md |

### 2. Ler a `_inbox/`

Raiz do projeto: `/Users/helenocarneiro/CLAUDECODE/LAB 360°/ENTREGAS_LAB 360°/`

**Para identidade-visual, logo, redes-sociais:**

Identificar em `_inbox/`:
- **arquivoFormulario**: arquivo com prefixo `form-`
- **arquivoDesign**: arquivo com prefixo `design-`
- **arquivoTexto**: arquivo com prefixo `texto-` (opcional)

Erros a verificar:
- Sem `form-*`: parar — "Não encontrei o formulário na `_inbox/`. Adicione `form-[slug].pdf`."
- Sem `design-*`: parar — "Não encontrei o design na `_inbox/`. Adicione `design-[slug].pdf`."
- Dois ou mais `form-*` ou `design-*`: listar e perguntar qual usar.

**Para calendario-editorial:**

Identificar em `_inbox/`:
- **arquivoEstrategia**: arquivo no padrão `calendario-[slug]-*.md`
  - Ex: `calendario-studioalma-jun-2025.md`
- **mesAno**: extrair do nome do arquivo — tudo após o segundo `-` até `.md`
  - Ex: `calendario-studioalma-jun-2025.md` → `jun-2025` → normalizar para `junho-2025`

Normalização do mês abreviado:
jan→janeiro, fev→fevereiro, mar→março, abr→abril, mai→maio, jun→junho,
jul→julho, ago→agosto, set→setembro, out→outubro, nov→novembro, dez→dezembro

Erro a verificar:
- Sem `calendario-*`: parar — "Não encontrei o arquivo de estratégia na `_inbox/`. Adicione `calendario-[slug]-[mes-ano].md`."

### 3. Criar estrutura de pastas

**Para identidade-visual, logo, redes-sociais:**
```bash
mkdir -p "[pastaRaiz][clienteSlug]/Formulario"
mkdir -p "[pastaRaiz][clienteSlug]/Design"
mkdir -p "[pastaRaiz][clienteSlug]/Texto"
```

**Para calendario-editorial:**
```bash
mkdir -p "gestao-de-redes-sociais/[clienteSlug]/[mesAno]"
```

Verificar conflito: se a pasta já existir, avisar e perguntar se deve sobrescrever.

### 4. Mover arquivos da `_inbox/`

**Para identidade-visual, logo, redes-sociais:**
```bash
mv "_inbox/[arquivoFormulario]" "[pastaRaiz][clienteSlug]/Formulario/"
mv "_inbox/[arquivoDesign]"     "[pastaRaiz][clienteSlug]/Design/"
```
Se existir texto:
```bash
mv "_inbox/[arquivoTexto]" "[pastaRaiz][clienteSlug]/Texto/"
```

**Para calendario-editorial:**
```bash
cp "_inbox/[arquivoEstrategia]" "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/estrategia.md"
rm "_inbox/[arquivoEstrategia]"
```

### 5. Ler os arquivos de conteúdo

**Para identidade-visual, logo, redes-sociais:**
- Ler o PDF de Formulario/
- Ler o PDF de Design/
- Ler `_template/LAB360-design-system.md` — seção conforme tipoServico (8A, 8B ou 8C)

**Para calendario-editorial:**
- Ler `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/estrategia.md`
- Ler `_template/LAB360-design-system.md` — seção 8D
- Ler `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md`

### 6. Adicionar card no index.html

Abrir `index.html` na raiz e adicionar novo `<a class="project-card">` dentro de `.projects-grid`:

**Para identidade-visual, logo, redes-sociais:**
```html
<a class="project-card" href="./[pastaRaiz][clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[nomeServicoLegível]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

**Para calendario-editorial:**
```html
<a class="project-card" href="./gestao-de-redes-sociais/[clienteSlug]/[mesAno]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">Calendário Editorial — [mesAno legível]</span>
  <span class="card-action">Ver Calendário</span>
</a>
```

Nomes legíveis por tipo:
- `identidade-visual` → `Identidade Visual Completa`
- `logo` → `Logo`
- `redes-sociais` → `Design para Redes Sociais`

### 7. Gerar o HTML

Invocar o skill `huashu-design` para gerar o HTML de entrega.

Briefing base:
- Estrutura LAB360°: fundo preto, canvas rizoma, nav dots, status bar, cursor cyan
- Seção do design system: conforme tipoServico

**Adicional para calendario-editorial:**
- Seção 8D — implementar grade mensal interativa com cards por dia
- Cor de cada card = cor do pilar definida em PILARES
- Cards clicáveis: expande detalhe com legenda completa (modal ou scroll para seção 5)
- Seção 4 (Calendário) é a peça central — dar espaço visual generoso

Salvar em:
- identidade-visual/logo/redes-sociais: `[pastaRaiz][clienteSlug]/index.html`
- calendario-editorial: `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/index.html`

### 8. Pausar para revisão

Mostrar ao Heleno:
- Seções geradas
- Para calendario-editorial: número de posts no calendário, pilares detectados com cores
- Caminho do arquivo gerado

Aguardar aprovação. Não commitar ainda.

### 9. Após aprovação: commit + push

**Para identidade-visual, logo, redes-sociais:**
```bash
git add "[pastaRaiz][clienteSlug]/" index.html
git commit -m "Gerar [tipoServico]: [clienteNome]"
git push
```

**Para calendario-editorial:**
```bash
git add "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/" index.html
git commit -m "Gerar calendário editorial: [clienteNome] — [mesAno legível]"
git push
```

Vercel deploya em ~30 segundos.

### 10. Criar página no Notion (somente se Texto/ tiver arquivo)

Aplicável apenas para identidade-visual, logo, redes-sociais.
Ler o arquivo em Texto/, criar página no Notion via MCP.

### 11. Entregar as URLs ao Heleno

**Para identidade-visual, logo, redes-sociais:**
```
✓ Entrega pronta para [clienteNome]

URL: https://lab360-entregas.vercel.app/[pastaRaiz][clienteSlug]/
```

**Para calendario-editorial:**
```
✓ Calendário pronto para [clienteNome]

URL: https://lab360-entregas.vercel.app/gestao-de-redes-sociais/[clienteSlug]/[mesAno]/
```
```

- [ ] **Verificar que `calendario-editorial` aparece nos tipos aceitos**

```bash
grep "calendario-editorial" ".claude/commands/novo-pedido.md"
```

Saída esperada: pelo menos 3 linhas com o tipo.

- [ ] **Commit**

```bash
git add ".claude/commands/novo-pedido.md"
git commit -m "feat: atualizar /novo-pedido para suportar calendario-editorial e nova estrutura"
```

---

### Task 8: Atualizar `/novo-cliente`

**Files:**
- Modify: `.claude/commands/novo-cliente.md`

- [ ] **Reescrever o arquivo completo**

Substituir todo o conteúdo de `.claude/commands/novo-cliente.md` por:

```markdown
# Comando: novo-cliente

Cria a estrutura de pastas para um novo cliente/serviço e adiciona o card no index.html.
Use quando quiser preparar a estrutura ANTES de ter os arquivos prontos.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `calendario-editorial`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Studio Alma — calendario-editorial`

Se o tipo não for informado, perguntar antes de prosseguir.

## O que fazer

### 1. Derivar slug e tipo

- **clienteNome**: antes do `—`
- **tipoServico**: depois do `—`
- **clienteSlug**: minúsculas, sem acentos, sem espaços, sem hífens

Mapa de label por serviço:
| tipoServico | labelCard | nomeLegivelCard |
|-------------|-----------|-----------------|
| `identidade-visual` | `Ver Identidade Visual` | `Identidade Visual Completa` |
| `logo` | `Ver Logo` | `Logo` |
| `redes-sociais` | `Ver Redes Sociais` | `Design para Redes Sociais` |
| `calendario-editorial` | `Ver Calendário` | `Calendário Editorial` |

### 2. Criar estrutura de pastas

**Para identidade-visual, logo, redes-sociais:**
```bash
mkdir -p "[pastaRaiz][clienteSlug]/Formulario"
mkdir -p "[pastaRaiz][clienteSlug]/Design"
mkdir -p "[pastaRaiz][clienteSlug]/Texto"
touch "[pastaRaiz][clienteSlug]/Formulario/.gitkeep"
touch "[pastaRaiz][clienteSlug]/Design/.gitkeep"
touch "[pastaRaiz][clienteSlug]/Texto/.gitkeep"
```

Mapa de pastaRaiz:
- `identidade-visual` → `identidade-visual/`
- `logo` → `logo/`
- `redes-sociais` → `redes-sociais/`

**Para calendario-editorial:**
```bash
mkdir -p "gestao-de-redes-sociais/[clienteSlug]"
touch "gestao-de-redes-sociais/[clienteSlug]/.gitkeep"
```
(não cria subpasta de mês — isso é feito no /novo-pedido quando o arquivo chegar)

Se a pasta raiz do cliente já existir (cliente com outro serviço anterior), apenas adicionar a nova subpasta.

### 3. Adicionar card no index.html

**Para identidade-visual, logo, redes-sociais:**
```html
<a class="project-card" href="./[pastaRaiz][clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[nomeLegivelCard]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

**Para calendario-editorial:**
```html
<a class="project-card" href="./gestao-de-redes-sociais/[clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">Calendário Editorial</span>
  <span class="card-action">Ver Calendário</span>
</a>
```

### 4. Git commit

```bash
git add "[pastaRaiz][clienteSlug]/" index.html
git commit -m "chore: criar estrutura para [clienteNome] — [tipoServico]"
```

### 5. Confirmar para o Heleno

Mostrar:
- Pastas criadas
- Próximo passo: colocar os arquivos em `_inbox/` e rodar `/novo-pedido [clienteNome] — [tipoServico]`
- URL futura: `https://lab360-entregas.vercel.app/[pastaRaiz][clienteSlug]/`
```

- [ ] **Commit**

```bash
git add ".claude/commands/novo-cliente.md"
git commit -m "feat: atualizar /novo-cliente para nova estrutura e calendario-editorial"
```

---

## Verificação Final

- [ ] **Testar estrutura de pastas**

```bash
find . -not -path "./.git/*" -not -path "./_inbox/*" -not -path "./.superpowers/*" -type f -name "*.html" | sort
```

Saída esperada: `./identidade-visual/quikciadadanca/index.html` e `./index.html`

- [ ] **Verificar que `clientes/` não existe mais**

```bash
ls clientes/ 2>&1
```

Saída esperada: `ls: clientes/: No such file or directory`

- [ ] **Verificar que index.html raiz aponta para o novo path**

```bash
grep "quikciadadanca" index.html
```

Saída esperada: `href="./identidade-visual/quikciadadanca/"`

- [ ] **Commit final de verificação (se houver arquivos pendentes)**

```bash
git status
git push
```

Vercel deploya em ~30s. Acessar `https://lab360-entregas.vercel.app/identidade-visual/quikciadadanca/` e confirmar que carrega.
