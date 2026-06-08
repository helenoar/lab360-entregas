# Workflow Inbox + Especialistas — Plano de Implementação

> **Para execução:** Use superpowers:executing-plans para executar tarefa por tarefa. Checkboxes indicam progresso.

**Goal:** Redesign do workflow LAB 360°: inbox por pastas sem prefixos, especialistas por tipo de serviço populados com pesquisa web, /novo-pedido sem argumentos, push imediato após geração.

**Architecture:** _inbox/[Cliente]/[tipo]/ — subpasta = tipo de serviço, detectado por `ls` sem leitura de arquivos. _especialistas/ com knowledge bases por tipo. /novo-pedido escaneia inbox, ativa especialista correto, gera HTML, faz push imediato e apaga inbox.

**Tech Stack:** Bash (ls, rm -rf, git), Markdown (comandos Claude), WebSearch (pesquisa especialistas)

---

## Arquivos modificados

| Arquivo | Ação |
|---------|------|
| `_especialistas/identidade-visual.md` | Criar |
| `_especialistas/logo.md` | Criar |
| `_especialistas/redes-sociais.md` | Criar |
| `_especialistas/gestao-de-redes-sociais.md` | Criar (migra + expande _knowledge-base.md) |
| `.claude/commands/novo-pedido.md` | Reescrever completamente |
| `.claude/commands/atualizar-especialista.md` | Criar |
| `.gitignore` | Atualizar seção _inbox |
| `gestao-de-redes-sociais/_knowledge-base.md` | Deletar após migração |
| `CLAUDE.md` | Atualizar seção Fluxo Principal |

---

## Task 1: Criar arquivos de especialistas via pesquisa web

**Files:**
- Create: `_especialistas/identidade-visual.md`
- Create: `_especialistas/logo.md`
- Create: `_especialistas/redes-sociais.md`
- Create: `_especialistas/gestao-de-redes-sociais.md`

