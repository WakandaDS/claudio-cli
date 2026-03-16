# figma-cli — Knowledge Base
#figma-cli #documentação #hub

> **Bem-vindo à Wiki do figma-cli 📚**
> CLI para controlar o Figma Desktop directamente, sem API key.
> Documentação organizada para ser lida e mantida no **Obsidian**, tirando partido de wikilinks, tags e estruturas visuais ricas.

---

## 🧭 Navegação Rápida

### 1️⃣ Guias Práticos
| Documento | Conteúdo |
|-----------|----------|
| [[introducao-comandos]] | Catálogo de todos os comandos disponíveis, com exemplos e flags. |
| [[tokens-e-variaveis]] | Como criar, listar e vincular tokens e variáveis no Figma. |
| [[render-e-componentes]] | JSX render engine, sintaxe, criação de componentes e auto-layout. |
| [[figjam]] | Comandos e cliente FigJam — stickies, shapes, connectors. |

### 2️⃣ Arquitectura e Under-the-hood 🔬
| Documento | Conteúdo |
|-----------|----------|
| [[visao-geral]] | Modelo CDP, conexão ao Figma Desktop, modos Yolo vs Safe. |
| [[motor-de-audit]] | Sistema de auditoria — parser de regras, anotações nativas, backlog Obsidian. |

### 3️⃣ Manutenção e Operações 🛠️
| Documento | Conteúdo |
|-----------|----------|
| [[tecnicas-avancadas]] | Padrões avançados: variable modes, scaling, batch ops, debugging. |
| [[historico-sessoes]] | Referência rápida de snippets e IDs de sessões anteriores. |

---

## 🔗 Referências Úteis

- **Ficheiro Figma (DS)**: `vh6fGpzJSKNG5WyuyeKnVW` — Design System principal
- **Regras de Audit**: Ver `audit/rules.md`
- **Backlog de Audit**: Ver `audit/backlog.md`
- **Referência de comandos completa**: Ver `REFERENCE.md` na raiz

---

## 📁 Estrutura do Projecto

```
figma-cli/
├── src/
│   └── index.js               ← Ponto de entrada — todos os comandos
├── src/blocks/                ← Layouts pré-construídos (dashboard-01, etc.)
├── audit/
│   ├── rules.md               ← Regras de auditoria (Obsidian)
│   └── backlog.md             ← Items detectados para revisão
├── docs/                      ← Este hub de conhecimento
│   ├── 1-guias/
│   ├── 2-arquitectura/
│   └── 3-manutencao/
└── CLAUDE.md                  ← Instruções do agente IA
```
