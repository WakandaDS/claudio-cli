# Figma Audit Rules

Regras de auditoria para o Design System. Cada linha define um padrão a detetar.

## Como gerir

Usa checkboxes para activar e desactivar regras directamente neste ficheiro (no Obsidian ou qualquer editor de texto):

```
- [x] [ID] [Mensagem da flag] regex: padrão
- [ ] [ID] [Mensagem da flag] regex: padrão
```

| Sintaxe | Estado |
|---------|--------|
| `- [x]` | Regra **activa** — será executada no próximo `audit` |
| `- [ ]` | Regra **inactiva** — ignorada pelo parser |

O link `[[...]]` no fim de cada linha é ignorado pelo parser — serve apenas para navegar para a documentação no Obsidian.

**Correr o audit:**
```
node src/index.js audit
```

**Ver estado das regras (activas/inactivas):**
```
node src/index.js audit --list
```

**Limpar todas as anotações:**
```
node src/index.js audit --clear
```

---

## Naming

- [x] [ERR_UNNAMED] [Layer sem nome semântico] regex: ^(Frame|Group|Rectangle|Ellipse|Line|Text|Vector|Component|Instance)\s\d+$ [[rules-reference#ERR_UNNAMED]]
- [x] [ERR_UNNAMED_SET] [Component Set sem nome semântico] regex: ^(Frame|Group|Rectangle|Ellipse|Line|Text|Vector|Component|Instance)\s\d+$ type: COMPONENT_SET [[rules-reference#ERR_UNNAMED_SET]]
- [x] [ERR_TEMP] [Layer temporária ou por organizar] regex: ^(temp|tmp|test|WIP|TODO|xxx|old|copy|copy of|untitled) [[rules-reference#ERR_TEMP]]

## Componentes Deprecated

- [x] [ERR_DEPRECATED] [Componente deprecated — migrar para v2] regex: ^🔴 [[rules-reference#ERR_DEPRECATED]]

## Naming Conventions

- [ ] [ERR_NAMING] [Naming incorreto — deve ser camelCase] regex: ^[A-Z][a-z]{1,20}\s[A-Z][a-z]{1,20}$ [[rules-reference#ERR_NAMING]]

---

> Edita este ficheiro no Obsidian para adicionar ou gerir regras.
> Documentação detalhada de cada regra: [[rules-reference]]
