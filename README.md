# ENTENDAI

ENTENDAI é uma aplicação web simples para explicar palavras, perguntas e ideias em português do Brasil usando Cloudflare Pages Functions e Cloudflare Workers AI.

A interface permite digitar um texto, escolher o estilo da explicação e receber uma resposta curta, clara e adaptada ao modo escolhido.

## Recursos

- Interface estática em HTML, CSS e JavaScript.
- Layout responsivo com suporte a tema claro/escuro pelo sistema.
- API serverless em Cloudflare Pages Functions.
- Integração com Workers AI pelo binding `AI`.
- Modos de explicação: `criança`, `iniciante`, `técnico`, `resumo` e `exemplo prático`.
- Respostas em português do Brasil com limite curto de palavras.
- Tratamento de carregamento, validação e erros na interface.

## Tecnologias

- Cloudflare Pages
- Cloudflare Pages Functions
- Cloudflare Workers AI
- Wrangler
- HTML, CSS e JavaScript puro

## Estrutura do projeto

```text
.
+-- functions/
|   +-- api/
|       +-- explicar.js      # Rota POST /api/explicar
+-- public/
|   +-- index.html           # Interface web do ENTENDAI
+-- package.json             # Scripts e dependência do Wrangler
+-- package-lock.json
+-- wrangler.jsonc           # Configuração do Cloudflare Pages e binding AI
+-- README.md
```

## Requisitos

- Node.js e npm instalados.
- Conta Cloudflare com acesso a Pages e Workers AI.
- Wrangler instalado pelo projeto via `npm install`.

## Como rodar localmente

Instale as dependências:

```bash
npm install
```

Inicie o ambiente local do Cloudflare Pages:

```bash
npm run dev
```

Depois, acesse a URL exibida pelo Wrangler no terminal.

O comando de desenvolvimento executa:

```bash
wrangler pages dev public --ai AI
```

## Scripts

```bash
npm run build
```

Não executa uma etapa de build, pois o projeto usa arquivos estáticos em `public/`.

```bash
npm run dev
```

Roda o projeto localmente com Cloudflare Pages Functions e binding de Workers AI.

```bash
npm run deploy
```

Publica o diretório `public/` no Cloudflare Pages usando o projeto `entendai`.

## API

### `POST /api/explicar`

Recebe um texto e um modo de explicação, chama o Workers AI e retorna uma explicação curta.

Exemplo de corpo da requisição:

```json
{
  "termo": "o que é computação em nuvem?",
  "modo": "iniciante"
}
```

Exemplo de resposta:

```json
{
  "resposta": "Computação em nuvem é usar servidores e serviços pela internet, sem precisar manter tudo no seu próprio computador."
}
```

Modos aceitos:

- `criança`
- `iniciante`
- `técnico`
- `resumo`
- `exemplo prático`

Se o modo enviado estiver ausente ou for inválido, a API usa `iniciante`.

### Validações e erros

- A rota aceita apenas requisições `POST`.
- O corpo deve ser JSON válido.
- O campo `termo` deve ser uma string não vazia.
- Erros de validação retornam status `400`.
- Falhas ao gerar a resposta retornam status `500` com uma mensagem genérica para o usuário.

## Configuração Cloudflare

O arquivo `wrangler.jsonc` define o projeto Pages, o diretório publicado e o binding de Workers AI:

```jsonc
{
  "$schema": "node_modules/wrangler/config-schema.json",
  "name": "entendai",
  "pages_build_output_dir": "./public",
  "compatibility_date": "2026-07-05",
  "ai": {
    "binding": "AI"
  }
}
```

A função em `functions/api/explicar.js` acessa o binding por `context.env.AI` e usa o modelo:

```text
@cf/meta/llama-3.2-3b-instruct
```

Se alterar bindings ou configurações do Wrangler, gere novamente os tipos quando necessário:

```bash
npx wrangler types
```

## Deploy

Para publicar no Cloudflare Pages:

```bash
npm run deploy
```

O script executa:

```bash
npx wrangler pages deploy public --project-name entendai
```

## Observações

- A aplicação não possui build complexa; `public/index.html` concentra a interface, estilos e JavaScript do cliente.
- A função serverless fica em `functions/api/explicar.js`.
- A interface envia requisições para `/api/explicar` usando `fetch`.
- O prompt da IA pede respostas em português do Brasil, com no máximo 80 palavras.
