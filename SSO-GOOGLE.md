# Configurar login único com Google (SSO)

O Aluga Cinema usa **Firebase Authentication** para autenticar contas Google. O site pode continuar hospedado no GitHub Pages; o Firebase é o serviço externo que processa o login.

## 1. Criar um projeto Firebase

1. Acesse https://console.firebase.google.com/ e entre com sua conta Google.
2. Crie um projeto, por exemplo `aluga-cinema`.
3. No projeto, adicione um aplicativo Web pelo ícone `</>`.
4. Registre o app e copie os valores do objeto `firebaseConfig` mostrado pelo Firebase.

Não é necessário configurar Analytics para esse login.

## 2. Ativar o provedor Google

1. No Console do Firebase, abra **Authentication**.
2. Entre em **Sign-in method** (ou **Método de login**).
3. Ative **Google** e selecione o e-mail de suporte do projeto.
4. Salve as alterações.

O Firebase configura a integração OAuth do provedor durante esse processo.

## 3. Autorizar o domínio do GitHub Pages

Em **Authentication → Settings → Authorized domains** (Configurações → Domínios autorizados), adicione:

```text
cesarsilvestrek-cyber.github.io
```

Informe somente o domínio, sem `https://` e sem o caminho `/aluga-cinema/`. O login deve ser aberto por:

`https://cesarsilvestrek-cyber.github.io/aluga-cinema/`

## 4. Colocar a configuração no site

No GitHub, abra `index.html` e procure por `const firebaseConfig = {` dentro do bloco `<script type="module">` perto do fim do arquivo.

Troque os quatro valores de exemplo pelos campos correspondentes copiados do Firebase:

```js
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "ID_DO_SEU_PROJETO",
  appId: "ID_DO_SEU_APP_WEB"
};
```

Mantenha as chaves, nomes dos campos, aspas e vírgulas. Faça commit de `index.html` na branch `main` e espere o GitHub Pages publicar a versão atualizada. Quando os marcadores `COLE_...` e `SEU_PROJETO` forem substituídos, o botão **Entrar com Google** poderá iniciar o fluxo real de autenticação.

## 5. Testar

1. Abra o site publicado por HTTPS.
2. Clique em **Entrar com Google**.
3. Escolha uma conta Google e conclua a autorização.
4. Confira se o site mostra a conta conectada e se o botão **Sair** encerra a sessão.

Se aparecer `auth/unauthorized-domain`, revise a lista de domínios autorizados no Firebase. Se o erro indicar que o provedor está desativado, volte a **Authentication → Sign-in method** e confirme que Google está habilitado.

## Segurança e limites

- Os valores de configuração do Firebase para aplicações Web são identificadores do cliente, não chaves privadas. **Nunca** coloque chaves privadas, credenciais de conta de serviço ou segredos OAuth no HTML ou em um repositório público.
- O Firebase Auth gerencia a identidade e a sessão de login. No estado atual, o carrinho e a conclusão do pedido continuam sendo uma demonstração, sem cobrança real.
- GitHub Pages publica arquivos estáticos. Um botão que esconde conteúdo na página não protege esse conteúdo; para futuras áreas privadas ou dados de usuários, configure regras de segurança do Firebase ou um backend que verifique a autenticação.
- Consulte a documentação oficial antes de colocar dados reais de clientes no projeto: https://firebase.google.com/docs/auth/web/google-signin
