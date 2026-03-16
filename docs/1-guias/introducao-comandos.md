# Introdução aos Comandos
#figma-cli #guia #comandos

> [!NOTE]
> Todos os comandos correm com `node src/index.js <comando>`. O daemon é iniciado automaticamente após `connect` para respostas mais rápidas.

---

## 🔌 Ligação

| Comando | O que faz |
|---------|-----------|
| `node src/index.js connect` | Yolo Mode — patcha o Figma uma vez, depois liga directamente |
| `node src/index.js connect --safe` | Safe Mode — usa plugin, sem modificar o Figma |
| `node src/index.js daemon status` | Ver estado do daemon |
| `node src/index.js daemon restart` | Reiniciar daemon |

> [!IMPORTANT]
> Ver [[visao-geral]] para a diferença entre Yolo Mode e Safe Mode antes de escolher.

---

## 🎨 Design Tokens

| Comando | O que faz |
|---------|-----------|
| `tokens preset shadcn` | 244 primitivas + 32 semânticos com Light/Dark mode |
| `tokens tailwind` | 242 cores primitivas (só paleta Tailwind) |
| `tokens ds` | Cores base do IDS |
| `var list` | Listar todas as variáveis |
| `var visualize` | Criar swatches no canvas |
| `var visualize "primitives"` | Filtrar por colecção |
| `var delete-all` | Eliminar todas as variáveis |
| `var delete-all -c "primitives"` | Eliminar só uma colecção |

---

## 🖼️ Canvas

| Comando | O que faz |
|---------|-----------|
| `canvas info` | O que está no canvas actual |
| `find "X"` | Encontrar nodes por nome |
| `verify` | Screenshot da selecção actual |
| `verify "123:456"` | Screenshot de um node específico |

---

## 🧱 Criação de Elementos

```bash
# Frame simples
node src/index.js create frame "Card" -w 320 -h 200 --fill "#fff" --radius 12

# Círculo
node src/index.js create circle "Avatar" --fill "var:primary"

# Texto
node src/index.js create text "Hello" -c "var:foreground"

# Linha
node src/index.js create line -c "var:border"

# Auto-layout
node src/index.js create autolayout "Container" --fill "var:muted"

# Ícone Lucide
node src/index.js create icon lucide:star -c "var:primary"
```

---

## 🧩 Blocos Pré-construídos

> [!NOTE]
> Para dashboards e layouts de página, **usa sempre `blocks create`**. Nunca construir manualmente com render.

```bash
node src/index.js blocks list                     # Ver blocos disponíveis
node src/index.js blocks create dashboard-01      # Criar dashboard completo
```

**dashboard-01** inclui: sidebar com ícones Lucide reais, cards de stats, gráfico de área, tabela de dados com paginação. Todas as cores vinculadas a variáveis shadcn (suporta Light/Dark).

---

## ⚙️ Componentes

```bash
node src/index.js node to-component "ID"     # Converter frame em componente
node src/index.js combos                     # Mostrar todas as combinações de variantes
node src/index.js sizes --base small         # Criar variantes de tamanho
```

---

## 📤 Export

```bash
node src/index.js export png                 # Exportar como PNG
node src/index.js export css                 # Exportar tokens como CSS custom properties
```

---

## 🔍 Audit

```bash
node src/index.js audit                      # Correr auditoria (regras de audit/rules.md)
node src/index.js audit --list               # Ver estado das regras
node src/index.js audit --clear              # Limpar anotações do Figma
```

> [!NOTE]
> Ver [[motor-de-audit]] para perceber como funciona o sistema de regras e anotações.

---

## 🎯 Slots

```bash
node src/index.js slot create "Content" --flex col --gap 8
node src/index.js slot list
node src/index.js slot reset
node src/index.js slot convert --name "Actions"
```

---

## 🧪 Eval Directo

```bash
node src/index.js eval "figma.currentPage.name"
node src/index.js eval "figma.currentPage.children.length"
```

> [!WARNING]
> O `eval` corre no contexto do Figma. Às vezes não devolve output mas o código executa na mesma. Verifica com `canvas info` ou `verify`.
