# Comando: novo-cliente

Cria a estrutura de pastas para um novo cliente no LAB 360° e adiciona o card no index.html.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — Nome do Projeto`

Exemplos:
- `Studio Alma — Identidade Visual`
- `Café Raiz — Branding 2025`
- `Maria Souza — Lançamento de Livro`

## O que fazer

### 1. Derivar slugs URL-safe

A partir de `$ARGUMENTS`, extrair:
- **clienteNome**: parte antes do `—` (ex: `Studio Alma`)
- **projetoNome**: parte depois do `—` (ex: `Identidade Visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem caracteres especiais (ex: `studioalma`)
- **projetoSlug**: idem (ex: `identidadevisual`)

Regras de slug: remover acentos (á→a, ç→c, etc.), converter para minúsculas, remover espaços e hífens.

### 2. Criar estrutura de pastas

Criar as seguintes pastas no repositório (raiz: `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/`):

```
clientes/[clienteSlug]/[projetoSlug]/Formulario/
clientes/[clienteSlug]/[projetoSlug]/Design/
clientes/[clienteSlug]/[projetoSlug]/Texto/
clientes/[clienteSlug]/[projetoSlug]/identidade-visual/
```

Criar um arquivo `.gitkeep` em cada pasta para que o git rastreie os diretórios vazios.

### 3. Adicionar card no index.html

Abrir `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/index.html` e adicionar um novo `<a class="project-card">` dentro de `.projects-grid`, logo após o último card existente.

Modelo do card:
```html
<a class="project-card" href="./clientes/[clienteSlug]/[projetoSlug]/identidade-visual/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[projetoNome]</span>
  <span class="card-action">Ver Identidade Visual</span>
</a>
```

### 4. Git commit

```
git add clientes/[clienteSlug]/ index.html
git commit -m "chore: criar estrutura para [clienteNome] — [projetoNome]"
```

### 5. Confirmar para o Heleno

Mostrar:
- Pastas criadas
- URL futura: `https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[projetoSlug]/identidade-visual/`
- Próximo passo: colocar o PDF do formulário em `Formulario/` e o PDF do design em `Design/`, depois pedir para gerar a identidade visual
