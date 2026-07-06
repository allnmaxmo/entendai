# ENTENDAI

ENTENDAI e uma aplicacao web simples para explicar palavras, perguntas e ideias em portugues do Brasil usando Cloudflare Pages Functions e Cloudflare Workers AI.

A interface permite digitar um texto, escolher o estilo da explicacao e receber uma resposta curta, clara e adaptada ao modo escolhido.

## Recursos

- Interface estatica em HTML, CSS e JavaScript.
- Layout responsivo com suporte a tema claro/escuro pelo sistema.
- API serverless em Cloudflare Pages Functions.
- Integracao com Workers AI pelo binding `AI`.
- Modos de explicacao: `criança`, `iniciante`, `técnico`, `resumo` e `exemplo prático`.
- Respostas em portugues do Brasil com limite curto de palavras.
- Tratamento de carregamento, validacao e erros na interface.

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
+-- package.json             # Scripts e dependencia do Wrangler
+-- package-lock.json
+-- wrangler.jsonc           # Configuracao do Cloudflare Pages e binding AI
+-- README.md
```

## Requisitos

- Node.js e npm instalados.
- Conta Cloudflare com acesso a Pages e Workers AI.
- Wrangler instalado pelo projeto via `npm install`.

## Como rodar localmente

Instale as dependencias:

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

Nao executa uma etapa de build, pois o projeto usa arquivos estaticos em `public/`.

```bash
npm run dev
```

Roda o projeto localmente com Cloudflare Pages Functions e binding de Workers AI.

```bash
npm run deploy
```

Publica o diretorio `public/` no Cloudflare Pages usando o projeto `entendai`.

## API

### `POST /api/explicar`

Recebe um texto e um modo de explicacao, chama o Workers AI e retorna uma explicacao curta.

Exemplo de corpo da requisicao:

```json
{
  "termo": "o que e computacao em nuvem?",
  "modo": "iniciante"
}
```

Exemplo de resposta:

```json
{
  "resposta": "Computacao em nuvem e usar servidores e servicos pela internet, sem precisar manter tudo no seu proprio computador."
}
```

Modos aceitos:

- `criança`
- `iniciante`
- `técnico`
- `resumo`
- `exemplo prático`

Se o modo enviado estiver ausente ou for invalido, a API usa `iniciante`.

### Validacoes e erros

- A rota aceita apenas requisicoes `POST`.
- O corpo deve ser JSON valido.
- O campo `termo` deve ser uma string nao vazia.
- Erros de validacao retornam status `400`.
- Falhas ao gerar a resposta retornam status `500` com uma mensagem generica para o usuario.

## Configuracao Cloudflare

O arquivo `wrangler.jsonc` define o projeto Pages, o diretorio publicado e o binding de Workers AI:

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

A funcao em `functions/api/explicar.js` acessa o binding por `context.env.AI` e usa o modelo:

```text
@cf/meta/llama-3.2-3b-instruct
```

Se alterar bindings ou configuracoes do Wrangler, gere novamente os tipos quando necessario:

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

## Observacoes

- A aplicacao nao possui build complexa; `public/index.html` concentra a interface, estilos e JavaScript do cliente.
- A funcao serverless fica em `functions/api/explicar.js`.
- A interface envia requisicoes para `/api/explicar` usando `fetch`.
- O prompt da IA pede respostas em portugues do Brasil, com no maximo 80 palavras.
