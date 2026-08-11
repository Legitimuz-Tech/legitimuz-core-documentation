---
name: domain
description: Vocabulário da Legitimuz — nomes de produto, termos do fluxo de verificação, quem é audiência de cada página e onde mora a verdade de cada assunto. Use antes de escrever qualquer texto novo ou de nomear uma página.
---

# Domínio

Esta doc é **derivada**. Nenhuma decisão de produto nasce aqui: ela nasce no repo do produto e é
descrita aqui depois. Se você não achou a fonte, você não pode documentar o comportamento — pergunte,
não infira do que "faz sentido".

## Onde mora a verdade

| Assunto                                                | Repo                             | Arquivo canônico                          |
| ------------------------------------------------------ | -------------------------------- | ----------------------------------------- |
| `mount()`, opções, eventos, erros `6xxx`, CDN e npm    | `legitimuz-core-websdk`          | `packages/websdk/src/index.ts` · `types.ts` |
| Fluxo de verificação, steps, liveness, captura         | `legitimuz-core-verification-web` | `src/shared/contract/step.ts`             |
| Painel do analista, permissões, módulos                | `legitimuz-core-dashboard`       | `src/services/access/{resources.ts,use-access.ts,can.tsx}` (checagem) · `src/features/organization/schemas/role.schema.ts` (modelo de papel) |

Regra prática: se a página descreve um símbolo público (`mount`, `MountOptions`, um código de erro,
um nome de evento), **abra o arquivo canônico antes de escrever** e copie o nome exato. Doc que
inventa um nome de campo é pior que doc ausente — ela é confiada.

## Produtos

Nome de produto é **PascalCase colado**, sem espaço e sem hífen. Escreva sempre por extenso na
primeira menção da página.

| Nome           | O que é                                                        |
| -------------- | -------------------------------------------------------------- |
| **LegitFace**  | prova de vida e biometria facial                               |
| **LegitID**    | verificação de identidade do titular                           |
| **LegitDoc**   | captura e validação de documento                               |
| **LegitCheck** | consultas e verificações de background / AML                   |
| **ECA Digital**| verificação de idade para o Estatuto da Criança e do Adolescente |

**Legitimuz** é a plataforma; não é um produto vendável isolado e não vai em título de página como  
se fosse.

## Termos do fluxo

Vocabulário herdado do `verification-web` — use estes, não sinônimos:

| Termo         | O que é aqui                                                                |
| ------------- | --------------------------------------------------------------------------- |
| **host**      | a página do integrador que embute o widget                                  |
| **embed**     | o app de verificação servido em `verify.legitimuz.com`                      |
| **Web SDK**   | `@legitimuz/websdk` — o pacote que o integrador instala                     |
| **step**      | uma tela do fluxo, decidida pelo servidor                                   |
| **retake**    | nova tentativa de captura — decidida pelo **servidor**                      |
| **provider**  | vendor de liveness (`legitface`, `facetec`)                                 |
| **titular**   | a pessoa sendo verificada                                                   |
| **integrador**| o cliente que instala o SDK. É quem lê a maior parte destas páginas         |
| **analista**  | quem opera o dashboard de compliance                                        |

Evite: "usuário" (ambíguo entre titular e integrador), "modal"/"popup" (é `iframe`), "SDK" solto
quando existe mais de um (diga **Web SDK** ou **Mobile SDK**).

## Audiência decide o tom

| Audiência   | O que ela quer                                     | Onde                       |
| ----------- | -------------------------------------------------- | -------------------------- |
| Integrador  | copiar código e ver funcionando em 10 minutos      | guias de integração, SDK   |
| Analista    | achar o botão e entender o status                  | guias do dashboard         |
| Compliance  | saber o que é coletado, por quê, e por quanto tempo | páginas de política e dados |

Uma página serve **uma** audiência. Guia de integração que para no meio para explicar regra de
compliance perdeu as duas.

## Idioma

Conteúdo público em **pt-BR** — o cliente é brasileiro. Identificador, nome de campo, código de
erro e trecho de código ficam como estão no produto (inglês), sem tradução: traduzir `session_token`
para "token de sessão" dentro de um bloco de código cria um nome que não existe.

Doc interna do repo (`CLAUDE.md`, skills, ADR) em pt-BR. **Commit em inglês** — ver `CLAUDE.md`.
