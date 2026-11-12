# Publicar a padaria no Render

## Preparar GitHub
1. Cria um repositorio privado no GitHub.
2. Carrega para a raiz desse repositorio os ficheiros desta pasta (index.html, admin.html, server.js, package.json, .node-version, etc.).
3. Nunca publiques `data/` nem ficheiros `.env` (estao no `.gitignore`).

## Render
1. New > Web Service > liga o repositorio GitHub.
2. Runtime: Node. Build Command: `echo 'Sem dependencias externas'`. Start Command: `npm start`.
3. Escolhe um plano pago que permita Persistent Disk e Shell.
4. No servico adiciona um disco persistente montado em `/opt/render/project/src/data`. So essa pasta persiste entre deploys. Confirma que o repositorio esta diretamente na raiz do projeto e que Root Directory esta vazio.
5. Na seccao Environment, define `COOKIE_SECURE=1`. A versao Node e definida em `.node-version`.
6. Faz Deploy e verifica a pagina inicial em `https://NOME.onrender.com`.

## Criar o administrador
Depois do deploy e montagem do disco persistente, abre a aba Shell do servico no Render e executa:

```sh
export ADMIN_EMAIL='dono@exemplo.pt'
read -s ADMIN_PASSWORD
export ADMIN_PASSWORD
node server.js set-admin
unset ADMIN_PASSWORD
```

Ao executar `read -s ADMIN_PASSWORD`, escreve a palavra-passe (minimo 12 caracteres) e carrega Enter. Ela nao aparece no ecra. Usa email verdadeiro.

Entra em `https://NOME.onrender.com/admin` e edita produtos e contactos.

## Dominio proprio
1. Compra o dominio .pt num registrador.
2. Render > Servico > Settings > Custom Domains > Add Custom Domain.
3. Adiciona `www.teudominio.pt` (e o dominio sem www, se necessario).
4. No painel DNS do fornecedor, usa os registos EXATOS indicados no Render. Em geral, `www` usa CNAME para `NOME.onrender.com`. Para o dominio raiz, segue as instrucoes do Render e do fornecedor DNS (A ou ALIAS).
5. Volta ao Render e carrega Verify. O HTTPS e emitido automaticamente.

## Antes de publicar comercialmente
- Troca os produtos, imagens e precos de demonstracao.
- Preenche todos os contactos, horario, informacoes da empresa, alergénios e obrigacoes legais.
- Revê seguranca: limitacao de tentativas de login, politica de copias de seguranca, controlo de carregamento de imagens, atualizacoes de Node e processo de recuperacao de acesso.
- Testa inicio de sessao, alteracoes, uploads e persistencia apos novo deploy.

Esta e uma base funcional de desenvolvimento e nao uma auditoria de seguranca nem uma implementacao de producao completamente preparada.
