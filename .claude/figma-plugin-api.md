# Referência da API de Plugins do Figma

## Tipos de Nó

### FrameNode
- `type: 'FRAME'`
- Criar: `figma.createFrame()`
- Layout: `layoutMode` ('NONE' | 'HORIZONTAL' | 'VERTICAL' | 'GRID')
- Padding: `paddingLeft`, `paddingRight`, `paddingTop`, `paddingBottom`
- Dimensionamento: `primaryAxisSizingMode`, `counterAxisSizingMode` ('FIXED' | 'AUTO')
- Dimensionamento de children: `layoutSizingHorizontal`, `layoutSizingVertical` ('FIXED' | 'HUG' | 'FILL')
- Alinhamento: `primaryAxisAlignItems` ('MIN' | 'CENTER' | 'MAX' | 'SPACE_BETWEEN')
- Alinhamento cruzado: `counterAxisAlignItems` ('MIN' | 'CENTER' | 'MAX' | 'BASELINE')
- Espaçamento: `itemSpacing`, `counterAxisSpacing`
- Wrap: `layoutWrap` ('NO_WRAP' | 'WRAP')
- `clipsContent`: boolean - corta overflow
- Cantos: `cornerRadius`, `cornerSmoothing`, cantos individuais (`topLeftRadius`, etc.)
- Estilo: `fills`, `strokes`, `effects`, `opacity`, `blendMode`
- Geometria: `x`, `y`, `width`, `height` (readonly), `resize()`, `resizeWithoutConstraints()`
- Children: `children` (readonly), `appendChild()`, `insertChild()`, `findAll()`, `findOne()`
- Variáveis: `boundVariables`, `setBoundVariable()`

### TextNode
- `type: 'TEXT'`
- Criar: `figma.createText()`
- **CRÍTICO**: Tem de carregar a fonte antes de definir `characters`: `await figma.loadFontAsync({family:'Inter',style:'Regular'})`
- `characters`: conteúdo de texto em bruto
- `fontSize`: number (min 1)
- `fontName`: `{family: string, style: string}`
- `fontWeight`: number (readonly, altera-se via fontName)
- `textAlignHorizontal`: 'LEFT' | 'CENTER' | 'RIGHT' | 'JUSTIFIED'
- `textAlignVertical`: 'TOP' | 'CENTER' | 'BOTTOM'
- `textAutoResize`: 'NONE' | 'WIDTH_AND_HEIGHT' | 'HEIGHT' | 'TRUNCATE'
- `textTruncation`: 'DISABLED' | 'ENDING'
- `maxLines`: number | null
- `letterSpacing`, `lineHeight`, `paragraphSpacing`, `paragraphIndent`
- `textDecoration`, `textCase`
- `hasMissingFont`: boolean (readonly)
- Métodos de range: `setRangeFontSize()`, `setRangeFontName()`, `setRangeFills()`, etc.
- `autoRename`: boolean - derivar nome automaticamente dos characters
- `hyperlink`: HyperlinkTarget | null

### ComponentNode
- `type: 'COMPONENT'`
- Herda todas as propriedades de FrameNode
- `createInstance()`: cria InstanceNode
- `getInstancesAsync()`: todas as instâncias no documento
- `componentPropertyDefinitions`: readonly, todas as propriedades com defaults
- `addComponentProperty(name, type, defaultValue, options?)`: retorna nome com sufixo ID
- `editComponentProperty(name, newValue)`: modificar propriedade existente
- `deleteComponentProperty(name)`: remover propriedade
- Tipos de propriedade: 'BOOLEAN' | 'TEXT' | 'INSTANCE_SWAP' | 'VARIANT'
- `key`: string (readonly) - para `figma.importComponentByKeyAsync()`
- `remote`: boolean - de team library
- `description`, `descriptionMarkdown`
- Criar a partir de frame: `figma.createComponentFromNode(frame)`

### ComponentSetNode
- `type: 'COMPONENT_SET'`
- Contentor para componentes variantes
- Todos os children são ComponentNodes
- `defaultVariant`: variante no canto superior esquerdo (readonly)
- `componentPropertyDefinitions`: todas as propriedades de variantes
- Criar: `figma.combineAsVariants(components, parent)`

