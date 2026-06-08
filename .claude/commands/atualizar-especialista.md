# Comando: atualizar-especialista

Atualiza um arquivo de especialista via pesquisa web + síntese.

## Como usar

```
/atualizar-especialista [tipo]
```

Tipos aceitos: `identidade-visual` | `logo` | `redes-sociais` | `gestao-de-redes-sociais`

Exemplo:
```
/atualizar-especialista redes-sociais
```

## O que fazer

### 1. Preparação

Ler o arquivo atual em `_especialistas/[tipo].md`. Documentar:
- **Data da última atualização** (no header do arquivo)
- **Seções e checklist de conteúdo** (o que deve estar presente)
- **Keywords para pesquisa** — qual é o tópico principal

Mapa de seções por tipo:

| tipo | seções esperadas | keywords pesquisa |
|------|------------------|-------------------|
| **identidade-visual** | O que entrega + Framework brief + 12 seções HTML + Best practices + Alertas + Diferencial premium | "brand identity design 2026", "design system best practices", "brand guidelines" |
| **logo** | O que entrega + Framework brief + 9 seções HTML + Best practices + Alertas + Diferencial premium | "logo design trends 2026", "logo variations 2026", "logo guidelines" |
| **redes-sociais** | O que entrega + Framework brief + 8 seções HTML + Best practices + Alertas + Diferencial premium | "Instagram dimensions 2026", "social media design trends", "safe zones", "Instagram algorithm" |
| **gestao-de-redes-sociais** | O que entrega + Framework brief + 6 seções HTML + Best practices + Alertas + Diferencial premium | "Instagram content calendar 2026", "social media strategy Brazil", "content pillars", "posting frequency" |

### 2. Pesquisar

Para cada keyword, executar:
```bash
# Simulado — em produção usar WebSearch tool
curl -s "https://www.google.com/search?q=<keyword>+2026" | grep -E 'text|href'
```

Ler 3-5 artigos de autoridade:
- **Design-focused**: Design Observer, Brand New, Brands & Branding, AIGA Eye on Design
- **Social media**: Later Blog, Meta Creator Resources, Buffer Social Media Blog, HubSpot Marketing Blog
- **Industry specific**: Dribbble Inspiration, Behance, IDEO, Awwwards

Documentar **dados novos ou contradições** com o arquivo atual:
- Mudanças em especificações técnicas (ex: Instagram dimensions)
- Novas tendências (ex: logo dinâmicos, algoritmo Brasil)
- Mudanças de frequência recomendada
- Novas proibições / armadilhas

### 3. Decidir se atualizar

**Atualizar se:**
- Mudanças técnicas comprovadas (ex: Instagram 3:4 é novo padrão permanente, não passageiro)
- Novo padrão comprovado em 3+ fontes confiáveis
- Dados com data 2024-2026 (não conjecturas de 2023)
- Alertas sobre prática que se tornou prejudicial

**Não atualizar se:**
- Conjetura / opinião sem dados ("acho que vai mudar")
- Mudança passageira (ex: "esta semana, trend de...")
- Conflito com único artigo (múltiplas fontes devem concordar)
- Dados de 2023 ou anteriores

### 4. Atualizar o arquivo

Abrir `_especialistas/[tipo].md` e:

1. **Atualizar header**:
   ```markdown
   > Atualizado via pesquisa web em YYYY-MM-DD. Para atualizar: /atualizar-especialista [tipo]
   ```

2. **Seção "Best practices atuais (2026)"**:
   - Revisar cada bullet point
   - Remover se obsoleto
   - Adicionar novo descoberto com fonte citada (ex: `— fonte: Meta Creator Resources`)
   - Atualizar números se mudaram

3. **Seção "Alertas e armadilhas"**:
   - Remover se o "alerta" se tornou prática padrão
   - Adicionar novo risco descoberto

4. **Seção "O que diferencia"**:
   - Revisar "Entrega premium" e calibrar com tendências atuais

### 5. Commit

```bash
git add "_especialistas/[tipo].md"
git commit -m "docs: atualizar especialista [tipo] — pesquisa web [data]

Alterações:
- [mudança 1]
- [mudança 2]
- [mudança 3]

Fontes: [links]"

git push
```

## Checklist de qualidade

Antes de commitar:

- [ ] Todas as mudanças têm fonte citada ou documento de referência
- [ ] Nenhuma mudança foi baseada em "acho que"
- [ ] Se trocou um número (ex: "2-3 Reels/semana" → "4-6"), há razão documentada
- [ ] Manteve estrutura do arquivo (mesmas seções)
- [ ] Data do header foi atualizada
- [ ] Header diz "Para atualizar: /atualizar-especialista [tipo]"

## Frequência recomendada

| tipo | frequência | motivo |
|------|-----------|--------|
| **identidade-visual** | 6 meses | Best practices mudam com tendências macro |
| **logo** | 6 meses | Tendências surgem, mas lentamente |
| **redes-sociais** | 3 meses | Plataformas mudam dimensions/specs com frequência |
| **gestao-de-redes-sociais** | 1-2 meses | Algoritmo muda semestral; frequência ideal é muito dinâmica |

Calendário ideal:
- Jan/Fev: atualizar gestao-de-redes-sociais (planejamento anual)
- Abr/Mai: atualizar redes-sociais (especificações)
- Jul/Ago: atualizar todos os 4 (check geral)
- Out: atualizar gestao-de-redes-sociais (planejamento Q4)
