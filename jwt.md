# Azure Function — Service‑to‑Service Authentication Using Microsoft Entra ID  
## Complete Step‑by‑Step Setup (Two App Registrations)

This guide explains how to secure an Azure Function using **Microsoft Entra ID (formerly Azure AD)** with **service‑to‑service (client credentials)** authentication.  
You will create:

- **API App Registration** → represents the Azure Function  
- **Client App Registration** → represents the calling service  

The client obtains an **app‑only access token** and calls the Function using `Authorization: Bearer <token>`.

---

# 1. Create the API App Registration (Azure Function Resource)

### 1.1 Register the API
1. Go to **Microsoft Entra ID → App registrations → New registration**
2. **Name:** `my-function-api`
3. **Supported account types:**  
   - *Accounts in this organizational directory only*
4. Leave **Redirect URI** empty
5. Click **Register**

---

### 1.2 Expose the API
1. Open the new app → **Expose an API**
2. Click **Set** next to *Application ID URI*  
   - Accept the default:  
     `api://<api-app-client-id>`
3. Under **Scopes defined by this API**, click **Add a scope**
   - **Scope name:** `access_as_app`
   - **Who can consent:** Admins only
   - **Admin consent display name:** `Access the Azure Function API`
   - **Admin consent description:** `Allows the client application to call the Azure Function API.`
   - **State:** Enabled
4. Save

---

### 1.3 Record the API values
You will need:

| Setting | Value |
|--------|-------|
| **API Application (client) ID** | `<api-app-client-id>` |
| **Tenant ID** | `<tenant-id>` |
| **Application ID URI** | `api://<api-app-client-id>` |
| **Scope** | `api://<api-app-client-id>/access_as_app` |

---

# 2. Create the Client App Registration (Calling Service)

### 2.1 Register the client
1. Go to **App registrations → New registration**
2. **Name:** `my-function-client`
3. **Supported account types:** same as API
4. Click **Register**

---

### 2.2 Add API permissions
1. Go to **API permissions → Add a permission**
2. Choose **My APIs**
3. Select **my-function-api**
4. Choose **Application permissions**
5. Select:  
   - `access_as_app`
6. Click **Add permissions**
7. Click **Grant admin consent**

---

### 2.3 Create client credentials
Go to **Certificates & secrets**:

- Add a **client secret**  
  **OR**
- Upload a **certificate**

Record:

| Setting | Value |
|--------|-------|
| **Client ID** | `<client-app-client-id>` |
| **Client Secret** | `<client-secret>` (if using) |
| **Tenant ID** | `<tenant-id>` |

---

# 3. Configure the Azure Function App to Require Entra ID Tokens

### 3.1 Enable Authentication
In the **Function App**:

1. Go to **Authentication**
2. Click **Add identity provider**
3. Choose **Microsoft**
4. **App registration type:** Use existing
5. Select the **API app registration** (`my-function-api`)
6. Save

---

### 3.2 Require authentication
Still under **Authentication**:

- **Unauthenticated requests:** `HTTP 401`
- Save

Azure now blocks all requests without a valid JWT access token.

---

# 4. Client Obtains a Token (Client Credentials Flow)

### 4.1 Token endpoint

### 4.2 POST bodyHere you go — a clean, complete, production‑ready Markdown file you can drop directly into your repo as AUTH-SETUP.md.
It covers the two–app‑registration, service‑to‑service model for Azure Functions protected by Microsoft Entra ID.

---

# Azure Function — Service‑to‑Service Authentication Using Microsoft Entra ID  
## Complete Step‑by‑Step Setup (Two App Registrations)

This guide explains how to secure an Azure Function using **Microsoft Entra ID (formerly Azure AD)** with **service‑to‑service (client credentials)** authentication.  
You will create:

- **API App Registration** → represents the Azure Function  
- **Client App Registration** → represents the calling service  

The client obtains an **app‑only access token** and calls the Function using `Authorization: Bearer <token>`.

---

# 1. Create the API App Registration (Azure Function Resource)

### 1.1 Register the API
1. Go to **Microsoft Entra ID → App registrations → New registration**
2. **Name:** `my-function-api`
3. **Supported account types:**  
   - *Accounts in this organizational directory only*
4. Leave **Redirect URI** empty
5. Click **Register**

---

### 1.2 Expose the API
1. Open the new app → **Expose an API**
2. Click **Set** next to *Application ID URI*  
   - Accept the default:  
     `api://<api-app-client-id>`
3. Under **Scopes defined by this API**, click **Add a scope**
   - **Scope name:** `access_as_app`
   - **Who can consent:** Admins only
   - **Admin consent display name:** `Access the Azure Function API`
   - **Admin consent description:** `Allows the client application to call the Azure Function API.`
   - **State:** Enabled
4. Save

---

### 1.3 Record the API values
You will need:

| Setting | Value |
|--------|-------|
| **API Application (client) ID** | `<api-app-client-id>` |
| **Tenant ID** | `<tenant-id>` |
| **Application ID URI** | `api://<api-app-client-id>` |
| **Scope** | `api://<api-app-client-id>/access_as_app` |

---

# 2. Create the Client App Registration (Calling Service)

### 2.1 Register the client
1. Go to **App registrations → New registration**
2. **Name:** `my-function-client`
3. **Supported account types:** same as API
4. Click **Register**

---

### 2.2 Add API permissions
1. Go to **API permissions → Add a permission**
2. Choose **My APIs**
3. Select **my-function-api**
4. Choose **Application permissions**
5. Select:  
   - `access_as_app`
6. Click **Add permissions**
7. Click **Grant admin consent**

---

### 2.3 Create client credentials
Go to **Certificates & secrets**:

- Add a **client secret**  
  **OR**
- Upload a **certificate**

Record:

| Setting | Value |
|--------|-------|
| **Client ID** | `<client-app-client-id>` |
| **Client Secret** | `<client-secret>` (if using) |
| **Tenant ID** | `<tenant-id>` |

---

# 3. Configure the Azure Function App to Require Entra ID Tokens

### 3.1 Enable Authentication
In the **Function App**:

1. Go to **Authentication**
2. Click **Add identity provider**
3. Choose **Microsoft**
4. **App registration type:** Use existing
5. Select the **API app registration** (`my-function-api`)
6. Save

---

### 3.2 Require authentication
Still under **Authentication**:

- **Unauthenticated requests:** `HTTP 401`
- Save

Azure now blocks all requests without a valid JWT access token.

---

# 4. Client Obtains a Token (Client Credentials Flow)

### 4.1 Token endpoint


https://login.microsoftonline.com//oauth2/v2.0/token


### 4.2 POST body


client_id= client_secret= grant_type=client_credentials scope=api:///.default


### Why `.default`?
Application permissions do **not** use individual scopes.  
They use the API’s **default permission set**, which includes `access_as_app`.

---

# 5. Call the Azure Function



GET https://.azurewebsites.net/api/ Authorization: Bearer <access_token>


Azure validates:

- Token signature  
- Issuer  
- Audience (`api://<api-app-client-id>`)  
- Expiration  
- App‑only permissions  

---

# 6. (Optional) Validate Token Inside the Function

If using **Easy Auth**, the token is already validated.  
You can read identity info from the `X-MS-CLIENT-PRINCIPAL` header.

Example (C# isolated):

```csharp
var principalHeader = req.Headers["X-MS-CLIENT-PRINCIPAL"].FirstOrDefault();
if (principalHeader == null)
{
    return new UnauthorizedResult();
}


---

Summary

You now have:

• A protected Azure Function
• A client app that can obtain app‑only tokens
• A clean service‑to‑service authentication model using Microsoft Entra ID
