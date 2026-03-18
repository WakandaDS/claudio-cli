# Memória do Figma CLI

## Ficheiros Principais
- `src/figma-client.js` - Parser JSX principal, gerador de código, wrapper da API Figma
- `src/index.js` - Ponto de entrada do CLI, todos os comandos
- Ver documentação detalhada: [figma-plugin-api.md](figma-plugin-api.md), [bugs-and-fixes.md](bugs-and-fixes.md)

## Padrões Críticos

### Variable Binding
- Coleções chamam-se `shadcn/primitives` e `shadcn/semantic` (NÃO apenas `shadcn`)
- Usar `c.name.startsWith('shadcn')` para encontrar todas as coleções shadcn
- `setBoundVariableForPaint(paint, 'color', variable)` retorna novo paint, tem de ser atribuído a fills
- Cor de fallback `rgb(0.5, 0.5, 0.5)` = cinzento quando variável não encontrada

### Dimensionamento Auto-Layout (Root Frame)
- Layout VERTICAL: eixo primário = height, eixo secundário = width
- Layout HORIZONTAL: eixo primário = width, eixo secundário = height
- Sem `h` explícito em frame vertical = HUG conteúdo (primaryAxisSizingMode='AUTO')
- Sem `w` explícito em frame horizontal = HUG conteúdo
- `h`/`w` explícito = FIXED sizing

### Dimensionamento Auto-Layout (Children)
- `layoutSizingHorizontal`/`layoutSizingVertical`: 'FIXED' | 'HUG' | 'FILL'
- TEM de ser definido DEPOIS de `appendChild()` - definir antes dá erro
- `grow={1}` mapeia para FILL na direcção flex do pai
- `w="fill"` mapeia para `layoutSizingHorizontal = 'FILL'`

### Quebra de Texto
- O pai E cada elemento Text precisa de `w="fill"`
- O pai tem de ter `flex="col"` ou `flex="row"`
- A fonte tem de ser carregada antes de definir characters

### Parser JSX
- Fazer parse de tags Frame open/close primeiro, depois self-closing fora dos ranges consumidos
- `frameOpenRegex` tem de saltar self-closing (`match[0].endsWith('/')`)
- `extractContent` usa non-greedy `[^>]*?` para evitar consumir `/` em `/>`

### Ícones (Lucide via Iconify)
- `<Icon name="lucide:icon-name" size={16} color="var:foreground" />`
- SVGs pré-carregados no lado Node.js a partir da API Iconify, incorporados no código Figma gerado
- Usa `figma.createNodeFromSvg()` para renderização SVG real (não rectângulos placeholder)
- Suporta binding de cor com `var:` (coloriza todos os fills/strokes na árvore SVG)
- 11 ícones usados em componentes shadcn: check, chevron-left/right/down/up, x, plus, bold, ellipsis, info, alert-circle

## Dois Caminhos de Código
1. **render-batch** (~linha 374): Sem suporte var:, usa `primaryAxisSizingMode`/`counterAxisSizingMode`
2. **single render com var:** (~linha 783): Suporte completo var:, usa `layoutSizingHorizontal`/`layoutSizingVertical` para children

## Avisos Importantes (da documentação da API)
- `layoutWrap = 'WRAP'` só funciona em HORIZONTAL, dá erro em VERTICAL
- Conflito STRETCH + AUTO: child auto-layout com STRETCH tem de ter FIXED sizing
- Fills/strokes são arrays imutáveis: clonar, modificar, reatribuir
- `createComponentFromNode()` falha se o nó estiver dentro de Component/ComponentSet/Instance
- Definir `layoutMode = 'NONE'` NÃO restaura as posições originais dos children
- `SPACE_BETWEEN` funciona em `primaryAxisAlignItems` (verificar mapeamento JSX)
- Nomes de propriedades de componentes recebem sufixo `#uniqueId` (ex: `ButtonText#0:1`)

## Configuração Remota
- `origin` = WakandaDS/claudio-cli
