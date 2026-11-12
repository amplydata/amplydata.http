# Amply Data

## Índice das APIs

- [Autenticar — POST /v1/auth](#autenticar)
- [Recuperar senha — POST /v1/auth/forgot](#recuperar-senha)
- [Validar token — POST /v1/auth/valid](#validar-token)
- [Listar empresas — GET /v1/company/all](#listar-empresas)
- [Consultar dashboard — GET /v1/dashboard/main](#consultar-dashboard)

Coleção de requisições HTTP para documentação e testes manuais das APIs da plataforma Amply Data. Os endpoints disponíveis nesta coleção estão na pasta [`http`](http).

## Configuração e execução

Abra os arquivos `.http` em um cliente compatível com o formato HTTP Client da JetBrains, incluindo os scripts de resposta (`client.global.set`).

Configure o arquivo local `http/http-client.private.env.json`, ignorado pelo Git, com as variáveis de cada ambiente. Exemplo com valores fictícios:

```json
{
  "dev": {
    "baseUrl": "http://localhost:8080",
    "username": "usuario@example.com",
    "password": "sua-senha"
  },
  "prod": {
    "baseUrl": "https://api.example.com",
    "username": "usuario@example.com",
    "password": "sua-senha"
  }
}
```

| Variável | Finalidade |
| --- | --- |
| `baseUrl` | URL base da API, sem a barra final. |
| `username` | E-mail usado na autenticação e na recuperação de senha. |
| `password` | Senha usada na autenticação. |
| `token` | Variável global preenchida pelo script de resposta da autenticação a partir de `response.body.token`. |

Selecione o ambiente desejado (`dev` ou `prod`) no cliente HTTP e execute **Autenticar** em `http/auth.http`. Depois, execute as consultas de empresas e dashboard, que usam o token obtido. Execute novamente a autenticação quando precisar renovar essa variável.

## Endpoints disponíveis

Todas as requisições usam `Accept: application/json` e `Content-Type: application/json`. Os caminhos abaixo são relativos a `{{baseUrl}}`.

| Método | Endpoint | Finalidade | Token enviado | Arquivo |
| --- | --- | --- | --- | --- |
| `POST` | `/v1/auth` | Autenticar o usuário. | Não | [auth.http](http/auth.http) |
| `POST` | `/v1/auth/forgot` | Solicitar recuperação de senha. | Não | [auth.http](http/auth.http) |
| `POST` | `/v1/auth/valid` | Validar um token. | No corpo | [auth.http](http/auth.http) |
| `GET` | `/v1/company/all` | Obter todas as empresas (companies). | Cabeçalho `token` | [company.http](http/company.http) |
| `GET` | `/v1/dashboard/main` | Obter o dashboard do usuário. | Cabeçalho `token` | [dashboard.http](http/dashboard.http) |

### Autenticar

`POST {{baseUrl}}/v1/auth`

```json
{
  "email": "{{username}}",
  "password": "{{password}}"
}
```

O script após a requisição lê o campo `token` do corpo da resposta e o armazena na variável global `token`, utilizada pelas próximas requisições.

### Recuperar senha

`POST {{baseUrl}}/v1/auth/forgot`

```json
{
  "email": "{{username}}"
}
```

### Validar token

`POST {{baseUrl}}/v1/auth/valid`

O token é enviado no corpo. O arquivo atual utiliza o seguinte template:

```http
{"token": {{token}}}
```

Se o token retornado for uma string, o valor precisa estar entre aspas para formar um JSON válido: `{"token": "{{token}}"}`.

### Listar empresas

`GET {{baseUrl}}/v1/company/all`

Envie o cabeçalho `token: {{token}}`. A requisição não define corpo.

### Consultar dashboard

`GET {{baseUrl}}/v1/dashboard/main`

Envie o cabeçalho `token: {{token}}`. O arquivo de teste inclui um corpo JSON vazio (`{}`).

## Escopo da documentação

Esta documentação descreve as requisições presentes na coleção. Os arquivos não especificam códigos de status esperados, schemas completos das respostas ou asserções automatizadas; apenas a autenticação contém um script para capturar o token.
