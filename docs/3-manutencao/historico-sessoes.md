# Histórico de Sessões
#figma-cli #manutenção #referência #sessões

> [!NOTE]
> Registo de IDs e contextos de sessões de trabalho anteriores. Útil para retomar trabalho sem ter de fazer `query` de novo.

---

## Sessão — Figma Design System (DS Principal)

**Ficheiro:** `vh6fGpzJSKNG5WyuyeKnVW`
**Nome:** Design System principal da equipa

### Audit — Component Sets com state `disabled`

Scan corrido em 2026-03-15. 21 component sets identificados com state `disabled` sem boolean implementado. Ver [[motor-de-audit]] para o sistema.

Backlog completo em `audit/backlog.md`.

Componentes afectados:
```
accordionComplex · buttonCalendar · buttonList · buttonQuaternary
buttonSecondary · buttonTertiary · cardBank · checkboxPrimary
checkboxSecondary · dropDownGeneral · dropDownMultiSelect
inputAmount · inputGeneral · inputTextArea · mainMenuPrimary
radioButton · selectorListItem · switch · tabPrimary · tabSecondary
```

---

## Sessão — Frames de Conteúdo (IDS AIVCC)

**Página:** IDS AIVCC 2026
**Frames:** `session-1` a `session-15` (1920×1080 cada)

IDs dos grupos de conteúdo (dentro de cada session frame):
```
1:92, 1:112, 1:134, 1:154, 1:179,
1:200, 1:223, 1:244, 1:269, 1:297,
1:315, 1:332, 1:355, 1:381, 1:405
```

> [!WARNING]
> Estes IDs são específicos do ficheiro desta sessão. Podem mudar se o ficheiro for reorganizado.

**Colecção de variáveis:** `"Mode (Alias)"` da library `"IDS_Tokens"`
- Modos: `Light Mode`, `Dark Mode`

**Operações feitas:**
- Renomeação de `Stream-X` → `session-X`
- Scaling de grupos de conteúdo (`rescale()` para 1.2 e 1.1 consoante o tipo)
- Centrar conteúdo em frames 1920×1080
- Mudança de mode Light/Dark via `setExplicitVariableModeForCollection()`

---

## Notas de operação

> [!IMPORTANT]
> Nunca usar `resize()` para escalar — distorce conteúdo. Usar sempre `rescale()` e reposicionar.

> [!IMPORTANT]
> Nunca eliminar nodes existentes sem confirmação — podem ser trabalho em curso do utilizador.
