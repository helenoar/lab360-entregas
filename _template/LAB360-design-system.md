# LAB 360° — Design System de Referência

Todo HTML gerado para entregas de clientes do LAB 360° (Heleno Carneiro) DEVE seguir este sistema.
Este documento substitui a leitura do arquivo `Referência estética_LAB 360°/index.html`.

## Estrutura de Pastas de Cada Projeto

O serviço é a subpasta direta do cliente. Cada cliente pode ter múltiplos serviços.

```
clientes/[clienteSlug]/
  identidade-visual/    ← Identidade Visual Completa
    Formulario/         ← Resposta do Tally (PDF ou .txt) — brief do cliente
    Design/             ← PDF do Canva com o design feito por Heleno
    Texto/              ← .txt/.md para Notion (opcional)
    index.html          ← gerado por Claude, deployado no Vercel
  logo/                 ← Logo
    Formulario/
    Design/
    Texto/
    index.html
  redes-sociais/        ← Design para Redes Sociais
    Formulario/
    Design/
    Texto/
    index.html
```

## Workflow de Geração do HTML

Ao gerar um documento, ler nesta ordem:

1. `Formulário/` — o que o cliente disse em suas próprias palavras (brief, valores, referências)
2. `Design/` — o que Heleno criou visualmente (cores, tipografia, logo, aplicações)
3. Este design system — estética LAB 360°

Formulários disponíveis (Tally):

- Formulário 1 — Identidade Visual Completa
- Formulário 2 — Logo
- Formulário 3 — Página de Divulgação / Vendas
- Formulário 4 — Design para Redes Sociais

---

## 1. Tokens de Design

```css
:root {
  --black: #161615;
  --white: #ffffff;
  --cyan: #4c97a8;
  --gray: #211f1e;
  --muted: rgba(255, 255, 255, 0.45);
  --ease: cubic-bezier(0.23, 1, 0.32, 1);
}
```

