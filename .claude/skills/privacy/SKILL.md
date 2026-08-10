---
name: privacy
description: O que nunca pode entrar num arquivo deste repo — PII de titular, credencial, nome de cliente, print com dado real. Use antes de colar qualquer exemplo, payload, screenshot ou log numa página.
---

# Dado em doc pública

**Tudo neste repo é público.** O site é indexado, e o repo espelha o site. Não existe "só um
exemplo": um CPF colado numa página fica no git para sempre, mesmo depois do `git rm`.

Este é o único assunto do repo onde o erro não tem desfazer. Trate como tal.

## Nunca entra

| Categoria                    | Exemplo do que não entra                                       |
| ---------------------------- | -------------------------------------------------------------- |
| PII de titular               | CPF, RG, nome completo, e-mail, telefone, endereço, data de nascimento |
| Biometria                    | selfie, foto de documento, template facial, vídeo de liveness  |
| Credencial                   | API key, `session_token`, JWT, fragmento `#v=...` de `sdkUrl`  |
| Identificador de cliente     | `tenant_id` real, subdomínio de cliente, nome de cliente sem autorização comercial |
| Infra interna                | host de banco, URL de serviço interno, nome de bucket, ID de projeto cloud |
| Print com dado real          | screenshot do dashboard sem redigir a tabela                   |

Se um segredo real já foi colado em algum lugar: **não basta apagar** — ele precisa ser rotacionado.
Avise o time antes de seguir.

## O que usar no lugar

Dado fictício, marcado como fictício, e **consistente entre as páginas** — o mesmo Maria em todos os
exemplos ajuda a leitura.

```json
{
  "reference_id": "abc-123",
  "name": "Maria Oliveira",
  "document": "000.000.000-00",
  "email": "maria@exemplo.com.br"
}
```

- CPF/CNPJ: use máscara neutra (`000.000.000-00`) ou marque `// fictício`. **Nunca gere um CPF que
  passe no dígito verificador** — ele pertence a alguém.
- Token: `<SEU_TOKEN>` ou `sk_test_exemplo`. Placeholder em maiúscula deixa óbvio que é para trocar.
- Domínio: `exemplo.com.br`, nunca o domínio de um cliente.
- URL do produto: `verify.legitimuz.com` e `api.legitimuz.com` são públicos e podem aparecer.

## Screenshot

Print do dashboard só entra **com dado fictício na tela** (ambiente de sandbox) ou com a região
redigida em cima da imagem — não com CSS, não com um retângulo que o leitor pode remover. Se a
tarja saiu num editor de imagem e o arquivo foi reexportado, está redigido; qualquer outra coisa,
não está.

Confira também o que entrou sem querer: aba do navegador com URL de tenant, nome no canto do avatar,
notificação do sistema.

## Antes de commitar

Passe o olho no diff inteiro, não no que você acha que mudou:

```bash
git diff --cached
```

E procure o que costuma vazar:

```bash
git grep -nE '[0-9]{3}\.[0-9]{3}\.[0-9]{3}-[0-9]{2}|eyJ[A-Za-z0-9_-]{10,}|(api[_-]?key|token|secret)["'\'':= ]' -- '*.mdx' '*.json'
```

Achou algo que não é placeholder? Pare e resolva antes de qualquer outra coisa.

## O que a doc precisa dizer sobre dado

O inverso também vale: páginas de integração **têm** que dizer ao integrador o que ele não deve
fazer com o dado do titular — não logar payload de verificação, não persistir imagem, não mandar
`sdkUrl` para tag manager. O SDK já é projetado para isso (ver skill `domain` para o repo canônico);
a doc é onde o integrador descobre.
