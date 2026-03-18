# Referencia de Comandos claudio-cli

Referencia completa de comandos para o CLI do Figma. Para inicio rapido, consulta o CLAUDE.md.

## Tokens de Design e Variaveis

### Criar Sistemas de Design

```bash
node src/index.js tokens ds              # Cores base IDS
node src/index.js tokens tailwind        # 22 familias de cores Tailwind (242 vars)
node src/index.js tokens spacing         # Tokens de espacamento
```

### Gerir Variaveis

```bash
node src/index.js var list               # Mostrar todas as variaveis
node src/index.js var list -t COLOR      # Filtrar por tipo
node src/index.js var visualize          # Mostrar cores no canvas
node src/index.js var create "name" -c "ColId" -t COLOR -v "#3b82f6"
```

### Associar Variaveis

```bash
node src/index.js bind fill "primary/500"
node src/index.js bind stroke "border/default"
node src/index.js bind radius "radius/md"
node src/index.js bind gap "spacing/md"
node src/index.js bind padding "spacing/lg"
node src/index.js bind list              # Listar variaveis disponiveis
```

## Criar Elementos

### Primitivos Rapidos

```bash
node src/index.js create rect "Card" -w 320 -h 200 --fill "#fff" --radius 12
node src/index.js create circle "Avatar" -w 48 --fill "#3b82f6"
node src/index.js create text "Hello" -s 24 -c "#000" -w bold
node src/index.js create line -l 200 -c "#e4e4e7"
node src/index.js create autolayout "Card" -d col -g 16 -p 24 --fill "#fff"
node src/index.js create icon lucide:star -s 24 -c "#f59e0b"
node src/index.js create image "https://example.com/photo.png" -w 200
node src/index.js create group "Header"
node src/index.js create component "Button"
```

### Criar com Associacao de Variaveis (Rapido)

Usa a sintaxe `var:name` para associar variaveis shadcn no momento da criacao:

```bash
node src/index.js create rect "Card" --fill "var:card" --stroke "var:border"
node src/index.js create circle "Avatar" --fill "var:primary"
node src/index.js create text "Hello" -c "var:foreground"
node src/index.js create line -c "var:border"
node src/index.js create frame "Section" --fill "var:background"
node src/index.js create autolayout "Container" --fill "var:muted"
node src/index.js create icon lucide:star -c "var:primary"
```

### Render com JSX

```bash
node src/index.js render '<Frame name="Card" w={320} h={180} bg="#fff" rounded={16} flex="col" gap={8} p={24}>
  <Text size={20} weight="bold" color="#111">Title</Text>
  <Text size={14} color="#666" w="fill">Description</Text>
</Frame>'
```

### Render com Associacao de Variaveis (Rapido)

Usa a sintaxe `var:name` para associar variaveis shadcn no momento da criacao (sem necessidade de comandos bind separados):

```bash
node src/index.js render '<Frame name="Card" w={320} h={180} bg="var:card" stroke="var:border" rounded={16} flex="col" gap={8} p={24}>
  <Text size={20} weight="bold" color="var:foreground">Title</Text>
  <Text size={14} color="var:muted-foreground" w="fill">Description</Text>
  <Frame bg="var:primary" px={16} py={8} rounded={8}>
    <Text color="var:primary-foreground">Button</Text>
  </Frame>
</Frame>'
```

Variaveis: `background`, `foreground`, `card`, `primary`, `secondary`, `muted`, `accent`, `border`, e as suas variantes `-foreground`.

### Render Batch (Multiplos Frames)

```bash
node src/index.js render-batch '[
  "<Frame name=\"Card 1\" w={300} h={200} bg=\"#fff\"><Text>Card 1</Text></Frame>",
  "<Frame name=\"Card 2\" w={300} h={200} bg=\"#fff\"><Text>Card 2</Text></Frame>"
]' -d row -g 40
```

Opcoes: `-d row|col` (direcao), `-g <n>` (espacamento)

## Modificar Elementos

```bash
node src/index.js set fill "#3b82f6"           # Alterar preenchimento (hex)
node src/index.js set fill "var:primary"       # Associar preenchimento a variavel (rapido)
node src/index.js set fill "#3b82f6" -n "1:234" # Num no especifico
node src/index.js set stroke "#e4e4e7" -w 1    # Adicionar contorno (hex)
node src/index.js set stroke "var:border"      # Associar contorno a variavel
node src/index.js set radius 12                # Raio dos cantos
node src/index.js set size 320 200             # Redimensionar
node src/index.js set pos 100 100              # Mover
node src/index.js set opacity 0.5              # Opacidade
node src/index.js set autolayout row -g 8 -p 16 # Aplicar auto-layout
node src/index.js set name "Header"            # Renomear
```