### InstanceNode
- `type: 'INSTANCE'`
- `mainComponent`: o componente de origem (ao definir, faz swap + limpa overrides)
- `getMainComponentAsync()`: versão assíncrona
- `componentProperties`: valores actuais (readonly)
- `setProperties({...})`: actualizar propriedades (TEXT/BOOLEAN/INSTANCE_SWAP precisam de sufixo #ID)
- `overrides`: campos directamente sobrepostos (readonly)
- `removeOverrides()`: limpar todos os overrides
- `swapComponent(comp)`: trocar preservando overrides
- `detachInstance()`: converter em frame
- `exposedInstances`: instâncias expostas aninhadas (readonly)
- `scaleFactor`: escala da instância
- **AVISO DE PERFORMANCE**: Evitar alternar escritas em ComponentNode com leituras de InstanceNode

## API de Variáveis (`figma.variables`)

### Métodos
```
getVariableByIdAsync(id: string): Promise<Variable | null>
getVariableCollectionByIdAsync(id: string): Promise<VariableCollection | null>
getLocalVariablesAsync(type?: VariableResolvedDataType): Promise<Variable[]>
getLocalVariableCollectionsAsync(): Promise<VariableCollection[]>
createVariable(name, collection, resolvedType): Variable
createVariableCollection(name): VariableCollection
createVariableAlias(variable): VariableAlias
setBoundVariableForPaint(paint, field, variable | null): SolidPaint
setBoundVariableForEffect(effect, field, variable | null): Effect
importVariableByKeyAsync(key): Promise<Variable>
```

### Objecto Variable
- `id`, `name`, `description`: string
- `remote`: boolean (readonly)
- `resolvedType`: VariableResolvedDataType ('BOOLEAN' | 'FLOAT' | 'STRING' | 'COLOR')
- `valuesByMode`: {[modeId]: value} (readonly, não resolve aliases)
- `setValueForMode(modeId, newValue)`: definir valor para modo
- `variableCollectionId`: string (readonly)
- `key`: string (readonly) - para importação
- `scopes`: VariableScope[] - visibilidade na UI
- `codeSyntax`: {WEB?, ANDROID?, iOS?} (readonly)
- `hiddenFromPublishing`: boolean
- `resolveForConsumer(node)`: obter valor resolvido
- `remove()`: apagar variável

### Objecto VariableCollection
- `id`, `name`: string
- `modes`: [{modeId, name}] (readonly)
- `defaultModeId`: string (readonly)
- `variableIds`: string[] (readonly)
- `remote`, `hiddenFromPublishing`: boolean
- `addMode(name)`: retorna modeId (limitado pelo plano de preço)
- `removeMode(modeId)`, `renameMode(modeId, name)`
- `remove()`: apagar coleção + todas as variáveis

### Vincular Variáveis a Fills
```javascript
// Criar paint vinculado
const paint = figma.variables.setBoundVariableForPaint(
  { type: 'SOLID', color: { r: 0.5, g: 0.5, b: 0.5 } },
  'color',
  variable
);
node.fills = [paint];
```

### Vincular Variáveis a Propriedades de Nó
```javascript
node.setBoundVariable('fills', 0, variable);  // vincular fill no índice
node.setBoundVariable('itemSpacing', variable);
node.setBoundVariable('paddingLeft', variable);
// etc. para: visible, opacity, cornerRadius, strokeWeight, ...
```

## boundVariables (REST API)
Propriedades que podem ter variáveis vinculadas:
- `size.x`, `size.y`
- `itemSpacing`, `counterAxisSpacing`
- `paddingLeft/Right/Top/Bottom`
- `visible`, `opacity`
- `topLeftRadius`, `topRightRadius`, `bottomLeftRadius`, `bottomRightRadius`
- `minWidth`, `maxWidth`, `minHeight`, `maxHeight`
- `fills[]`, `strokes[]`, `effects[]`
- `characters`
- `fontSize[]`, `fontFamily[]`, `fontWeight[]`
- `letterSpacing[]`, `lineHeight[]`
- `paragraphSpacing[]`, `paragraphIndent[]`

## Regras de Auto-Layout

### Mapeamento de Eixos
| Modo de Layout | Eixo Primário | Eixo Secundário |
|----------------|---------------|-----------------|
| VERTICAL       | Height (Y)    | Width (X)       |
| HORIZONTAL     | Width (X)     | Height (Y)      |

### Modos de Dimensionamento
- `primaryAxisSizingMode`: 'FIXED' (tamanho explícito) | 'AUTO' (ajustar ao conteúdo)
- `counterAxisSizingMode`: 'FIXED' | 'AUTO'
- Estes são para a própria frame

### Dimensionamento de Children (definir DEPOIS de appendChild!)
- `layoutSizingHorizontal`: 'FIXED' | 'HUG' | 'FILL'
- `layoutSizingVertical`: 'FIXED' | 'HUG' | 'FILL'
- FILL = esticar para preencher o pai (apenas em pai com auto-layout)
- HUG = encolher para caber no conteúdo
- FIXED = width/height explícitos via resize()

### Padrões Comuns
```javascript
// Frame que ajusta ao conteúdo verticalmente, largura fixa
frame.layoutMode = 'VERTICAL';
frame.primaryAxisSizingMode = 'AUTO';  // height ajusta
frame.counterAxisSizingMode = 'FIXED'; // width fixo
frame.resize(300, 1);

// Child que preenche a largura do pai
parent.appendChild(child);
child.layoutSizingHorizontal = 'FILL'; // TEM de ser depois de appendChild

// Padrão grow (espaçador)
spacer.layoutSizingHorizontal = 'FILL'; // em pai horizontal
spacer.layoutSizingVertical = 'FILL';   // em pai vertical
```

## Métodos Globais figma
- `figma.createFrame()`, `figma.createText()`, `figma.createRectangle()`
- `figma.createComponentFromNode(node)`: converter frame em componente
- `figma.combineAsVariants(components[], parent)`: criar conjunto de variantes
- `figma.loadFontAsync({family, style})`: obrigatório antes de operações com texto
- `figma.getNodeByIdAsync(id)`: encontrar nó
- `figma.currentPage`: página activa
- `figma.root`: raiz do documento
- `figma.variables`: namespace VariablesAPI

## Avisos Importantes

### layoutWrap apenas em HORIZONTAL
- `layoutWrap = 'WRAP'` dá erro em frames VERTICAL
- `counterAxisSpacing` e `counterAxisAlignContent` só funcionam com WRAP

### Conflito STRETCH + AUTO
- Child com `layoutAlign = 'STRETCH'` que é auto-layout: TEM de definir o sizing correspondente como FIXED
- Uma frame não pode simultaneamente esticar-para-preencher E ajustar-aos-children
- O mesmo aplica-se a `layoutGrow = 1` em child frames com auto-layout

### SPACE_BETWEEN funciona
- `primaryAxisAlignItems = 'SPACE_BETWEEN'` funciona na API
- A nota no CLAUDE.md sobre ser instável pode ser um problema de mapeamento JSX

### Fills/Strokes são Arrays Imutáveis
- Tem de clonar, modificar, reatribuir: `const fills = [...node.fills]; fills[0] = ...; node.fills = fills;`
- Pattern fills requerem `setFillsAsync()`, não atribuição directa

### Restrições de createComponentFromNode
- O nó não pode já ser um Component ou ComponentSet
- O nó não pode estar dentro de um Component, ComponentSet ou Instance
- Violar estas regras dá erro

### counterAxisSpacing
- Definir como `null` para sincronizar com `itemSpacing`
- Uma vez definido como número, nunca retorna null novamente

### width/height são Readonly
- Usar `resize()`, `resizeWithoutConstraints()`, ou propriedades layoutSizing
- `resize()` respeita constraints dos children, `resizeWithoutConstraints()` ignora-as

### Definir layoutMode como NONE é Destrutivo
- Reverter de auto-layout para NONE NÃO restaura as posições originais dos children

## API Depreciada (evitar)
- `getVariableById()` -> usar `getVariableByIdAsync()`
- `getLocalVariables()` -> usar `getLocalVariablesAsync()`
- `getLocalVariableCollections()` -> usar `getLocalVariableCollectionsAsync()`
- `layoutGrow` -> usar `layoutSizingHorizontal/Vertical = 'FILL'`
- `layoutAlign` -> usar `layoutSizingHorizontal/Vertical`
- `primaryAxisSizingMode`/`counterAxisSizingMode` para children -> usar `layoutSizingHorizontal/Vertical`
