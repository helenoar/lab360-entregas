# Comando: novo-pedido

Processa um novo pedido de cliente do LAB 360° de ponta a ponta.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `calendario-editorial`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Maria Souza — redes-sociais`
- `Studio Alma — calendario-editorial`

## O que fazer — passo a passo

### 1. Derivar slug e tipo de serviço

A partir de `$ARGUMENTS`, extrair:
- **clienteNome**: parte antes do `—` (ex: `Studio Alma`)
- **tipoServico**: parte depois do `—` (ex: `identidade-visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem hífens (ex: `studioalma`)

Regras de slug: á→a, ã→a, â→a, é→e, ê→e, í→i, ó→o, ô→o, õ→o, ú→u, ç→c, remover espaços e hífens.

Ler o config do serviço em `_servicos/[tipoServico]/config.md` (ou `_servicos/gestao-de-redes-sociais/calendario-editorial/config.md` para calendário editorial).

Mapa de configurações:
| tipoServico | pastaRaiz | labelCard | seçãoDS | tipoInbox |
|-------------|-----------|-----------|---------|-----------|
| `identidade-visual` | `identidade-visual/` | `Ver Identidade Visual` | 8A | form + design PDF |
| `logo` | `logo/` | `Ver Logo` | 8B | form + design PDF |
| `redes-sociais` | `redes-sociais/` | `Ver Redes Sociais` | 8C | form + design PDF |
| `calendario-editorial` | `gestao-de-redes-sociais/` | `Ver Calendário` | 8D | calendario .md |

### 2. Ler a `_inbox/`

Raiz do projeto: `/Users/helenocarneiro/CLAUDECODE/LAB 360°/ENTREGAS_LAB 360°/`

**Para identidade-visual, logo, redes-sociais:**

Identificar em `_inbox/`:
- **arquivoFormulario**: arquivo com prefixo `form-`
- **arquivoDesign**: arquivo com prefixo `design-`
- **arquivoTexto**: arquivo com prefixo `texto-` (opcional)

Erros a verificar:
- Sem `form-*`: parar — "Não encontrei o formulário na `_inbox/`. Adicione `form-[slug].pdf`."
- Sem `design-*`: parar — "Não encontrei o design na `_inbox/`. Adicione `design-[slug].pdf`."
- Dois ou mais `form-*` ou `design-*`: listar e perguntar qual usar.

**Para calendario-editorial:**

Identificar em `_inbox/`:
- **arquivoEstrategia**: arquivo no padrão `calendario-[slug]-*.md`
  - Ex: `calendario-studioalma-jun-2025.md`
- **mesAno**: extrair do nome do arquivo — tudo após o segundo `-` até `.md`
  - Ex: `calendario-studioalma-jun-2025.md` → `jun-2025` → normalizar para `junho-2025`

Normalização do mês abreviado:
jan→janeiro, fev→fevereiro, mar→março, abr→abril, mai→maio, jun→junho,
jul→julho, ago→agosto, set→setembro, out→outubro, nov→novembro, dez→dezembro

Erro a verificar:
- Sem `calendario-*`: parar — "Não encontrei o arquivo de estratégia na `_inbox/`. Adicione `calendario-[slug]-[mes-ano].md`."

### 3. Criar estrutura de pastas

**Para identidade-visual, logo, redes-sociais:**
```bash
mkdir -p "[pastaRaiz][clienteSlug]/Formulario"
mkdir -p "[pastaRaiz][clienteSlug]/Design"
mkdir -p "[pastaRaiz][clienteSlug]/Texto"
```

**Para calendario-editorial:**
```bash
mkdir -p "gestao-de-redes-sociais/[clienteSlug]/[mesAno]"
```

Verificar conflito: se a pasta já existir, avisar e perguntar se deve sobrescrever.

### 4. Mover arquivos da `_inbox/`

