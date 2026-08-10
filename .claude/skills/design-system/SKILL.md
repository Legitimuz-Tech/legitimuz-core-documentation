---
name: design-system
description: Tokens de cor, tipografia, raio, espaçamento e os padrões de layout herdados do produto — e como cada um vira config da Mintlify, custom.css ou componente MDX. Use antes de mexer em docs.json, custom.css, logo, ou de propor qualquer coisa visual.
---

# Design system

## Cor

| Papel                    | Hex       | Onde aparece no produto                       |
| ------------------------ | --------- | --------------------------------------------- |
| **Marca / ação primária**| `#fd7138` | botão "New verification", "Save Changes"      |
| Borda da ação primária   | `#e95619` | 1px mais escuro que o preenchimento           |
| Item de navegação ativo  | `#fd7138` | label e sublinhado da tab ativa               |
| Texto forte              | `#171717` | corpo, rótulo de campo                        |
| Título                   | `#161616` | título de página e de card                    |
| Texto secundário         | `#606060` | descrição, label de botão secundário          |
| Texto suave              | `#5c5c5c` | valor de tabela, ícone de menu                |
| Ícone soft               | `#a3a3a3` | ícone inativo, placeholder                    |
| Borda                    | `#ebebeb` | **toda** borda: card, input, botão, linha     |
| Fundo fraco              | `#f7f7f7` | sidebar, header de tabela, item ativo do menu |
| Fundo                    | `#ffffff` | card, conteúdo                                |

Estados — cada um é um par `base` (texto e borda) + `lighter` (preenchimento):

| Estado      | base      | lighter   | Uso na doc                          |
| ----------- | --------- | --------- | ----------------------------------- |
| Sucesso     | `#189050` | `#def9ea` | `<Check>`                           |
| Erro        | `#fb3748` | `#ffebec` | `<Warning>`, `<Danger>`             |
| Informação  | `#335cff` | `#ebf1ff` | `<Info>`, `<Note>`                  |
| Atenção     | `#fa7319` | `#fff3eb` | `<Tip>` — quase a cor de marca      |

⚠️ `#fa7319` (atenção) e `#fd7138` (marca) são vizinhos. No produto isso convive porque o botão é
sólido e o badge é claro; na doc, **não** use o laranja de marca para significar aviso, nem o
laranja de aviso para significar ação. Um é "clique aqui", o outro é "cuidado".

**Como vira config:** `docs.json → colors` aceita só três hex — `primary`, `light` (usado no modo
escuro) e `dark`. `primary` é `#fd7138`; `dark` é `#e95619` (a borda do botão, o tom escuro natural
da marca); `light` é `#ff8a5c`, **derivado** por clareamento — não existe no Figma. Se o design
publicar um tom de marca para fundo escuro, troque este.

Todo o resto vive em `custom.css` como `--lz-*`.

## Tipografia

**Open Runde em tudo**, título e corpo — é a fonte do projeto inteiro. Não existe segunda família:
se você viu Inter em algum lugar, é resquício.

| Papel               | Peso           | Tamanho / linha | Tracking   |
| ------------------- | -------------- | --------------- | ---------- |
| Título de página    | Medium (500)   | 24 / 28         | —          |
| Descrição de página | Regular (400)  | 16 / 20         | —          |
| Label (botão, menu) | Medium (500)   | 14 / 20         | `-0.084px` |
| Parágrafo pequeno   | Regular (400)  | 14 / 20         | `-0.6%`    |
| Label X Small       | Medium (500)   | 12 / 16         | 0          |

Note a densidade: título de 24px com linha de 28 é **apertado de propósito**, e a descrição vem
logo abaixo em cinza, não em preto. É a assinatura do header do produto — a doc repete isso no par
`title` + `description` do frontmatter.

⚠️ Algumas variáveis de texto do Figma ainda dizem `Inter` (`Paragraph/Small`, `Label/X Small`).
São tokens legados; o produto renderiza Open Runde. **Não** reintroduza Inter por causa delas.

### Como a fonte é servida

Open Runde não está no Google Fonts, então ela é **self-hosted**: os quatro `.woff2` vivem em
`fonts/`, vindos de `legitimuz-core-dashboard/src/assets/fonts/open-runde/`.

O `docs.json` registra dois pesos — é o que o schema permite, um por papel:

| Papel     | Peso | Arquivo                       |
| --------- | ---- | ----------------------------- |
| `heading` | 600  | `OpenRunde-Semibold.woff2`    |
| `body`    | 400  | `OpenRunde-Regular.woff2`     |

Os outros dois entram por `@font-face` no `custom.css`: **500** (Medium, o peso de label e título
do produto) e **700** (Bold). Sem eles, `**negrito**` e qualquer peso 500 caem em **bold sintético**
— o browser engorda o traço da Regular, e o resultado destoa do produto de um jeito difícil de
nomear e fácil de ver.

Ao trocar qualquer arquivo: `source` apontando para caminho inexistente **não quebra o build**, só
cai no fallback em silêncio. Confira no `mint dev`.

⚠️ Open Runde é licenciada sob a **SIL OFL 1.1**, que exige a licença junto do arquivo quando ele é
redistribuído — e este repo é público. O `.woff2` veio do dashboard, que também não carrega o texto
da licença. Traga o `OFL.txt` para `fonts/` antes de o site ir ao ar.

