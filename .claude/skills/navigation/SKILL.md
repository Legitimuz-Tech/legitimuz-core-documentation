---
name: navigation
description: Como registrar uma página, organizar grupos e tabs no docs.json, e o que ainda é resquício do starter da Mintlify. Use ao criar página, mover conteúdo ou mexer em qualquer coisa fora do corpo de um .mdx.
---

# Navegação e `docs.json`

`docs.json` é o site inteiro: navegação, tema, cor, logo, links de topo e rodapé. É o arquivo mais
fácil de quebrar sem perceber, porque erro nele não aparece na página que você editou.

A skill `mintlify` tem o schema completo e a tabela de qual padrão de navegação usar quando. Aqui
está o estado **deste** repo e as decisões nossas.

## Estado atual: ainda é o starter

O `docs.json` não foi personalizado. Estes campos são da Mintlify, não nossos, e precisam cair antes
de qualquer publicação séria:

| Campo                | Hoje                                          | Precisa virar                          |
| -------------------- | --------------------------------------------- | -------------------------------------- |
| `name`               | `"Mintlify Starter Kit"`                      | o nome do nosso site                   |
| `colors`             | verde da Mintlify (`#16A34A`)                 | a paleta da Legitimuz                  |
| `navigation.global.anchors` | apontam para mintlify.com/docs e /blog | nossos recursos, ou remover            |
| `navbar.links`       | `mailto:hi@mintlify.com`                      | nosso canal de suporte                 |
| `navbar.primary`     | `app.mintlify.com`                            | o dashboard do cliente                 |
| `footer.socials`     | perfis da Mintlify                            | os nossos, ou remover                  |
| `logo` / `favicon`   | arte do starter                               | a marca da Legitimuz                   |

Não corrija isso "de passagem" no meio de outra tarefa — é mudança visível de marca, merece commit
próprio.

## Registrar uma página

Página que não está em `navigation` **fica oculta**: acessível por URL, invisível na sidebar. Isso é
um recurso (serve para página que só o assistente e a busca precisam achar), e é também o jeito mais
comum de "criei a página e ela não apareceu".

```json
{
  "group": "Integração",
  "pages": ["integracao/web-sdk", "integracao/eventos"]
}
```

O valor é o caminho do arquivo **a partir da raiz do repo, sem `.mdx`** — `integracao/web-sdk.mdx`
vira `"integracao/web-sdk"`.

A ordem do array é a ordem da sidebar, e ela é **editorial**: sequência que o leitor percorre, não
ordem alfabética. Página de referência que ninguém lê em ordem vai por último no grupo.

## Grupos, tabs e anchors

Hoje só existe o grupo `Getting Started`. Enquanto o site couber em poucos grupos, é o que basta —
não introduza tab antes de existir conteúdo para preencher.

Quando crescer, o corte que faz sentido para este produto é **por audiência**, não por formato (ver
skill `domain`):

- **Integração** — quem instala o SDK.
- **Dashboard** — quem opera o painel de compliance.
- **Referência de API** — gerada do OpenAPI (abaixo).

Tabs entram quando esses públicos pararem de se sobrepor. Grupo aninhado: no máximo um nível — a
partir do segundo, o leitor não acha o que já viu.

## Referência de API

Quando existir spec, o caminho é **gerar do OpenAPI**, não escrever endpoint à mão: página escrita à
mão diverge do backend na primeira versão e ninguém percebe.

```json
"openapi": ["openapi.yaml"]
```

Declare no nível do grupo ou da tab para as páginas herdarem. Endpoint manual (`api: "POST /..."` no
frontmatter) só para fluxo que a spec não descreve — e com a ressalva escrita na página.

## Mover ou renomear página

URL publicada é contrato. Ao mover ou renomear, registre o redirect no mesmo commit:

```json
"redirects": [{ "source": "/quickstart", "destination": "/integracao/web-sdk" }]
```

Sem isso, o link que o cliente colou no chamado dele vira 404 — e nós só descobrimos pelo suporte.
Depois do redirect, rode `mint broken-links` para achar os links internos que ainda apontam para o
caminho velho.

## Verificação

```bash
mint validate       # schema do docs.json e build
mint broken-links   # página órfã e link para caminho que não existe
mint dev            # confira a sidebar com o olho
```

`mint validate` valida o JSON contra o schema; ele **não** diz que você esqueceu de registrar a
página nova — para isso, olhe a sidebar no preview.

Nunca use `mint.json`: formato morto, e ter os dois no repo faz o deploy escolher o errado.
