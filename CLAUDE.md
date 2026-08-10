# legitimuz-core-documentation

Site público de documentação da Legitimuz, construído em Mintlify. É o que o **integrador** lê para
embutir o fluxo de KYC/biometria no produto dele, e o que o **analista** lê para operar o dashboard
de compliance.

Duas restrições moldam quase toda decisão daqui, e nenhuma é detalhe:

1. **Tudo aqui é público.** O site é indexado e o repo espelha o site. PII de titular, credencial,
   nome de cliente ou print com dado real que entrem num commit ficam no git para sempre — o `git rm`
   não desfaz. É o único assunto do repo sem botão de voltar.
2. **Esta doc é derivada.** Nenhuma decisão de produto nasce aqui. Assinatura, nome de campo, código
   de erro e default têm dono, e o dono é o repo de produto. Doc que inventa um nome é pior que doc
   ausente, porque é confiada.

## Estado atual

O repo saiu do **starter da Mintlify** pela metade. O que já é nosso:

- `docs.json` — nome, cor de marca, fontes e biblioteca de ícone vêm do design do dashboard; as
  âncoras, o suporte e os socials da Mintlify foram removidos.
- `custom.css` — a camada de tokens `--lz-*` e os pesos da Open Runde que o `docs.json` não registra.
- `logo/` e `favicon.svg` — logotipo e símbolo reais, vindos do dashboard.
- `fonts/` — os quatro `.woff2` da Open Runde, self-hosted.

O que ainda é do starter:

- `index.mdx` e `quickstart.mdx` são texto placeholder. Copie a estrutura, nunca o conteúdo.
- Falta URL do dashboard e canal de suporte para a navbar, e o `OFL.txt` da Open Runde.
- Não há `images/`, `snippets/` nem spec de OpenAPI ainda.

A lista completa do que falta está no fim da skill `design-system`.

Não conserte isso de passagem no meio de outra tarefa: marca e navegação são mudança visível, e
merecem commit próprio.

## Vocabulário

| Termo          | O que é aqui                                                      |
| -------------- | ----------------------------------------------------------------- |
| **titular**    | a pessoa sendo verificada                                         |
| **integrador** | o cliente que instala o SDK — quem lê a maior parte das páginas   |
| **analista**   | quem opera o dashboard de compliance                              |
| **host**       | a página do integrador que embute o widget                        |
| **embed**      | o app de verificação servido em `verify.legitimuz.com`            |
| **step**       | uma tela do fluxo, decidida pelo servidor                         |
| **página oculta** | `.mdx` que existe mas não está em `docs.json` — some da sidebar |

Lista completa, com os produtos (LegitFace, LegitID, LegitDoc, LegitCheck, ECA Digital) e os
sinônimos a evitar, na skill `domain`.

## Stack

| Camada    | Escolha                             | Por quê                                                    |
| --------- | ----------------------------------- | ---------------------------------------------------------- |
| Site      | Mintlify                            | MDX + um arquivo de config; sem build próprio para manter  |
| Conteúdo  | `.mdx` com frontmatter YAML         | `title` e `description` em toda página                     |
| Config    | `docs.json`                         | **nunca** `mint.json` — formato morto                      |
| Visual    | tokens do dashboard em `custom.css` | mesma marca do produto — ver skill `design-system`         |
| Preview   | `mint` CLI (`npm i -g mint`)        | `dev`, `validate`, `broken-links`, `a11y`                  |
| Assets    | `images/` na raiz                   | referência root-relative (`/images/x.png`), tudo minúsculo |
| Reuso     | `snippets/`                         | só para trecho **idêntico** em mais de uma página          |
| Rascunho  | `drafts/` e `*.draft.mdx`           | já excluídos do deploy pelo `.mintignore`                  |
| Deploy    | GitHub app da Mintlify              | automático no push da branch default — não há gate manual  |

## Estrutura

```
docs.json              # o site inteiro: navegação, tema, cor, links, redirects
custom.css             # tokens `--lz-*` herdados do dashboard
index.mdx              # entrada
quickstart.mdx         # primeiro sucesso do integrador
<secao>/<pagina>.mdx   # uma pasta por grupo de navegação
images/                # todo asset
snippets/              # conteúdo importado por mais de uma página
drafts/                # rascunho, fora do deploy
.mintignore            # o que não é publicado
```

Pasta espelha grupo de navegação. Se a página não cabe em grupo nenhum, o problema é de navegação —
resolva no `docs.json` antes de criar o arquivo.

## Antes de editar, carregue a skill do assunto

| Vou mexer em                                      | Skill            | Arquivo canônico para ler |
| ------------------------------------------------- | ---------------- | ------------------------- |
| Componente, frontmatter, schema da Mintlify       | `mintlify`       | —                         |
| Página nova, frontmatter, estrutura de guia, MDX  | `pages`          | `quickstart.mdx`          |
| `docs.json`, grupo, tab, redirect, página oculta  | `navigation`     | `docs.json`               |
| Cor, fonte, raio, logo, `custom.css`, layout      | `design-system`  | `custom.css`              |
| Prosa, título, voz, termo proibido                | `writing`        | —                         |
| Bloco de código, snippet, versão, `curl`          | `code-samples`   | —                         |
| Nome de produto, termo do fluxo, audiência        | `domain`         | —                         |
| Exemplo com dado, payload, screenshot, token      | `privacy`        | `.mintignore`             |

