# LAB 360° — Instruções de Projeto

## Design System

**SEMPRE ler antes de gerar qualquer HTML:**
```
_template/LAB360-design-system.md
```

Este arquivo contém todos os tokens, componentes, animações e estrutura base. **NUNCA ler** `Referência estética_LAB 360°/index.html` — foi substituído pelo design system.

## Estrutura de Pastas

```
Clientes/[Cliente]/[Projeto]/
  Formulário/           ← resposta do Tally exportada (PDF ou .txt)
  Design/               ← PDF do Canva/programa de design
  Texto/                ← .txt/.md para Notion (conteúdo para revisão)
  identidade-visual/
    index.html          ← Claude gera e commita aqui
```

A pasta de saída é **sempre** `identidade-visual/` — mesmo que o projeto seja só logo, só página ou só redes sociais. Todo trabalho é apresentado como documento de identidade visual completo.

## Workflow de Geração de HTML

1. Ler `[Projeto]/Formulário/` — brief do cliente em voz própria
2. Ler `[Projeto]/Design/` — PDF do design visual
3. Ler `_template/LAB360-design-system.md` — estética LAB 360°
4. **Invocar skill `huashu-design`** para gerar o HTML
5. Salvar em `[Projeto]/identidade-visual/index.html`
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

## Links Importantes

- Plano de implementação: `docs/planos/`
- Design spec: `docs/superpowers/specs/`
