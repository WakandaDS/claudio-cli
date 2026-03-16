# Como Colaborar

Guia para membros da equipa que trabalham neste repositório com Claude Code.

---

## Estrutura de branches

```
main          ← código e conhecimento partilhado por toda a equipa
└── [nome]    ← branch pessoal de cada pessoa (ex: bruno, joana, miguel)
```

**`main`** contém tudo o que é partilhado:
- `src/` — código do CLI
- `docs/` — documentação técnica
- `audit/rules*.md` — regras de auditoria
- `CLAUDE.md` — instruções do projecto para o Claude
- `onboarding/` — templates e guias de setup

**A tua branch pessoal** é onde trabalhas no dia-a-dia. Quando algo está pronto para partilhar, abres um PR para `main`.

---

## Onboarding — primeira vez

### 1. Clona o repositório e cria a tua branch

```bash
git clone https://github.com/WakandaDS/bruno-figma-cli.git
cd bruno-figma-cli
git checkout -b [o-teu-nome]
npm install
```

### 2. Configura o Claude

```bash
node src/index.js onboarding
```

O comando vai:
- Copiar o template de configuração pessoal para `~/.claude/CLAUDE.md` (se ainda não existires)
- Criar a pasta de memórias local `.claude/memory/`
- Confirmar que tudo está pronto

### 3. Liga ao Figma

```bash
node src/index.js connect
```

---

## O que vai para `main` e o que não vai

| Ficheiro | Vai para `main`? |
|----------|-----------------|
| `src/index.js` | ✅ Sim |
| `docs/` | ✅ Sim |
| `audit/rules.md` | ✅ Sim |
| `audit/rules-reference.md` | ✅ Sim |
| `CLAUDE.md` (do projecto) | ✅ Sim |
| `~/.claude/CLAUDE.md` (pessoal) | ❌ Nunca — fica na tua máquina |
| `.claude/memory/` | ❌ Nunca — memórias locais |
| `.obsidian/` | ❌ Nunca — config do editor |

---

## Como partilhar melhorias

### Código novo ou corrigido

```bash
# Na tua branch
git add src/index.js
git commit -m "feat: novo comando X"
git push origin [o-teu-nome]
# Abre PR para main no GitHub
```

### Nova técnica ou padrão descoberto

Se descobriste algo que melhora o workflow — um padrão JSX que funciona melhor, uma técnica de tokens, um fluxo mais eficiente — **commita no `CLAUDE.md` do projecto** via PR.

Quando os outros fizerem `git pull`, o Claude deles já sabe essa técnica também.

```bash
git add CLAUDE.md
git commit -m "docs: padrão X para componentes com texto longo"
git push origin [o-teu-nome]
# Abre PR para main
```

### Nova regra de auditoria

1. Adiciona a regra em `audit/rules.md` com `- [x]`
2. Documenta em `audit/rules-reference.md` com uma secção nova
3. PR para `main`

---

## Sincronizar com `main`

Para ficares actualizado com o que os colegas partilharam:

```bash
git checkout main
git pull origin main
git checkout [o-teu-nome]
git merge main
```

---

## Convenção de commits

```
feat: novo comando ou funcionalidade
fix: correcção de bug
docs: actualização de documentação ou CLAUDE.md
audit: nova regra ou ajuste de regras
chore: manutenção, dependências
```
