# LAB 360° — Instruções de Projeto

## 🔴 CRÍTICO: Design System LAB360° ≠ Identidade Visual do Cliente

### Design System LAB360° = ESTRUTURA do documento
- Arquivo: `_template/LAB360-design-system.md`
- É SEMPRE IGUAL para todos os clientes
- Define: moldura (fundo preto, nav dots, status bar, canvas rizoma)
- É a MOLDURA/APRESENTAÇÃO do documento

### Identidade Visual do Cliente = CONTEÚDO do documento
- Fonte: PDF do Canva em `clientes/[clienteslug]/[tiposervico]/Design/`
- MUDA a cada cliente
- Define: paleta, tipografia, logo DO CLIENTE
- É o que está DENTRO da moldura

**O HTML mostra a identidade visual do cliente, apresentada na estrutura LAB360°.**

---

**SEMPRE ler Design System antes de gerar HTML:**
```
_template/LAB360-design-system.md
```
Leitura é para entender a ESTRUTURA, não para replicar cores/tipografia no cliente.

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

Slugs de clientes sempre em minúsculas, sem espaços, sem acentos, sem hífens.

| Nome real | Slug correto |
|-----------|-------------|
| Quik Cia de Dança | `quikciadadanca` |
| Studio Alma | `studioalma` |
| Café Raiz | `caferaiz` |

Mês/ano: `junho-2025`, `julho-2025`, etc. (por extenso, com hífen).

## Criar estrutura manualmente (sem inbox)

Use `/novo-cliente` quando quiser criar a estrutura de pastas SEM ter os arquivos ainda — por exemplo, para já deixar a pasta pronta antes do cliente enviar os materiais.

```
/novo-cliente Nome do Cliente — tipo-de-servico
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais`

Cria as subpastas em `clientes/[clienteslug]/[tipo-de-servico]/`, adiciona card no `index.html` e faz commit. Se o cliente já existe, apenas adiciona o novo serviço.

> Para o fluxo completo (com arquivos prontos), use `/novo-pedido` — ver seção abaixo.

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

## Workflow de Geração de HTML (fluxo manual/interno)

> Este fluxo é executado automaticamente pelo `/novo-pedido`. Use manualmente só se precisar regerar um HTML sem usar o pipeline completo.

1. Ler `Formulario/` — brief do cliente em voz própria
2. Ler `Design/` — PDF do design visual
3. Ler `_template/LAB360-design-system.md` — seção 8 para identificar as seções conforme o serviço
4. **Invocar skill `huashu-design`** para gerar o HTML
5. Salvar em `clientes/[clienteslug]/[tipo-de-servico]/index.html`
6. `git add` + `git commit` + `git push`
7. Vercel auto-deploya em ~30s
8. Entregar URL ao Heleno

### Seções por serviço

As seções de cada tipo de entrega estão definidas em `_template/LAB360-design-system.md` seção 8 (8A, 8B, 8C). Sempre consultar antes de gerar.

Resumo:
- **8A — Identidade Visual Completa** (12 seções): Hero → Sobre a Marca → Posicionamento → Público → Personalidade → Logo → Paleta → Tipografia → Elementos Visuais → Voz & Tom → Aplicações → Guia Rápido
- **8B — Logo** (9 seções): Hero → Conceito → Versões do Logo → Paleta → Tipografia → Sobre Fundos → Área de Respiro → Usos Corretos & Incorretos → Aplicações
- **8C — Design para Redes Sociais** (8 seções): Hero → Estratégia → Galeria das Peças → Detalhamento por Peça → Tipografia → Paleta → Especificações Técnicas → Como Usar
- **8D — Calendário Editorial** (6 seções): Hero → Estratégia do Mês → Pilares de Conteúdo → Calendário Visual → Detalhamento dos Posts → O que esperar

Se falta material em alguma seção, adaptar com o que existe — **nunca omitir a seção**.

## Hosting

- Projeto Vercel: `lab360-entregas.vercel.app`
- **Sem `vercel.json`** — Vercel detecta site estático automaticamente
- Git push → deploy automático em ~30s

## Notion

Usado apenas para documentos de texto puro (conteúdo de posts, revisões, etc.). Claude cria a página diretamente no Notion via MCP a partir do arquivo em `Texto/`.

## Git

Claude gerencia tudo. Heleno não toca em git.

Commits sempre com:
```
git commit -m "Gerar [tipo de serviço]: [Cliente] — [Projeto]"
```

Exemplos:
- `Gerar identidade visual: Studio Alma — Branding 2025`
- `Gerar logo: Café Raiz — Logo 2025`
- `Gerar redes sociais: Maria Souza — Pack Janeiro`
- `Gerar calendário editorial: Studio Alma — Junho 2025`

## Comunicação

- Sempre português brasileiro
- Ao terminar, entregar a URL pronta para Heleno enviar ao cliente
- Tom direto, sem rodeios

## Contato

WhatsApp do Heleno (aprovações): (31) 99686-2968
