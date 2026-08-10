---
name: code-samples
description: Como escrever exemplo de código que não mente — de onde copiar a assinatura, o que verificar antes de publicar e como o snippet fica sincronizado com o repo de produto. Use ao adicionar ou alterar qualquer bloco de código numa página.
---

# Exemplos de código

O exemplo é a única parte da página que o integrador copia sem ler. Ele é lido como contrato, e
envelhece em silêncio: o produto muda, a página não, e o erro só aparece no chamado de suporte.

Por isso a regra deste repo é uma só — **exemplo é copiado da fonte, não escrito de memória.**

## Antes de escrever o bloco

1. Abra o arquivo canônico do assunto (tabela na skill `domain`).
2. Copie o nome exato do símbolo, dos campos e dos valores default de lá.
3. Se algo não estiver na fonte, é `{/* TODO: confirmar ... */}`, não uma suposição plausível.

Assinatura, nome de opção, nome de evento e código de erro têm **um** dono, e não é esta página.

## Forma do bloco

- **Toda** cerca leva linguagem: `bash`, `ts`, `tsx`, `json`, `html`, `mdx`. Sem exceção.
- Título no bloco quando houver mais de um na seção, em sentence case:
  ` ```ts title="Montar o fluxo" `.
- **Valor realista, nunca `foo`/`bar`.** `reference_id: "pedido-4471"` ensina; `foo` não.
- **Um exemplo bom, não três variações.** Variação vira `<CodeGroup>` só quando são caminhos
  genuinamente alternativos (npm × CDN), não gostos.
- Sem `// ...` no meio de um trecho que o leitor precisa colar inteiro. Se ficou grande demais para
  colar, o exemplo está fazendo duas coisas.
- Nada de `console.log` decorativo nem `try/catch` vazio: o leitor copia isso também.

## Placeholder e dado

Todo dado é fictício e todo segredo é placeholder — a regra completa está na skill `privacy`, e ela
não tem exceção para "é só um exemplo".

```ts
import { mount } from "@legitimuz/websdk";

mount({
  sdkUrl: "<SUA_SDK_URL>",
  onEvent: (event) => console.log(event.type),
});
```

Placeholder em MAIÚSCULA entre `<>`: o leitor vê o que ele tem que trocar sem ler a prosa em volta.

## Versão

Comando de instalação envelhece por versão. Prefira a forma que não fixa:

```bash
npm install @legitimuz/websdk
```

Se a página **precisa** citar uma versão (breaking change, tag de CDN, URL com hash de SRI), cite-a
uma vez, num lugar só, e diga de onde ela sai. Versão repetida em cinco páginas é cinco lugares para
esquecer — se chegou nisso, é caso de `snippets/`.

## Reuso: `snippets/`

Trecho idêntico em mais de uma página vai para `snippets/` e é importado. Bloco de instalação e
bloco de autenticação são os candidatos naturais.

Não use snippet quando cada página precisa de uma variação: a prop que resolve isso torna o snippet
mais difícil de ler que a duplicação que ele evitou.

## Verificação

**Rode o exemplo antes de publicar.** Não "confira se parece certo" — rode.

- Trecho de SDK: cole no playground do `legitimuz-core-websdk` (`apps/playground/`) e veja o evento
  chegar. É o harness que existe exatamente para isso.
- `curl` de API: execute contra sandbox e confira o shape da resposta que você colou na página.
- Comando de terminal: rode num diretório limpo.

Depois:

```bash
mint validate       # bloco malformado quebra o build
mint broken-links
```

Se você não conseguiu rodar (falta credencial, falta ambiente), diga isso no PR. Exemplo não
verificado é aceitável quando declarado, e não é aceitável quando calado.

## Quando o produto muda

Mudou assinatura pública num dos repos de produto? A doc muda no **mesmo** ciclo, não depois. O
caminho é o inverso do normal: encontre as páginas afetadas antes de abrir o PR do produto.

```bash
git grep -n 'mount(' -- '*.mdx'
git grep -n 'sdkUrl\|onEvent\|session_token' -- '*.mdx'
```

Página que descreve um símbolo removido é o pior estado possível — pior que não ter página, porque o
integrador confia nela e culpa o produto.
