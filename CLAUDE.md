# claudio-cli

CLI que controla o Figma Desktop diretamente. Não precisa de API key.

---

## Contexto do utilizador

**Quem és:** Design System Architect. Trabalhas com designers e devs, defines a estrutura e os padrões do sistema de design.

**Língua:** Recebes instruções em **Português de Portugal (PT-PT)**. Responde sempre em PT-PT. Componentes, layers e tokens em **inglês** (conforme convenção do sistema de design). Comentários e feedback em PT-PT.

**Modo de trabalho:** Recebes um pedido em linguagem natural → executas silenciosamente → confirms o resultado. Nunca mostres comandos de terminal ao utilizador a menos que ele peça.

---

## Frases em PT-PT → Comandos

| O utilizador diz | Comando |
|-----------------|---------|
| "liga ao figma" / "conecta" | `node src/index.js connect` |
| "o que está no canvas" / "mostra o canvas" | `node src/index.js canvas info` |
| "lista os tokens" / "mostra as variáveis" | `node src/index.js var list` |
| "mostra as cores" / "visualiza os tokens" | `node src/index.js var visualize` |
| "adiciona tokens shadcn" | `node src/index.js tokens preset shadcn` |
| "adiciona cores tailwind" | `node src/index.js tokens tailwind` |
| "cria um componente X" | `render` + `node to-component` |
| "converte em componente" | `node src/index.js node to-component "ID"` |
| "cria variantes de tamanho" | `node src/index.js sizes --base small` |
| "mostra todas as combinações" / "gera variantes" | `node src/index.js combos` |
| "audita as cores" / "verifica contraste" | `node src/index.js lint --rule color-contrast` |
| "faz lint" / "verifica o ficheiro" | `node src/index.js lint` |
| "analisa tipografia" | `node src/index.js analyze typography` |
| "analisa espaçamentos" | `node src/index.js analyze spacing` |
| "encontra X" / "procura X" | `node src/index.js find "X"` |
| "exporta como PNG/SVG" | `node src/index.js export png` |
| "exporta CSS" / "exporta os tokens" | `node src/index.js export css` |
| "cria um dashboard" | `node src/index.js blocks create dashboard-01` |
| "cria um slot" | `node src/index.js slot create "Name"` |
| "elimina todas as variáveis" | `node src/index.js var delete-all` |
| "verifica" / "tira screenshot" | `node src/index.js verify` |

---

## Convenções do sistema de design