- [ ] **Step 1: Criar pasta _especialistas/**

```bash
mkdir -p "/Users/helenocarneiro/CLAUDECODE/LAB 360°/ENTREGAS_LAB 360°/_especialistas"
```

- [ ] **Step 2: Pesquisar e criar especialista — identidade-visual**

Executar as seguintes WebSearches:
- `"brand identity design process best practices 2025 deliverables"`
- `"visual identity system components what to include brand guidelines"`
- `"brand identity presentation structure client deliverable"`

Escrever `_especialistas/identidade-visual.md` seguindo esta estrutura com o conteúdo pesquisado:

```markdown
# Especialista: Identidade Visual

> Atualizado via pesquisa web. Para atualizar: /atualizar-especialista identidade-visual

## O que este serviço entrega

[Descrição do que é uma entrega de identidade visual completa — sistema visual coeso que define como a marca se apresenta visualmente ao mundo. Inclui logo, paleta, tipografia, elementos visuais, voz e guia de aplicação.]

## Framework de análise do brief (formulário do cliente)

O que extrair do formulário:
- Nome da marca e do projeto
- Setor/nicho de atuação
- Público-alvo (faixa etária, comportamento, aspirações)
- Valores e personalidade da marca (3-5 adjetivos)
- Referências visuais que o cliente aprecia e rejeita
- Concorrentes diretos e como se diferenciar
- Plataformas onde a marca vai aparecer

## Componentes da entrega HTML (seção 8A)

12 seções obrigatórias:
1. Hero — nome da marca + tagline visual impactante
2. Sobre a Marca — propósito, história, missão em linguagem da marca
3. Posicionamento — onde se posiciona no mercado, diferencial
4. Público — perfil detalhado do cliente ideal
5. Personalidade — os 5 arquétipos/traços que guiam o tom
6. Logo — apresentação das versões (principal, reduzida, monocromática)
7. Paleta — cores primárias e secundárias com HEX, RGB, CMYK, uso de cada
8. Tipografia — display e corpo, hierarquia, uso correto e incorreto
9. Elementos Visuais — padrões, texturas, ícones, ilustrações da marca
10. Voz & Tom — como a marca fala, exemplos de copy certo e errado
11. Aplicações — mockups em contexto real (cartão, embalagem, digital)
12. Guia Rápido — cheatsheet de uso para o cliente

## Melhores práticas atuais

[Inserir conhecimento pesquisado sobre tendências de identidade visual 2025, o que diferencia uma entrega mediana de uma premium, como apresentar sistemas visuais para clientes não-designers]

## Alertas e armadilhas

- Não replicar cores e tipografia do design system LAB360° — cada seção usa a identidade DO CLIENTE
- Se o brief for vago em personalidade, inferir dos exemplos visuais e confirmar no texto
- Mockups devem ser realistas — evitar contextos que o cliente nunca usará
```

- [ ] **Step 3: Pesquisar e criar especialista — logo**

Executar as seguintes WebSearches:
- `"logo design process best practices 2025 professional"`
- `"logo variations guidelines primary secondary monochrome"`
- `"logo presentation client deliverable what to include"`

Escrever `_especialistas/logo.md`:

```markdown
# Especialista: Logo

> Atualizado via pesquisa web. Para atualizar: /atualizar-especialista logo

## O que este serviço entrega

[Entrega focada exclusivamente no símbolo/marca da empresa: o logotipo em suas versões, regras de uso, área de proteção e aplicações básicas. Não inclui o sistema visual completo — isso é identidade visual.]

## Framework de análise do brief

O que extrair:
- Nome completo da marca e pronuncia/grafia preferida
- Setor e público (para calibrar nível de formalidade)
- Estilo visual desejado (moderno, clássico, minimalista, orgânico)
- Uso principal (digital, impresso, ambos)
- Se existe algum símbolo/elemento que deve ser preservado
- Cores preferenciais ou proibidas
- Referências de logos que admira e que rejeita

## Componentes da entrega HTML (seção 8B)

9 seções obrigatórias:
1. Hero — logo principal em destaque
2. Conceito — o raciocínio criativo por trás do design
3. Versões do Logo — principal, horizontal, ícone/símbolo, badge
4. Paleta — cores do logo com códigos HEX, Pantone quando relevante
5. Tipografia — fonte(s) do logo e família tipográfica relacionada
6. Sobre Fundos — quais fundos o logo suporta (claro, escuro, colorido)
7. Área de Respiro — espaçamento mínimo obrigatório ao redor do logo
8. Usos Corretos & Incorretos — grid visual mostrando o que fazer e não fazer
9. Aplicações — logo em contexto (cartão de visita, avatar, assinatura)

## Melhores práticas atuais

[Inserir conhecimento pesquisado: tendências de logo design 2025, importância de versatilidade (horizontal/ícone), o que faz um logo funcionar em contextos digitais vs físicos, logos com boa redução para ícone]

## Alertas e armadilhas

- Seção de usos incorretos é tão importante quanto os corretos — clientes precisam ver o que NÃO fazer
- Área de respiro deve ter medida relativa (ex: "1x a altura da letra")
- Apresentar sempre versão monocromática — cliente vai usar em bordado, gravação, carimbo
```

- [ ] **Step 4: Pesquisar e criar especialista — redes-sociais**

Executar as seguintes WebSearches:
- `"social media design best practices 2025 instagram post design"`
- `"social media visual content guidelines brand consistency"`
- `"instagram content design specifications 2025 formats"`

Escrever `_especialistas/redes-sociais.md`:

```markdown
# Especialista: Design para Redes Sociais

> Atualizado via pesquisa web. Para atualizar: /atualizar-especialista redes-sociais

## O que este serviço entrega

[Pacote de peças visuais prontas para uso em redes sociais: posts feed, stories, reels cover, destaques. Cada peça é apresentada com especificações técnicas e orientação de uso.]

## Framework de análise do brief

O que extrair:
- Plataformas prioritárias (Instagram, TikTok, LinkedIn, outros)
- Número e tipos de peças no pack (feed quadrado, carrossel, stories, etc.)
- Identidade visual já existente ou nova (paleta, fontes)
- Objetivo de cada peça (engajamento, conversão, institucional)
- Tom de voz (formal, descontraído, inspirador, técnico)
- Referências visuais do nicho que funcionam

## Componentes da entrega HTML (seção 8C)

8 seções obrigatórias:
1. Hero — destaque visual do pack completo
2. Estratégia — objetivo de cada peça e quando usar
3. Galeria das Peças — todas as peças em visão geral
4. Detalhamento por Peça — cada peça individualmente com nome e uso
5. Tipografia — fonte e tamanhos usados nas peças
6. Paleta — cores do pack com HEX
7. Especificações Técnicas — dimensões, formatos, resolução de cada peça
8. Como Usar — orientação prática para o cliente publicar

## Melhores práticas atuais

[Inserir conhecimento pesquisado: formatos com melhor desempenho no Instagram 2025, especificações técnicas atualizadas (Reels: 1080x1920, Feed: 1080x1080, Carrossel: 1080x1080), o que diferencia conteúdo que converte]

## Especificações técnicas base (2025)

| Formato | Dimensões | Proporção | Tamanho máx |
|---------|-----------|-----------|-------------|
| Feed quadrado | 1080x1080px | 1:1 | 30MB |
| Feed retrato | 1080x1350px | 4:5 | 30MB |
| Stories/Reels | 1080x1920px | 9:16 | 4GB (vídeo) |
| Carrossel | 1080x1080px | 1:1 | 30MB/slide |

## Alertas e armadilhas

- Textos em peças devem ocupar no máximo 20% da área (regra geral — verificar plataforma)
- Sempre entregar versão para stories além do feed
- Paleta da entrega deve vir do brief do cliente, NÃO do design system LAB360°
```

- [ ] **Step 5: Criar especialista — gestao-de-redes-sociais (migrar + expandir)**

Pesquisar:
- `"editorial calendar strategy instagram 2025 content pillars"`
- `"social media content strategy best practices 2025 brazil"`

Escrever `_especialistas/gestao-de-redes-sociais.md` com o conteúdo migrado do `_knowledge-base.md` existente + conteúdo pesquisado:

```markdown
# Especialista: Gestão de Redes Sociais — Calendário Editorial

> Atualizado via pesquisa web. Para atualizar: /atualizar-especialista gestao-de-redes-sociais

## O que este serviço entrega

Calendário editorial mensal completo: estratégia do mês, pilares de conteúdo com proporções, e grade visual interativa de todos os posts planejados. Substitui a necessidade de usar Claude Desktop para geração de estratégia.

## Framework de análise do formulário (Tally)

O que extrair:
- Nome da marca/cliente
- Mês e ano de referência
- Plataforma principal (Instagram, TikTok, ambas)
- Fase da conta (crescimento, consolidação, manutenção)
- Objetivo do mês (novos seguidores, engajamento, conversão, lançamento)
- Nicho/setor da marca
- Eventos ou datas importantes do mês para o nicho
- Tom de comunicação da marca

## Geração da estratégia (feito internamente, sem Claude Desktop)

### 1. Definir pilares
Selecionar 3–5 pilares do framework abaixo adequados ao objetivo do mês.

### 2. Definir mix
Aplicar a proporção do mix conforme objetivo principal:

| Objetivo | Educativo | Produto | Institucional | Engajamento | Entret. |
|---|---|---|---|---|---|
| Crescimento de seguidores | 25% | 15% | 15% | 20% | 25% |
| Engajamento da comunidade | 35% | 20% | 20% | 25% | 0% |
| Conversão/vendas | 25% | 40% | 15% | 15% | 5% |
| Reconhecimento de marca | 20% | 20% | 30% | 15% | 15% |

### 3. Calcular volume
| Posts/semana | Posts/mês | Perfil |
|---|---|---|
| 3x | 12–14 | Manutenção |
| 4x | 16–18 | Crescimento moderado |
| 5x | 20–22 | Crescimento ativo |
| 7x | 28–30 | Crescimento agressivo |

### 4. Distribuir no calendário
- Nunca dois posts do mesmo pilar consecutivos
- Alternar: post de valor (educativo/produto) → post relacional (institucional/engajamento)
- Terça a quinta = prioridade (maior engajamento)
- Verificar datas comemorativas do mês e do nicho

## Framework de Pilares de Conteúdo

| Pilar | Objetivo principal | Melhor formato | KPI |
|---|---|---|---|
| Educativo | Autoridade e salvamentos | Carrossel, Reels explicativo | Salvamentos |
| Institucional | Confiança e humanização | Feed estático, Stories | Comentários, DMs |
| Produto/Serviço | Conversão e leads | Reels, Stories com link | Cliques, DMs |
| Engajamento | Interações ativas | Stories (enquete/quiz) | Votos, respostas |
| Entretenimento | Alcance e novos seguidores | Reels (trends) | Alcance, seguidores |

## Instagram — Boas Práticas 2024–2025

### Algoritmo e formatos
- **Reels**: maior distribuição orgânica. Ideal: 3–5x/semana
- **Carrossel**: maior salvamento. Algoritmo reexibe se não visto completamente
- **Feed estático**: menor alcance orgânico. Use com moderação
- **Stories**: mantêm relacionamento, não ampliam alcance. Use diariamente

### Frequência por fase da conta
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
1. **Hook** (primeiras 125 chars — antes do "mais"): pergunta, dado surpreendente, afirmação provocativa
2. **Corpo**: parágrafos de 1–2 linhas, quebras, emojis como marcadores
3. **CTA claro no final**: uma ação — "salva", "comenta X", "compartilha"

### Hashtags
3–5 hashtags específicas superam 30 genéricas. Mix: 2-3 nicho (<500k) + 1-2 médio porte + máx 1 alto alcance.

## TikTok — Boas Práticas 2024–2025
- Baseado em conclusão do vídeo e compartilhamento, não seguidores
- Trending sounds aumentam distribuição ~30%
- Hook nos primeiros 2 segundos é decisivo
- Frequência mínima: 3x/semana — abaixo disso o algoritmo desacelera a conta

## Componentes da entrega HTML (seção 8D)

6 seções obrigatórias:
1. Hero — nome do cliente + mês de referência
2. Estratégia do Mês — objetivo, pilares escolhidos, justificativa
3. Pilares de Conteúdo — cada pilar com cor, descrição e proporção
4. Calendário Visual — grade mensal interativa com cards coloridos por pilar (peça central da entrega)
5. Detalhamento dos Posts — cada post com data, pilar, formato, ideia de conteúdo
6. O que Esperar — métricas esperadas, dicas de publicação

## Alertas e armadilhas
- A grade do calendário (seção 4) deve ser interativa — cards clicáveis que expandem o detalhe
- Cor de cada card = cor do pilar definida na seção 3
- Extrair o mes-ano do formulário; se não estiver explícito, perguntar antes de gerar
- Posts marcados como TBD devem ser visíveis (não invisíveis) — usar cor mais suave do pilar
```

- [ ] **Step 6: Commit dos especialistas**

```bash
cd "/Users/helenocarneiro/CLAUDECODE/LAB 360°/ENTREGAS_LAB 360°"
git add _especialistas/
git commit -m "feat: criar arquivos de especialistas por tipo de serviço"
git push
```

Verificar: `ls _especialistas/` deve listar 4 arquivos `.md`.

---

## Task 2: Atualizar .gitignore

**Files:**
- Modify: `.gitignore`

- [ ] **Step 1: Substituir regras de inbox**

No `.gitignore`, localizar o bloco:
```
# Inbox — arquivos temporários de clientes (não commitar)
_inbox/*.pdf
_inbox/*.txt
_inbox/*.md
_inbox/*.png
_inbox/*.jpg
_inbox/*.jpeg
```

Substituir por:
```
# Inbox — pastas de clientes temporárias (não commitar)
_inbox/*/
```

Isso ignora todas as subpastas de `_inbox/` mas mantém `.gitkeep` e o diretório raiz.

- [ ] **Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: atualizar .gitignore para novo formato de inbox por pasta"
git push
```

