# figma-ds-cli

CLI that controls Figma Desktop directly. No API key needed.

## Quick Reference

| User says | Command |
|-----------|---------|
| "connect to figma" | `node src/index.js connect` |
| "add shadcn colors" | `node src/index.js tokens preset shadcn` |
| "add tailwind colors" | `node src/index.js tokens tailwind` |
| "show colors on canvas" | `node src/index.js var visualize` |
| "create cards/buttons" | `render-batch` + `node to-component` |
| "create a rectangle/frame" | `node src/index.js render '<Frame>...'` |
| "convert to component" | `node src/index.js node to-component "ID"` |
| "list variables" | `node src/index.js var list` |
| "find nodes named X" | `node src/index.js find "X"` |
| "what's on canvas" | `node src/index.js canvas info` |
| "export as PNG/SVG" | `node src/index.js export png` |

**Full command reference:** See REFERENCE.md

---

## Design Tokens

"Add shadcn colors":
```bash
node src/index.js tokens preset shadcn   # 244 primitives + 32 semantic (Light/Dark)
```

"Add tailwind colors":
```bash
node src/index.js tokens tailwind        # 242 primitive colors only
```

"Create design system":
```bash
node src/index.js tokens ds              # IDS Base colors
```

**shadcn vs tailwind:**
- `tokens preset shadcn` = Full shadcn system (primitives + semantic tokens with Light/Dark mode)
- `tokens tailwind` = Just the Tailwind color palette (primitives only)

"Delete all variables":
```bash
node src/index.js var delete-all                    # All collections
node src/index.js var delete-all -c "primitives"    # Only specific collection
```

**Note:** `var list` only SHOWS existing variables. Use `tokens` commands to CREATE them.

---

## Connection Modes

### Yolo Mode (Recommended)
Patches Figma once, then connects directly. Fully automatic.
```bash
node src/index.js connect
```

### Safe Mode
Uses plugin, no Figma modification. Start plugin each session.
```bash
node src/index.js connect --safe
```
Then: Plugins → Development → FigCli

---

## Creating Components

When user asks to "create cards", "design buttons":

1. **Each component = separate frame** (NOT inside parent gallery)
2. **Convert to component** after creation
3. **Use variables** for colors

```bash
# Step 1: Create separately
node src/index.js render-batch '[
  "<Frame name=\"Card 1\" w={320} h={200} bg=\"#18181b\" rounded={12} flex=\"col\" p={24}><Text color=\"#fff\">Title</Text></Frame>",
  "<Frame name=\"Card 2\" w={320} h={200} bg=\"#18181b\" rounded={12} flex=\"col\" p={24}><Text color=\"#fff\">Title</Text></Frame>"
]'

# Step 2: Convert
node src/index.js node to-component "ID1" "ID2"

# Step 3: Bind variables
node src/index.js bind fill "zinc/900" -n "ID1"
```

---

## Creating Webpages

Create ONE parent frame with vertical auto-layout containing all sections:

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

## JSX Syntax (render command)

```jsx
// Layout
flex="row"              // or "col"
gap={16}                // spacing between items
p={24}                  // padding all sides
px={16} py={8}          // padding x/y
pt={8} pr={16} pb={8} pl={16}  // individual padding

// Alignment
justify="center"        // main axis: start, center, end, between
items="center"          // cross axis: start, center, end

// Size
w={320} h={200}         // fixed size
w="fill" h="fill"       // fill parent
minW={100} maxW={500}   // constraints
minH={50} maxH={300}

// Appearance
bg="#fff"               // fill color (or var:Name)
stroke="#000"           // stroke color
strokeWidth={2}         // stroke thickness
strokeAlign="inside"    // inside, outside, center
opacity={0.8}           // 0..1
blendMode="multiply"    // multiply, overlay, etc.

// Corners
rounded={16}            // all corners
roundedTL={8} roundedTR={8} roundedBL={0} roundedBR={0}  // individual
cornerSmoothing={0.6}   // iOS squircle (0..1)

// Effects
shadow="4px 4px 12px rgba(0,0,0,0.25)"  // drop shadow
blur={8}                // layer blur
overflow="hidden"       // clip content
rotate={45}             // rotation degrees

// Text
<Text size={18} weight="bold" color="#000" font="Inter">Hello</Text>
```

### Auto-Layout

```jsx
// Wrap: items flow to next row when full
wrap={true}             // layoutWrap = 'WRAP'
rowGap={12}             // gap between rows (counterAxisSpacing)

// Grow: expand to fill remaining space
grow={1}                // layoutGrow = 1

// Stretch: fill cross-axis
stretch={true}          // layoutAlign = 'STRETCH'

// Absolute: position freely within parent
position="absolute" x={12} y={12}  // must have name for x/y to work
```

**Complete example:**
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

**Common mistakes (silently ignored, no error!):**
```
WRONG                    RIGHT
layout="horizontal"   →  flex="row"
padding={24}          →  p={24}
fill="#fff"           →  bg="#fff"
cornerRadius={12}     →  rounded={12}
fontSize={18}         →  size={18}
fontWeight="bold"     →  weight="bold"
```

**Complete card example:**
```bash
node src/index.js render '<Frame name="Card" w={320} h={200} bg="#18181b" rounded={12} flex="col" p={24} gap={12}>
  <Text size={18} weight="bold" color="#fff">Title</Text>
  <Text size={14} color="#a1a1aa" w="fill">Description text</Text>
  <Frame bg="#3b82f6" px={16} py={8} rounded={6}>
    <Text size={14} weight="medium" color="#fff">Button</Text>
  </Frame>
</Frame>'
```

---

## Key Rules

1. **Always use `render` for frames** - has smart positioning
2. **Never use `eval` to create** - no positioning, overlaps at (0,0)
3. **Never use `npx figma-use render`** - no smart positioning
4. **For multiple frames:** Use `render-batch`
5. **Convert to components:** `node to-component` after creation

---

## Onboarding ("Initiate Project")

**Never show terminal commands to users.** Run silently, give friendly feedback.

1. Run `npm install` silently
2. Ask connection mode (Yolo or Safe)
3. Run `node src/index.js connect` (or `--safe`)
4. When connected, say: "Connected! What would you like to create?"

If permission error (macOS): System Settings → Privacy → Full Disk Access → Add Terminal

---

## Variable Visualization

"Show colors on canvas" / "display variables" / "create palette":
```bash
node src/index.js var visualize              # All collections
node src/index.js var visualize "primitives" # Filter
```

Creates shadcn-style color swatches bound to variables.

---

## Website Recreation

```bash
node src/index.js recreate-url "https://example.com" --name "Page"
node src/index.js screenshot-url "https://example.com"
```

---

## Speed Daemon

`connect` auto-starts daemon for 10x faster commands.

```bash
node src/index.js daemon status
node src/index.js daemon restart
```
