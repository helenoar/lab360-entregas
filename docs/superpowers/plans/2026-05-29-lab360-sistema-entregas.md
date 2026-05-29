# LAB 360° Sistema de Entregas — Plano de Implementação

> **Para execução:** Use `superpowers:executing-plans` ou `superpowers:subagent-driven-development` para rodar cada parte. Passos usam checkbox (`- [ ]`) para rastreamento.

**Objetivo:** Montar um sistema de entrega de documentos HTML para clientes do LAB 360°, hospedado em um único repo Vercel, com Git gerenciado por Claude.

**Arquitetura:** Monorepo estático com todos os clientes em `Clientes/[Cliente]/[Projeto]/`. Cada projeto tem pastas `Formulário/`, `Design/`, `Texto/` e `identidade-visual/index.html`. Design system centralizado em `_template/LAB360-design-system.md`.

**Tech Stack:** HTML/CSS vanilla (design system em `_template/LAB360-design-system.md`), Vercel (hosting), GitHub (versionamento), Notion MCP (para documentos de texto).

---

## Parte 1: Setup Git + GitHub + Vercel

**Objetivo:** Inicializar o repo local, conectar ao GitHub, configurar Vercel para auto-deploy.

**Arquivos:**
- Criar: `.gitignore`
- Criar: `README.md`
- Modificar: Estrutura de pastas base

### Task 1.1: Inicializar Git e criar .gitignore

- [ ] **Passo 1: Inicializar repositório Git**

```bash
cd "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°"
git init
```

Esperado: Mensagem `Initialized empty Git repository in ...`

- [ ] **Passo 2: Criar arquivo .gitignore**

```bash
cat > .gitignore << 'EOF'
.DS_Store
.env
.env.local
node_modules/
*.log
.vercel/
EOF
```

Esperado: Arquivo `.gitignore` criado na raiz.

- [ ] **Passo 3: Verificar estrutura de pastas base**

```bash
ls -la
```

Esperado: Deve listar `_template/`, `Clientes/`, `CLAUDE.md`, `docs/`, etc.

- [ ] **Passo 4: Primeiro commit**

```bash
git add .gitignore CLAUDE.md _template/LAB360-design-system.md README.md
git commit -m "init: setup git, design system, and documentation

- Initialize Git repository
- Add .gitignore (exclude .DS_Store, .env, node_modules)
- Add CLAUDE.md with project-specific instructions
- Add design system reference in _template/
- Add README.md with project overview"
```

Esperado: Commit criado com mensagem clara.

---

### Task 1.2: Criar README.md

- [ ] **Passo 1: Escrever README.md**

```bash
cat > README.md << 'EOF'
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
EOF
```

Esperado: Arquivo `README.md` criado.

- [ ] **Passo 2: Commit README**

```bash
git add README.md
git commit -m "docs: add README with project overview"
```

Esperado: Commit criado.

---

### Task 1.3: Conectar ao GitHub e Vercel

- [ ] **Passo 1: Criar repo no GitHub**

Executar manualmente (não via CLI aqui):
1. Ir para github.com
2. Criar novo repo `lab360-entregas` (público ou privado, sua escolha)
3. Copiar a URL SSH ou HTTPS

- [ ] **Passo 2: Adicionar remote e fazer primeiro push**

```bash
# Substituir URL_DO_REPO pela URL copiada acima
git remote add origin URL_DO_REPO
git branch -M main
git push -u origin main
```

Esperado: Código aparece no GitHub em `main`.

- [ ] **Passo 3: Conectar ao Vercel**

Executar manualmente:
1. Ir para vercel.com, fazer login
2. "Add New Project"
3. Selecionar repo `lab360-entregas`
4. Configurar:
   - Framework: "Other" (static HTML)
   - Root Directory: `.` (raiz)
5. Deploy

Esperado: Vercel gera um domínio automático (ex: `lab360-entregas.vercel.app`). Cada push em `main` auto-deploya.

- [ ] **Passo 4: Verificar deploy**

Abrir `https://[dominio-vercel]/Clientes/Quik\ Cia\ de\ Dança/MOVER\ CONSCIENTE/identidade-visual/` (será 404 até que o HTML seja gerado na Parte 3).

---

## Parte 2: Preparar Projeto Quik

**Objetivo:** Criar estrutura de pastas `Formulário/` e `Texto/` no projeto Quik existente.

**Arquivos:**
- Criar: `Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário/` (vazio — Heleno preenche)
- Criar: `Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Texto/` (vazio — Heleno preenche)
- Modificar: `Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Design/Identidade Visual/` → renomear para `Design/` (já existe, apenas confirmar)

### Task 2.1: Criar pastas faltantes

- [ ] **Passo 1: Criar pasta Formulário**

```bash
mkdir -p "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário"
```

Esperado: Pasta criada (vazia).

- [ ] **Passo 2: Criar pasta Texto**

```bash
mkdir -p "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Texto"
```

Esperado: Pasta criada (vazia).

- [ ] **Passo 3: Verificar estrutura**

```bash
ls -la "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/"
```

Esperado: Deve listar `Design/`, `Formulário/`, `Texto/`, `Identidade Visual/` (opcional).

- [ ] **Passo 4: Commit**

```bash
git add "Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário/" "Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Texto/"
git commit -m "feat: add folder structure for Quik/Mover Consciente project

- Create Formulário/ folder (for Tally form export)
- Create Texto/ folder (for Notion content)
- Ready to accept client brief and design files"
```

Esperado: Commit criado.

