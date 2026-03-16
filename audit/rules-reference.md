# Audit Rules — Referência
#figma-cli #audit #regras

Documentação detalhada de cada regra de auditoria. Edita aqui para alterar o comportamento esperado, adicionar exemplos ou arquivar regras obsoletas.

Para activar/desactivar uma regra, edita o ficheiro [[rules]].

---

## ERR_UNNAMED

**Detecta:** Layers com nomes automáticos do Figma (e.g. `Frame 42`, `Rectangle 7`)

**Porquê é um problema:** Layers sem nome semântico tornam impossível navegar o ficheiro, auditar o sistema, ou perceber a estrutura de um componente sem abrir cada node individualmente.

**Como corrigir:** Renomeia o layer para algo que descreva o seu propósito — `card`, `heroSection`, `avatarWrapper`.

**Regex:** `^(Frame|Group|Rectangle|Ellipse|Line|Text|Vector|Component|Instance)\s\d+$`

---

## ERR_UNNAMED_SET

**Detecta:** Component Sets com nomes automáticos do Figma (e.g. `Component 12`)

**Porquê é um problema:** Component Sets são o ponto de entrada de um componente no Design System. Um nome automático impede a descoberta, o Code Connect e qualquer forma de rastreabilidade.

**Como corrigir:** Renomeia o Component Set para o nome canónico do componente em camelCase — `buttonPrimary`, `cardHeader`.

**Regex:** `^(Frame|Group|Rectangle|Ellipse|Line|Text|Vector|Component|Instance)\s\d+$`
**Filtro:** `type: COMPONENT_SET`

---

## ERR_TEMP

**Detecta:** Layers com prefixos de trabalho temporário — `temp`, `tmp`, `test`, `WIP`, `TODO`, `xxx`, `old`, `copy`, `copy of`, `untitled`

**Porquê é um problema:** Layers temporárias acumulam-se silenciosamente e poluem o ficheiro. Numa auditoria ou entrega, indicam trabalho inacabado.

**Como corrigir:** Ou renomeia para o nome definitivo, ou apaga se o layer já não for necessário.

**Regex:** `^(temp|tmp|test|WIP|TODO|xxx|old|copy|copy of|untitled)`

---

## ERR_DEPRECATED

**Detecta:** Componentes marcados com `🔴` no início do nome

**Porquê é um problema:** Componentes deprecated devem ser migrados para a versão actual. Este flag serve de aviso visual e de auditoria — qualquer instância ainda em uso precisa de ser substituída.

**Como corrigir:** Substitui todas as instâncias pelo equivalente em v2 e remove o prefixo `🔴` quando a migração estiver concluída.

**Regex:** `^🔴`

---

## ERR_NAMING *(desactivado)*

**Detecta:** Layers com naming em Title Case com espaço (e.g. `Button Primary`, `Card Header`)

**Porquê está desactivado:** A convenção do Design System usa camelCase (`buttonPrimary`, `cardHeader`), mas há ainda muitos layers legados com Title Case que não foram migrados. Activar esta regra agora geraria demasiado ruído.

**Quando activar:** Após a migração de naming estar concluída, activar esta regra para impedir regressões.

**Como corrigir:** Renomeia para camelCase.

**Regex:** `^[A-Z][a-z]{1,20}\s[A-Z][a-z]{1,20}$`

---

## Adicionar uma nova regra

1. Abre [[rules]] e adiciona uma linha com o formato:
   ```
   - [x] [ERR_ID] [Mensagem da flag] regex: padrão
   ```
2. Adiciona uma secção neste ficheiro a documentar o que a regra detecta, porquê é um problema e como corrigir.
3. Corre `node src/index.js audit --list` para confirmar que a regra foi reconhecida.