**Para identidade-visual, logo, redes-sociais:**
```bash
mv "_inbox/[arquivoFormulario]" "[pastaRaiz][clienteSlug]/Formulario/"
mv "_inbox/[arquivoDesign]"     "[pastaRaiz][clienteSlug]/Design/"
```
Se existir texto:
```bash
mv "_inbox/[arquivoTexto]" "[pastaRaiz][clienteSlug]/Texto/"
```

**Para calendario-editorial:**
```bash
cp "_inbox/[arquivoEstrategia]" "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/estrategia.md"
rm "_inbox/[arquivoEstrategia]"
```

### 5. Ler os arquivos de conteúdo

**Para identidade-visual, logo, redes-sociais:**
- Ler o PDF de Formulario/
- Ler o PDF de Design/
- Ler `_template/LAB360-design-system.md` — seção conforme tipoServico (8A, 8B ou 8C)

**Para calendario-editorial:**
- Ler `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/estrategia.md`
- Ler `_template/LAB360-design-system.md` — seção 8D
- Ler `_servicos/gestao-de-redes-sociais/calendario-editorial/knowledge-base.md`

### 6. Adicionar card no index.html

Abrir `index.html` na raiz e adicionar novo `<a class="project-card">` dentro de `.projects-grid`:

**Para identidade-visual, logo, redes-sociais:**
```html
<a class="project-card" href="./[pastaRaiz][clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[nomeServicoLegível]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

**Para calendario-editorial:**
```html
<a class="project-card" href="./gestao-de-redes-sociais/[clienteSlug]/[mesAno]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">Calendário Editorial — [mesAno legível]</span>
  <span class="card-action">Ver Calendário</span>
</a>
```

Nomes legíveis por tipo:
- `identidade-visual` → `Identidade Visual Completa`
- `logo` → `Logo`
- `redes-sociais` → `Design para Redes Sociais`

### 7. Gerar o HTML

Invocar o skill `huashu-design` para gerar o HTML de entrega.

Briefing base:
- Estrutura LAB360°: fundo preto, canvas rizoma, nav dots, status bar, cursor cyan
- Seção do design system: conforme tipoServico

**Adicional para calendario-editorial:**
- Seção 8D — implementar grade mensal interativa com cards por dia
- Cor de cada card = cor do pilar definida em PILARES
- Cards clicáveis: expande detalhe com legenda completa (modal ou scroll para seção 5)
- Seção 4 (Calendário) é a peça central — dar espaço visual generoso

Salvar em:
- identidade-visual/logo/redes-sociais: `[pastaRaiz][clienteSlug]/index.html`
- calendario-editorial: `gestao-de-redes-sociais/[clienteSlug]/[mesAno]/index.html`

### 8. Pausar para revisão

Mostrar ao Heleno:
- Seções geradas
- Para calendario-editorial: número de posts no calendário, pilares detectados com cores
- Caminho do arquivo gerado

Aguardar aprovação. Não commitar ainda.

### 9. Após aprovação: commit + push

**Para identidade-visual, logo, redes-sociais:**
```bash
git add "[pastaRaiz][clienteSlug]/" index.html
git commit -m "Gerar [tipoServico]: [clienteNome]"
git push
```

**Para calendario-editorial:**
```bash
git add "gestao-de-redes-sociais/[clienteSlug]/[mesAno]/" index.html
git commit -m "Gerar calendário editorial: [clienteNome] — [mesAno legível]"
git push
```

Vercel deploya em ~30 segundos.

### 10. Criar página no Notion (somente se Texto/ tiver arquivo)

Aplicável apenas para identidade-visual, logo, redes-sociais.
Ler o arquivo em Texto/, criar página no Notion via MCP.

### 11. Entregar as URLs ao Heleno

**Para identidade-visual, logo, redes-sociais:**
```
✓ Entrega pronta para [clienteNome]

URL: https://lab360-entregas.vercel.app/[pastaRaiz][clienteSlug]/
```

**Para calendario-editorial:**
```
✓ Calendário pronto para [clienteNome]

URL: https://lab360-entregas.vercel.app/gestao-de-redes-sociais/[clienteSlug]/[mesAno]/
```