---

## Task 3: Reescrever .claude/commands/novo-pedido.md

**Files:**
- Modify: `.claude/commands/novo-pedido.md` (reescrita total)

- [ ] **Step 1: Reescrever o arquivo**

Conteúdo completo do novo `.claude/commands/novo-pedido.md`:

```markdown
# Comando: novo-pedido

Processa tudo que estiver em `_inbox/` de ponta a ponta, sem argumentos.

## Como usar

`/novo-pedido` — sem argumentos.

Heleno cria a pasta no Finder antes de rodar:
```
_inbox/
  [Nome do Cliente]/
    [tipo-servico]/
      arquivo1.pdf
      arquivo2.pdf
```

## O que fazer — passo a passo

### 1. Escanear inbox

Raiz do projeto: `/Users/helenocarneiro/CLAUDECODE/LAB 360°/ENTREGAS_LAB 360°/`

```bash
ls "_inbox/"
```

Ignorar arquivos soltos (`.gitkeep`, etc.) — processar apenas subpastas.

Se inbox vazia: parar com mensagem:
> "Nenhum cliente em `_inbox/`. Crie `_inbox/[Nome do Cliente]/[tipo-servico]/` e adicione os arquivos."

### 2. Para cada pasta de cliente

**2a. Derivar identificadores**

- `clienteNome` = nome da pasta (ex: `Quik Cia de Dança`)
- `clienteSlug` = minúsculas, sem espaços, sem acentos, sem hífens

Regras de slug: á→a, ã→a, â→a, é→e, ê→e, í→i, ó→o, ô→o, õ→o, ú→u, ç→c.

Exemplos:
| clienteNome | clienteSlug |
|-------------|-------------|
| Quik Cia de Dança | `quikciadadanca` |
| Studio Alma | `studioalma` |
| Café Raiz | `caferaiz` |

**2b. Detectar tipo de serviço**

```bash
ls "_inbox/[clienteNome]/"
```

O nome da subpasta = `tipoServico`. Tipos válidos:

| tipoServico | Arquivos esperados |
|-------------|-------------------|
| `identidade-visual` | formulario.pdf + design.pdf |
| `logo` | formulario.pdf + design.pdf |
| `redes-sociais` | formulario.pdf + design.pdf |
| `gestao-de-redes-sociais` | formulario.pdf (resposta do Tally) |

Erros:
- Tipo não reconhecido → parar: "Tipo `[nome]` inválido. Tipos aceitos: identidade-visual, logo, redes-sociais, gestao-de-redes-sociais"
- Arquivo obrigatório ausente → parar: "Faltando [arquivo] em `_inbox/[clienteNome]/[tipo]/`"
- Dois PDFs sem distinção → inferir pelo nome do arquivo qual é form vs design; se impossível, perguntar

### 3. Carregar especialista

Ler: `_especialistas/[tipoServico].md`

Este arquivo contém o framework de análise, melhores práticas e o que incluir na entrega.

### 4. Ler arquivos do inbox

Ler todos os arquivos em `_inbox/[clienteNome]/[tipoServico]/`.

### 5. Criar estrutura de pasta de destino

**Para identidade-visual, logo, redes-sociais:**

Mapa de pastas raiz:
| tipoServico | pastaRaiz |
|-------------|-----------|
| `identidade-visual` | `identidade-visual/` |
| `logo` | `logo/` |
| `redes-sociais` | `redes-sociais/` |

```bash
mkdir -p "[pastaRaiz][clienteSlug]/Formulario"
mkdir -p "[pastaRaiz][clienteSlug]/Design"
mkdir -p "[pastaRaiz][clienteSlug]/Texto"
```

Mover arquivos:
```bash
mv "_inbox/[clienteNome]/[tipoServico]/[arquivoFormulario]" "[pastaRaiz][clienteSlug]/Formulario/"
mv "_inbox/[clienteNome]/[tipoServico]/[arquivoDesign]"     "[pastaRaiz][clienteSlug]/Design/"
```

**Para gestao-de-redes-sociais:**

Extrair `mesAno` do formulário (ex: "junho 2025" → `junho-2025`). Se não encontrar, perguntar ao Heleno antes de prosseguir.

```bash
mkdir -p "gestao-de-redes-sociais/[clienteSlug]/[mesAno]"
cp "_inbox/[clienteNome]/gestao-de-redes-sociais/[arquivoFormulario]" "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/formulario.pdf"
```

### 6. Gerar HTML

Consultar `_template/LAB360-design-system.md` seção correspondente ao tipo.

**Para identidade-visual** → seção 8A (12 seções):
Invocar skill `huashu-design`. Salvar em `identidade-visual/[clienteSlug]/index.html`.

**Para logo** → seção 8B (9 seções):
Invocar skill `huashu-design`. Salvar em `logo/[clienteSlug]/index.html`.

**Para redes-sociais** → seção 8C (8 seções):
Invocar skill `huashu-design`. Salvar em `redes-sociais/[clienteSlug]/index.html`.

**Para gestao-de-redes-sociais** → seção 8D (6 seções):
1. Usar o especialista carregado no step 3 para gerar a estratégia editorial internamente:
   - Identificar pilares de conteúdo a partir do formulário
   - Definir mix por pilar com base no objetivo do mês
   - Montar o calendário com distribuição semanal equilibrada
   - Detalhar cada post com data, pilar, formato e ideia de conteúdo
2. Salvar estratégia em `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/estrategia.md`
3. Invocar skill `huashu-design` com a estratégia gerada. Salvar em `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/index.html`.

### 7. Adicionar card no index.html

O `index.html` da raiz agrupa projetos por cliente com `data-cliente="[slug]"`.

**Verificar se cliente já existe:** buscar `data-cliente="[clienteSlug]"` no arquivo.

URL base: `https://lab360-entregas.vercel.app/`

