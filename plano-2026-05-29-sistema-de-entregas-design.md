# Design — Sistema de Entregas LAB 360°
**Data:** 2026-05-29
**Status:** Aprovado pelo usuário

---

## Contexto

LAB 360° é a empresa de comunicação digital de Heleno Carneiro. Ele cria designs no Canva (e outros programas) para clientes e precisa entregar esses projetos como documentos HTML profissionais, hospedados em um único lugar, sem precisar criar um novo repositório ou projeto Vercel a cada cliente.

---

## Decisões de Arquitetura

| Decisão | Escolha | Motivo |
|---------|---------|--------|
| Hosting | Um único projeto Vercel | Sem novo repo por cliente |
| Autenticação | Nenhuma | URLs com `noindex`, sem dados sensíveis |
| Estrutura de arquivos | Monorepo estático (opção A) | HTMLs autossuficientes, funcionam offline |
| Git | Claude gerencia tudo | Heleno não quer tocar em git |
| Design | `_template/LAB360-design-system.md` | Referência compacta, sem ler HTML original |

---

## Estrutura de Pastas

```
SISTEMA DE ENTREGAS_LAB 360°/          ← raiz do repo
  _template/
    LAB360-design-system.md            ← referência do design system (já criado)
  Clientes/
    [Nome do Cliente]/
      [Nome do Projeto]/
        Formulário/                    ← resposta exportada do Tally (PDF ou .txt)
        Design/                        ← PDF do design feito por Heleno
        Texto/                         ← .txt/.md para criar página no Notion
        identidade-visual/
          index.html                   ← Claude gera e commita aqui
```

A pasta de saída é **sempre** `identidade-visual/` independente do tipo de projeto contratado. Todo trabalho do LAB 360° é tratado como parte de um sistema de identidade — mesmo que o cliente tenha contratado apenas logo, página ou redes sociais.

### Formulários disponíveis (Tally)
- Formulário 1 — Identidade Visual Completa
- Formulário 2 — Logo
- Formulário 3 — Página de Divulgação / Vendas
- Formulário 4 — Design para Redes Sociais

---

## Workflow de Entrega (HTML)

**Trigger:** Heleno pede "cria o documento do projeto X do cliente Y"

**Passo a passo:**
1. Ler `_template/LAB360-design-system.md`
2. Ler arquivo(s) em `[Projeto]/Formulário/` — brief em voz do cliente
3. Ler arquivo(s) em `[Projeto]/Design/` — PDF com design visual
4. Invocar skill `huashu-design` para gerar o HTML
5. Salvar em `[Projeto]/identidade-visual/index.html`
6. `git add` + `git commit` + `git push`
7. Vercel auto-deploya em ~30s
8. Entregar URL ao Heleno

**Conteúdo do HTML — seções fixas para todos os tipos de projeto:**
1. Hero — nome do projeto + eyebrow com tipo de entrega + tagline
2. Conceito — origem do nome, intenção, contexto (vem do formulário)
3. Paleta de Cores — swatches com hex, nome, sensação
4. Tipografia — fontes com exemplos, hierarquia
5. Logo — variações, área de respiro, usos corretos
6. Voz & Tom — como a marca fala, palavras-chave (vem do formulário)
7. Aplicações — exemplos visuais de aplicação da identidade

Se algum elemento não foi definido no projeto (ex: projeto era só logo, sem tipografia escolhida), a seção aparece com o que existe ou com orientações gerais extraídas do formulário. Nunca omitir a seção — adaptar o conteúdo disponível.

**Meta tags obrigatórias em todo HTML:**
```html
<meta name="robots" content="noindex, nofollow">
<title>[Projeto] — [Tipo] · LAB 360°</title>
```

---

## Workflow de Entrega (Notion)

**Trigger:** Heleno pede "cria a página no Notion do projeto X"

**Passo a passo:**
1. Ler arquivo(s) em `[Projeto]/Texto/`
2. Criar página no Notion via MCP com o conteúdo formatado
3. Entregar link da página ao Heleno

**Uso:** Documentos de texto puro sem referência visual — ex: conteúdo de posts de Instagram para o cliente revisar e comentar.

---

## Setup Inicial (a ser executado na Parte 1 do plano)

- [ ] `git init` na raiz do projeto
- [ ] Criar `.gitignore` (excluir `.DS_Store`, etc.)
- [ ] Primeiro commit com estrutura base + design system
- [ ] Criar repo no GitHub
- [ ] Push inicial
- [ ] Conectar ao Vercel (auto-deploy a cada push em `main`)
- [ ] Criar pastas `Formulário/` e `Texto/` no projeto Quik/Mover Consciente existente

---

## Escopo Fora deste Sistema

- Não há autenticação ou painel de administração
- Não há geração automática (sempre disparado manualmente pelo Heleno)
- Não há versionamento de entregas (git history serve esse propósito)
- Não há preview antes do deploy (Vercel deploya direto de `main`)
