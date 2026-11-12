# Site da padaria com painel de administração

Primeira versão funcional com catálogo público, gestão de produtos, preços, fotos, disponibilidade, contactos, horários e ligações sociais. Não recebe encomendas nem pagamentos.

## Requisitos

- Node.js 22.13+ (usa o SQLite integrado do Node; sem dependências npm)
- Servidor com disco persistente para `data/` e HTTPS para utilização em produção

## Iniciar localmente

1. Abrir o terminal dentro da pasta `padaria-site`.
2. Criar a conta do dono **antes da primeira utilização**, substituindo os dados pelos seus:

   macOS / Linux:
   ```sh
   ADMIN_EMAIL='dono@exemplo.pt' ADMIN_PASSWORD='uma-palavra-passe-segura-e-unica' node server.js set-admin
   ```
   Windows PowerShell:
   ```powershell
   $env:ADMIN_EMAIL='dono@exemplo.pt'
   $env:ADMIN_PASSWORD='uma-palavra-passe-segura-e-unica'
   node server.js set-admin
   ```
3. Iniciar: `node server.js`
4. Site: http://localhost:3000/ ; painel privado: http://localhost:3000/admin
5. Entrar com email/palavra-passe escolhidos, editar e guardar.

**Não partilhe credenciais.** `data/` contém base de dados e fotografias: faça cópias de segurança e não o coloque num repositório público. O servidor não expõe o diretório `data/`, exceto imagens públicas guardadas em `data/uploads/`.

## Publicação

Configurar domínio, HTTPS, proxy inverso e alojamento Node.js com armazenamento persistente. Definir `COOKIE_SECURE=1` ao correr por HTTPS. O servidor deve ficar atrás de um proxy inverso confiável com limite de pedidos, proteção adicional contra tentativas repetidas de login e atualizações de segurança. **Não publicar diretamente sem endurecimento adicional e revisão de segurança.**

## Notas

- O ficheiro `index.html` também abre sem servidor como demonstração, mas a gestão e a gravação só funcionam com `node server.js`.
- Fotos carregadas: JPG, PNG e WebP até 4 MB.
- Os produtos e preços iniciais são exemplos fictícios; confirmar tudo antes de tornar público.
- Completar entidade exploradora, NIF, RAL, alergénios, política de privacidade, cookies, contactos e obrigações legais antes de lançar.
- A política de privacidade e cookies incluída é apenas texto-modelo e precisa de adaptação ao alojamento final e às funcionalidades implementadas.
- A criação de utilizadores e reposição de palavras-passe é feita no servidor, pelo comando `set-admin`.
