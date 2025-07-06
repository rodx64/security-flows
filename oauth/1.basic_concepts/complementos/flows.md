
# Fluxos de Autorização no OAuth 2.0

O OAuth 2.0 define **quatro fluxos principais** (grant types), cada um indicado para diferentes tipos de aplicação e cenários de segurança. Abaixo, explicamos cada fluxo com exemplos, quando usar e um diagrama representando o processo.

---

## 1. Authorization Code Flow (Código de Autorização)

### Quando usar?
- Aplicações confidenciais (backends, web apps)
- Cenários com redirecionamento via navegador
- Alta segurança

### Etapas:
1. O usuário é redirecionado para o Authorization Server.
2. Ele faz login e consente o acesso.
3. O Authorization Server redireciona de volta com um `authorization_code`.
4. O client troca o código por um `access_token`.

### Diagrama:

```text
+--------+                                +---------------+
|        |--(1) Authorization Request---->|               |
|        |                                | Authorization |
|        |<----(2) Authorization Code-----|     Server    |
| Client |                                |               |
|        |--(3) Token Request (code)----->|               |   
|        |   (client_id + client_secret)  |               | 
|        |                                |               |
+--------+ -----<-(4) Access Token--------+---------------+
```

### Características:
- Seguro, pois o token não é exposto ao navegador.
- Usa `client_secret`.
- Recomendado para apps server-side.

---

## 2. Authorization Code Flow com PKCE

### Quando usar?
- Aplicações públicas (SPAs, apps móveis)
- Sem `client_secret`
- Alta segurança contra interceptação

### Diferença?
Adiciona um **PKCE challenge** (hash temporário) ao passo de autorização.

### Diagrama (com PKCE):

```text
+--------+                                               +---------------+
|        |--(1) Authorization Request + code_challenge-->|               |
|        |                                               | Authorization |
|        |<---------(2) Authorization Code---------------|     Server    |
| Client |                                               |               | 
|        |------(3) Token Request + code_verifier ------>|               |
|        |                                               |               | 
+--------+<---------(4) Access Token---------------------+---------------+
```

### Características:
- Não exige `client_secret`.
- Impede interceptação de `authorization_code`.
- Padrão moderno recomendado para apps móveis e SPAs.

---

## 3. Client Credentials Flow

### Quando usar?
- Autenticação máquina a máquina (M2M)
- Sem usuário envolvido
- Microserviços, scripts, backends

### Etapas:
1. O client envia `client_id` e `client_secret` diretamente ao Authorization Server.
2. Recebe um `access_token` válido para chamadas à API.

### Diagrama:

```text
+--------+                                 +---------------+
|        |-------(1) Token Request ------->|               |
|        |   (client_id + client_secret)   | Authorization |
| Client |                                 |     Server    |
|        |<-----(2) Access Token ----------|               |
+--------+                                 +---------------+
``` 

### Características:
- Simples e direto.
- Somente para aplicações de backend.
- Não envolve autenticação de usuário.

---

## 4. Implicit Flow (⚠️ Obsoleto)

### Quando era usado?
- Aplicações client-side (JS) no passado.
- Quando não era possível armazenar segredos ou lidar com redirecionamentos.

### Motivo de obsolescência:
- Tokens expostos diretamente no navegador.
- Vulnerável a interceptações.

### Diagrama:

```text
+--------+                                      +---------------+
|        |------(1) Authorization Request ----->|               |
|        |                                      | Authorization |
| Client |<-----(2) Access Token (no código)----|     Server    |
+--------+                                      +---------------+
```

### Recomendação:
🚫 **Não usar mais.**
Substituído por Authorization Code + PKCE.

---

## 5. Resource Owner Password Credentials (⚠️ Obsoleto)

### Quando era usado?
- Quando o usuário confiava muito no client.
- Enviava login e senha diretamente para o client.

### Problemas:
- Viola o princípio do OAuth (evitar fornecer senha ao client).
- Pouco seguro e difícil de auditar.

### Recomendação:
🚫 **Evitar sempre que possível.** Só aceitável em ambientes internos ou legados.

---

## Tabela Resumo

| Flow                       | Usuário Interage? | Requer client_secret? | Uso Recomendado                |
|----------------------------|-------------------|------------------------|-------------------------------|
| Authorization Code         | ✅ Sim             | ✅ Sim                 | Web apps, servidores          |
| Authorization Code + PKCE | ✅ Sim             | ❌ Não                 | SPAs, apps móveis             |
| Client Credentials         | ❌ Não            | ✅ Sim                 | Microsserviços, M2M           |
| Implicit (obsoleto)        | ✅ Sim             | ❌ Não                 | ⚠️ Evitar                     |
| Password Credentials       | ✅ Sim             | ✅ Sim                 | ⚠️ Evitar (uso legado)        |

---

## Recursos oficiais

- [RFC 6749 - OAuth 2.0](https://datatracker.ietf.org/doc/html/rfc6749)
- [OAuth 2.1 (Draft)](https://oauth.net/2.1/)
- [OAuth 2.0 PKCE Explained](https://oauth.net/2/pkce/)
- [OAuth Security Best Practices](https://oauth.net/2/oauth-best-practices/)
