# Comando: novo-cliente

Cria a estrutura de pastas para um novo cliente no LAB 360° e adiciona o card no index.html.

## Entrada esperada

`$ARGUMENTS` deve ser no formato: `Nome do Cliente — tipo-de-servico`

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais`

Exemplos:
- `Studio Alma — identidade-visual`
- `Café Raiz — logo`
- `Maria Souza — redes-sociais`

Se o tipo não for informado, perguntar ao Heleno antes de prosseguir.

## O que fazer

### 1. Derivar slug e tipo de serviço

A partir de `$ARGUMENTS`, extrair:
- **clienteNome**: parte antes do `—` (ex: `Studio Alma`)
- **tipoServico**: parte depois do `—` (ex: `identidade-visual`)
- **clienteSlug**: minúsculas, sem espaços, sem acentos, sem caracteres especiais (ex: `studioalma`)

Regras de slug: remover acentos (á→a, ç→c, etc.), converter para minúsculas, remover espaços e hífens.

Mapa de label por serviço:
| tipoServico | labelCard |
|-------------|-----------|
| `identidade-visual` | `Ver Identidade Visual` |
| `logo` | `Ver Logo` |
| `redes-sociais` | `Ver Redes Sociais` |

### 2. Criar estrutura de pastas

Criar as seguintes pastas no repositório (raiz: `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/`):

```
clientes/[clienteSlug]/[tipoServico]/Formulario/
clientes/[clienteSlug]/[tipoServico]/Design/
clientes/[clienteSlug]/[tipoServico]/Texto/
```

Criar um arquivo `.gitkeep` em cada subpasta para que o git rastreie os diretórios vazios.
Não criar `.gitkeep` na pasta `[tipoServico]/` em si — o `index.html` vai existir ali depois.

Se `clientes/[clienteSlug]/` já existir (cliente com outro serviço anterior), apenas adicionar a nova subpasta de serviço.

### 3. Adicionar card no index.html

Abrir `/Users/helenocarneiro/CLAUDECODE/SISTEMA DE ENTREGAS_LAB 360°/index.html` e adicionar um novo `<a class="project-card">` dentro de `.projects-grid`, logo após o último card existente.

Modelo do card:
```html
<a class="project-card" href="./clientes/[clienteSlug]/[tipoServico]/">
  <span class="client-name">[clienteNome]</span>
  <span class="project-name">[tipoServico em texto legível]</span>
  <span class="card-action">[labelCard]</span>
</a>
```

Texto legível do serviço:
- `identidade-visual` → `Identidade Visual Completa`
- `logo` → `Logo`
- `redes-sociais` → `Design para Redes Sociais`

### 4. Git commit

```
git add clientes/[clienteSlug]/ index.html
git commit -m "chore: criar estrutura para [clienteNome] — [tipoServico]"
```

### 5. Confirmar para o Heleno

Mostrar:
- Pastas criadas
- Tipo de serviço: `[tipoServico]`
- URL futura: `https://lab360-entregas.vercel.app/clientes/[clienteSlug]/[tipoServico]/`
- Próximo passo: colocar o PDF do formulário em `Formulario/` e o PDF do design em `Design/`, depois pedir para gerar a entrega
