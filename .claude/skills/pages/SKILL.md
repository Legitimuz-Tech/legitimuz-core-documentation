---
name: pages
description: Como uma página deste repo é feita — onde o arquivo mora, o frontmatter que sempre setamos, o formato de guia de integração e as armadilhas de MDX. Use antes de criar ou editar qualquer .mdx.
---

# Páginas

A skill `mintlify` é a autoridade sobre o **produto** (lista completa de componentes, campos de
frontmatter, schema do `docs.json`). Carregue ela para "o que existe". Esta aqui é "o que fazemos
neste repo", e é ela que vale quando as duas divergirem.

## Leia primeiro

| Arquivo         | Exemplifica                                              |
| --------------- | -------------------------------------------------------- |
| `index.mdx`     | página de entrada com cards — ainda no texto do starter  |
| `quickstart.mdx`| guia com `<Steps>` — ainda no texto do starter           |
| `docs.json`     | onde a página precisa ser registrada (skill `navigation`) |

⚠️ Os dois `.mdx` são **placeholder do starter da Mintlify**, não exemplo de estilo nosso. Copie a
estrutura (frontmatter, `<Steps>`, `<Card>`), nunca o texto.

## Onde o arquivo mora

```
index.mdx                 # entrada
quickstart.mdx            # primeiro sucesso do integrador
<secao>/<pagina>.mdx      # uma pasta por seção de navegação
images/                   # todo asset de imagem
snippets/                 # conteúdo reusado em mais de uma página
drafts/                   # rascunho — ignorado pelo build (.mintignore)
```

- Nome de arquivo em **kebab-case**, sem acento: `integracao-web-sdk.mdx`.
- Pasta espelha o grupo de navegação. Se a página não cabe em nenhum grupo existente, o problema é
  de navegação, não de arquivo — resolva lá primeiro.
- Rascunho vai em `drafts/` ou `*.draft.mdx`. O `.mintignore` já exclui os dois do deploy; é assim
  que se trabalha em página não publicável sem branch de longa duração.

## Frontmatter

`title` e `description` em **toda** página. Sem exceção — `description` é o que aparece na busca e
no resultado do Google, e página sem ela vira um link mudo.

```mdx
---
title: "Integração com o Web SDK"
description: "Instale o @legitimuz/websdk e abra o primeiro fluxo de verificação."
---
```

- `title` em **sentence case**, com a primeira letra maiúscula e o resto não (`Integração com o Web
  SDK`, não `Integração Com O Web SDK`). Nome de produto mantém o case dele.
- `description` é uma frase, com ponto final, dizendo o que o leitor sai sabendo fazer. Não repita
  o título.
- `sidebarTitle` só quando o `title` não couber na sidebar.
- `icon` só se **todo** o grupo tiver ícone. Grupo com metade dos itens com ícone fica pior que
  nenhum.

## Formato de guia de integração

É o formato que mais vamos escrever. A ordem não é negociável, porque é a ordem em que o integrador
trava:

1. **Uma frase** dizendo o que ele terá no fim.
2. **Pré-requisitos** — conta, credencial, versão mínima. Antes de qualquer código.
3. **`<Steps>`** com o caminho feliz. Um passo faz uma coisa e mostra o código dela.
4. **Verificação** — como ele sabe que funcionou (o evento que chega, a tela que aparece).
5. **Próximo passo** em `<Card>`.

Erro e caso de borda vão **depois** do caminho feliz, em `<Accordion>`, nunca intercalados nos
passos. Guia que trata o erro no passo 2 faz o leitor achar que ele já errou.

## Componentes

Use os da Mintlify; não escreva HTML nem componente próprio para o que já existe. A escolha de qual
está na skill `mintlify`. O que é decisão **daqui**:

- **`<Warning>` é reservado para o que causa dano**: perder dado do titular, quebrar produção do
  integrador, expor credencial. Usar em "atenção, a ordem importa" gasta o único componente que
  ainda é lido de verdade.
- **Um `<Tip>` por página, no máximo.** O starter abre com um; ele sai quando a página for escrita.
- **`<CodeGroup>` sempre que houver mais de uma forma de instalar** (npm e CDN, por exemplo). Duas
  seções separadas fazem o leitor seguir a errada.
- **Nada de emoji e nada de negrito decorativo.** Negrito é para elemento de UI (`clique em
  **Configurações**`) e para o termo que a frase define.

## Onde o MDX morde

| Situação                                | O que acontece                                                                 |
| --------------------------------------- | ------------------------------------------------------------------------------- |
| `<` solto no texto (`use < 100ms`)      | o MDX tenta abrir uma tag e o build quebra. Use crase ou `&lt;`                 |
| `{` solto no texto (`{token}`)          | vira expressão JSX e some da página. Use crase                                 |
| Bloco de código sem linguagem           | perde o highlight. **Toda** cerca leva linguagem — `bash`, `ts`, `json`, `mdx`  |
| Link com extensão (`/quickstart.mdx`)   | 404 no site. Sempre root-relative sem extensão: `/quickstart`                   |
| Link relativo (`../quickstart`)         | não é suportado — root-relative sempre                                          |
| Imagem sem `alt`                        | passa no build e falha no `mint a11y`                                           |
| Página criada e não registrada          | some da sidebar (fica acessível só por URL) — ver skill `navigation`            |
| `mint.json`                             | formato morto. A config deste repo é **só** `docs.json`                         |

Case de arquivo importa no deploy e **não** importa no macOS: `/images/Logo.png` referenciado como
`/images/logo.png` funciona local e 404 em produção. Escreva tudo minúsculo.

## Incerteza

Se você não confirmou um valor no repo de produto (default, nome de campo, código de erro), **não
chute** — marque e siga:

```mdx
{/* TODO: confirmar o timeout default no packages/websdk/src/defaults.ts */}
```

Um TODO visível no diff custa uma pergunta. Um valor errado publicado custa um ticket de suporte
por semana. Ver skill `domain` para onde confirmar cada assunto.

## Verificação

```bash
mint dev            # sobe o preview em localhost:3000
mint validate       # erro de build e de schema
mint broken-links   # link interno quebrado
mint a11y           # alt faltando, hierarquia de heading
```

Abra a página no preview antes de dizer que terminou. O `validate` não pega texto do starter
esquecido no meio da página; o olho pega.
