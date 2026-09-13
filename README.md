# Galpão — versão com conta de usuário e Mercado Livre

## O que mudou desde o protótipo
- Antes: os dados ficavam só no seu navegador.
- Agora: existe um **servidor** (`server.js`) com um arquivo `db.json` que
  guarda de verdade os usuários, produtos e pedidos. Cada pessoa que
  criar uma conta só vê os próprios dados.
- Também dá pra **conectar a conta do Mercado Livre** (login oficial deles)
  para, no futuro, puxar os pedidos automaticamente.

## Como rodar no seu computador
1. Instale o [Node.js](https://nodejs.org) (versão 18 ou mais nova).
2. Abra o terminal na pasta `galpao`.
3. Rode: `node server.js`
4. Abra `http://localhost:3000` no navegador.

Não precisa instalar mais nada — o servidor não usa nenhuma biblioteca externa.

## Como publicar na internet (para vender de verdade)
Diferente do protótipo anterior (só HTML), agora existe um servidor rodando
o tempo todo — por isso **Netlify não serve mais**. Use um destes, que têm
plano gratuito para começar:

- **Render.com** → "New Web Service", conecte seu repositório, comando de
  start: `node server.js`.
- **Railway.app** → parecido com o Render, também simples.

Depois de publicar, você terá um link (tipo `https://seu-app.onrender.com`)
que qualquer cliente pode acessar e criar a própria conta.

## Como ligar o botão "Conectar Mercado Livre"
1. Crie um app em https://developers.mercadolivre.com.br/devcenter
2. Pegue o **Client ID** e o **Client Secret**.
3. Configure a **Redirect URI** do app no Mercado Livre como:
   `https://SEU-LINK-PUBLICADO/api/ml/callback`
4. No seu servidor, defina três variáveis de ambiente antes de rodar:
   - `ML_CLIENT_ID`
   - `ML_CLIENT_SECRET`
   - `ML_REDIRECT_URI` (a mesma URL do passo 3)
5. Pronto — o botão vai redirecionar para o login oficial do Mercado Livre
   e voltar já conectado.

Sem essas 3 variáveis preenchidas, o resto do sistema (contas, estoque,
pedidos manuais) funciona normalmente — só a conexão com o Mercado Livre
fica desligada.

## Limitações desta versão (para você saber o que evoluir depois)
- Sessões de login ficam na memória do servidor: se o servidor reiniciar,
  todo mundo precisa entrar de novo.
- `/api/ml/sync` só mostra quantos pedidos existem no Mercado Livre, como
  exemplo — trazer os pedidos reais para dentro do Galpão é o próximo passo.
- `db.json` funciona bem para poucos usuários. Com muitos clientes ao mesmo
  tempo, o passo seguinte é trocar por um banco de verdade (Postgres, por
  exemplo) — a estrutura do código já foi pensada pra essa troca ser simples.