A skill `mintlify` é a autoridade sobre o **produto**. As outras são a convenção **deste repo** — e
elas vencem quando as duas divergirem.

## Fluxo de trabalho

1. **Carregue a skill.** Ela tem a armadilha que a doc da lib não tem.
2. **Ache a fonte antes de escrever.** Símbolo público sai do repo de produto, não da memória — a
   tabela de onde mora a verdade está na skill `domain`.
3. **Leia 2–3 páginas parecidas** antes de escrever a nova, para pegar o tom. Enquanto só existir
   texto do starter, isso não se aplica: siga a skill `pages`.
4. **Registre a página no `docs.json`** no mesmo commit em que a cria. Página não registrada fica
   oculta, e isso passa despercebido.
5. **Rode a cadeia local:**
   `mint validate && mint broken-links && mint a11y`, mais `mint dev` para conferir com o olho.

⚠️ **Não há CI nem revisão automática neste repo, e o deploy é automático no push.** A cadeia acima
é a única verificação que existe, e ela é sua responsabilidade antes de commitar.

## Regras que não se negociam

- **Nada é commitado sem pedido explícito.** Formato abaixo.
- **PII, credencial e nome de cliente nunca entram em arquivo nenhum.** Nem em exemplo, nem em
  screenshot, nem em payload de resposta. Dado fictício, placeholder em maiúscula, CPF com máscara
  neutra. Ver skill `privacy` — segredo que já foi commitado precisa ser **rotacionado**, não só
  apagado.
- **Exemplo de código é copiado da fonte e rodado antes de publicar.** Não verificou? Diga no PR.
  Exemplo não verificado declarado é aceitável; calado, não.
- **Valor que você não confirmou vira TODO, não suposição.** `{/* TODO: confirmar o default em ... */}`
  custa uma pergunta; valor errado publicado custa um ticket por semana.
- **URL publicada é contrato.** Mover ou renomear página exige `redirects` no `docs.json`, no mesmo
  commit.
- **Toda cerca de código leva linguagem, toda imagem leva `alt`, toda página leva `description`.**
- **Link interno é root-relative e sem extensão** (`/quickstart`). Nada de `../`, nada de `.mdx`.
- **Uma página serve uma audiência.** Guia de integração que para para explicar compliance perdeu as
  duas.
- **Sem marketing, sem emoji, sem "basta"/"simplesmente".** Ver skill `writing`.

## Commits

**Conventional Commits em inglês**, subject no imperativo com verbo de ação (`add`, `update`,
`remove`, `fix`, `move`, `rename`). **Só o assunto, sem body.**

```
docs(quickstart): add web sdk installation steps
docs: replace starter placeholder on the introduction page
fix(navigation): correct the redirect for the renamed events page
chore: replace mintlify starter branding in docs.json
```

Tipos: `build` · `chore` · `ci` · `docs` · `feat` · `fix` · `perf` · `refactor` · `revert` ·
`style` · `test`. Neste repo, na prática: `docs` para conteúdo, `fix` para informação errada
publicada, `chore` para config e marca, `feat` para capacidade nova do site (referência de API,
reestruturação de navegação). Escopo é a seção ou a página.

Proibido:

- **Body.** O porquê vai no PR, não na mensagem. Uma linha por commit.
- **Co-author de agente.** Nunca `Co-Authored-By: Claude`, Copilot ou similar.
- **Assinatura de agente.** Nunca "Generated with Claude Code" nem equivalente.

Diferente dos repos de produto, aqui **não há commitlint**: isto é convenção, não regra de máquina.
Se virar problema, o setup dos outros repos (`commitlint` + `lefthook` no hook `commit-msg`) é
copiável — `legitimuz-core-verification-web/commitlint.config.js` é o mais próximo do que
precisaríamos.

## Onde o preview mente

`mint dev` passa em coisas que quebram em produção:

| Situação                                   | O que acontece                                                    |
| ------------------------------------------ | ----------------------------------------------------------------- |
| `/images/Logo.png` referenciado minúsculo  | funciona no macOS (FS case-insensitive) e **404 no deploy**       |
| Página criada e não registrada             | build passa; ela some da sidebar e você não percebe               |
| Link com `.mdx` ou relativo (`../`)        | o `dev` pode resolver; o site publicado não                       |
| `<` ou `{` solto no texto                  | o MDX interpreta como tag/expressão — quebra ou some da página    |
| Texto do starter esquecido no meio         | nenhum comando pega. Só o olho no preview                         |
| `mint.json` junto do `docs.json`           | o deploy escolhe o errado — nunca crie                            |
| Página em `drafts/` linkada de uma publicada | link quebrado só em produção: o `.mintignore` não publica o alvo |

## Histórico

Os padrões deste repo (formato de skill, `CLAUDE.md`, convenção de commit, comentário curto com o
porquê) vêm dos repos de produto — `legitimuz-core-dashboard`, `legitimuz-core-websdk` e
`legitimuz-core-verification-web`. Quando uma decisão daqui parecer excesso, o `docs/adr/` daqueles
repos costuma ter o porquê e a alternativa descartada.

Este repo ainda não tem `docs/adr/` nem `docs/plans/`. Quando a primeira decisão estrutural aparecer
(idioma do site, versionamento, referência de API gerada por OpenAPI), ela vira ADR aqui — não
comentário no `docs.json`.