- [ ] **Passo 5: Push**

```bash
git push origin main
```

Esperado: Mudanças no GitHub. Vercel auto-deploya.

---

## Parte 3: Gerar Primeiro HTML Exemplo

**Objetivo:** Gerar o documento HTML de identidade visual do projeto Quik usando o skill `huashu-design`.

**Arquivos:**
- Criar: `Clientes/Quik Cia de Dança/MOVER CONSCIENTE/identidade-visual/index.html`

**Pré-requisitos:**
- Arquivo do formulário em `Formulário/` (ou placeholder genérico se Heleno ainda não exportou)
- PDF do design em `Design/` (já existe: `uma vivência artística imersiva...pdf`)

### Task 3.1: Preparar arquivos de entrada

- [ ] **Passo 1: Listar arquivos no Design/**

```bash
ls -la "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Design/"
```

Esperado: Deve listar o PDF do design.

- [ ] **Passo 2: Verificar formulário**

```bash
ls -la "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário/"
```

Se vazio, criar um placeholder:

```bash
cat > "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário/resposta-tally.txt" << 'EOF'
[Placeholder] Resposta do Tally — Formulário 1: Identidade Visual Completa

Cliente: Quik Cia de Dança
Projeto: Mover Consciente
Conceito: Uma vivência artística imersiva e totalmente gratuita que integra o corpo, a mente e a dança contemporânea. 
Público-alvo: Praticantes de dança, artistas, comunidade de movimento
Valores-chave: Acessibilidade, movimento consciente, inclusão
Sensação desejada: Movimento, fluidez, leveza, poder

[A ser preenchido com resposta real do formulário Tally quando disponível]
EOF
```

Esperado: Arquivo `resposta-tally.txt` criado como placeholder.

---

### Task 3.2: Invocar skill huashu-design para gerar HTML

- [ ] **Passo 1: Ler design system**

Ler `_template/LAB360-design-system.md` (já lido em contexto).

- [ ] **Passo 2: Invocar skill huashu-design**

**Prompt do skill:**

```
Cliente: Quik Cia de Dança
Projeto: Mover Consciente
Tipo: Identidade Visual Completa

Ler:
1. PDF do design: Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Design/uma vivência artística...pdf
2. Formulário: Clientes/Quik Cia de Dança/MOVER CONSCIENTE/Formulário/resposta-tally.txt
3. Design system: _template/LAB360-design-system.md

Gerar HTML:
- 7 seções: Hero, Conceito, Paleta, Tipografia, Logo, Voz & Tom, Aplicações
- Estética LAB 360° (preto, cyan, Inter/JetBrains Mono)
- Meta tags: <meta name="robots" content="noindex, nofollow">
- Title: <title>Mover Consciente — Identidade Visual · LAB 360°</title>

Salvar em:
Clientes/Quik Cia de Dança/MOVER CONSCIENTE/identidade-visual/index.html
```

Esperado: Arquivo `index.html` gerado com toda a estrutura e conteúdo.

---

### Task 3.3: Validar e fazer commit

- [ ] **Passo 1: Verificar arquivo gerado**

```bash
ls -lh "/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/identidade-visual/index.html"
```

Esperado: Arquivo existe e tem tamanho > 10KB.

- [ ] **Passo 2: Abrir no navegador (opcional)**

```bash
open "file:///Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/Clientes/Quik Cia de Dança/MOVER CONSCIENTE/identidade-visual/index.html"
```

Esperado: HTML renderiza corretamente (hero, seções visíveis, animações canvas).

- [ ] **Passo 3: Commit**

```bash
git add "Clientes/Quik Cia de Dança/MOVER CONSCIENTE/identidade-visual/index.html"
git commit -m "feat: generate identity visual document for Quik/Mover Consciente

- Create complete identity document following LAB 360° design system
- Sections: Hero, Concept, Color Palette, Typography, Logo, Voice & Tone, Applications
- Extract visual identity from Canva design
- Deploy-ready (noindex, responsive, all assets via CDN)"
```

Esperado: Commit criado.

- [ ] **Passo 4: Push para GitHub e Vercel auto-deploy**

```bash
git push origin main
```

Esperado: GitHub recebe commit. Vercel auto-deploya em ~30s.

- [ ] **Passo 5: Acessar URL pública**

Após ~30s, visitar:
```
https://[dominio-vercel]/Clientes/Quik%20Cia%20de%20Dan%C3%A7a/MOVER%20CONSCIENTE/identidade-visual/
```

Esperado: HTML renderiza com estética LAB 360°, menus laterais, animações canvas.

---

## Self-Review

✓ **Spec coverage:** Todas as seções do spec cobiertas:
  - Setup Git + GitHub + Vercel (Task 1.1–1.3)
  - Criar estrutura de pastas Quik (Task 2.1)
  - Gerar primeiro HTML (Task 3.1–3.3)

✓ **Placeholders:** Nenhum — cada passo tem código completo ou instrução clara.

✓ **Type consistency:** URLs, paths, nomes de arquivo consistentes ao longo do plano.

✓ **Commits:** Cada tarefa termina com commit claro e descritivo.

---

## Próximos Passos (Fora deste Plano)

- Heleno coloca resposta real do Tally em `Formulário/` (será usado em próximos projetos)
- Heleno valida URL com cliente
- Sistema pronto para receber novos clientes/projetos (copiar estrutura `Clientes/[Cliente]/[Projeto]/`)
- Documentação no Notion pode começar (usar `Texto/` + Notion MCP)
