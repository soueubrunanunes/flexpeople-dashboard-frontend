# Frontend — Dashboard RH Flex People

O mesmo dashboard de sempre (`index.html`, um arquivo único com todo o HTML/CSS/JS), agora com
uma tela de login na frente. Depois de logado:

- **Master**: vê o botão de carregar Excel/Base de Entrevistas normalmente, do jeito que já
  usava. Toda vez que uma planilha é processada, o resultado também é enviado pro backend, pra
  ficar disponível pro supervisor ver.
- **Supervisor**: não vê os botões de upload — só o dashboard já preenchido com os últimos dados
  que o master carregou.

## Testado localmente

O fluxo completo (tela de login, senha errada barrada, login master mostrando os botões de
upload, upload sincronizando com o backend, um segundo login como supervisor recebendo os
mesmos dados automaticamente sem precisar carregar nada, botões de upload escondidos pro
supervisor, e logout voltando pra tela de login) foi verificado neste ambiente com um backend de
teste equivalente ao real. Depois que você publicar o backend de verdade no Render, vale abrir o
site uma vez e repetir esse fluxo manualmente antes de divulgar o link pro time.

## 1) Configure a URL do backend

Abra o arquivo `config.js` e troque pela URL pública do seu backend publicado no Render:

```js
window.API_BASE = "https://flexpeople-dashboard-backend.onrender.com";
```

(sem barra `/` no final). Sem isso configurado, a tela de login mostra um aviso pedindo pra
configurar o `config.js`.

## 2) Publicando no GitHub Pages

1. Crie um repositório novo no GitHub (pode ser privado) e suba os dois arquivos desta pasta
   (`index.html` e `config.js`).
2. No repositório, vá em **Settings** → **Pages**.
3. Em "Source", escolha a branch (geralmente `main`) e a pasta `/ (root)`.
4. Salve. Em alguns minutos o GitHub mostra a URL pública do site (algo como
   `https://seu-usuario.github.io/nome-do-repositorio/`).
5. Volte no backend (Render) e configure a variável `FRONTEND_ORIGIN` com essa URL exata, pra
   travar o backend pra só aceitar chamadas vindas dali.

## Logins

As credenciais de master e supervisor não ficam neste arquivo — elas são configuradas no
backend (Render → Environment → `MASTER_USERNAME`/`MASTER_PASSWORD` e
`SUPERVISOR_USERNAME`/`SUPERVISOR_PASSWORD`). O frontend só pergunta usuário/senha e manda pro
backend conferir.

## O que muda pro dia a dia

Nada na experiência de carregar a planilha muda para você (master): mesmo botão, mesmo parsing,
mesmas telas. A única diferença é que, depois de processar o arquivo, o dashboard também salva
esse resultado no backend — é isso que faz o supervisor enxergar os mesmos números sem precisar
subir nada.
