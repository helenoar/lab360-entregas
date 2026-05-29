# LAB 360° — Design System de Referência

Todo HTML gerado para entregas de clientes do LAB 360° (Heleno Carneiro) DEVE seguir este sistema.
Este documento substitui a leitura do arquivo `Referência estética_LAB 360°/index.html`.

## Estrutura de Pastas de Cada Projeto

```
Clientes/[Cliente]/[Projeto]/
  Formulário/       ← Resposta do Tally exportada (PDF ou .txt) — brief do cliente
  Design/           ← PDF do Canva/programa com o design feito por Heleno
  Texto/            ← .txt/.md para criação de página no Notion (revisão de conteúdo)
  identidade-visual/
    index.html      ← gerado por Claude, deployado no Vercel
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
    --black:  #000000;
    --white:  #FFFFFF;
    --cyan:   #00F0FF;
    --gray:   #1A1A1A;
    --muted:  rgba(255,255,255,0.4);
    --ease:   cubic-bezier(0.23, 1, 0.32, 1);
}
```

- Fundo: sempre `--black`
- Texto principal: `--white`
- Destaque / acento: `--cyan` (#00F0FF)
- Blocos de apoio: `--gray` (#1A1A1A)
- Texto secundário: `rgba(255,255,255,0.5)` a `rgba(255,255,255,0.75)`

---

## 2. Tipografia

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
```

| Uso | Fonte | Peso |
|-----|-------|------|
| Títulos / impacto | Inter | 900 |
| Subtítulos / corpo forte | Inter | 700 |
| Corpo de texto | JetBrains Mono | 400 |
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
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700;900&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet">
<meta name="robots" content="noindex, nofollow">
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
    position: fixed; width: 56px; height: 56px;
    background: radial-gradient(circle, var(--cyan) 0%, transparent 70%);
    border-radius: 50%; pointer-events: none; z-index: 9999;
    transform: translate(-50%, -50%);
    mix-blend-mode: screen; filter: blur(5px); opacity: 0.75;
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
.section { padding: 15vh 1.5rem; max-width: 1100px; margin: 0 auto; }
main { position: relative; z-index: 1; }
```

- Conteúdo máximo: 1100px, centrado
- Padding vertical generoso: 10–15vh entre seções
- Grid de 2 colunas para cards em desktop, 1 em mobile

---

## 6. Animações

```css
@keyframes pulse-cyan {
    0%, 100% { opacity: 0.7; text-shadow: 0 0 8px var(--cyan); }
    50%       { opacity: 1;   text-shadow: 0 0 20px var(--cyan), 0 0 40px var(--cyan); }
}
@keyframes fade-up {
    from { opacity: 0; transform: translateY(40px); }
    to   { opacity: 1; transform: translateY(0); }
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
<div style="display:grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap:1px; background:rgba(255,255,255,0.08);">
  <div style="background:[HEX]; padding:3rem 2rem;">
    <div class="eyebrow" style="color:rgba(255,255,255,0.5)">[NOME DA COR]</div>
    <div class="mono" style="font-size:1.1rem; color:white; margin-top:0.5rem">[HEX]</div>
    <div class="body-text" style="margin-top:0.5rem; font-size:0.8rem">[SENSAÇÃO/SIGNIFICADO]</div>
  </div>
</div>
```

---

## 8. Estrutura HTML Base para Documento de Identidade Visual

Seções sempre presentes (na ordem):
1. **Hero** — eyebrow com tipo do documento + nome do projeto, headline com tagline do cliente
2. **Conceito** — origem do nome, intenção, contexto emocional
3. **Paleta de Cores** — swatches com hex, nome, sensação (ver seção 7)
4. **Tipografia** — fontes com exemplos de uso, hierarquia
5. **Logo** — variações, área de respiro, usos corretos
6. **Voz & Tom** — como a marca fala, palavras-chave, exemplos
7. **Aplicações** — exemplos visuais de como a identidade se aplica

Rodapé sempre inclui:
```html
<!-- LAB 360° · Heleno Carneiro -->
```

---

## 9. Meta Tags Obrigatórias

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="robots" content="noindex, nofollow">
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
