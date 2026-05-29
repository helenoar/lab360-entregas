# LAB 360° — Sistema de Entregas

Documentos HTML profissionais para clientes do LAB 360°, hospedados em um único repo Vercel.

## Estrutura

```
Clientes/[Cliente]/[Projeto]/
  Formulário/           ← Resposta do Tally (brief do cliente)
  Design/               ← PDF do design
  Texto/                ← .txt para Notion
  identidade-visual/
    index.html          ← Documento gerado
```

## Como gerar um documento

1. Heleno coloca PDF em `Design/`, resposta do Tally em `Formulário/`, opcionalmente .txt em `Texto/`
2. Pede para Claude gerar o documento
3. Claude lê o design system, formulário e design, invoca `huashu-design`, commita, push
4. Vercel deploya automaticamente
5. URL entregue ao cliente

## Design System

Sempre ler: `_template/LAB360-design-system.md`

## Instruções do Projeto

Ver: `CLAUDE.md`

## Hosting

- **Vercel:** Um único projeto para todos os clientes
- **URLs:** `[dominio]/Clientes/[Cliente]/[Projeto]/identidade-visual`
- **Meta:** `noindex, nofollow` — não aparecer em buscadores
