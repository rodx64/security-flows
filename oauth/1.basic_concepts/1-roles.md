# Papéis no Fluxo OAuth 2.0

OAuth 2.0 é um protocolo de autorização amplamente utilizado para permitir que aplicações de terceiros acessem recursos de um usuário de forma segura, sem expor suas credenciais. Neste documento, abordo os principais papéis (roles) envolvidos no fluxo OAuth e forneço exemplos para facilitar a compreensão.

## 1. Resource Owner (Proprietário do Recurso)

### O que é?
O **Resource Owner** é o usuário que possui os dados ou recursos protegidos. É quem autoriza o acesso aos seus dados.

### Exemplo
Eu quero usar um aplicativo chamado "PhotoPrint" para imprimir minhas fotos que estão armazenadas no Google Fotos. Eu sou o Resource Owner, pois sou o dono das fotos.

---

## 2. User Agent (Agente do Usuário)

### O que é?
O **User Agent** é o meio pelo qual o usuário interage com o client e o authorization server. Podem ser de diversos tipos, dependendo da necessidade:

#### Navegadores Web (tradicional)
        
Usado em SPAs, apps web tradicionais. Mais comuns no Authorization Code Flow:
- Chrome, Firefox, Safari, Edge... 

#### WebViews (embutidos em apps móveis ou desktop)
    
Permitido, mas não recomendado por motivos de segurança (como roubo de credenciais). Melhores práticas recomendam uso do navegador externo via OAuth Custom Tabs / SafariViewController.
- Android WebView, iOS WKWebView, Electron BrowserWindow.

#### Ferramentas de Desenvolvimento de APIs
    
Essas são bastante usadas por desenvolvedores durante testes e integração com APIs:

- **Postman**

    Pode iniciar Authorization Code Flow, PKCE e Client Credentials.
Atua como client + user agent (emula navegador para autenticação).

- **Insomnia**

    Similar ao Postman, com suporte a fluxos OAuth.

- **curl + navegador**
    
    Desenvolvedor inicia o fluxo manualmente no navegador, copia o code da URL, e usa curl para trocar pelo token.

- **Browsers headless** (Puppeteer, Selenium)

    Usado para testes automatizados.
    Não recomendado para produção.


### Função no fluxo
O user agent é responsável por:
- Redirecionar o usuário do client para o authorization server.
- Exibir a tela de login e consentimento.
- Redirecionar o authorization code de volta ao client após o consentimento.

## 3. Client (Cliente)

### O que é?
O **Client** é a aplicação que deseja acessar os recursos protegidos em nome do resource owner. Pode ser um app mobile, uma aplicação web, ou um serviço backend.

### Exemplo
O aplicativo "PhotoPrint" é o client. Ele quer acessar as fotos que eu preciso imprimir.

---

## 4. Authorization Server (Servidor de Autorização)

### O que é?
O **Authorization Server** é responsável por autenticar o resource owner e obter o seu consentimento, emitindo tokens de acesso para o client. É quem fornece o *access token* e, opcionalmente, o *refresh token*.

### Exemplo
O servidor de autenticação do Google (accounts.google.com) é o authorization server quando o PhotoPrint pede permissão para acessar o meu Google Fotos.

---

## 5. Resource Server (Servidor de Recursos)

### O que é?
O **Resource Server** é a API ou serviço que hospeda os recursos protegidos. Ele valida o access token antes de permitir o acesso.

### Exemplo
A API do Google Fotos que fornece acesso às minhas fotos é o Resource Server.

---

## Resumo Visual

![roles](<assets/roles.png>)
