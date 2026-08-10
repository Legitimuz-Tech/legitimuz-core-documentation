# ADR: arquitetura de navegação e primeiro preenchimento

- **Data:** 2026-08-09
- **Status:** aceita
- **Fontes:** pesquisa de concorrentes em
  `legitimuz-core-vault/raw/research/legitimuz/concorrentes/` (Didit, Unico, Persona, Sumsub,
  Veriff, SEON, Serasa Datatrust) e levantamento da superfície implementada em
  `legitimuz-core-websdk`, `legitimuz-core-verification-web`, `legitimuz-core-dashboard` e
  `legitimuz-core-api`.

## Contexto

O site saiu do starter e precisava da primeira estrutura real. A pesquisa de categoria mostra
consenso em três pontos: split guia × referência de API visível na URL, corte secundário por
audiência/estágio (não por formato), e páginas canônicas para contratos transversais (status,
eventos, erros, webhooks). O contraexemplo também é claro: a Didit duplica o contrato de webhook em
satélites por produto e eles divergem; a Unico bifurca PT/EN em tabs e as duas árvores descolam.

Do lado do produto, só uma parte da plataforma tem contrato implementado hoje: o Web SDK
(`@legitimuz/websdk` + `-react`) e o fluxo de verificação (`verification-web`). A API pública de
integrador (`get-sdk-url`, `x-api-key`), o disparo de webhooks e as telas de operação do dashboard
estão nas Fases 2–4 da DD-17 — documentá-los como disponíveis seria falso.

## Decisão

1. **Duas tabs — `Documentação` e `API`** (split guia × referência, o consenso da categoria).
   Dentro de `Documentação`, grupos por audiência: `Comece aqui` → `Conceitos` → `Web SDK` →
   `Dashboard` → `Ajuda`. *(Revisado em 2026-08-10: a versão original desta ADR adiava tabs; o
   dono do repo pediu a malha de menus dos concorrentes, e a tab API entrou com as decisões já
   fechadas da DD-17.)*
2. **Só se publica o que tem fonte.** Contrato implementado é documentado por inteiro; contrato
   **decidido mas não implementado** (DD-17) é publicado com `tag: "Em breve"` na sidebar, aviso
   `<Info>` no topo e TODO nos pontos abertos — nunca com endpoint, campo ou header inventado.
3. **Uma página canônica por contrato transversal**: eventos em `/web-sdk/eventos`, erros em
   `/web-sdk/erros`. Outras páginas linkam, nunca repetem tabela.
4. **`/errors/:code` redireciona para `/web-sdk/erros`.** O `doc_url` dos erros do Web SDK
   (`errors.ts`) já aponta para `docs.legitimuz.com/errors/<code>`; a URL é contrato desde já.
5. **Idioma pt-BR** — já fixado na skill `domain`. Inglês, se vier, será camada de i18n
   (`languages` da Mintlify), nunca fork de árvore (lição Unico).

## Alternativas descartadas

- **Tab por produto (estilo Didit)** — não há conteúdo para preencher; tab vazia é pior que grupo.
- **Documentar a API pública desde já** — sem OpenAPI e sem endpoint implementado, cada nome seria
  invenção. Vai para `drafts/`.
- **Página por código de erro (estilo Didit `warnings-*`)** — 20+ páginas para manter; o redirect
  paramétrico entrega a mesma URL pública com uma página só.

## Consequências

- Quando `get-sdk-url` (Fase 2) sair, promove-se `drafts/api/` para um grupo `API` — e aí sim
  avalia-se tab `Referência de API` com `openapi` no `docs.json`.
- Divergências encontradas entre repos foram anotadas como TODO nas páginas, não resolvidas aqui:
  URL de CDN só existe em prosa do README do websdk; `6005` tem `recoverable` diferente nos dois
  catálogos; credencial migrou de query para fragmento (`#v=`) e o README do websdk ainda mostra o
  formato antigo; `sandboxScenario` não tem consumidor no `verification-web`.