- Fundo: sempre `--black` — **nunca preto puro (`#000`)**, mas também nunca um cinza claro/frio de tela de sistema. O alvo é um preto quase-preto de tom quente/neutro (ex: `#161615`) — a referência é o preto profundo de uma editorial de moda/revista impressa, não o cinza de UI de software. Se parecer "cinza estranho" ao vivo, está claro demais ou com tinta fria demais (evitar tons puxando pro azulado).
- Texto principal: `--white`
- **Blocos de conteúdo (cards, steps, células de calendário, itens de grid) usam `--gray` (#211f1e), nunca `--black`.** Se um bloco de conteúdo usa a mesma cor do fundo da página, o documento lê como "chapado" — só linhas de 1px separando tudo. `--gray` existe exatamente para dar profundidade real (um degrau acima do `--black`, não um cinza médio); usá-lo é obrigatório em qualquer `.card`/`.item`/`.step`/célula que precise se diferenciar do fundo.
- Texto secundário: `rgba(255,255,255,0.5)` a `rgba(255,255,255,0.75)`
- **Destaque / acento (`--cyan` #4C97A8 — ciano dessaturado, não neon):** usar com moderação em pontos de hierarquia — palavra de destaque em headline (`.acc`), barras de topo de card, eyebrows de destaque, hover de botão/modal, dots ativos do side nav (3.4), glow do cursor (3.3). **Não é mais "uso restrito só a nav+cursor"** (isso deixou o documento cinza demais/sem vida) **nem "em tudo"** (isso competia com a paleta por pilar/encontro do cliente e cansava). Regra prática: se o elemento já tem uma cor própria vinda do cliente (paleta de pilares/encontros), não sobrepor cyan nele; se é um elemento neutro de estrutura/chrome do LAB 360° (títulos, labels, bordas de callout, hovers), pode receber o acento.

**Referências de direção visual** (tipografia forte, grid disciplinado, área vazia generosa, cortes de foto inesperados, paleta consistente entre peças): MIT Museum (Pentagram) e a campanha InDance (Alphabet Studio). Usar como norte para identidade visual completa e calendários editoriais — não anular a "moldura" LAB 360° (rizoma/cyan/mono), mas informar hierarquia tipográfica e disciplina de grid.

---

## 2. Tipografia

```html
<link
  href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&display=swap"
  rel="stylesheet"
/>
```

| Uso                        | Fonte          | Peso    |
| -------------------------- | -------------- | ------- |
| Títulos / impacto          | Inter          | 900     |
| Subtítulos / corpo forte   | Inter          | 700     |
| Corpo de texto             | JetBrains Mono | 400     |
| Labels / eyebrows / código | JetBrains Mono | 400/700 |

**Escalas:**

- `headline--hero`: `clamp(3rem, 12vw, 9rem)`, line-height 0.92, letter-spacing -0.03em
- `headline`: `clamp(2.5rem, 8vw, 6.5rem)`, line-height 0.92
- `headline--sm`: `clamp(1.8rem, 4vw, 3rem)`
- `statement`: `clamp(1.7rem, 3.5vw, 2.8rem)`, line-height 1.12
- `body-text`: `clamp(0.9rem, 1.5vw, 1.1rem)`, line-height 1.7
- `eyebrow`: 0.7rem, letter-spacing 0.5em, uppercase, cor `--muted`
- `pull-quote`: `clamp(1.2rem, 3vw, 2rem)`, JetBrains Mono, centralizado

---

## 3. Componentes Obrigatórios em Todo HTML

### 3.1 CDNs no `<head>`

```html
<script src="https://cdn.tailwindcss.com"></script>
<link
  href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&display=swap"
  rel="stylesheet"
/>
<meta name="robots" content="noindex, nofollow" />
```

### 3.2 Canvas Rizoma (background animado)

- Canvas fixo, `z-index: 0`, `pointer-events: none`
- ~250 partículas (120 em mobile) conectadas por linhas cyan quando próximas (raio 180px)
- Partículas flutuam com movimento senoidal orgânico (spring + friction)
- Mouse repele/ilumina partículas num raio de 180px
- Linhas: `rgba(0,240,255, alpha)` onde alpha varia com distância (0.12 a 0.46)
- Partículas: `rgba(255,255,255, alpha)` onde alpha base é 0.08–0.26

### 3.3 Cursor Customizado (apenas desktop)

```css
#cursor {
  position: fixed;
  width: 56px;
  height: 56px;
  background: radial-gradient(circle, var(--cyan) 0%, transparent 70%);
  border-radius: 50%;
  pointer-events: none;
  z-index: 9999;
  transform: translate(-50%, -50%);
  mix-blend-mode: screen;
  filter: blur(5px);
  opacity: 0.75;
}
```

- Segue o mouse com LERP suave (fator 0.11)
- Escondido em `pointer: coarse` (touch)

### 3.4 Side Nav (âncoras de seção)

- Posição fixa, lado direito, 50% vertical
- Dots com label que aparece no hover/active
- Dot ativo: cyan com glow
- Escondido em mobile (< 768px)

### 3.5 Status Bar

- Posição fixa, canto inferior esquerdo
- JetBrains Mono, 0.6rem, letter-spacing 0.35em, uppercase, opacidade 0.45
- Dot cyan pulsante ao lado
- Texto: `LAB 360° · [Nome do Projeto ou Cliente]`

---

## 4. Classes Utilitárias

```css
/* Animações */
.reveal          /* opacity:0 + translateY(40px), ativado pelo IntersectionObserver */
.glow-pulse      /* animation: pulse-cyan 4s ease-in-out infinite */
.accent          /* color: var(--cyan) */
.impact          /* font-family: Inter, weight 900 */
.mono            /* font-family: JetBrains Mono */

/* Divisores */
.line-h          /* 1px rgba(255,255,255,0.1) horizontal */
.line-cyan       /* 1px cyan com glow, horizontal */

/* Blocos de conteúdo */
.chamber         /* bg: --gray, padding 2.5rem 3rem */
.chamber--border /* borda esquerda 2px cyan, padding esquerdo 2.5rem */

/* Brand System Frame */
.brand-system-frame   /* border: 1px solid cyan, box-shadow cyan sutil, padding 5rem 3rem */
.brand-system-tag     /* label absoluto no topo da frame, mono uppercase cyan */

/* Botões */
.btn--outline    /* border rgba(255,255,255,0.25), hover → cyan */
.btn--solid      /* bg white, color black, hover → cyan */
.btn--cyan       /* border cyan, bg transparent, hover → bg rgba cyan */

/* Cards */
.service-card    /* bg black, hover bg rgba cyan 0.03 */
.service-card.selected /* border cyan, bg rgba cyan 0.05 */
```

---

## 5. Layout

```css
.section {
  padding: 15vh 1.5rem;
  max-width: 1100px;
  margin: 0 auto;
}
main {
  position: relative;
  z-index: 1;
}
```

- Conteúdo máximo: **1200px em TODAS as seções**, centrado — usar o mesmo max-width para toda seção do documento (hero, texto, calendário, grids largos). Nunca variar o max-width entre seções (ex: uma seção com 1100px e outra com 1340px): isso desalinha a margem esquerda de cada seção em telas largas e é o bug mais comum de "cada seção começa numa margem diferente".
- Padding vertical generoso: 10–15vh entre seções
- Grid de 2 colunas para cards em desktop, 1 em mobile
- **Grids CSS de N colunas (calendário, cards, etc.):** sempre `grid-template-columns: repeat(N, minmax(0,1fr))`, nunca `repeat(N,1fr)` sozinho. Sem o `minmax(0,...)`, colunas com conteúdo maior "roubam" espaço de colunas vizinhas e a grade fica com larguras desiguais (ex: uma coluna de calendário com 14px de largura ao lado de outra com 97px). Adicionar `min-width:0` aos itens do grid quando o conteúdo pode ser longo.

---

## 6. Animações

```css
@keyframes pulse-cyan {
  0%,
  100% {
    opacity: 0.7;
    text-shadow: 0 0 8px var(--cyan);
  }
  50% {
    opacity: 1;
    text-shadow:
      0 0 20px var(--cyan),
      0 0 40px var(--cyan);
  }
}
@keyframes fade-up {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

**Scroll Reveal (IntersectionObserver):**

- Threshold: 0.12
- Delay escalonado: `i * 80ms`
- Classe `.reveal` vira `.reveal.visible` quando entra na viewport

---

## 7. Paleta de Cores para Swatches (uso nos documentos de cliente)

Quando exibir paleta de um cliente, usar este componente:

```html
<div
  style="display:grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap:1px; background:rgba(255,255,255,0.08);"
>
  <div style="background:[HEX]; padding:3rem 2rem;">
    <div class="eyebrow" style="color:rgba(255,255,255,0.5)">[NOME DA COR]</div>
    <div class="mono" style="font-size:1.1rem; color:white; margin-top:0.5rem">
      [HEX]
    </div>
    <div class="body-text" style="margin-top:0.5rem; font-size:0.8rem">
      [SENSAÇÃO/SIGNIFICADO]
    </div>
  </div>
</div>
```

---

## 8. Estrutura HTML por Tipo de Serviço

O tipo de serviço determina quais seções gerar e em que ordem. Ler o formulário do cliente para identificar o serviço antes de gerar.

Rodapé sempre inclui:

```html
<!-- LAB 360° · Heleno Carneiro -->
```

---

### 8A. IDENTIDADE VISUAL COMPLETA

_Brand book interativo. O mais longo. 12 seções._

| #   | Seção                      | Conteúdo                                                          | Fonte                 |
| --- | -------------------------- | ----------------------------------------------------------------- | --------------------- |
| 1   | **Hero**                   | Nome da marca + logo principal + tagline                          | Formulário Q1, Q2     |
| 2   | **Sobre a Marca**          | O que é, há quanto tempo existe, por que buscou identidade agora  | Formulário Q2, Q3, Q4 |
| 3   | **Posicionamento**         | Diferencial competitivo, concorrentes, o que a distingue          | Formulário Q6, Q7     |
| 4   | **Público**                | Para quem a marca fala, perfil de quem ela quer alcançar          | Formulário Q8         |
| 5   | **Personalidade da Marca** | Como a marca seria se fosse uma pessoa — atributos, comportamento | Formulário Q9, Q10    |
| 6   | **Logo**                   | Versões: principal, horizontal, vertical, símbolo solo, negativo  | Design PDF            |
| 7   | **Paleta de Cores**        | Swatches com hex, nome da cor, sensação/emoção associada          | Design PDF + Q11      |
| 8   | **Tipografia**             | Fontes primária e secundária, hierarquia, exemplos de uso         | Design PDF + Q12      |
| 9   | **Elementos Visuais**      | Padrões gráficos, ícones, texturas, grafismos                     | Design PDF + Q13      |
| 10  | **Voz & Tom**              | Como a marca escreve, palavras que usa, o que evita               | Formulário Q9, Q12    |
| 11  | **Aplicações**             | Mockups nos canais principais onde a marca aparece                | Design PDF + Q14      |
| 12  | **Guia Rápido**            | Regras resumidas: o que pode, o que não pode, como aplicar        | Síntese               |

---

### 8B. LOGO

_Logo kit focado. Entrega cirúrgica sobre o símbolo. 9 seções._

| #   | Seção                                | Conteúdo                                                                        | Fonte                      |
| --- | ------------------------------------ | ------------------------------------------------------------------------------- | -------------------------- |
| 1   | **Hero**                             | Logo principal em grande destaque, limpo, com o nome                            | Design PDF + Q2            |
| 2   | **Conceito**                         | Por que as escolhas foram feitas assim — raciocínio criativo por trás do design | Formulário Q5, Q6, Q7, Q10 |
| 3   | **Versões do Logo**                  | Principal, horizontal, vertical, símbolo solo, com tagline se houver            | Design PDF + Q4, Q13       |
| 4   | **Paleta do Logo**                   | Cores usadas com hex, nome, aplicação                                           | Design PDF + Q9            |
| 5   | **Tipografia**                       | Fonte(s) usadas no logo, nome e uso                                             | Design PDF                 |
| 6   | **Sobre Fundos**                     | Logo em fundo claro, escuro, monocromático, negativo                            | Design PDF + Q12           |
| 7   | **Área de Respiro & Tamanho Mínimo** | Regra visual de espaçamento e escala mínima aceitável                           | Design PDF                 |
| 8   | **Usos Corretos & Incorretos**       | O que não fazer com o logo (distorcer, trocar cor, fundo conflitante)           | Design PDF                 |
| 9   | **Aplicações**                       | Onde aparece: social, site, impresso, produto                                   | Design PDF + Q11           |

---

### 8C. DESIGN PARA REDES SOCIAIS

_Content kit. Não é sobre quem é a marca — é sobre o que foi criado. 8 seções._

| #   | Seção                       | Conteúdo                                                              | Fonte                 |
| --- | --------------------------- | --------------------------------------------------------------------- | --------------------- |
| 1   | **Hero**                    | Nome da marca + objetivo visual das peças                             | Formulário Q1, Q2     |
| 2   | **Estratégia**              | Objetivo das peças, tom visual, plataformas-alvo                      | Formulário Q2, Q3, Q5 |
| 3   | **Galeria das Peças**       | Preview de cada peça entregue, organizada por tipo/formato            | Design PDF + Q1, Q8   |
| 4   | **Detalhamento por Peça**   | Para cada tipo: o que comunica, quando usar, onde publicar            | Formulário Q8, Q3     |
| 5   | **Tipografia das Peças**    | Fontes usadas, hierarquia de texto nas peças                          | Design PDF            |
| 6   | **Paleta**                  | Cores usadas nas peças com hex                                        | Design PDF + Q6       |
| 7   | **Especificações Técnicas** | Tamanhos em px por plataforma (feed 1080×1080, Story 1080×1920, etc.) | Formulário Q3         |
| 8   | **Como Usar**               | Instruções de edição no Canva, onde substituir texto/imagem           | Formulário Q4         |

### 8D. CALENDÁRIO EDITORIAL

_Apresentação do planejamento mensal de conteúdo. Entrega recorrente (mensal). 6 seções._

Fonte dos dados: `_inbox/calendario-[slug]-[mes-ano].md` — arquivo gerado pelo agente de estratégia.
Antes de gerar, ler `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md` para referência estratégica.

| #   | Seção                      | Conteúdo                                                                                                                                      | Fonte no arquivo .md                           |
| --- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| 1   | **Hero**                   | Nome da marca, mês de referência, plataformas cobertas                                                                                        | `META.cliente`, `META.mes`, `META.plataformas` |
| 2   | **Estratégia do Mês**      | Objetivo central, tema do mês, tom de voz, público                                                                                            | `ESTRATÉGIA` completo                          |
| 3   | **Pilares de Conteúdo**    | Cards com nome, cor hex, percentual e descrição de cada pilar                                                                                 | `PILARES` completo                             |
| 4   | **Calendário Visual**      | Grade mensal interativa — cada dia com card clicável: pilar (cor do card), formato, tema. Clique expande detalhe. Dias sem post ficam vazios. | `POSTS` — iterar por data                      |
| 5   | **Detalhamento dos Posts** | Lista de todos os posts: data formatada, pilar, formato, tema e legenda completa                                                              | `POSTS` — todos os campos                      |
| 6   | **O que esperar**          | Lista de indicadores de foco do mês, sem prometer números                                                                                     | `EXPECTATIVAS`                                 |

**Componente do Calendário Visual (seção 4):**

- Grade CSS de 7 colunas (dom–sáb): `grid-template-columns: repeat(7, minmax(0,1fr))` (nunca `repeat(7,1fr)` sozinho — ver seção 5 "Layout")
- Dias sem post: célula vazia com número do dia em `rgba(255,255,255,0.15)`
- Dias com post: card clicável com cor de fundo do pilar (opacidade 0.15), borda esquerda sólida na cor do pilar, número do dia, ícone do formato e título curto
- Clique no card: expande modal ou seção âncora com legenda completa
- Legenda do pilar: lista colorida abaixo do calendário como key
- Mobile: grade de 3 colunas ou lista por semana
- **Tamanhos mínimos de fonte dentro da grade** (letra pequena é o erro mais comum aqui): número do dia ≥ `.8rem`, dias da semana (cabeçalho) ≥ `.7rem`, chip/card de publicação ≥ `.7rem`, tag "TBD"/aguardando data ≥ `.65rem`. Nunca usar tamanhos abaixo de `.6rem` em texto dentro do calendário.
- Altura mínima de célula (`.cd` ou equivalente) ≥ `110px` para caber número do dia + 1-2 chips sem espremer

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

## 9. Meta Tags Obrigatórias

```html
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<meta name="robots" content="noindex, nofollow" />
<title>[Nome do Projeto] — [Tipo do Documento] · LAB 360°</title>
```

---

## 10. Identidade Verbal do LAB 360°

O tom dos textos de interface (labels, status bar, eyebrows) segue:

- Frases curtas, diretas, sem rodeios
- Verbo no presente ou infinitivo
- Sem ponto final em títulos e eyebrows
- Maiúscula apenas em nomes próprios e abreviações
- Português brasileiro

Exemplos de eyebrows: `Identidade Visual · [Cliente]`, `Paleta de Cores`, `Tipografia`, `Aplicações`
