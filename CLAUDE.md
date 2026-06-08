# LAB 360° — Instruções de Projeto

## 🔴 CRÍTICO: Design System LAB360° ≠ Identidade Visual do Cliente

### Design System LAB360° = ESTRUTURA do documento
- Arquivo: `_template/LAB360-design-system.md`
- É SEMPRE IGUAL para todos os clientes
- Define: moldura (fundo preto, nav dots, status bar, canvas rizoma)
- É a MOLDURA/APRESENTAÇÃO do documento

### Identidade Visual do Cliente = CONTEÚDO do documento
- Fonte: PDF do Canva em `clientes/[clienteSlug]/[nomeProjeto]/[servico]/Design/`
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

Cliente → Projeto → Serviço → Arquivos. Um cliente pode ter múltiplos projetos; cada projeto tem vários serviços.

```
clientes/
  [clienteSlug]/
    [nomeProjeto]/
      [tipo-servico]/      ← identidade-visual | logo | redes-sociais
        Formulario/        ← brief do cliente (PDF ou .txt)
        Design/            ← PDF do Canva com o design
        Texto/             ← .txt/.md para Notion (opcional)
        index.html         ← Claude gera e commita aqui
      gestao-de-redes-sociais/
        [mes-ano]/         ← ex: junho-2025
          estrategia.md    ← gerado pelo Claude a partir do formulário
          index.html       ← Claude gera e commita aqui
```

Serviços disponíveis e suas URLs:
| Serviço | Pasta | URL base |
|---------|-------|----------|
| Identidade Visual | `clientes/[slug]/[projeto]/identidade-visual/` | `lab360-entregas.vercel.app/clientes/[slug]/[projeto]/identidade-visual/` |
| Logo | `clientes/[slug]/[projeto]/logo/` | `lab360-entregas.vercel.app/clientes/[slug]/[projeto]/logo/` |
| Design Redes Sociais | `clientes/[slug]/[projeto]/redes-sociais/` | `lab360-entregas.vercel.app/clientes/[slug]/[projeto]/redes-sociais/` |
| Calendário Editorial | `clientes/[slug]/[projeto]/gestao-de-redes-sociais/[mes-ano]/` | `lab360-entregas.vercel.app/clientes/[slug]/[projeto]/gestao-de-redes-sociais/[mes-ano]/` |

### Regra de nomenclatura de pastas (OBRIGATÓRIO)

**Slugs de clientes:** minúsculas, sem espaços, sem acentos, sem hífens.

| Nome real | Slug correto |
|-----------|-------------|
| Quik Cia de Dança | `quikciadadanca` |
| Studio Alma | `studioalma` |
| Café Raiz | `caferaiz` |

**Nomes de projetos:** minúsculas, hífens separando palavras (sem espaços/acentos).

| Nome real | Slug correto |
|-----------|-------------|
| Mover Consciente | `mover-consciente` |
| Branding 2025 | `branding-2025` |
| Pack Janeiro | `pack-janeiro` |

**Mês/ano:** `junho-2025`, `julho-2025`, etc. (por extenso, com hífen).

## Criar estrutura manualmente (sem inbox)

Use `/novo-cliente` quando quiser criar a estrutura de pastas SEM ter os arquivos ainda — por exemplo, para já deixar a pasta pronta antes do cliente enviar os materiais.

```
/novo-cliente Nome do Cliente — Nome do Projeto — tipo-de-servico
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais`

Cria as subpastas em `clientes/[clienteSlug]/[nomeProjeto]/[tipoServico]/`, adiciona card no `index.html` e faz commit. Se o cliente ou projeto já existe, apenas adiciona o novo serviço dentro da pasta apropriada.

> Para o fluxo completo (com arquivos prontos), use `/novo-pedido` — ver seção abaixo.

## Fluxo Principal — Novo Pedido (use este)

Quando Heleno traz um novo cliente/pedido, o fluxo é:

**Heleno cria no Finder e joga os arquivos:**
```
_inbox/
  [Nome do Cliente]/
    [Nome do Projeto]/
      [tipo-servico]/
        arquivo1.pdf
        arquivo2.pdf
```

Tipos de subpasta aceitos: `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

Arquivos esperados por tipo:
- `identidade-visual`, `logo`, `redes-sociais`: formulário do Tally (PDF) + design do Canva (PDF)
- `gestao-de-redes-sociais`: apenas o formulário do Tally (PDF)

**Depois Heleno digita:** `/novo-pedido`

**Claude faz tudo:**
1. Escaneia `_inbox/` automaticamente (detecta cliente, projeto e tipo pela estrutura de pastas)
2. Cria `clientes/[slug]/[projeto]/[tipo]/` — se o cliente/projeto já existe, apenas adiciona o serviço
3. Move os arquivos do inbox para dentro
4. Lê o especialista do tipo (`_especialistas/[tipo].md`)
5. Gera HTML de entrega
6. Commit + push imediato
7. Vercel deploya em ~30s
8. Entrega URL ao Heleno
9. Apaga `_inbox/[Nome do Cliente]/[Nome do Projeto]/`

**Tipos aceitos:** `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

## Workflow de Geração de HTML (fluxo manual/interno)

> Este fluxo é executado automaticamente pelo `/novo-pedido`. Use manualmente só se precisar regerar um HTML sem usar o pipeline completo.

1. Ler `Formulario/` — brief do cliente em voz própria
2. Ler `Design/` — PDF do design visual
3. Ler `_template/LAB360-design-system.md` — seção 8 para identificar as seções conforme o serviço
4. **Invocar skill `huashu-design`** para gerar o HTML
5. Salvar em `clientes/[clienteSlug]/[nomeProjeto]/[tipoServico]/index.html`
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
