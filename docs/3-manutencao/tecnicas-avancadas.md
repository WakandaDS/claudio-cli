# Técnicas Avançadas
#figma-cli #manutenção #técnicas #eval

> [!NOTE]
> Padrões de `eval` para operações que não têm comando directo na CLI. Todos estes snippets correm via `node src/index.js eval "..."`.

---

## Variable Mode Switching (variáveis de library)

### O problema
`figma.variables.getLocalVariableCollections()` só devolve colecções locais. Variáveis de libraries externas não são directamente acessíveis.

### A solução
Encontrar a colecção através dos `boundVariables` de um node que já usa essa variável:

```javascript
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

// Aplicar modo a múltiplos nodes
const node = figma.getNodeById('1:92');
const found = findModeCollection(node);
if (found) {
  const mode = found.modes.find(m => m.name.includes('Light')); // ou 'Dark'
  ['1:92', '1:112', '1:134', '1:154', '1:179'].forEach(id => {
    const n = figma.getNodeById(id);
    if (n) n.setExplicitVariableModeForCollection(found.col, mode.modeId);
  });
}
```

---

## Scaling sem distorção

### O problema
`resize()` pode quebrar layers e distorcer conteúdo.

### A solução
Usar `rescale()` que escala proporcionalmente, depois reposicionar:

```javascript
// Escalar a partir do canto superior direito
const node = figma.getNodeById('1:92');
const oldRight = node.x + node.width;
const oldTop = node.y;
node.rescale(1.2);
node.x = oldRight - node.width;
node.y = oldTop;

// Escalar e centrar num frame
const frameW = 1920, frameH = 1080;
node.rescale(1.2);
node.x = (frameW - node.width) / 2;
node.y = (frameH - node.height) / 2;
```

---

## Operações em batch

### Renomear múltiplos nodes

```javascript
const page = figma.currentPage;
page.children
  .filter(n => n.name.startsWith('Stream-'))
  .forEach((frame, i) => { frame.name = `session-${i + 1}`; });
```

### Renomear filhos de frames

```javascript
page.children
  .filter(n => n.name.startsWith('session-'))
  .forEach(session => {
    const group = session.children.find(c => c.type === 'GROUP');
    if (group) group.name = 'content';
  });
```

### Scaling diferente por conteúdo

```javascript
page.children
  .filter(n => n.name.startsWith('session-'))
  .forEach(session => {
    const content = session.children.find(c => c.name === 'content');
    if (!content) return;
    const scale = session.name.includes('One Speaker') ? 1.2 : 1.1;
    content.rescale(scale);
    content.x = (1920 - content.width) / 2;
    content.y = (1080 - content.height) / 2;
  });
```

---

## Selecção programática

```javascript
// Seleccionar por IDs
const nodes = ['1:92', '1:112', '1:134']
  .map(id => figma.getNodeById(id))
  .filter(Boolean);
figma.currentPage.selection = nodes;

// Limpar selecção
figma.currentPage.selection = [];

// Ver selecção actual
figma.currentPage.selection.map(n => `${n.name} (${n.id})`).join(', ');
```

---

## Verificar posições antes de criar

```javascript
const nodes = figma.currentPage.children.map(n => ({
  name: n.name, x: n.x, width: n.width
}));
const maxX = Math.max(0, ...nodes.map(n => n.x + n.width)) + 100;
// → posicionar novo componente em x: maxX
```

---

## Debugging

### Eval não devolve output

O código executa mas o return é `undefined`. Acontece com operações que modificam nodes.

**Verificar se correu:**
```bash
node src/index.js raw query "//GROUP"          # Listar nodes modificados
node src/index.js canvas info                  # Ver estado do canvas
node src/index.js verify                       # Screenshot da selecção
```

### Encontrar IDs de nodes

```bash
node src/index.js raw query "//FRAME"
# Output: [FRAME] "session-1" (1:90) 1920×1080
```

### Inspecionar estrutura de um node

```javascript
const node = figma.getNodeById('1:90');
node.children.map(c => `${c.name} (${c.type})`).join(', ');
```

### Verificar bindings de variáveis

```javascript
const node = figma.getNodeById('1:92');
JSON.stringify(node.boundVariables, null, 2);
```