## Layout e Dimensionamento

```bash
node src/index.js sizing hug                   # Ajustar ao conteudo
node src/index.js sizing fill                  # Preencher contentor
node src/index.js sizing fixed 320 200         # Tamanho fixo
node src/index.js padding 16                   # Todos os lados
node src/index.js padding 16 24                # Vertical, horizontal
node src/index.js gap 16                       # Definir espacamento
node src/index.js align center                 # Alinhar elementos
```

## Encontrar e Selecionar

```bash
node src/index.js find "Button"                # Encontrar por nome
node src/index.js find "Card" -t FRAME         # Filtrar por tipo
node src/index.js select "1:234"               # Selecionar no
node src/index.js get                          # Obter propriedades da selecao
node src/index.js get "1:234"                  # Obter no especifico
```

## Operacoes de Canvas

```bash
node src/index.js canvas info                  # O que esta no canvas
node src/index.js canvas next                  # Proxima posicao livre
node src/index.js arrange -g 100               # Organizar frames
node src/index.js arrange -g 100 -c 3          # 3 colunas
```

## Duplicar e Eliminar

```bash
node src/index.js duplicate                    # Duplicar selecao
node src/index.js dup "1:234" --offset 50      # Com desvio
node src/index.js delete                       # Eliminar selecao
node src/index.js delete "1:234"               # Eliminar por ID
```

## Operacoes de Nos

```bash
node src/index.js node tree                    # Mostrar estrutura em arvore
node src/index.js node tree "1:234" -d 5       # Maior profundidade
node src/index.js node bindings                # Mostrar associacoes de variaveis
node src/index.js node to-component "1:234"    # Converter em componente
node src/index.js node delete "1:234"          # Eliminar por ID
```

## Slots

Funcionalidade nativa de slots do Figma para areas de conteudo flexiveis em componentes.

```bash
node src/index.js slot create "Content"        # Criar slot no componente
node src/index.js slot create "Actions" --flex row --gap 8 --padding 16
node src/index.js slot list                    # Listar slots no componente
node src/index.js slot list "1:234"            # Listar por ID do componente
node src/index.js slot preferred "Slot#1:2" "comp-id-1" "comp-id-2"  # Definir preferidos
node src/index.js slot reset                   # Repor slot para valores predefinidos
node src/index.js slot add "slot-id" --component "comp-id"  # Adicionar ao slot
node src/index.js slot add "slot-id" --frame   # Adicionar frame vazio
node src/index.js slot add "slot-id" --text "Hello"  # Adicionar texto
```

## Exportar

```bash
node src/index.js export css                   # Variaveis como CSS
node src/index.js export tailwind              # Configuracao Tailwind
node src/index.js export screenshot -o out.png # Captura de ecra (selecao ou pagina)
node src/index.js export screenshot -s 2 -f png # PNG com escala 2x
node src/index.js export screenshot -f svg     # Formato SVG
node src/index.js export node "1:234" -o card.png         # Exportar no por ID
node src/index.js export node "1:234" -s 2 -f png         # PNG com escala 2x
node src/index.js export node "1:234" -f svg -o card.svg  # Exportar SVG
node src/index.js export-jsx "1:234"           # Exportar como JSX
node src/index.js export-jsx "1:234" -o Card.jsx --pretty
node src/index.js export-storybook "1:234"     # Historias Storybook
```

## Analise e Linting

```bash
node src/index.js lint                         # Verificar todas as regras
node src/index.js lint --fix                   # Corrigir automaticamente
node src/index.js lint --rule color-contrast   # Regra especifica
node src/index.js lint --preset accessibility  # Usar preset
node src/index.js analyze colors               # Utilizacao de cores
node src/index.js analyze typography           # Tipografia
node src/index.js analyze spacing              # Espacamento
node src/index.js analyze clusters             # Encontrar padroes
```

Regras de lint: `no-default-names`, `no-deeply-nested`, `no-empty-frames`, `prefer-auto-layout`, `no-hardcoded-colors`, `color-contrast`, `touch-target-size`, `min-text-size`

Presets: `recommended`, `strict`, `accessibility`, `design-system`

## Consultas XPath

```bash
node src/index.js raw query "//FRAME"
node src/index.js raw query "//COMPONENT"
node src/index.js raw query "//*[contains(@name, 'Button')]"
node src/index.js raw select "1:234"
node src/index.js raw export "1:234" --scale 2
```

## Recreacao de Websites

