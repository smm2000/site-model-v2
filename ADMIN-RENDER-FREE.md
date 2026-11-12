# Criar um administrador de testes no Render Free

Esta alteracao permite criar a primeira conta de administrador **sem guardar a palavra-passe no GitHub**.

## Atualizar o projeto
1. Descompacta o ZIP e envia os ficheiros para a **raiz** do repositorio `site-model-v2`, substituindo os ficheiros com o mesmo nome. O ficheiro indispensavel para esta funcionalidade e `server.js`.
2. No Render abre o servico atual > **Environment** > **Add Environment Variable** e adiciona:

   - `TEMP_ADMIN_EMAIL` = o teu email de teste (exemplo: `teste@exemplo.pt`)
   - `TEMP_ADMIN_PASSWORD` = uma palavra-passe de testes **unica, com pelo menos 12 caracteres**
   - `COOKIE_SECURE` = `1`

   Introduz os valores no painel do Render, **nao** em ficheiros `.env` publicados nem em mensagens ou capturas de ecra. Se o Render disponibilizar entrada secreta/oculta, utiliza-a.
3. Guarda as variaveis e publica a ultima versao do GitHub (Manual Deploy > Deploy latest commit, se necessario). O ultimo deploy tem de incluir o novo `server.js`.
4. Quando aparecer `Live`, abre `https://TEU-SERVICO.onrender.com/admin` e inicia sessao com os valores que definiste.

## Comportamento e limites
- Na primeira inicializacao sem administradores, o servidor cria a conta a partir destas variaveis. A palavra-passe e guardada na SQLite apenas como hash `scrypt` com sal; nao e exposta no site publico nem escrita nos logs.
- Se ja existir uma conta nessa base de dados, o arranque **nao altera** a palavra-passe existente.
- Render Free **nao tem disco persistente**: um novo deploy ou reinicio pode apagar a base de dados, fotografias e a conta. Enquanto as variaveis estiverem definidas, a conta de teste e recriada quando a base de dados recomeça vazia.
- Se removeres as variaveis do Render, um novo deploy pode fazer desaparecer a conta temporaria; essa e uma limitacao esperada.
- Nao uses estas credenciais em sites reais e nao reutilizes palavras-passe pessoais. Desativa/apaga as variaveis e prepara uma conta definitiva com armazenamento persistente antes de entregar o site a um cliente.
- Usa apenas imagens e dados de demonstracao nesta fase.

## Verificar
- `/healthz` deve mostrar `{ "ok": true }`.
- `/admin` deve apresentar o login e permitir entrar com a conta de teste.
- Depois de entrares, cria um produto ficticio e verifica que aparece no catalogo. **Nao contes que persista depois de um redeploy em Free.**

## Produção
Utiliza Render com disco persistente ou uma base de dados/armazenamento externos; configura backups, autenticacao e revisao de seguranca antes de comercializar.
