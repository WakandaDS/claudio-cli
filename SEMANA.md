# Planeamento da Semana — Perguntas Base

Questões para definir o que faz mais sentido desenvolver esta semana.

---

## 1. Sistema de Flags / Anotações

O comando `flag` está funcional. Qual é o próximo passo natural?

- Queres um comando de **lint automático** que varre um componente (ou página inteira) e gera flags sem intervenção manual — tipo `node src/index.js lint --flags`?
- Ou preferes que o fluxo seja sempre **manual e cirúrgico** — tu decides o que flaggar e eu executo?
- Faz sentido ter um comando `flag list` para listar todas as anotações Claude numa página, com a opção de as exportar?

---

## 2. Escopo de Auditoria

Quando auditamos um componente, o que devo verificar automaticamente?

- Tokens em falta ou hardcoded (fills, strokes, typography)?
- Componentes deprecated (como o chipGroup desta sessão)?
- Naming incorreto (layers sem convenção, componentes sem prefixo)?
- Contraste de acessibilidade?
- Outra coisa que eu ainda não deteto?

---

## 3. Fluxo de Trabalho com o Rafa

Há outro designer a trabalhar no mesmo ficheiro. Como é que as flags devem funcionar em colaboração?

- As flags Claude são para uso interno (só vós dois veem) ou também para o Rafa agir em cima delas?
- Devo limpar as flags depois de resolvidas, ou ficam como histórico?
- Faz sentido um sistema de **status** nas anotações — ex: `[pendente]`, `[resolvido]`?

---

## 4. Componentes a Construir

Há componentes prioritários a criar ou migrar para v2 esta semana?

- Quais são os componentes em `🔴 Deprecated` que precisam de v2 com mais urgência?
- Preferes que eu construa os v2 a partir do zero com `render`/`eval`, ou fazes tu e eu audito?
- Há um sistema de design tokens já definido para os v2, ou isso ainda está em aberto?

---

## 5. Infraestrutura do CLI

A ligação ao Figma tem sido instável (port 9222 fecha depois de algum tempo). Vale a pena resolver isto esta semana?

- Queres investir tempo a estabilizar a ligação (auto-reconnect robusto)?
- Ou aceitamos o `connect` manual como parte do fluxo e focamos nas features?

---

## 6. Prioridade Geral

Se tivesses de escolher **uma coisa** para ter pronta no fim da semana, o que seria?