Paths por tipo:
| tipoServico | path | labelServiço |
|-------------|------|--------------|
| `identidade-visual` | `identidade-visual/[slug]` | `Identidade Visual` |
| `logo` | `logo/[slug]` | `Logo` |
| `redes-sociais` | `redes-sociais/[slug]` | `Design para Redes Sociais` |
| `gestao-de-redes-sociais` | `gestao-de-redes-sociais/[slug]/[mesAno]` | `Calendário Editorial · [mês abrev]/[ano]` |

**Se cliente JÁ existe** — adicionar card dentro do `.projects-grid` e atualizar contador:
```html
<a class="project-card" href="https://lab360-entregas.vercel.app/[path]/" target="_blank" rel="noopener">
  <span class="project-service">[labelServiço]</span>
  <span class="project-name">[nomeProjeto]</span>
  <span class="card-action">Ver entrega</span>
</a>
```

**Se cliente NÃO existe** — criar novo bloco antes do `</main>`:
```html
<div class="clients-sep"></div>
<div class="client-group" data-cliente="[clienteSlug]">
  <div class="client-header">
    <span class="client-name">[clienteNome]</span>
    <span class="client-count">1 projeto</span>
    <button class="copy-link" onclick="copyClientLink('[clienteSlug]', this)">Copiar link</button>
  </div>
  <div class="projects-grid">

    <a class="project-card" href="https://lab360-entregas.vercel.app/[path]/" target="_blank" rel="noopener">
      <span class="project-service">[labelServiço]</span>
      <span class="project-name">[nomeProjeto]</span>
      <span class="card-action">Ver entrega</span>
    </a>

  </div>
</div>
```

