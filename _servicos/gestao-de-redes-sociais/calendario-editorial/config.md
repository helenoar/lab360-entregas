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
