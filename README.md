# legitimuz-core-documentation

Site público de documentação da Legitimuz, construído em [Mintlify](https://mintlify.com). É o que
o **integrador** lê para embutir o fluxo de KYC/biometria no produto dele, e o que o **analista** lê
para operar o dashboard de compliance.

Regras do projeto, convenções de commit e as armadilhas do preview local estão em
**[`CLAUDE.md`](./CLAUDE.md)**. Guia de instruções por skill para agentes está em
**[`AGENTS.md`](./AGENTS.md)**. Leia um dos dois antes de editar qualquer coisa.

## Stack

- **Site**: Mintlify — MDX + `docs.json`, sem build próprio para manter.
- **Conteúdo**: páginas `.mdx` com frontmatter `title`/`description`, organizadas por pasta por
  grupo de navegação (`help/`, `api/`, `concepts/`, `dashboard/`, `ai/`, `web-sdk/`).
- **Visual**: tokens `--lz-*` do dashboard em `custom.css`, fontes Open Runde self-hosted em
  `fonts/`, logotipo e favicon em `logo/` e `favicon.svg`.
- **Deploy**: GitHub app da Mintlify — automático a cada push na branch default. Não há CI nem
  revisão automática.

## Desenvolvimento

Instale o [Mintlify CLI](https://www.npmjs.com/package/mint):

```bash
npm i -g mint
```

Na raiz do repo, onde está o `docs.json`:

```bash
mint dev
```

Preview local em `http://localhost:3000`. Antes de qualquer commit, rode a cadeia de verificação:

```bash
mint validate && mint broken-links && mint a11y
```

O `mint dev` passa em coisas que quebram em produção (case-sensitivity de asset, página não
registrada no `docs.json`, link relativo) — a lista completa está no `CLAUDE.md`.

## Publicação

O push na branch default dispara o deploy automaticamente via GitHub app da Mintlify. Não há gate
manual: a cadeia de verificação acima é a única checagem antes de publicar.

## Estrutura

```
docs.json              # navegação, tema, cor, links, redirects
custom.css             # tokens --lz- herdados do dashboard
index.mdx              # entrada
quickstart.mdx         # primeiro sucesso do integrador
<secao>/<pagina>.mdx    # uma pasta por grupo de navegação
docs/                   # ADRs e material interno — nunca publicado (.mintignore)
drafts/                 # rascunho, fora do deploy
.mintignore             # o que não é publicado
```
