# Render e Componentes
#figma-cli #guia #render #componentes #jsx

> [!NOTE]
> O comando `render` aceita JSX e cria nodes no Figma com posicionamento automático. É a forma principal de construir UI — não usar `eval` para criação de frames.

---

## Sintaxe JSX base

```jsx
<Frame
  name="Card"
  w={320} h={200}        // Dimensões fixas
  w="fill"               // Ou preenche o pai
  bg="#18181b"           // Cor de fundo
  bg="var:card"          // Ou vínculo a token
  stroke="var:border"    // Borda
  rounded={12}           // Corner radius
  p={24}                 // Padding all sides
  px={16} py={8}         // Padding x / y
  flex="col"             // Auto-layout vertical
  flex="row"             // Auto-layout horizontal
  gap={16}               // Espaçamento entre filhos
  justify="center"       // Alinhamento no eixo principal
  items="center"         // Alinhamento no eixo cruzado
  overflow="hidden"      // Clip de conteúdo
>
  <Text size={18} weight="bold" color="#fff" w="fill">Título</Text>
  <Icon name="lucide:star" size={16} color="var:primary" />
</Frame>
```

---

## Erros comuns (silenciosos — sem mensagem de erro!)

| Errado | Correcto |
|--------|----------|
| `layout="horizontal"` | `flex="row"` |
| `padding={24}` | `p={24}` |
| `fill="#fff"` | `bg="#fff"` |
| `cornerRadius={12}` | `rounded={12}` |
| `fontSize={18}` | `size={18}` |
| `fontWeight="bold"` | `weight="bold"` |
| `justify="between"` | Usar `grow={1}` como spacer |

---

## Regra crítica: texto que pode quebrar linha

Para o texto não ficar cortado, é **obrigatório**:
1. Frame pai com `w` fixo ou `w="fill"`
2. **Todos** os `<Text>` com `w="fill"` (incluindo títulos curtos)
3. Frame pai com `flex="col"` ou `flex="row"`

```jsx
// ✗ Errado — texto corta
<Frame flex="col" gap={8}>
  <Text size={16} weight="bold" color="#fff">Título Wireless Noise-Canceling</Text>
</Frame>

// ✓ Correcto — texto quebra linha
<Frame flex="col" gap={8} w="fill">
  <Text size={16} weight="bold" color="#fff" w="fill">Título Wireless Noise-Canceling</Text>
  <Text size={14} color="#a1a1aa" w="fill">Descrição que pode ser longa...</Text>
</Frame>
```

---

## Padrões de layout

### Navbar (items às extremidades)
```jsx
// justify="between" não funciona — usar spacer grow={1}
<Frame flex="row" items="center" w="fill">
  <Frame>Logo</Frame>
  <Frame grow={1} justify="center">Nav Links</Frame>
  <Frame>Buttons</Frame>
</Frame>
```

### Input ao fundo (padrão chat)
```jsx
<Frame flex="col" h={400}>
  <Text>Mensagem 1</Text>
  <Frame grow={1} />    // ← spacer que empurra o input para o fundo
  <Frame>Input</Frame>
</Frame>
```

### Toggle switch — ON/OFF com flex
```jsx
// ON (knob à direita)
<Frame w={52} h={28} bg="#3b82f6" rounded={14} flex="row" items="center" p={2} justify="end">
  <Frame w={24} h={24} bg="#fff" rounded={12} />
</Frame>

// OFF (knob à esquerda)
<Frame w={52} h={28} bg="#27272a" rounded={14} flex="row" items="center" p={2} justify="start">
  <Frame w={24} h={24} bg="#52525b" rounded={12} />
</Frame>
```

### Posicionamento absoluto (badge sobre avatar)
```jsx
// x/y é relativo ao padding do pai
<Frame p={24}>
  <Frame w={100} h={100} rounded={50} bg="#333" />
  <Frame name="Badge" w={20} h={20} bg="#ef4444" rounded={10}
         position="absolute" x={114} y={114} />
</Frame>
```

---

## Ícones Lucide

```jsx
// Ícones reais como SVG (via Iconify API)
<Icon name="lucide:home" size={20} color="#fff" />
<Icon name="lucide:settings" size={20} color="var:foreground" />
<Icon name="lucide:chevron-right" size={16} color="var:muted-foreground" />
```

> [!NOTE]
> Não usar emojis como ícones — renderizam de forma inconsistente. Usar Lucide ou formas geométricas como fallback.

---

## Converter em componente

Depois de criar com `render`, converter para componente Figma nativo:

```bash
node src/index.js node to-component "FRAME_ID"
```

**Sempre verificar visualmente após conversão:**
```bash
node src/index.js verify "NODE_ID"
```

---

## Criar múltiplos frames (render-batch)

```bash
node src/index.js render-batch '<Frame name="Card 1" ...>...</Frame>' '<Frame name="Card 2" ...>...</Frame>'
```

> [!WARNING]
> `render-batch` **não renderiza texto correctamente em Safe Mode**. Usar `eval` com a Figma API nativa para componentes com texto em Safe Mode. Ver [[visao-geral]].

---

## Criar componentes complexos via eval

Para componentes com múltiplas variantes ou texto preciso, usar `eval` directamente com a Figma API:

```bash
node src/index.js eval "(async () => {
  await figma.loadFontAsync({ family: 'Inter', style: 'Bold' });
  await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });

  const card = figma.createFrame();
  card.name = 'Card';
  card.resize(340, 1);
  card.layoutMode = 'VERTICAL';
  card.primaryAxisSizingMode = 'AUTO';
  card.counterAxisSizingMode = 'FIXED';
  card.paddingTop = card.paddingBottom = card.paddingLeft = card.paddingRight = 20;
  card.itemSpacing = 16;
  card.cornerRadius = 12;

  const title = figma.createText();
  title.fontName = { family: 'Inter', style: 'Bold' };
  title.characters = 'Título';
  title.fontSize = 14;
  card.appendChild(title);
  title.layoutSizingHorizontal = 'FILL'; // DEPOIS do appendChild

  const comp = figma.createComponentFromNode(card);
  return { id: comp.id };
})()"
```

**Regra crítica de auto-layout:**
1. `resize(LARGURA, 1)` + `primaryAxisSizingMode = 'FIXED'` no frame pai
2. `layoutSizingHorizontal = 'FILL'` nos filhos — **sempre depois do `appendChild`**
3. A ordem importa: `appendChild` primeiro, depois `layoutSizingHorizontal`

---

## Verificar antes de criar

Verificar o que já existe no canvas para evitar sobreposições:

```javascript
const nodes = figma.currentPage.children.map(n => ({ name: n.name, x: n.x, width: n.width }));
const maxX = Math.max(0, ...nodes.map(n => n.x + n.width)) + 100;
// Posicionar novo componente a partir de maxX
```
