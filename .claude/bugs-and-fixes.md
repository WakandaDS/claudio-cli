# Histórico de Bugs e Correções

## Fallback Cinzento no Variable Binding (f8d519b)
**Sintoma**: Todos os componentes renderizados com fills cinzentos `rgb(128,128,128)`
**Causa**: O carregador de variáveis procurava `c.name === 'shadcn'` mas as coleções chamam-se `shadcn/primitives` e `shadcn/semantic`
**Correção**: Alterado para `c.name.startsWith('shadcn')` e iterar todas as coleções correspondentes
**Localização**: Dois sítios em figma-client.js (render-batch ~linha 357, single render ~linha 1090)

## Overflow de Conteúdo / Altura Fixa (f8d519b)
**Sintoma**: Conteúdo do Settings Panel transborda a frame, fica cortado
**Causa**: Sem `h` explícito, a root frame assume `h=200` com FIXED sizing
**Correção**: Quando não há altura explícita, usar `primaryAxisSizingMode = 'AUTO'` (HUG) em vez de FIXED. O eixo depende da direcção do layout (VERTICAL: primary=height, HORIZONTAL: primary=width)
**Localização**: Dois caminhos de código - render-batch e single render com var:

## Parser de Frame Self-Closing (3353bbd)
**Sintoma**: Rectângulos cinzentos flutuantes aparecem como siblings em vez de children aninhados
**Causa**: `frameOpenRegex` (`/<Frame...>/`) fazia match com `<Frame ... />` self-closing porque o `>` no final de `/>` correspondia
**Correção**:
1. Fazer parse de tags Frame open/close PRIMEIRO, depois self-closing fora dos ranges consumidos
2. Saltar self-closing no frameOpenRegex: `if (match[0].endsWith('/>')) continue`
3. Regex non-greedy no extractContent: `[^>]*?` em vez de `[^>]*`

## FILL Antes de appendChild (c69d448)
**Sintoma**: Erro "FILL can only be set on children of auto-layout frames"
**Causa**: `layoutSizingHorizontal = 'FILL'` definido antes de `appendChild()`
**Correção**: Mover a atribuição de FILL sizing para depois da chamada appendChild

## Padding por Defeito em Frames Aninhadas (c69d448)
**Sintoma**: Componentes parecem partidos com espaçamento inesperado
**Causa**: Padding por defeito 16/10 aplicado a TODAS as frames aninhadas (destinado apenas a botões)
**Correção**: Alterados os defaults para 0/0: `fPx = item.px !== undefined ? item.px : (fP !== null ? fP : 0)`

## grow={1} A Usar API Depreciada (c69d448)
**Sintoma**: grow não funciona correctamente, overflow vertical
**Causa**: Usava `layoutGrow` (depreciado) em vez de `layoutSizingHorizontal/Vertical = 'FILL'`
**Correção**: Mapear grow baseado na direcção flex do pai usando a nova API

## Conflito API Antiga vs Nova do Figma (c69d448)
**Sintoma**: Conflitos de sizing entre API antiga e nova
**Causa**: Uso misto de `primaryAxisSizingMode`/`counterAxisSizingMode` (antiga) e `layoutSizingHorizontal`/`layoutSizingVertical` (nova)
**Correção**: Usar `layoutSizingHorizontal`/`layoutSizingVertical` para children, manter `primaryAxisSizingMode`/`counterAxisSizingMode` apenas para self-sizing da root frame