```bash
node src/index.js recreate-url "https://example.com" --name "My Page"
node src/index.js recreate-url "https://example.com" -w 375 -h 812  # Telemovel
node src/index.js analyze-url "https://example.com" --screenshot
node src/index.js screenshot-url "https://example.com" --full
```

## Imagens

```bash
node src/index.js create image "https://example.com/photo.png"
node src/index.js screenshot-url "https://example.com"
node src/index.js remove-bg                    # Remover fundo (necessita chave API)
```

## FigJam

```bash
node src/index.js fj list                      # Listar paginas
node src/index.js fj sticky "Text" -x 100 -y 100 --color "#FEF08A"
node src/index.js fj shape "Label" -x 200 -y 100 -w 200 -h 100
node src/index.js fj connect "ID1" "ID2"       # Ligar elementos
node src/index.js fj nodes                     # Mostrar elementos
node src/index.js fj delete "ID"
node src/index.js fj eval "figma.currentPage.children.length"
```

Tipos de forma: `ROUNDED_RECTANGLE`, `RECTANGLE`, `ELLIPSE`, `DIAMOND`, `TRIANGLE_UP`, `TRIANGLE_DOWN`, `PARALLELOGRAM_RIGHT`, `PARALLELOGRAM_LEFT`

## Daemon e Conexao

```bash
node src/index.js connect                  # Ligar (Modo Yolo)
node src/index.js connect --safe           # Ligar (Modo Seguro, plugin)
node src/index.js daemon status            # Verificar estado do daemon
node src/index.js daemon status --debug    # Informacao detalhada de token e conexao
node src/index.js daemon diagnose          # Diagnostico completo (resolucao de problemas)
node src/index.js daemon start             # Iniciar daemon manualmente
node src/index.js daemon start --force     # Forcar reinicio
node src/index.js daemon restart           # Reiniciar com token novo
node src/index.js daemon stop              # Parar daemon
node src/index.js daemon reconnect         # Reconectar ao Figma
node src/index.js files                    # Listar ficheiros Figma abertos (JSON)
```

### Resolucao de Erros de Autenticacao

Se vires "Unauthorized: Invalid or missing token":

```bash
node src/index.js daemon diagnose          # Ver qual e o problema
node src/index.js daemon restart           # Normalmente resolve
```

Localizacao do ficheiro de token: `~/.figma-ds-cli/.daemon-token`

## Combinacoes de Componentes (combos)

Gerar todas as combinacoes de variantes como componentes individuais:

```bash
node src/index.js combos                   # Usar selecao
node src/index.js combos "1:234"           # Por ID do no
node src/index.js combos --dry-run         # Pre-visualizar sem criar
node src/index.js combos --gap 60          # Espacamento personalizado entre componentes
node src/index.js combos --no-boolean      # Excluir propriedades booleanas
```

**Como funciona:**
1. Seleciona um conjunto de componentes (ou qualquer variante/instancia)
2. Executa `combos` para gerar todas as combinacoes
3. Cria **componentes individuais** diretamente no canvas (sem frame contentor)
4. Cada componente nomeado: `Button/Small/Default`, `Button/Small/Hover`, etc.
5. Organizados numa grelha (ultima propriedade = colunas, restantes = linhas)
6. Etiquetas de linha/coluna adicionadas automaticamente (usa `--no-labels` para omitir)

## Variantes de Tamanho (sizes)

Gerar variantes Small/Medium/Large a partir de um unico componente:

```bash
node src/index.js sizes                       # Usar selecao
node src/index.js sizes "1:234"               # Por ID do no
node src/index.js sizes --base small          # Origem e tamanho Small
node src/index.js sizes --base large          # Origem e tamanho Large
node src/index.js sizes --gap 60              # Espacamento personalizado
```

**Como funciona:**
1. Seleciona um componente ou frame
2. Executa `sizes --base <size>` para especificar qual o tamanho de origem
3. Cria variantes Small, Medium, Large com escala proporcional
4. Escala: dimensoes, tamanhos de fonte, padding, raio dos cantos, espacamentos

## JavaScript Eval

```bash
node src/index.js eval "figma.currentPage.name"
node src/index.js eval --file /tmp/script.js
node src/index.js run /tmp/script.js
```

## Sintaxe JSX do Render

**Elementos:** `<Frame>`, `<Rectangle>`, `<Ellipse>`, `<Text>`, `<Line>`, `<Image>`, `<SVG>`, `<Icon>`

**Tamanho:** `w={320} h={200}`, `w="fill"`, `minW={100} maxW={500}`

**Layout:** `flex="row|col"`, `gap={16}`, `wrap={true}`, `justify="start|center|end|between"`, `items="start|center|end"`

