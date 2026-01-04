# 🔑 O que é o Authorization Code no OAuth 2.0?

O **Authorization Code** é um **código temporário e de uso único** emitido pelo **Authorization Server** depois que o usuário concede permissão a uma aplicação. Ele faz parte do [**Authorization Code Flow**](./flows.md), que é o fluxo OAuth mais seguro e comum em aplicações web.

---

## 📚 Função do Authorization Code

O Authorization Code funciona como um **comprovante de autorização**. Ele:

* é entregue ao `client` **após o login e consentimento do usuário**;
* é trocado **somente no backend** da aplicação (evita vazamentos);
* serve como intermediário para obter um **Access Token** com segurança.

---

## 🚀 Características

| Atributo      | Detalhes                                                   |
| ------------- | ---------------------------------------------------------- |
| Tipo          | String aleatória (ex: `abc123xyz`)                         |
| Tempo de vida | Curto (geralmente segundos ou minutos)                     |
| Reutilização  | Proibida (só pode ser usado uma vez)                       |
| Transporte    | Passado via `redirect_uri` após o consentimento do usuário |
| Vinculação    | Ao `client_id`, `redirect_uri`, e (opcionalmente) PKCE     |

---

## 🌐 Exemplo prático

### 1. Após consentimento:

```http
GET /callback?code=abc123xyz&state=xyz
```

### 2. Troca por token:

```http
POST /token
Content-Type: application/x-www-form-urlencoded
Authorization: Basic base64(client_id:client_secret)

grant_type=authorization_code&
code=abc123xyz&
redirect_uri=https://client-app.com/callback
```

### 3. Resposta com token:

```json
{
  "access_token": "eyJ...",
  "expires_in": 3600,
  "token_type": "Bearer"
}
```

---

## 🔒 Por que é seguro?

* O código é **efêmero** e é usado apenas **uma vez**;
* Somente o backend com o `client_secret` consegue usá-lo;
* É trocado por token via [canal](../3.channels.md) seguro (back-channel);
* Em aplicações públicas, pode usar **PKCE** para reforçar a segurança.

---

## 📖 Referência oficial

* RFC 6749 - OAuth 2.0 Authorization Framework ([https://datatracker.ietf.org/doc/html/rfc6749#section-4.1](https://datatracker.ietf.org/doc/html/rfc6749#section-4.1))