- **Componentes:** camelCase — ex: `dividerLine`, `buttonPrimary`, `cardHeader`
- **Layers/elementos internos:** Title Case com espaço — ex: `Label`, `Content`, `Icon`, `Left Content`
- **Tokens:** uma coleção com dois modos (Light/Dark). Estrutura em 4 camadas:
  - `primitives` — valores brutos, flat. Ex: `primitives.neutral1000` (#F2F2F2), `primitives.neutral000` (#000), `primitives.green`, `primitives.red`
  - `base` — decisões de tema. Ex: `base.primaryHue.color`, `base.secondaryHue.color`, `base.typography.fontFamily`, `base.radius.allCard`, `base.shadow.allDisplay`
  - `semantic` — intenção. Ex: `semantic.negative.color`, `semantic.text.label.color`, `semantic.text.placeholder.color`, `semantic.avatar.background`
  - `advanced` — tokens por componente. Ex: `advanced.buttonPrimary.text.color`, `advanced.cardPrimary.background.color`, `advanced.dividerLine.background.color`
- **Naming de tokens:** camelCase em todos os níveis. Leaf properties: `color`, `background`, `text`, `icon`, `radius`, `shadow`, `display`. Referências: `{camada.grupo.propriedade}`
- **Comentários e anotações no Figma:** PT-PT
- **Variantes:** propriedades em inglês (`Size`, `State`, `Variant`), valores em inglês (`small`, `hover`, `outlined`)

---

## Fluxo padrão para criar componentes

1. Verificar o que já existe no canvas (`canvas info`)
2. Criar estrutura com `render` (JSX) — sempre com `var:` para ligar tokens existentes
3. Converter em componente (`node to-component`)
4. Verificar visualmente (`verify`)
5. Confirmar ao utilizador em PT-PT

---

## Referência Rápida

| O utilizador diz | Comando |
|-----------|---------|
| "liga ao figma" | `node src/index.js connect` |
| "adiciona cores shadcn" | `node src/index.js tokens preset shadcn` |
| "adiciona cores tailwind" | `node src/index.js tokens tailwind` |
| "mostra as cores no canvas" | `node src/index.js var visualize` |
| "cria um dashboard" | `node src/index.js blocks create dashboard-01` |
| "lista os blocos" | `node src/index.js blocks list` |
| "cria cards/botões" | `render-batch` + `node to-component` |
| "cria um retângulo/frame" | `node src/index.js render '<Frame>...'` |
| "converte em componente" | `node src/index.js node to-component "ID"` |
| "lista as variáveis" | `node src/index.js var list` |
| "encontra nós com o nome X" | `node src/index.js find "X"` |
| "o que está no canvas" | `node src/index.js canvas info` |
| "exporta como PNG/SVG" | `node src/index.js export png` |
| "mostra todas as variantes" | `node src/index.js combos` |
| "cria variantes de tamanho" | `node src/index.js sizes --base small` |
| "cria um slot" | `node src/index.js slot create "Name"` |
| "lista os slots" | `node src/index.js slot list` |
| "reinicia o slot" | `node src/index.js slot reset` |
| "verifica a criação" | `node src/index.js verify` |

**Referência completa de comandos:** Ver REFERENCE.md

---

## Verificação por IA (Interno)

Após criar qualquer componente, executa `verify` para obter um screenshot pequeno para validação:

```bash
node src/index.js verify              # Screenshot da seleção
node src/index.js verify "123:456"    # Screenshot de um nó específico
```

Devolve JSON com imagem base64 (máx. 2000px, redimensionado automaticamente para ficar dentro dos limites da API).

**Verificar sempre após:**
- `render` ou `render-batch`
- `node to-component`
- Qualquer criação visual

Isto é para verificações internas da IA, não é mostrado aos utilizadores.

---

## Blocos (Layouts Pré-construídos)

**Usa SEMPRE `blocks create` para dashboards e layouts de página.** Nunca os construas manualmente com render/eval.

```bash
node src/index.js blocks list                    # Mostra blocos disponíveis
node src/index.js blocks create dashboard-01     # Cria dashboard no Figma
```

**dashboard-01**: Dashboard de analítica completo com:
- Barra lateral com ícones Lucide reais (layout-dashboard, refresh-cw, bar-chart-3, folder, users, etc.)
- Cards de estatísticas (Revenue, Customers, Accounts, Growth)
- Gráfico de área com dois conjuntos de dados
- Tabela de dados com paginação
- Todas as cores ligadas a variáveis shadcn (suporta modo Light/Dark)

**Cópia em modo escuro**: Após criar, clona e muda o modo:
```javascript
// Via eval:
var clone = dashboard.clone();
clone.name = 'Dashboard (Dark)';
clone.setExplicitVariableModeForCollection(semanticCollection, darkModeId);
```

Ficheiros fonte dos blocos: `src/blocks/`

---

## Tokens de Design

"Adicionar cores shadcn":
```bash
node src/index.js tokens preset shadcn   # 244 primitives + 32 semantic (Light/Dark)
```

"Adicionar cores tailwind":
```bash
node src/index.js tokens tailwind        # 242 cores primitivas apenas
```

"Criar sistema de design":
```bash
node src/index.js tokens ds              # Cores base IDS
```

**shadcn vs tailwind:**
- `tokens preset shadcn` = Sistema shadcn completo (primitives + tokens semânticos com modo Light/Dark)
- `tokens tailwind` = Apenas a paleta de cores Tailwind (apenas primitives)

"Eliminar todas as variáveis":
```bash
node src/index.js var delete-all                    # Todas as coleções
node src/index.js var delete-all -c "primitives"    # Apenas coleção específica
```

**Nota:** `var list` apenas MOSTRA variáveis existentes. Usa os comandos `tokens` para as CRIAR.

---

## Ligação Rápida de Variáveis (sintaxe var:)

Usa a sintaxe `var:name` para ligar variáveis diretamente no momento da criação (atualmente pesquisa nas coleções shadcn):

### Comandos de criação com var:
```bash
node src/index.js create rect "Card" --fill "var:card" --stroke "var:border"
node src/index.js create circle "Avatar" --fill "var:primary"
node src/index.js create text "Hello" -c "var:foreground"
node src/index.js create line -c "var:border"
node src/index.js create frame "Section" --fill "var:background"
node src/index.js create autolayout "Container" --fill "var:muted"
node src/index.js create icon lucide:star -c "var:primary"
```

### render JSX com var:
```bash
node src/index.js render '<Frame bg="var:card" stroke="var:border" rounded={12} p={24}>
  <Text color="var:foreground" size={18}>Title</Text>
</Frame>'
```

### Comandos set com var:
```bash
node src/index.js set fill "var:primary"
node src/index.js set stroke "var:border"
```

**Variáveis:** `background`, `foreground`, `card`, `primary`, `secondary`, `muted`, `accent`, `border`, e as respetivas variantes `-foreground`.

---

## Modos de Conexão

### Modo Yolo (Recomendado)
Faz patch ao Figma uma vez, depois liga diretamente. Totalmente automático.
```bash
node src/index.js connect
```

### Modo Safe
Usa plugin, sem modificação do Figma. Inicia o plugin em cada sessão.
```bash
node src/index.js connect --safe
```
Depois: Plugins → Development → FigCli

**Notas do Modo Safe:**
- Todos os comandos funcionam via daemon (sem dependência do figma-use)
- Timeout de 60s (igual ao Modo Yolo)
- **CRÍTICO: `render-batch` NÃO renderiza texto corretamente no Modo Safe!**
- Usa `eval` com a API direta do Figma para componentes com texto

---

## Criar Componentes (Modo Safe)

**NÃO uses render-batch para componentes com texto no Modo Safe.** Usa `eval` com a API nativa do Figma:

```javascript
node src/index.js eval "(async () => {
  // 1. Carregar fontes PRIMEIRO
  await figma.loadFontAsync({ family: 'Inter', style: 'Bold' });
  await figma.loadFontAsync({ family: 'Inter', style: 'Regular' });

  // 2. Criar frame com largura FIXA
  const card = figma.createFrame();
  card.name = 'Card';
  card.x = 100; card.y = 100;
  card.resize(340, 1); // Largura fixa!
  card.layoutMode = 'HORIZONTAL';
  card.primaryAxisSizingMode = 'FIXED'; // Manter largura fixa
  card.counterAxisSizingMode = 'AUTO';  // Altura ajusta ao conteúdo
  card.paddingTop = card.paddingBottom = card.paddingLeft = card.paddingRight = 20;
  card.itemSpacing = 16;
  card.cornerRadius = 12;
  card.fills = [{ type: 'SOLID', color: { r: 0.094, g: 0.094, b: 0.106 } }];

  // 3. Frame de conteúdo deve PREENCHER o espaço restante
  const content = figma.createFrame();
  content.fills = [];
  content.layoutMode = 'VERTICAL';
  content.itemSpacing = 4;
  card.appendChild(content);
  content.layoutSizingHorizontal = 'FILL'; // Crítico!

  // 4. Texto deve PREENCHER para quebrar linha
  const title = figma.createText();
  title.fontName = { family: 'Inter', style: 'Bold' };
  title.characters = 'Title here';
  title.fontSize = 14;
  title.fills = [{ type: 'SOLID', color: { r: 1, g: 1, b: 1 } }];
  content.appendChild(title);
  title.layoutSizingHorizontal = 'FILL'; // Crítico!

  // 5. Converter em componente
  const comp = figma.createComponentFromNode(card);
  return { id: comp.id, name: comp.name };
})()"
```

**Regras de Auto-Layout (Prevenção de Texto Cortado):**
1. O frame pai precisa de `resize(WIDTH, 1)` + `primaryAxisSizingMode = 'FIXED'`
2. Os frames filhos de conteúdo precisam de `layoutSizingHorizontal = 'FILL'` APÓS appendChild
3. TODOS os nós de texto precisam de `layoutSizingHorizontal = 'FILL'` APÓS appendChild
4. A ordem importa: appendChild primeiro, depois definir layoutSizingHorizontal

**Antes de Criar - Verificar Posições:**
```javascript
// Verificar o que está na página para evitar sobreposição
const nodes = figma.currentPage.children.map(n => ({
  name: n.name,
  x: n.x,
  width: n.width
}));
// Encontrar a extremidade mais à direita, colocar novos componentes depois
const maxX = Math.max(0, ...nodes.map(n => n.x + n.width)) + 100;
```

**NUNCA elimines nós existentes** - os utilizadores podem ter componentes que querem manter!

**Após Criar - Verificar Sempre:**
```bash
node src/index.js verify "NODE_ID"  # Tirar screenshot e verificar visualmente
```

---

## Componentes Complexos (Pricing Cards, etc.)

Para componentes complexos com múltiplos elementos, usa um **único eval** com a API nativa do Figma em vez de JSX:

### Padrão
1. **Verificar variáveis primeiro** - não assumir que alguma coleção existe
2. **Usar cores de fallback** quando não existem variáveis
3. **Único eval** - criar tudo numa única chamada à API
4. **Orientado a dados** - definir conteúdo em array, iterar para criar
5. **Altura igual** - usar `layoutAlign: "STRETCH"` e `layoutGrow: 1`

### Cores de Fallback (Tema Escuro)
```javascript
const colors = {
  bg: { r: 0.09, g: 0.09, b: 0.11 },       // #17171c
  card: { r: 0.11, g: 0.11, b: 0.13 },     // #1c1c21
  border: { r: 0.2, g: 0.2, b: 0.22 },     // #333338
  primary: { r: 0.23, g: 0.51, b: 0.97 },  // #3b82f8
  text: { r: 0.98, g: 0.98, b: 0.98 },     // #fafafa
  muted: { r: 0.6, g: 0.6, b: 0.65 },      // #999aa6
  white: { r: 1, g: 1, b: 1 }
};
```

### Deteção de Variáveis
```javascript
// Verificar QUAISQUER variáveis, não apenas shadcn
const collections = await figma.variables.getLocalVariableCollectionsAsync();
if (collections.length > 0) {
  // Perguntar ao utilizador qual coleção usar
} else {
  // Usar cores de fallback
}
```

### Cards com Altura Igual
```javascript
// Após criar cards no contentor:
for (const card of container.children) {
  card.layoutAlign = 'STRETCH';           // Preencher altura do contentor
  card.primaryAxisSizingMode = 'FIXED';   // Manter largura fixa
  for (const child of card.children) {
    if (child.name === 'Features') {
      child.layoutGrow = 1;               // Secção de features cresce
    }
  }
}
```

---

## Criar Páginas Web

Cria UM frame pai com auto-layout vertical contendo todas as secções:

```bash
node src/index.js render '<Frame name="Landing Page" w={1440} flex="col" bg="#0a0a0f">
  <Frame name="Hero" w="fill" h={800} flex="col" justify="center" items="center" gap={24} p={80}>
    <Text size={64} weight="bold" color="#fff">Headline</Text>
    <Frame bg="#3b82f6" px={32} py={16} rounded={8}><Text color="#fff">CTA</Text></Frame>
  </Frame>
  <Frame name="Features" w="fill" flex="row" gap={40} p={80} bg="#111">
    <Frame flex="col" gap={12} grow={1}><Text size={24} weight="bold" color="#fff">Feature 1</Text></Frame>
  </Frame>
</Frame>'
```

---

## Slots

A funcionalidade nativa de slots do Figma permite áreas de conteúdo flexíveis em componentes. Os slots permitem que os designers adicionem, removam e reordenem conteúdo em instâncias sem fazer detach.

### Comandos de Slots

```bash
# Criar slot no componente selecionado
node src/index.js slot create "Content" --flex col --gap 8 --padding 16

# Listar slots no componente
node src/index.js slot list
node src/index.js slot list "component-id"

# Definir componentes preferidos para um slot
node src/index.js slot preferred "Slot#1:2" "component-id-1" "component-id-2"

# Reiniciar slot na instância para os valores padrão
node src/index.js slot reset
node src/index.js slot reset "slot-node-id"

# Converter frame em slot (tem de estar dentro de um componente)
node src/index.js slot convert --name "Actions"

# Adicionar conteúdo ao slot numa instância
node src/index.js slot add "slot-id" --component "component-id"
node src/index.js slot add "slot-id" --frame
node src/index.js slot add "slot-id" --text "Hello"
```

### Sintaxe JSX para Slots

Usa `<Slot>` em JSX para criar slots. Quando o pai é um componente, cria um SLOT real. Caso contrário, recorre a um frame.

```jsx
<Frame name="Card" w={300} h={200} bg="#18181b" rounded={12} flex="col" p={16} gap={12}>
  <Text size={18} weight="bold" color="#fff">Card Title</Text>
  <Slot name="Content" flex="col" gap={8} w="fill">
    <Text size={14} color="#a1a1aa">Default slot content</Text>
  </Slot>
</Frame>
```

**Propriedades do Slot:**
- `name` - Nome do slot (mostrado no painel de propriedades)
- `flex` - Direção do layout: "row" ou "col"
- `gap` - Espaçamento entre itens
- `p`, `px`, `py` - Padding
- `w`, `h` - Tamanho ("fill" ou fixo)
- `bg` - Preenchimento de fundo

**Slot auto-fechado (vazio):**
```jsx
<Slot name="Actions" flex="row" gap={8} />
```

### Fluxo de Trabalho com Slots

1. **Criar componente com slot:**
```bash
# Renderizar estrutura do componente
node src/index.js render '<Frame name="Card" ...>
  <Slot name="Content" flex="col" w="fill" />
</Frame>'

# Converter em componente
node src/index.js node to-component "frame-id"
```

2. **Ou adicionar slot a componente existente:**
```bash
# Selecionar componente, depois:
node src/index.js slot create "Content" --flex col --gap 8
```

3. **Definir componentes preferidos:**
```bash
node src/index.js slot preferred "Slot#1:2" "button-comp-id" "icon-comp-id"
```

**CRÍTICO: `isSlot = true` NÃO funciona em eval!**
Definir `frame.isSlot = true` diretamente no código da API do Figma NÃO cria um slot. TENS de usar:
```bash
node src/index.js slot convert "frame-id" --name "SlotName"
```

4. **Nas instâncias, os slots permitem:**
- Adicionar qualquer conteúdo (ou apenas os preferidos, se definidos)
- Reordenar filhos
- Remover filhos
- Reiniciar para os valores padrão com `slot reset`

---

## Sintaxe JSX (comando render)

```jsx
// Layout
flex="row"              // ou "col"
gap={16}                // espaçamento entre itens
p={24}                  // padding em todos os lados
px={16} py={8}          // padding x/y
pt={8} pr={16} pb={8} pl={16}  // padding individual

// Alinhamento
justify="center"        // eixo principal: start, center, end, between
items="center"          // eixo transversal: start, center, end

// Tamanho
w={320} h={200}         // tamanho fixo
w="fill" h="fill"       // preencher o pai
minW={100} maxW={500}   // restrições
minH={50} maxH={300}

// Aparência
bg="#fff"               // cor de preenchimento
bg="var:card"           // ligar a variável (RÁPIDO, ligação inline)
stroke="#000"           // cor do traço
stroke="var:border"     // ligar traço a variável
strokeWidth={2}         // espessura do traço
strokeAlign="inside"    // inside, outside, center
opacity={0.8}           // 0..1
blendMode="multiply"    // multiply, overlay, etc.

// Cantos
rounded={16}            // todos os cantos
roundedTL={8} roundedTR={8} roundedBL={0} roundedBR={0}  // individual
cornerSmoothing={0.6}   // squircle iOS (0..1)

// Efeitos
shadow="4px 4px 12px rgba(0,0,0,0.25)"  // sombra projetada
blur={8}                // desfoque de camada
overflow="hidden"       // recortar conteúdo
rotate={45}             // graus de rotação

// Texto
<Text size={18} weight="bold" color="#000" font="Inter">Hello</Text>
<Text color="var:foreground">Text with variable color</Text>

// Ícones (Lucide via Iconify API - nós SVG reais, não placeholders)
<Icon name="lucide:chevron-left" size={16} color="#fff" />
<Icon name="lucide:check" size={14} color="var:primary-foreground" />
// Qualquer ícone Lucide: lucide:plus, lucide:x, lucide:search, lucide:settings, etc.
// Lista completa: https://lucide.dev/icons
```

### Ligação Rápida de Variáveis (sintaxe var:)

Usa a sintaxe `var:name` para ligar variáveis diretamente no momento da criação (RÁPIDO, sem necessidade de comandos bind separados):

```jsx
// Frame com preenchimento e traço ligados
<Frame bg="var:card" stroke="var:border">
  <Text color="var:foreground">Bound text</Text>
  <Frame bg="var:primary">
    <Text color="var:primary-foreground">Button</Text>
  </Frame>
</Frame>
```

**Variáveis shadcn disponíveis:**
- `background`, `foreground` (fundo/texto da página)
- `card`, `card-foreground` (fundos de cards)
- `primary`, `primary-foreground` (botões, acentos)
- `secondary`, `secondary-foreground`
- `muted`, `muted-foreground` (texto subtil)
- `accent`, `accent-foreground`
- `border`, `input`, `ring`

**Vantagens sobre comandos `bind` separados:**
- Uma única chamada render liga todas as variáveis de uma vez
- Sem timeouts ou múltiplas chamadas à API
- Funciona com estruturas aninhadas complexas

**Também funciona com comandos `set`:**
```bash
node src/index.js set fill "var:primary"    # Ligar preenchimento a elemento existente
node src/index.js set stroke "var:border"   # Ligar traço a elemento existente
```

### Auto-Layout

```jsx
// Wrap: itens fluem para a próxima linha quando cheio
wrap={true}             // layoutWrap = 'WRAP'
rowGap={12}             // espaçamento entre linhas (counterAxisSpacing)

// Grow: expandir para preencher espaço restante
grow={1}                // layoutGrow = 1

// Stretch: preencher eixo transversal
stretch={true}          // layoutAlign = 'STRETCH'

// Absolute: posicionar livremente dentro do pai
position="absolute" x={12} y={12}  // precisa de name para x/y funcionar
```

**Exemplo completo:**
```bash
node src/index.js render '<Frame name="Card" w={300} flex="col" bg="#18181b" rounded={12} overflow="hidden">
  <Frame w="fill" h={100} bg="#333" />
  <Frame name="Badge" w={40} h={20} bg="#ef4444" rounded={4} position="absolute" x={12} y={12} />
  <Frame name="Tags" flex="row" wrap={true} rowGap={8} gap={8} p={16}>
    <Frame w={60} h={24} bg="#3b82f6" rounded={12} />
    <Frame w={70} h={24} bg="#22c55e" rounded={12} />
    <Frame w={80} h={24} bg="#a855f7" rounded={12} />
  </Frame>
  <Frame flex="row" p={16} gap={8}>
    <Frame w={40} h="fill" bg="#222" />
    <Frame h="fill" bg="#333" grow={1} />
  </Frame>
</Frame>'
```

**Erros comuns (ignorados silenciosamente, sem erro!):**
```
ERRADO                   CORRETO
layout="horizontal"   →  flex="row"
padding={24}          →  p={24}
fill="#fff"           →  bg="#fff"
cornerRadius={12}     →  rounded={12}
fontSize={18}         →  size={18}
fontWeight="bold"     →  weight="bold"
justify="between"     →  usar grow={1} como espaçador
```

### Padrões de Layout

**Empurrar itens para as extremidades (padrão navbar):**
```jsx
// justify="between" não funciona de forma fiável, usar grow como espaçador
<Frame flex="row" items="center">
  <Frame>Logo</Frame>
  <Frame grow={1} justify="center">Nav Links</Frame>
  <Frame>Buttons</Frame>
</Frame>
```

**Badge no canto do avatar:**
```jsx
// x/y absoluto é relativo ao padding do pai
// Avatar com padding=24, tamanho=100, badge=20
// Posição: padding + tamanhoAvatar - tamanhoBadge/2 = 24 + 100 - 10 = 114
<Frame p={24}>
  <Frame w={100} h={100} rounded={50} />
  <Frame name="Badge" w={20} h={20} position="absolute" x={114} y={114} />
</Frame>
```

**Input no fundo (padrão chat):**
```jsx
<Frame flex="col" h={400}>
  <Frame>Message 1</Frame>
  <Frame>Message 2</Frame>
  <Frame grow={1} />
  <Frame>Input field</Frame>
</Frame>
```

**Evitar overflow de conteúdo:**
```jsx
// MAU: altura fixa demasiado pequena para filhos com tamanho automático
<Frame h={160} p={24}><Frame h={139} /></Frame>  // 139+48 > 160!

// BOM: garantir que a altura cabe no conteúdo + padding
<Frame h={200} p={24}><Frame h={139} /></Frame>  // 139+48 < 200 ✓
```

**Exemplo completo de card:**
```bash
node src/index.js render '<Frame name="Card" w={320} h={200} bg="#18181b" rounded={12} flex="col" p={24} gap={12}>
  <Text size={18} weight="bold" color="#fff">Title</Text>
  <Text size={14} color="#a1a1aa" w="fill">Description text</Text>
  <Frame bg="#3b82f6" px={16} py={8} rounded={6}>
    <Text size={14} weight="medium" color="#fff">Button</Text>
  </Frame>
</Frame>'
```

### Erros Comuns

**1. Texto cortado (CRÍTICO):**
```jsx
// MAU: Texto sem w="fill" fica numa única linha e é cortado
<Frame flex="col" gap={8}>
  <Text size={16} weight="semibold" color="#fff">Title cut off</Text>
  <Text size={14} color="#a1a1aa">Description also cut off...</Text>
</Frame>

// BOM: Adicionar w="fill" ao Frame pai E A TODOS os elementos Text
<Frame flex="col" gap={8} w="fill">
  <Text size={16} weight="semibold" color="#fff" w="fill">Title wraps properly</Text>
  <Text size={14} color="#a1a1aa" w="fill">Description wraps properly.</Text>
</Frame>
```
**Regra:** Para o texto quebrar linha, precisas de:
1. Frame pai com `w="fill"` ou largura fixa
2. **TODOS** os elementos Text precisam de `w="fill"` (não apenas descrições!)
3. O pai tem de ter `flex="col"` ou `flex="row"`

**IMPORTANTE:** TODO o texto que possa quebrar linha precisa de `w="fill"`:
- Títulos (ex: "Wireless Noise-Canceling Headphones")
- Descrições
- Labels
- Qualquer texto com várias palavras

**Exemplo real - card com título E descrição:**
```jsx
<Frame name="Card" w={340} bg="#18181b" rounded={16} flex="col" p={20} gap={16}>
  <Frame flex="col" gap={8} w="fill">
    <Text size={16} weight="semibold" color="#fff" w="fill">Wireless Noise-Canceling Headphones</Text>
    <Text size={14} color="#a1a1aa" w="fill">Premium audio experience with 40-hour battery life.</Text>
  </Frame>
</Frame>
```

**2. Toggle switches - usar flex, não absolute:**
```jsx
// MAU: Posicionamento absoluto para o botão
<Frame w={52} h={28} bg="#3b82f6" rounded={14} p={2}>
  <Frame w={24} h={24} bg="#fff" rounded={12} position="absolute" x={26} y={2} />
</Frame>

// BOM: Flex com justify para estado ON/OFF
// Estado ON (botão à direita)
<Frame w={52} h={28} bg="#3b82f6" rounded={14} flex="row" items="center" p={2} justify="end">
  <Frame w={24} h={24} bg="#fff" rounded={12} />
</Frame>
// Estado OFF (botão à esquerda)
<Frame w={52} h={28} bg="#27272a" rounded={14} flex="row" items="center" p={2} justify="start">
  <Frame w={24} h={24} bg="#52525b" rounded={12} />
</Frame>
```

**3. Botões precisam de flex + largura fixa para texto centrado:**
```jsx
// MAU: Sem flex, texto não centrado
<Frame bg="#3b82f6" px={16} py={10} rounded={10}>
  <Text>Button</Text>
</Frame>

// BOM: Flex centra o conteúdo
<Frame bg="#3b82f6" px={16} py={10} rounded={10} flex="row" justify="center" items="center">
  <Text>Button</Text>
</Frame>

// MELHOR (para componentes): Largura fixa + auto-layout + texto preenche
<Frame w={100} h={40} bg="#3b82f6" rounded={8} flex="row" justify="center" items="center" px={16} py={10}>
  <Text color="#fff" w="fill" align="center">Button</Text>
</Frame>
```

**Padrão de componente botão (para variantes):**
```javascript
// Ao criar componentes de botão programaticamente:
frame.layoutMode = "HORIZONTAL";
frame.primaryAxisSizingMode = "FIXED";    // Manter largura fixa
frame.counterAxisSizingMode = "FIXED";    // Manter altura fixa
frame.resize(100, 40);                     // Definir tamanho APÓS layout mode
frame.primaryAxisAlignItems = "CENTER";
frame.counterAxisAlignItems = "CENTER";
frame.paddingLeft = frame.paddingRight = 16;
frame.paddingTop = frame.paddingBottom = 10;

// Texto dentro do botão
text.textAlignHorizontal = "CENTER";
text.layoutAlign = "STRETCH";              // Preencher largura disponível
text.layoutGrow = 1;                       // Crescer para preencher
```

**4. Sem emojis - usar ícones Lucide reais ou formas:**
```jsx
// MAU: Emojis renderizam de forma inconsistente
<Text>🏠</Text>

// MELHOR: Usar ícones Lucide reais (obtidos como SVG da Iconify API)
<Icon name="lucide:home" size={20} color="#fff" />
<Icon name="lucide:settings" size={20} color="var:foreground" />

// OK: Usar formas como placeholders de ícones
<Frame w={20} h={20} rounded={4} stroke="#fff" strokeWidth={2} />  // ícone quadrado
<Frame w={20} h={20} rounded={10} stroke="#fff" strokeWidth={2} /> // ícone circular
```

**5. Ícone de menu três pontos:**
```jsx
<Frame flex="row" gap={3} justify="center" items="center">
  <Frame w={4} h={4} bg="#52525b" rounded={2} />
  <Frame w={4} h={4} bg="#52525b" rounded={2} />
  <Frame w={4} h={4} bg="#52525b" rounded={2} />
</Frame>
```

**6. Classificação por estrelas com formas:**
```jsx
<Frame flex="row" gap={4}>
  <Frame w={14} h={14} bg="#fbbf24" rounded={2} />
  <Frame w={14} h={14} bg="#fbbf24" rounded={2} />
  <Frame w={14} h={14} bg="#fbbf24" rounded={2} />
  <Frame w={14} h={14} bg="#fbbf24" rounded={2} />
  <Frame w={14} h={14} bg="#fbbf24" rounded={2} />
</Frame>
```

---

## Regras Importantes

1. **Usa sempre `render` para frames** - tem posicionamento inteligente
2. **Nunca uses `eval` para criar** - sem posicionamento, sobrepõe-se em (0,0)
3. **Nunca uses `npx figma-use render`** - sem posicionamento inteligente
4. **Para múltiplos frames:** Usa `render-batch`
5. **Converter em componentes:** `node to-component` após a criação

---

## Onboarding ("Iniciar Projeto")

**Nunca mostres comandos de terminal aos utilizadores.** Executa silenciosamente, dá feedback amigável.

1. Executar `npm install` silenciosamente
2. Perguntar o modo de conexão (Yolo ou Safe)
3. Executar `node src/index.js connect` (ou `--safe`)
4. Quando ligado, dizer: "Ligado! O que gostarias de criar?"

Se erro de permissão (macOS): Definições do Sistema → Privacidade → Acesso Total ao Disco → Adicionar Terminal

---

## Visualização de Variáveis

"Mostra as cores no canvas" / "mostra as variáveis" / "cria paleta":
```bash
node src/index.js var visualize              # Todas as coleções
node src/index.js var visualize "primitives" # Filtrar
```

Cria amostras de cor estilo shadcn ligadas às variáveis.

---

## Recriar Websites

```bash
node src/index.js recreate-url "https://example.com" --name "Page"
node src/index.js screenshot-url "https://example.com"
```

---

## Speed Daemon

O `connect` inicia automaticamente o daemon para comandos 10x mais rápidos.

```bash
node src/index.js daemon status
node src/index.js daemon restart
```
