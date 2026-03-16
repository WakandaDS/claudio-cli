# Tokens e Variáveis
#figma-cli #guia #tokens #variáveis

> [!NOTE]
> O figma-cli usa as variáveis nativas do Figma (não CSS variables). Cada token é uma `Variable` numa `VariableCollection` com modos (ex: Light/Dark).

---

## Estrutura de tokens recomendada

Quatro camadas hierárquicas:

| Camada | Exemplo | Função |
|--------|---------|--------|
| `primitives` | `primitives.neutral1000` → `#F2F2F2` | Valores brutos, flat, sem semântica |
| `base` | `base.primaryHue.color` | Decisões de tema — o "o que" |
| `semantic` | `semantic.negative.color` | Intenção — o "porquê" |
| `advanced` | `advanced.buttonPrimary.text.color` | Tokens por componente |

**Naming:** camelCase em todos os níveis. Leaf properties: `color`, `background`, `text`, `icon`, `radius`, `shadow`, `display`.

---

## Criar tokens

```bash
# Sistema shadcn completo (recomendado)
node src/index.js tokens preset shadcn
# → 244 primitivas + 32 semânticos + colecção com modos Light/Dark

# Só paleta Tailwind
node src/index.js tokens tailwind
# → 242 cores primitivas

# Sistema IDS base
node src/index.js tokens ds
```

> [!IMPORTANT]
> `var list` **mostra** variáveis existentes. Os comandos `tokens` **criam-nas**. São coisas diferentes.

---

## Listar e visualizar

```bash
node src/index.js var list                     # Todas as variáveis
node src/index.js var visualize                # Swatches no canvas (todas as colecções)
node src/index.js var visualize "primitives"   # Filtrar por colecção
```

---

## Vincular variáveis — sintaxe `var:`

A forma mais rápida de vincular tokens ao criar elementos:

```bash
# Em create commands
node src/index.js create rect "Card" --fill "var:card" --stroke "var:border"

# Em JSX render
node src/index.js render '<Frame bg="var:card" stroke="var:border">
  <Text color="var:foreground">Texto</Text>
</Frame>'

# Em set commands (elementos já existentes)
node src/index.js set fill "var:primary"
node src/index.js set stroke "var:border"
```

**Variáveis shadcn disponíveis:**

| Token | Uso |
|-------|-----|
| `background`, `foreground` | Fundo de página e texto base |
| `card`, `card-foreground` | Fundos de card |
| `primary`, `primary-foreground` | Botões, destaques |
| `secondary`, `secondary-foreground` | Elementos secundários |
| `muted`, `muted-foreground` | Texto subtil, fundos neutros |
| `accent`, `accent-foreground` | Hover states |
| `border`, `input`, `ring` | Bordas e focos |

---

## Eliminar variáveis

```bash
node src/index.js var delete-all                      # Todas as colecções
node src/index.js var delete-all -c "primitives"      # Só uma colecção específica
```

> [!WARNING]
> Esta operação não é reversível. Componentes que usem as variáveis perdem os bindings.

---

## Mudar modo (Light/Dark) via eval

Quando as variáveis vêm de uma library externa, não são acessíveis via `getLocalVariableCollections()`. A solução é encontrar a colecção através dos `boundVariables` do próprio nó:

```javascript
const node = figma.getNodeById('1:92');

function findModeCollection(n) {
  if (n.boundVariables) {
    for (const [prop, binding] of Object.entries(n.boundVariables)) {
      const b = Array.isArray(binding) ? binding[0] : binding;
      if (b?.id) {
        try {
          const variable = figma.variables.getVariableById(b.id);
          if (variable) {
            const col = figma.variables.getVariableCollectionById(variable.variableCollectionId);
            if (col?.modes.length > 1) return { col, modes: col.modes };
          }
        } catch(e) {}
      }
    }
  }
  if (n.children) {
    for (const c of n.children) {
      const found = findModeCollection(c);
      if (found) return found;
    }
  }
  return null;
}

const found = findModeCollection(node);
if (found) {
  const mode = found.modes.find(m => m.name.includes('Light')); // ou 'Dark'
  ['1:92', '1:112', '1:134'].forEach(id => {
    const n = figma.getNodeById(id);
    if (n) n.setExplicitVariableModeForCollection(found.col, mode.modeId);
  });
}
```

> [!NOTE]
> Ver [[tecnicas-avancadas]] para mais padrões de manipulação de variáveis.
