---
name: writing
description: Voz, pessoa, tempo verbal e o que nunca entra no texto — adaptação para pt-BR das writing standards da Mintlify. Use antes de escrever ou revisar qualquer prosa de página.
---

# Escrita

A skill `mintlify` traz as writing standards da plataforma; elas são escritas para inglês. Esta aqui
é a versão que vale aqui, em **pt-BR**, com o que muda na tradução.

## Voz

- **Segunda pessoa, tratamento direto.** "Instale o pacote", "você recebe o evento". Nunca "o
  usuário deve instalar", nunca "instalaremos".
- **Imperativo em instrução.** "Adicione", "configure", "rode" — não "é necessário adicionar", não
  "você deveria configurar".
- **Voz ativa.** "O servidor decide o próximo step", não "o próximo step é decidido pelo servidor".
- **Presente.** "O SDK emite `ready`", não "o SDK irá emitir".
- **Uma ideia por frase.** Frase com duas orações subordinadas vira duas frases.

## Título e heading

Sentence case, sempre: **Primeira palavra maiúscula, o resto minúsculo**, salvo nome próprio e nome
de produto.

```
✅ Instalar o Web SDK
✅ Tratamento de erros
❌ Instalar O Web SDK
❌ Tratamento De Erros
```

Title case é hábito de inglês e em português fica errado, não só estranho. Heading descreve o que a
seção entrega — `Tratamento de erros`, não `Sobre erros`.

## O que nunca entra

**Marketing.** "Poderoso", "robusto", "de ponta", "solução completa", "sem esforço". Doc não vende;
quem chegou aqui já comprou.

**Encheção de linguiça.** "É importante notar que", "vale ressaltar", "com o objetivo de" (use
"para"), "neste artigo vamos ver", "além disso", "ademais", "por fim".

**Minimização.** "Simplesmente", "basta", "é só", "facilmente", "obviamente". Quando o leitor
travar — e ele vai — essas palavras dizem que o problema é ele.

**Introdução e conclusão genéricas.** Não abra com "a verificação de identidade é fundamental para
o compliance moderno" e não feche com "neste guia, vimos como...". Comece pelo que o leitor faz e
termine no próximo passo.

**Emoji, e negrito decorativo.** Negrito serve para elemento de UI (`clique em **Salvar**`) e para o
termo que a frase está definindo. Mais que isso, ninguém lê nenhum.

## Padrões de texto gerado

Revise o seu próprio texto procurando por:

- Frase em três partes por reflexo ("rápido, seguro e escalável").
- Travessão em excesso — um por parágrafo é o teto.
- Repetição do conceito com outras palavras no parágrafo seguinte.
- Parágrafo que resume o que a lista logo abaixo já diz.
- Formalidade que ninguém fala: "efetuar", "realizar a instalação", "proceder com".

Escreva "instale", "rode", "abra".

## Termos

Use o vocabulário fixado na skill `domain` — **titular**, **integrador**, **host**, **step**,
**Web SDK**. Um conceito, um nome, em todas as páginas.

Estrangeirismo consagrado fica: `endpoint`, `token`, `webhook`, `deploy`, `iframe`. Não traduza por
esporte — "ponto de extremidade" não ajuda ninguém. Já `error handling` → **tratamento de erros**, e
`overview` → **visão geral**.

## Prosa versus componente

Se a frase vira uma lista, faça a lista. Se as opções são mutuamente exclusivas, é `<Tabs>`. Se é
uma sequência de ações, é `<Steps>`. Prosa é o formato mais caro de ler: use quando o valor está na
conexão entre as ideias, não quando dá para tabelar.

Parágrafo de mais de quatro linhas é sinal de que uma lista está escondida ali dentro.

## Verificação

Nenhum comando pega texto ruim. O que funciona:

1. Leia em voz alta o primeiro parágrafo. Se soa como um post de LinkedIn, reescreva.
2. Corte a primeira frase de cada seção e veja se faltou algo. Quase nunca falta.
3. Confira que a página responde à pergunta do título — e só a ela.

```bash
mint a11y   # hierarquia de heading e alt de imagem
mint dev    # leia na página, não no editor
```
