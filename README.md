# claudio-cli

<p align="center">
  <a href="https://github.com/WakandaDS/claudio-cli"><img src="https://img.shields.io/badge/WakandaDS-bruno--figma--cli-ff6b35" alt="WakandaDS"></a>
  <img src="https://img.shields.io/badge/Figma-Desktop-purple" alt="Figma Desktop">
  <img src="https://img.shields.io/badge/No_API_Key-Required-green" alt="No API Key">
  <img src="https://img.shields.io/badge/Claude_Code-Ready-blue" alt="Claude Code">
</p>

<p align="center">
  <b>Controla o Figma Desktop com o Claude Code.</b><br>
  Acesso completo de leitura/escrita. Sem necessidade de API key.<br>
  Basta falar com o Claude sobre os teus designs.
</p>

```
  ███████╗██╗ ██████╗ ███╗   ███╗ █████╗       ██████╗ ███████╗       ██████╗██╗     ██╗
  ██╔════╝██║██╔════╝ ████╗ ████║██╔══██╗      ██╔══██╗██╔════╝      ██╔════╝██║     ██║
  █████╗  ██║██║  ███╗██╔████╔██║███████║█████╗██║  ██║███████╗█████╗██║     ██║     ██║
  ██╔══╝  ██║██║   ██║██║╚██╔╝██║██╔══██║╚════╝██║  ██║╚════██║╚════╝██║     ██║     ██║
  ██║     ██║╚██████╔╝██║ ╚═╝ ██║██║  ██║      ██████╔╝███████║      ╚██████╗███████╗██║
  ╚═╝     ╚═╝ ╚═════╝ ╚═╝     ╚═╝╚═╝  ╚═╝      ╚═════╝ ╚══════╝       ╚═════╝╚══════╝╚═╝
```

## O que é isto?

Uma CLI que se liga diretamente ao Figma Desktop e te dá controlo total:

- **Componentes shadcn/ui** — Gera todos os 30 componentes oficiais shadcn com ícones Lucide reais e binding de variáveis
- **Design Tokens** — Cria variáveis, coleções, modos (Light/Dark), liga a nós
- **Cria qualquer coisa** — Frames, texto, formas, ícones (150k+ do Iconify), componentes
- **Slots** — Cria e gere a funcionalidade de Slots do Figma para conteúdo flexível em componentes
- **Team Libraries** — Importa e usa componentes, estilos e variáveis de qualquer biblioteca
- **Analisa designs** — Cores, tipografia, espaçamentos, encontra padrões repetidos
- **Lint e Acessibilidade** — Verificação de contraste, alvos tácteis, regras de design
- **Exportação** — PNG, SVG, JSX, Storybook stories, variáveis CSS, configuração Tailwind
- **Operações em lote** — Renomeia layers, encontra/substitui texto, cria 100 variáveis de uma vez
- **Funciona com o Claude Code** — Basta pedir em linguagem natural, o Claude conhece todos os comandos

---

## Pacote de Componentes shadcn/ui

Gera componentes shadcn/ui prontos para produção diretamente no Figma. Todos os 30 componentes com 58 variantes, seguindo as especificações oficiais do shadcn/ui.

### Início Rápido

```bash
# 1. Adicionar design tokens shadcn (modo Light/Dark)
node src/index.js tokens preset shadcn

# 2. Gerar todos os componentes
node src/index.js shadcn add --all

# Ou escolher componentes específicos
node src/index.js shadcn add button card input tabs

# Listar componentes disponíveis
node src/index.js shadcn list
```

### O que se obtém

**30 componentes, 58 variantes:**

