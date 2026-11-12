# Site da padaria — atualização

## O que inclui
- Site responsivo com catálogo e filtros por categoria.
- Painel privado em `/admin` para gerir produtos, fotos, contactos, horários e textos.
- Backend Node.js com SQLite.
- Endpoint de saúde `/healthz` para o Render.
- Limite básico de tentativas repetidas de login.
- Correção de inicialização do catálogo para não repor exemplos quando o catálogo for apagado.

## Atualizar o GitHub e Render
1. Faça backup de qualquer base de dados ou fotos reais antes de substituir ficheiros.
2. Descompacte o ZIP e carregue para a raiz do repositório GitHub existente `site-padaria`, substituindo os ficheiros antigos.
3. Render: serviço Web > Settings > Health Check Path: `/healthz`.
4. Render: Build Command: `echo "Sem dependencias"`; Start Command: `npm start`.
5. Confirme o deploy e abra `/` e `/admin`.

## Dados persistentes e administrador
- Em produção: usar plano do Render compatível com Persistent Disk, montado em `/opt/render/project/src/data`, **antes de adicionar dados reais**.
- Definir `COOKIE_SECURE=1` no Render.
- A base de dados fica no diretório `data`; opcionalmente definir `DATA_DIR` para outro diretório absoluto.
- Na consola do Render, com o disco montado:
  ```sh
  export ADMIN_EMAIL='dono@exemplo.pt'
  read -s ADMIN_PASSWORD
  export ADMIN_PASSWORD
  node server.js set-admin
  unset ADMIN_PASSWORD
  ```
- Não guardar credenciais nem ficheiros da base de dados no GitHub. Fazer backups regulares, incluindo o ficheiro SQLite e as imagens em `data/uploads`.

## Antes de entregar a um cliente
- Trocar todos os produtos/preços de exemplo, fotos e textos.
- Preencher nome comercial, contactos, horário, Google Maps e redes sociais.
- Confirmar dados legais, privacidade/cookies e alergénios com a empresa.
- Testar login, criação/edição/eliminação de produtos, upload e logout.
- Implementar uma rotina de backups externos e testar restauração.
- Garantir que o domínio pertence ao cliente e registar no contrato os custos de renovação.

**Não é aconselhável vender esta versão como pronta para produção sem uma revisão técnica de segurança e dos conteúdos legais.**