`nomeProjeto` vem do brief. Se não houver nome de projeto, usar o nome da marca.

### 8. Commit e push imediato

**Para identidade-visual, logo, redes-sociais:**
```bash
git add "[pastaRaiz][clienteSlug]/" index.html
git commit -m "Gerar [labelServiço]: [clienteNome]"
git push
```

**Para gestao-de-redes-sociais:**
```bash
git add "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/" index.html
git commit -m "Gerar calendário editorial: [clienteNome] — [mesAno legível]"
git push
```

Vercel deploya em ~30s.

### 9. Limpar inbox

```bash
rm -rf "_inbox/[clienteNome]/"
```

### 10. Entregar URLs

```
✓ [labelServiço] pronto para [clienteNome]

URL: https://lab360-entregas.vercel.app/[path]/
```

Se múltiplos clientes foram processados, listar todas as URLs.

**Revisão:** Heleno abre a URL no browser. Se precisar de ajuste, solicita → Claude corrige → commit + push → URL atualizada em ~30s.
```

- [ ] **Step 2: Commit**

```bash
git add ".claude/commands/novo-pedido.md"
git commit -m "feat: reescrever /novo-pedido — inbox por pasta, sem argumentos, push imediato"
git push
```

---

## Task 4: Criar .claude/commands/atualizar-especialista.md

**Files:**
- Create: `.claude/commands/atualizar-especialista.md`

- [ ] **Step 1: Criar o arquivo**

Conteúdo completo:

```markdown
# Comando: atualizar-especialista