| Componente | Variantes |
|-----------|----------|
| Button | Default, Secondary, Destructive, Outline, Ghost, Link, Small, Large, Icon |
| Badge | Default, Secondary, Destructive, Outline |
| Card | Card completo com Header, Content, Footer |
| Input | Default, Filled, With Label |
| Textarea | Default |
| Label | Default |
| Alert | Default (ícone info), Destructive (ícone alert) |
| Avatar | Default, Small |
| Switch | On, Off |
| Separator | Horizontal, Vertical |
| Skeleton | Text, Circle, Card |
| Progress | 60%, 30% |
| Toggle | Default, Active |
| Checkbox | Unchecked, Checked (com ícone check) |
| Tabs | Tabs completos com painel de conteúdo |
| Table | Cabeçalho + 3 linhas |
| Radio Group | Unchecked, Checked, Full group |
| Select | Closed, Filled, Open (com dropdown + ícone check) |
| Slider | Com thumb |
| Breadcrumb | Com separadores chevron |
| Pagination | Com ícones chevron + ellipsis |
| Kbd | Tecla única, Combinação de teclas |
| Spinner | Small, Medium |
| Tooltip | Tooltip + trigger |
| Dialog | Com botão de fechar, campos de formulário |
| Dropdown Menu | Com itens + separador |
| Accordion | Itens abertos + fechados |
| Navigation Menu | Itens ativos + inativos |
| Sheet | Painel lateral com formulário |
| Hover Card | Card de perfil |

### Ícones Lucide Reais

Os componentes utilizam ícones SVG Lucide reais (não formas de placeholder), obtidos da API Iconify e renderizados como nós vetoriais no Figma:

- **Pagination**: chevron-left, chevron-right, ellipsis
- **Select**: chevron-down, chevron-up, check
- **Accordion**: chevron-down, chevron-right
- **Checkbox**: check
- **Dialog/Sheet**: x (botão fechar)
- **Alert**: info, alert-circle
- **Button/Icon**: plus
- **Toggle**: bold
- **Breadcrumb**: chevron-right
- **Navigation Menu**: chevron-down

### Integração com Design Tokens

Todos os componentes usam a sintaxe `var:` para fazer binding direto às variáveis shadcn. Quando adicionas tokens com `tokens preset shadcn`, os componentes utilizam automaticamente as tuas cores Light/Dark:

- `background`, `foreground` — fundo da página/texto
- `card`, `card-foreground` — fundos de cards
- `primary`, `primary-foreground` — botões, destaques
- `secondary`, `secondary-foreground` — ações secundárias
- `muted`, `muted-foreground` — texto subtil, estados desativados
- `accent`, `accent-foreground` — estados de hover
- `destructive`, `destructive-foreground` — estados de erro
- `border`, `input`, `ring` — bordas, inputs, anéis de foco

---

## Porquê esta CLI?

Este projeto inclui um ficheiro `CLAUDE.md` que o Claude lê automaticamente. Contém:

- Todos os comandos disponíveis e a sua sintaxe
- Boas práticas (ex: "usar `render` para designs com muito texto")
- Pedidos comuns mapeados para soluções

**Queres ensinar novos truques ao Claude?** Basta atualizar o `CLAUDE.md`. Sem necessidade de alterar código.

**Exemplo:** Escreves "Criar cores Tailwind" -> O Claude já sabe executar `node src/index.js tokens tailwind` porque está documentado no `CLAUDE.md`.

---

## O que precisas

