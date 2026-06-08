# Comando: novo-cliente

Cria a estrutura de pastas para um novo cliente/serviço e adiciona o card no index.html.
Use quando quiser preparar a estrutura ANTES de ter os arquivos prontos.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `calendario-editorial`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Studio Alma — calendario-editorial`

Se o tipo não for informado, perguntar antes de prosseguir.

## O que fazer

### 1. Derivar slug e tipo

- **clienteNome**: antes do `—`
- **tipoServico**: depois do `—`
- **clienteSlug**: minúsculas, sem acentos, sem espaços, sem hífens

Mapa de label por serviço:
| tipoServico | labelCard | nomeLegivelCard |
|-------------|-----------|-----------------|
| `identidade-visual` | `Ver Identidade Visual` | `Identidade Visual Completa` |
| `logo` | `Ver Logo` | `Logo` |
| `redes-sociais` | `Ver Redes Sociais` | `Design para Redes Sociais` |
| `calendario-editorial` | `Ver Calendário` | `Calendário Editorial` |

### 2. Criar estrutura de pastas

**Para identidade-visual, logo, redes-sociais:**
```bash
mkdir -p "[pastaRaiz][clienteSlug]/Formulario"
mkdir -p "[pastaRaiz][clienteSlug]/Design"
mkdir -p "[pastaRaiz][clienteSlug]/Texto"
touch "[pastaRaiz][clienteSlug]/Formulario/.gitkeep"
touch "[pastaRaiz][clienteSlug]/Design/.gitkeep"
touch "[pastaRaiz][clienteSlug]/Texto/.gitkeep"
```

Mapa de pastaRaiz:
- `identidade-visual` → `identidade-visual/`
- `logo` → `logo/`
- `redes-sociais` → `redes-sociais/`

**Para calendario-editorial:**
```bash
mkdir -p "gestao-de-redes-sociais/[clienteSlug]"
touch "gestao-de-redes-sociais/[clienteSlug]/.gitkeep"
```
(não cria subpasta de mês — isso é feito no /novo-pedido quando o arquivo chegar)

Se a pasta raiz do cliente já existir (cliente com outro serviço anterior), apenas adicionar a nova subpasta.

### 3. Adicionar card no index.html

**Para identidade-visual, logo, redes-sociais:**
```html
<a class="project-card" href="./[pastaRaiz][clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[nomeLegivelCard]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

**Para calendario-editorial:**
```html
<a class="project-card" href="./gestao-de-redes-sociais/[clienteSlug]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">Calendário Editorial</span>
  <span class="card-action">Ver Calendário</span>
</a>
```

### 4. Git commit

```bash
git add "[pastaRaiz][clienteSlug]/" index.html
git commit -m "chore: criar estrutura para [clienteNome] — [tipoServico]"
```

### 5. Confirmar para o Heleno

Mostrar:
- Pastas criadas
- Próximo passo: colocar os arquivos em `_inbox/` e rodar `/novo-pedido [clienteNome] — [tipoServico]`
- URL futura: `https://lab360-entregas.vercel.app/[pastaRaiz][clienteSlug]/`