Atualiza o knowledge base de um especialista do LAB 360° com pesquisa web atual.

## Como usar

`/atualizar-especialista [tipo-servico]`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

## O que fazer

### 1. Validar tipo

Se tipo não reconhecido:
> "Tipo `[nome]` inválido. Tipos aceitos: identidade-visual, logo, redes-sociais, gestao-de-redes-sociais"

### 2. Pesquisar — 4 a 6 buscas

Termos de busca por tipo:

**identidade-visual:**
- `"brand identity design process best practices [ano atual]"`
- `"visual identity deliverables what to include client"`
- `"brand guidelines structure professional 2025"`

**logo:**
- `"logo design principles best practices [ano atual]"`
- `"logo design process deliverables professional"`
- `"logo variations guidelines monochrome primary secondary"`

**redes-sociais:**
- `"social media design best practices instagram [ano atual]"`
- `"social media post design guidelines brand consistency"`
- `"instagram content design specifications formats [ano atual]"`

**gestao-de-redes-sociais:**
- `"editorial calendar strategy instagram [ano atual]"`
- `"social media content strategy pillars best practices"`
- `"instagram algorithm [ano atual] what works"`
- `"content marketing calendar brazil social media"`

### 3. Reescrever o arquivo

Reescrever `_especialistas/[tipoServico].md` mantendo a estrutura de seções original, atualizando:
- Melhores práticas com dados do ano atual
- Especificações técnicas (dimensões, limites, formatos)
- O que o algoritmo prioriza atualmente
- Tendências relevantes para o nicho

