# Design — Sistema de Entregas LAB 360° (expansão)

**Data:** 2026-06-08  
**Status:** aprovado pelo usuário

---

## Contexto

O sistema atual entrega identidade visual, logo e design para redes sociais via Vercel. Este documento especifica a expansão para suportar qualquer tipo de entrega visual/estratégica — começando pelo calendário editorial — com arquitetura escalável por tipo de serviço.

---

## Decisões de Design

### 1. Estrutura de pastas invertida

Raiz por **serviço**, depois cliente, depois variante.

```
[servico]/
  [clienteslug]/
    index.html                    ← identidade visual, logo, redes-sociais
    [mes-ano]/
      index.html                  ← calendario-editorial (entrega recorrente)

identidade-visual/
  quikciadadanca/
    index.html
  studioalma/
    index.html

gestao-de-redes-sociais/
  studioalma/
    junho-2025/
      index.html
    julho-2025/
      index.html

_inbox/                           ← arquivos temporários de entrada
_servicos/                        ← configs por tipo de serviço
_template/                        ← design system LAB360°
index.html                        ← dashboard raiz
```

**URLs resultantes:**
- `lab360-entregas.vercel.app/identidade-visual/studioalma/`
- `lab360-entregas.vercel.app/gestao-de-redes-sociais/studioalma/junho-2025/`

### 2. Configs por serviço (`_servicos/`)

Cada tipo de serviço tem uma pasta autodescritiva:

```
_servicos/
  identidade-visual/
    config.md
  logo/
    config.md
  redes-sociais/
    config.md
  gestao-de-redes-sociais/
    calendario-editorial/
      config.md
      knowledge-base.md     ← conhecimento de estratégia de redes 2024–2025
```

`config.md` define: slug, pasta de saída, formato de input esperado em `_inbox/`, referência às seções do design system.

`knowledge-base.md` contém conhecimento de domínio que Claude usa ao gerar o HTML (algoritmos de plataforma, pilares de conteúdo, boas práticas de frequência e legenda).

### 3. Pipeline unificado

Comando padrão para todos os serviços:

```
/novo-pedido Nome do Cliente — tipo-de-servico
```

| Serviço | Comando | Arquivos em `_inbox/` |
|---|---|---|
| Identidade Visual | `/novo-pedido Studio Alma — identidade-visual` | `form-[slug].pdf` + `design-[slug].pdf` |
| Logo | `/novo-pedido Studio Alma — logo` | `form-[slug].pdf` + `design-[slug].pdf` |
| Design Redes Sociais | `/novo-pedido Studio Alma — redes-sociais` | `form-[slug].pdf` + `design-[slug].pdf` |
| Calendário Editorial | `/novo-pedido Studio Alma — calendario-editorial` | `calendario-[slug]-[mes][ano].md` |

Para calendário editorial, o mês/ano é extraído do nome do arquivo em `_inbox/`.

### 4. Seções do HTML — Calendário Editorial (8D)

Nova seção a adicionar ao `_template/LAB360-design-system.md`:

| # | Seção | Conteúdo | Fonte |
|---|---|---|---|
| 1 | **Hero** | Nome da marca, mês, plataformas cobertas | `META` do `estrategia.md` |
| 2 | **Estratégia do Mês** | Objetivo, tema central, tom de voz, público | `ESTRATÉGIA` do `estrategia.md` |
| 3 | **Pilares de Conteúdo** | Cards com nome, cor, %, descrição | `PILARES` do `estrategia.md` |
| 4 | **Calendário Visual** | Grade mensal interativa — cada dia com card clicável (pilar, formato, tema) | `POSTS` do `estrategia.md` |
| 5 | **Detalhamento dos Posts** | Lista completa com data, pilar, formato, tema e legenda expandida | `POSTS` do `estrategia.md` |
| 6 | **O que esperar** | Indicadores de foco do mês (sem prometer números) | `EXPECTATIVAS` do `estrategia.md` |

### 5. Formato do arquivo de estratégia (`calendario-[slug]-[mes][ano].md`)

Output do agente de estratégia externo. Claude Code lê este arquivo para gerar o HTML.

```markdown
# Estratégia — [Nome do Cliente] — [Mês Ano]

## META
cliente: Studio Alma
slug: studioalma
mes: junho-2025
plataformas: Instagram, TikTok
objetivo_do_mes: Aumentar reconhecimento de marca com foco em educação

## ESTRATÉGIA
tom_de_voz: educativo, próximo, direto
tema_central: Como cuidar da sua pele no inverno
publico: Mulheres 25–40 anos interessadas em skincare natural

## PILARES
- nome: Educativo
  cor: #5B8DEF
  percentual: 35%
  descricao: Dicas, mitos e verdades sobre skincare

- nome: Produto
  cor: #34D399
  percentual: 30%
  descricao: Apresentação de produtos e resultados

- nome: Institucional
  cor: #A78BFA
  percentual: 20%
  descricao: Bastidores, equipe, valores da marca

- nome: Engajamento
  cor: #F59E0B
  percentual: 15%
  descricao: Perguntas, votações, interações

## POSTS
- data: 2025-06-02
  pilar: Educativo
  formato: Reels
  tema: 3 erros que ressecam a pele no inverno
  legenda: |
    Você faz isso sem saber? ❄️
    
    O inverno pede atenção redobrada com a pele — mas a maioria erra nos básicos.
    
    Salva esse vídeo e me conta nos comentários qual erro você cometia!

## EXPECTATIVAS
- Crescimento de seguidores no Instagram (meta: +5%)
- Aumento de salvamentos em posts educativos
- Engajamento nos stories acima de 10%
```

### 6. Agente de estratégia (externo)

Claude Project dedicado (claude.ai) com system prompt que:
- Recebe respostas do formulário Tally
- Guia Heleno pelas decisões de estratégia
- Entrega o arquivo `.md` no formato acima

O prompt completo foi entregue em conversa — Heleno cria o Project manualmente.

### 7. Migração

O único cliente existente precisa ser movido:

```
clientes/quikciadadanca/moverconsciente/ → identidade-visual/quikciadadanca/
```

O `index.html` do dashboard raiz e o CLAUDE.md precisam ser atualizados para refletir a nova estrutura.

---

## O que muda em cada arquivo existente

| Arquivo | Mudança |
|---|---|
| `CLAUDE.md` | Estrutura de pastas nova, novos tipos de serviço, novo formato de inbox para calendário |
| `_template/LAB360-design-system.md` | Adicionar seção 8D (Calendário Editorial) |
| `index.html` (raiz) | Atualizar dashboard para nova estrutura |
| `.claude/commands/novo-pedido.md` | Suportar `calendario-editorial` como tipo |
| `.claude/commands/novo-cliente.md` | Suportar novos tipos |

---

## O que é criado de novo

| Arquivo | Conteúdo |
|---|---|
| `_servicos/identidade-visual/config.md` | Config do serviço existente |
| `_servicos/logo/config.md` | Config do serviço existente |
| `_servicos/redes-sociais/config.md` | Config do serviço existente |
| `_servicos/gestao-de-redes-sociais/calendario-editorial/config.md` | Config do novo serviço |
| `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md` | Conhecimento de estratégia de redes 2024–2025 |
| `gestao-de-redes-sociais/.gitkeep` | Cria a pasta raiz do serviço |
| `identidade-visual/.gitkeep` | Cria a pasta raiz migrada |