**Padding:** `p={24}`, `px={16} py={8}`, `pt={8} pr={16} pb={8} pl={16}`

**Aparencia:** `bg="#fff"`, `stroke="#000"`, `strokeWidth={1}`, `opacity={0.5}`

**Cantos:** `rounded={16}`, `roundedTL={8}`, `overflow="hidden"`

**Efeitos:** `shadow="0 4 12 #0001"`, `blur={10}`, `rotate={45}`

**Texto:** `<Text size={18} weight="bold" color="#000" font="Inter">Hello</Text>`

**ERRADO vs CORRETO:**
```
layout="horizontal"  →  flex="row"
padding={24}         →  p={24}
fill="#fff"          →  bg="#fff"
cornerRadius={12}    →  rounded={12}
```

## Exemplos Avancados

### Mudar para Modo Escuro
```javascript
node src/index.js eval "
const node = figma.getNodeById('1:234');
function findModeCollection(n) {
  if (n.boundVariables) {
    for (const [prop, binding] of Object.entries(n.boundVariables)) {
      const b = Array.isArray(binding) ? binding[0] : binding;
      if (b && b.id) {
        const variable = figma.variables.getVariableById(b.id);
        if (variable) {
          const col = figma.variables.getVariableCollectionById(variable.variableCollectionId);
          if (col && col.modes.length > 1) return { col, modes: col.modes };
        }
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
  const darkMode = found.modes.find(m => m.name.includes('Dark'));
  if (darkMode) node.setExplicitVariableModeForCollection(found.col, darkMode.modeId);
}
"
```

### Criar Instancia de Componente
```javascript
node src/index.js eval "(function() {
  const comp = figma.currentPage.findOne(n => n.type === 'COMPONENT' && n.name === 'Button');
  if (!comp) return 'Componente nao encontrado';
  const instance = comp.createInstance();
  instance.x = 100;
  instance.y = 100;
  return instance.id;
})()"
```

### Posicionamento Inteligente
```javascript
let smartX = 0;
figma.currentPage.children.forEach(n => { smartX = Math.max(smartX, n.x + n.width); });
smartX += 100;
const frame = figma.createFrame();
frame.x = smartX;
```

## Modo Seguro

O Modo Seguro usa uma conexao baseada em plugin em vez de CDP (Chrome DevTools Protocol). Usa-o quando:
- MacBook da empresa com definicoes de privacidade restritas
- Permissao Full Disk Access nao disponivel
- Preferencia por nao modificar o Figma

### Conexao
```bash
node src/index.js connect --safe
```

Depois no Figma: Plugins → Development → FigCli

### Diferencas em Relacao ao Modo Yolo

| Funcionalidade | Modo Yolo | Modo Seguro |
|----------------|-----------|-------------|
| Conexao | CDP direto | Bridge via plugin |
| Configuracao | Modifica o Figma uma vez | Iniciar plugin em cada sessao |
| Velocidade | ~10x mais rapido | Normal |
| Timeout | 60 segundos | 60 segundos |

### Suporte de Comandos

Todos os comandos funcionam em ambos os modos. No Modo Seguro, os comandos usam a API nativa do Figma em vez de figma-use:

| Comando | Modo Yolo | Modo Seguro |
|---------|-----------|-------------|
| `render` | figma-use | daemon (API nativa) |
| `render-batch` | figma-use | daemon (API nativa) |
| `node to-component` | figma-use | API nativa |
| `node delete` | figma-use | API nativa |
| `node tree` | figma-use | API nativa |
| `node bindings` | figma-use | API nativa |
| `lint` | figma-use | API nativa |
| `analyze colors/typography/spacing/clusters` | figma-use | API nativa |
| `export-jsx` | figma-use | API nativa |
| `export-storybook` | figma-use | API nativa |
| Todos os outros comandos | daemon | daemon |

### Dicas para o Modo Seguro

1. **Manter payloads mais pequenos**: Dividir ecras complexos em multiplas chamadas `render`
2. **Todos os comandos funcionam**: As implementacoes nativas correspondem a funcionalidade do figma-use
3. **Timeout**: Ambos os modos tem agora 60s de timeout

### Quando o render-batch falha

Se o `render-batch` exceder o timeout com JSX complexo, divide-o:

```bash
# Em vez de um batch grande
node src/index.js render-batch '[array enorme]'

# Usa multiplos batches mais pequenos
node src/index.js render '<Frame>...</Frame>'
node src/index.js render '<Frame>...</Frame>'
```

Ou usa `eval` com a API nativa do Figma para maximo controlo (ve "Complex Components" no CLAUDE.md).