- **Node.js 18+** — `brew install node` (ou [descarregar](https://nodejs.org/))
- **Figma Desktop** (conta gratuita funciona)
- **Claude Code** ([obtém aqui](https://www.anthropic.com/claude-code))
- **macOS ou Windows** (macOS recomendado, Windows suportado)
- **macOS Full Disk Access** para o Terminal (apenas Yolo Mode -- não necessário para [Safe Mode](#-safe-mode--para-ambientes-restritos))

---

## Instalação

```bash
git clone https://github.com/WakandaDS/claudio-cli.git
cd claudio-cli
npm install
npm run setup-alias
source ~/.zshrc
```

É só isto. Agora abre um **novo terminal** e escreve:

```bash
fig-start
```

Isto irá:
1. Iniciar o Figma (se não estiver a correr)
2. Ligar ao Figma (Yolo Mode: faz patch ao Figma uma vez para acesso direto)
3. Mostrar os teus ficheiros Figma abertos: escolhe um com as setas
4. Lançar o Claude Code com todos os comandos pré-carregados

**Pronto.** Fala com o Claude sobre o teu ficheiro Figma.

> **Nota:** `fig-start` funciona a partir de qualquer diretório. O script de setup guarda a localização do repositório em `~/.figma-cli/config.json`.

### Opções do fig-start

| Comando | Descrição |
|---------|-----------|
| `fig-start` | Yolo Mode (predefinido), seleção interativa de ficheiro |
| `fig-start --safe` | Safe Mode (baseado em plugin, sem patching) |
| `fig-start --setup` | Alterar o caminho do repositório figma-cli |

### Safe Mode (sem patching)

Se não consegues conceder Full Disk Access ou preferes não fazer patch ao Figma:

```bash
fig-start --safe
```

Isto usa um plugin do Figma em vez de patching. Consulta [Safe Mode](#-safe-mode--para-ambientes-restritos) para detalhes.

### Configuração Manual (sem fig-start)

```bash
cd figma-cli
claude
```

Depois diz ao Claude: `Connect to Figma`

---

## Como usar

Uma vez ligado, basta falar com o Claude:

> "Adiciona cores shadcn ao meu projeto"

> "Adiciona todos os componentes shadcn"

> "Adiciona um componente card com botão e input"

> "Verifica a acessibilidade"

> "Exporta variáveis como CSS"

O ficheiro `CLAUDE.md` incluído ensina ao Claude todos os comandos automaticamente. Sem necessidade de manual.

**Utilizadores de Safe Mode:** Iniciem o plugin FigCli sempre que abrirem o Figma.

---

## Renderização JSX com Ícones

A CLI inclui uma sintaxe tipo JSX para criar layouts complexos. Os ícones são renderizados como vetores SVG reais:

```jsx
// Ícones Lucide reais em JSX
<Frame name="Nav" flex="row" items="center" gap={8} bg="var:card" p={12} rounded={8}>
  <Icon name="lucide:home" size={20} color="var:foreground" />
  <Text size={14} weight="medium" color="var:foreground">Home</Text>
  <Frame grow={1} />
  <Icon name="lucide:settings" size={20} color="var:muted-foreground" />
</Frame>
```

Suporte de ícones:
- Qualquer ícone do [Lucide](https://lucide.dev/icons) (2000+ ícones)
- Qualquer conjunto de ícones do [Iconify](https://iconify.design/) (150.000+ ícones): `mdi:home`, `heroicons:star`, etc.
- Binding de cor com variáveis usando sintaxe `var:`
- Tamanhos personalizados

---

## Dois Modos de Ligação

### 🚀 Yolo Mode (Recomendado)

**O que faz:** Aplica um patch ao Figma uma vez para ativar uma porta de depuração, depois liga-se diretamente.

**Vantagens:**
- Totalmente automático (sem passos manuais após configuração)
- Execução ligeiramente mais rápida
- Seguro: porta aleatória, autenticação por token, apenas localhost, encerramento automático quando inativo

**Desvantagens:**
- Requer patch único ao Figma
- Necessita de Full Disk Access no macOS (uma vez)

```
┌─────────────┐      WebSocket (CDP)      ┌─────────────┐
│     CLI     │ <------------------------> │   Figma     │
└─────────────┘    localhost:random port  └─────────────┘
```

```bash
node src/index.js connect
```

---

### 🔒 Safe Mode -- Para Ambientes Restritos

**O que faz:** Usa um plugin do Figma para comunicar. Sem modificação do Figma.

**Vantagens:**
- Sem patching, sem modificação da aplicação
- Funciona em qualquer lugar (empresarial, pessoal, qualquer ambiente)
- Sem necessidade de Full Disk Access
- **Paridade total de funcionalidades** com o Yolo Mode (todos os comandos funcionam)

**Desvantagens:**
- Iniciar o plugin manualmente em cada sessão (2 cliques)
- Ligeiramente mais lento que o Yolo Mode

```
┌─────────────┐     WebSocket     ┌─────────────┐     Plugin API     ┌─────────────┐
│     CLI     │ <---------------> │   Daemon    │ <----------------> │   Plugin    │
└─────────────┘   localhost:3456  └─────────────┘                    └─────────────┘
```

**Passo 1:** Iniciar o Safe Mode
```bash
fig-start --safe
```
Ou manualmente: `node src/index.js connect --safe`

**Passo 2:** Importar o plugin (apenas uma vez)
1. No Figma: **Plugins -> Development -> Import plugin from manifest**
2. Seleciona `plugin/manifest.json` deste projeto
3. Clica em **Open**

**Passo 3:** Iniciar o plugin (cada sessão)
1. No Figma: **Plugins -> Development -> FigCli**
2. O terminal mostra: `Plugin connected!`

**Dica:** Clica com o botão direito no plugin -> **Add to toolbar** para acesso rápido.

---

### Que modo devo usar?

| Situação | Comando |
|---|---|
| Utilizador pela primeira vez | `fig-start` (Yolo Mode) |
| Mac pessoal | `fig-start` (Yolo Mode) |
| Portátil empresarial | `fig-start --safe` |
| Erros de permissão com Yolo | `fig-start --safe` |
| Não é possível modificar aplicações | `fig-start --safe` |

Ambos os modos têm **paridade total de funcionalidades**. O Safe Mode usa implementações nativas da Figma Plugin API em vez de figma-use, portanto todos os comandos funcionam de forma idêntica.

---

## Resolução de problemas

### Erro de permissão ao fazer patch (macOS)

Se vires `EPERM: operation not permitted, open '.../app.asar'`:

**1. Conceder Full Disk Access ao Terminal**

O macOS bloqueia o acesso a ficheiros sem esta permissão, mesmo com sudo.

1. Abre **System Settings** -> **Privacy & Security** -> **Full Disk Access**
2. Clica no botão **+**
3. Adiciona o **Terminal** (ou iTerm, VS Code, Warp, etc.)
4. **Reinicia o Terminal completamente** (fecha e reabre)

**2. Certifica-te de que o Figma está completamente fechado**
```bash
# Verificar se o Figma ainda está a correr
ps aux | grep -i figma

# Forçar encerramento se necessário
killall Figma
```

**3. Executar connect novamente**
```bash
node src/index.js connect
```

Se continuar a falhar, tenta com sudo: `sudo node src/index.js connect`

**4. Patch manual (último recurso)**

Se nada funcionar, podes fazer o patch manualmente:

```bash
# Fazer backup do original
sudo cp /Applications/Figma.app/Contents/Resources/app.asar ~/app.asar.backup

# O patch altera uma string no ficheiro
# De: removeSwitch("remote-debugging-port")
# Para: removeSwitch("remote-debugXing-port")

# Usa um editor hexadecimal ou este comando:
sudo sed -i '' 's/remote-debugging-port/remote-debugXing-port/g' /Applications/Figma.app/Contents/Resources/app.asar

# Re-assinar a aplicação
sudo codesign --force --deep --sign - /Applications/Figma.app
```

### Windows

O Windows é suportado mas menos testado que o macOS.

**Erro de permissão:** Executa a Linha de Comandos ou PowerShell como Administrador, depois executa `node src/index.js connect`.

**Localização do Figma:** A CLI espera o Figma em `%LOCALAPPDATA%\Figma\Figma.exe` (localização de instalação predefinida).

**Safe Mode:** Se o Yolo Mode não funcionar, usa o Safe Mode: `node src/index.js connect --safe`

### O Figma não liga

1. Certifica-te de que o Figma Desktop está a correr (não a versão web)
2. Abre um ficheiro de design no Figma (não apenas o ecrã inicial)
3. Reinicia a ligação: `node src/index.js connect`

---

## Atualização

```bash
cd ~/path/to/figma-cli
git pull
npm install
```

## Como funciona

Liga-se ao Figma Desktop via Chrome DevTools Protocol (CDP). Não é necessária API key porque utiliza a tua sessão Figma existente.

```
┌─────────────┐      WebSocket (CDP)      ┌─────────────┐
│ figma-ds-cli │ <------------------------> │   Figma     │
│    (CLI)    │   localhost:9222-9322     │  Desktop    │
└─────────────┘      (random port)        └─────────────┘
```

### Segurança

A CLI executa um daemon local para execução mais rápida de comandos. Funcionalidades de segurança:

- **Autenticação por token de sessão**: Token aleatório de 32 bytes necessário para todos os pedidos
- **Sem cabeçalhos CORS**: Bloqueia pedidos cross-origin do navegador
- **Validação do cabeçalho Host**: Apenas aceita localhost/127.0.0.1
- **Timeout por inatividade**: Encerramento automático após 10 minutos de inatividade (configurável)
- **Porta aleatória**: O CDP usa uma porta aleatória entre 9222-9322 por sessão

O token é armazenado em `~/.figma-ds-cli/.daemon-token` com permissões apenas para o proprietário (0600).

---

## Lista completa de funcionalidades

### Componentes shadcn/ui

- **30 componentes, 58 variantes** seguindo as especificações oficiais do shadcn/ui
- **Ícones SVG Lucide** reais (chevrons, check, x, plus, info, alert-circle, bold, ellipsis)
- **Binding de design tokens** via sintaxe `var:` (liga automaticamente às variáveis Light/Dark do shadcn)
- Componentes: Button (9), Badge (4), Card, Input (3), Textarea, Label, Alert (2), Avatar (2), Switch (2), Separator (2), Skeleton (3), Progress (2), Toggle (2), Checkbox (2), Tabs, Table, Radio Group (3), Select (3), Slider, Breadcrumb, Pagination, Kbd (2), Spinner (2), Tooltip, Dialog, Dropdown Menu, Accordion, Navigation Menu, Sheet, Hover Card

### Design Tokens e Variáveis

- **Presets de cores** -- shadcn (276 variáveis com modo Light/Dark), Radix UI (156 variáveis)
- Criar paletas de cores Tailwind CSS (todas as 22 famílias de cores, tons 50-950)
- Criar e gerir coleções de variáveis
- **Modos de variáveis** (Light/Dark/Mobile) com valores por modo
- **Criação em lote** de até 100 variáveis de uma vez
- **Atualização em lote** de valores de variáveis entre modos
- Ligar variáveis a propriedades de nós (fill, stroke, gap, padding, radius)
- Exportar variáveis como propriedades personalizadas CSS
- Exportar variáveis como configuração Tailwind

### Criar Elementos

- Frames com auto-layout
- Retângulos, círculos, elipses
- Texto com fontes, tamanhos e pesos personalizados
- Linhas
- Ícones (150.000+ do Iconify: Lucide, Material Design, Heroicons, etc.)
- Grupos
- Componentes a partir de frames
- Instâncias de componentes
- **Conjuntos de componentes com variantes**

### Renderização JSX

- **Sintaxe tipo JSX** para layouts complexos (`<Frame>`, `<Text>`, `<Icon>`, `<Slot>`)
- **Ícones Lucide/Iconify reais** renderizados como vetores SVG (não placeholders)
- **Binding de variáveis** com sintaxe `var:name` para fills, strokes, cores de texto e ícones
- Propriedades de auto-layout: `flex`, `gap`, `p`/`px`/`py`, `justify`, `items`, `grow`, `wrap`
- Dimensionamento: `w`/`h` (fixo), `w="fill"` (esticar), auto-hug
- Aparência: `bg`, `stroke`, `strokeWidth`, `strokeAlign`, `rounded`, `shadow`, `opacity`, `overflow`
- **Slots** para áreas de conteúdo de componentes

### Modificar Elementos

- Alterar cores de fill e stroke
- Definir raio dos cantos
- Redimensionar e mover
- Aplicar auto-layout (linha/coluna, gap, padding)
- Definir modo de dimensionamento (hug/fill/fixed)
- Renomear nós
- Duplicar nós
- Eliminar nós
- **Inverter nós** (horizontal/vertical)
- **Escalar vetores**

### Encontrar e Selecionar

- Encontrar nós por nome
- Encontrar nós por tipo (FRAME, COMPONENT, TEXT, etc.)
- **Queries tipo XPath** (`//FRAME[@width > 300]`)
- Selecionar nós por ID
- Obter propriedades de nós
- Obter estrutura em árvore de nós

### Operações de Canvas

- Listar todos os nós no canvas
- Organizar frames em grelha ou coluna
- Eliminar todos os nós
- Zoom para ajustar ao conteúdo
- Posicionamento inteligente (colocação automática sem sobreposições)

### Exportação

- **Exportar nó por ID** (`export node "1:234" -s 2 -f png`)
- Exportar nós como PNG (com fator de escala)
- Exportar nós como SVG
- **Exportar múltiplos tamanhos** (@1x, @2x, @3x)
- Tirar screenshots
- **Exportar para JSX** (código React)
- **Exportar para Storybook** stories
- Exportar variáveis como CSS
- Exportar variáveis como configuração Tailwind

### Suporte FigJam

- Criar notas adesivas
- Criar formas com texto
- Ligar elementos com setas
- Listar elementos FigJam
- Executar JavaScript no contexto FigJam

### Team Libraries

- Listar coleções de variáveis de bibliotecas disponíveis
- Importar variáveis de bibliotecas
- Importar componentes de bibliotecas
- Criar instâncias de componentes de bibliotecas
- Importar e aplicar estilos de bibliotecas (cor, texto, efeito)
- Ligar variáveis de bibliotecas a propriedades de nós
- Trocar instâncias de componentes para componentes de outra biblioteca
- Listar todas as bibliotecas ativadas

### Utilitários para Designers

- **Renomeação de layers em lote** (com padrões: {n}, {name}, {type})
- **Conversão de maiúsculas/minúsculas** (camelCase, PascalCase, snake_case, kebab-case)
- **Gerador de Lorem ipsum** (palavras, frases, parágrafos)
- **Preencher texto com conteúdo placeholder**
- **Inserir imagens a partir de URL**
- **Integração com Unsplash** (fotos de stock aleatórias por palavra-chave)
- **Verificador de contraste** (conformidade WCAG AA/AAA)
- **Verificar contraste de texto** contra fundo
- **Encontrar e substituir texto** em todas as layers
- **Selecionar semelhantes** (fill, stroke, fonte, tamanho)
- **Simulação de daltonismo** (deuteranopia, protanopia, tritanopia)

### Consulta e Análise

- **Analisar cores** -- frequência de uso, bindings de variáveis
- **Analisar tipografia** -- todas as combinações de fontes usadas
- **Analisar espaçamentos** -- valores de gap/padding, conformidade com grelha
- **Encontrar clusters** -- detetar padrões repetidos (potenciais componentes)
- **Diff visual** -- comparar dois nós
- **Criar patch de diff** -- patches estruturais entre versões

### Lint e Acessibilidade

- **Linting de design** com 8+ regras:
  - `no-default-names` -- detetar layers sem nome
  - `no-deeply-nested` -- sinalizar aninhamento excessivo
  - `no-empty-frames` -- encontrar frames vazias
  - `prefer-auto-layout` -- sugerir auto-layout
  - `no-hardcoded-colors` -- verificar uso de variáveis
  - `color-contrast` -- conformidade WCAG AA/AAA
  - `touch-target-size` -- verificação mínima de 44x44
  - `min-text-size` -- texto mínimo de 12px
- **Snapshot de acessibilidade** -- extrair árvore de elementos interativos

### Variantes de Componentes

- Criar conjuntos de componentes com variantes
- Adicionar propriedades de variantes
- Combinar frames em conjuntos de componentes
- **Organizar variantes** em grelha com etiquetas
- **Gerar automaticamente conjuntos de componentes** a partir de frames semelhantes

### Documentação de Componentes

- **Adicionar descrições** a componentes (suporta markdown)
- **Documentar com template** (utilização, props, notas)
- Ler descrições de componentes

### CSS Grid Layout

- Configurar layout de grelha com colunas e linhas
- Configurar gaps de colunas/linhas
- Reorganizar automaticamente os filhos numa grelha

### Consola e Depuração

- **Listar ficheiros Figma abertos** (comando `files`, usado pelo fig-start)
- **Capturar logs da consola** do Figma
- **Executar código com captura de logs**
- **Recarregar página**
- **Navegar para ficheiros**

### Avançado

- Executar qualquer código da Figma Plugin API diretamente
- Renderizar UI complexa a partir de sintaxe tipo JSX
- Controlo programático completo sobre o Figma
- Corresponder vetores a ícones do Iconify

### Não suportado (requer REST API)

- Comentários (ler/escrever/eliminar) -- requer Figma API key
- Histórico de versões
- Gestão de equipas/projetos

---

## Autor

**WakandaDS** — [github.com/WakandaDS](https://github.com/WakandaDS)

## Licença

MIT