## Raio, borda e sombra

| Elemento              | Raio  |
| --------------------- | ----- |
| Botão, input, card    | 12px  |
| Container interno     | 8px   |
| Badge, avatar, pill   | full  |

- **Borda de 1px em tudo, `#ebebeb`.** É ela que separa, não sombra.
- **Sem sombra.** Os dois frames não têm nenhuma. Profundidade vem de borda + fundo `#f7f7f7`.
- O botão primário tem borda `#e95619` sobre preenchimento `#fd7138`. Parece detalhe e é o que dá o
  acabamento — botão sólido sem borda fica chapado ao lado dos secundários.

## Espaçamento e densidade

Escala: **4 · 6 · 8 · 10 · 12 · 16 · 24 · 32**.

| Medida                          | Valor        |
| ------------------------------- | ------------ |
| Padding horizontal do conteúdo  | 32px         |
| Altura de controle (botão/input)| 36px         |
| Altura de item de menu          | 32px         |
| Linha de tabela                 | 62px         |
| Ícone padrão                    | 20px         |
| Ícone em contexto denso         | 16px         |
| Gap entre ações                 | 12px         |
| Gap ícone → label               | 4px a 6px    |

## Layout

O produto é `rail 64px + navegação 240px + conteúdo`. A Mintlify tem a própria moldura (sidebar +
TOC) e **não vale a pena lutar com ela**: o que herdamos é o miolo.

Do header de página do produto, que a doc repete página a página:

1. Título curto, denso, sem prefixo.
2. Descrição de uma frase em cinza, imediatamente abaixo.
3. A ação primária vive **no canto superior direito da seção**, não no fim da página. Na doc, o
   equivalente é o `<Card>` de próximo passo no topo quando a página é um índice.

Do card do produto: **título + descrição + ação, nessa ordem, com a ação alinhada à direita do
título**. É o padrão de seção — na doc, um `##` seguido de uma linha de contexto antes do conteúdo.

## Ícones

O produto usa Remix Icon (linha, grade de 20px, cantos arredondados). A Mintlify não oferece Remix;
a biblioteca mais próxima em peso e forma é **Lucide**, e é a que está no `docs.json`
(`icons.library`). Não misture: escolher ícone de outra biblioteca no meio quebra a espessura de
traço da página.

## O que é config, o que é CSS, o que é componente

| Quero mudar                            | Onde                                              |
| -------------------------------------- | ------------------------------------------------- |
| Cor de marca, fonte, biblioteca de ícone | `docs.json`                                     |
| Token novo, ajuste fino de elemento    | `custom.css`                                      |
| Card, badge, passo, aviso              | componente MDX da Mintlify — **nunca HTML na mão** |

**Não recrie componente do produto dentro do `.mdx`.** Um "card" feito com `<div style=...>` não
herda tema, não tem modo escuro, não é responsivo e envelhece na primeira mudança de token. Se o
componente da Mintlify não faz o que você quer, o ajuste vai no `custom.css` mirando o componente
dela — e só depois de você ter visto o seletor real no `mint dev`.

`custom.css` guarda **tokens** (`--lz-*`) por padrão. Regra visual só entra depois de verificada no
preview: o HTML da Mintlify é interno e muda entre versões, então seletor chutado é regra morta que
ninguém percebe.

## Marca

| Arquivo          | O que é                | Origem                                              |
| ---------------- | ---------------------- | --------------------------------------------------- |
| `logo/light.svg` | logotipo, texto escuro | `legitimuz-core-dashboard/src/assets/brand/logotype.svg` |
| `logo/dark.svg`  | o mesmo, texto branco  | idem, com `#282828` → `#FFFFFF`                     |
| `favicon.svg`    | símbolo 28×28          | `.../brand/symbol.svg`                              |

O `logotype.svg` do dashboard tem um `<rect fill="white">` de fundo. Ele foi **removido** nas duas
cópias: numa navbar escura, aquele retângulo vira um bloco branco atrás do logo. Se você reimportar
o arquivo do dashboard, remova de novo.

O laranja do logotipo é `#fd7b46`, não `#fd7138`. A diferença é imperceptível lado a lado e os dois
são corretos: `#fd7138` é a **ação** (botão, link, item ativo — é o que `colors.primary` controla) e
`#fd7b46` é a **marca** no símbolo. Não unifique um no outro sem o design; está em `custom.css` como
`--lz-brand-logo`.

## Ainda pendente

Coisas que este arquivo não resolve e dependem de você:

- URL do dashboard para o botão primário da navbar, e o canal de suporte.
- Tom de marca para fundo escuro (hoje `light` é derivado por clareamento).
- O `OFL.txt` da Open Runde em `fonts/`.

## Verificação

```bash
mint validate   # docs.json contra o schema — pega hex inválido e campo desconhecido
mint dev        # o resto: cor, fonte, contraste
```

Cor nova passa por contraste antes de virar texto: `#a3a3a3` sobre branco não alcança AA para
corpo, e no produto ele só aparece em ícone e placeholder. Mantenha assim.