Adicionar no topo: `> Atualizado em [data]. Para atualizar: /atualizar-especialista [tipo]`

### 4. Commit e push

```bash
git add "_especialistas/[tipoServico].md"
git commit -m "Atualizar especialista: [tipoServico] — [data]"
git push
```

Reportar: "Especialista `[tipo]` atualizado com pesquisa de [data]."
```

- [ ] **Step 2: Commit**

```bash
git add ".claude/commands/atualizar-especialista.md"
git commit -m "feat: criar comando /atualizar-especialista"
git push
```

---

## Task 5: Atualizar CLAUDE.md e migrar knowledge-base

**Files:**
- Modify: `CLAUDE.md`
- Delete: `gestao-de-redes-sociais/_knowledge-base.md`

- [ ] **Step 1: Atualizar seção "Fluxo Principal" no CLAUDE.md**

Localizar a seção `## Fluxo Principal — Novo Pedido (use este)` e substituir pelo seguinte:

```markdown
## Fluxo Principal — Novo Pedido (use este)

Quando Heleno traz um novo cliente/pedido:

1. Heleno cria no Finder: `_inbox/[Nome do Cliente]/[tipo-servico]/`
2. Heleno coloca os arquivos dentro da subpasta do tipo
3. Heleno digita: `/novo-pedido`
4. Claude detecta cliente e serviço pelo nome das pastas (sem ler arquivos)
5. Claude ativa o especialista do tipo de serviço (`_especialistas/[tipo].md`)
6. Claude lê os arquivos, gera estratégia (se calendário) e HTML
7. Claude commita, faz push, apaga a pasta do inbox
8. Vercel deploya em ~30s — URL pronta para o cliente

**Tipos aceitos (nome da subpasta):** `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

**Para calendário editorial:** Heleno coloca o formulário do Tally (PDF) na pasta. Claude gera a estratégia internamente — não é mais necessário usar o Claude Desktop.

**Revisão:** Heleno abre a URL no Vercel e solicita ajustes se necessário. Push imediato, sem etapa de aprovação local.
```

- [ ] **Step 2: Atualizar referência a _knowledge-base no CLAUDE.md**

Localizar qualquer menção a `_knowledge-base.md` e substituir por `_especialistas/gestao-de-redes-sociais.md`.

- [ ] **Step 3: Deletar knowledge-base antiga**

```bash
rm "gestao-de-redes-sociais/_knowledge-base.md"
```

- [ ] **Step 4: Commit final**

```bash
git add CLAUDE.md
git rm "gestao-de-redes-sociais/_knowledge-base.md"
git commit -m "chore: atualizar CLAUDE.md e migrar knowledge-base para _especialistas/"
git push
```

---

## Verificação final

- [ ] `ls _especialistas/` → 4 arquivos: identidade-visual.md, logo.md, redes-sociais.md, gestao-de-redes-sociais.md
- [ ] `cat .gitignore | grep inbox` → deve mostrar `_inbox/*/`
- [ ] `.claude/commands/novo-pedido.md` existe e começa com "sem argumentos"
- [ ] `.claude/commands/atualizar-especialista.md` existe
- [ ] `gestao-de-redes-sociais/_knowledge-base.md` não existe mais
- [ ] CLAUDE.md menciona `_especialistas/` e não menciona Claude Desktop
