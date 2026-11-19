# Amply Data

Coleção de requisições HTTP para documentação e testes manuais da API Amply Data. Abra os arquivos da pasta [http](http) no HTTP Client da JetBrains.

## Configuração e execução

Configure o ambiente em `http/http-client.private.env.json`, que é ignorado pelo Git. Os valores abaixo são exemplos.

```json
{
  "dev": {
    "baseUrl": "http://localhost:8080",
    "username": "usuario@example.com",
    "password": "senha-de-login",
    "idAccount": "id-da-conta",
    "idCompany": "id-da-empresa",
    "idDashboard": "main",
    "idProfile": 1,
    "accountEmail": "conta-de-teste@example.com",
    "companyDocument": "12345678000190",
    "pin": "000000",
    "newPassword": "senha-de-teste"
  }
}
```

Selecione o ambiente no cliente HTTP e execute **Autenticar** em [auth.http](http/auth.http). O script de resposta salva `response.body.token` na variável global `token`, usada no cabeçalho das rotas protegidas e no corpo de **Validar token**. Se precisar selecionar a empresa no login, acrescente `"idCompany": "{{idCompany}}"` ao JSON de autenticação.

Preencha os IDs com os registros retornados pelas listagens. Mantenha `idProfile` como número e `pin` como texto, inclusive quando começar com zero. Use o PIN recebido ao executar **Solicitar PIN**. A variável `newPassword` é usada na criação de conta e na alteração de senha por PIN.

As listagens paginadas de contas e empresas exemplificam `registers=10&page=1` e aceitam o parâmetro opcional `search`. Os exemplos de criação, atualização, exclusão e vínculo alteram dados do ambiente selecionado.

## Endpoints

| Método | Caminho | Operação | Permissão |
| --- | --- | --- | --- |
| `POST` | `/v1/auth` | [Autenticar](http/auth.http) | Pública |
| `POST` | `/v1/auth/forgot` | [Esqueci minha senha](http/auth.http) | Pública |
| `POST` | `/v1/auth/pin` | [Solicitar Pin](http/auth.http) | Pública |
| `PUT` | `/v1/auth/changepasswordwithpin` | [Alterar senha com Pin](http/auth.http) | Pública |
| `POST` | `/v1/auth/valid` | [Validar Token](http/auth.http) | Pública |
| `GET` | `/v1/account` | [Listar contas com paginação](http/account.http) | `account_management` |
| `GET` | `/v1/account/all` | [Obter todas as contas](http/account.http) | `account_management` |
| `GET` | `/v1/account/{idAccount}` | [Obter conta por ID](http/account.http) | `account_management` |
| `POST` | `/v1/account` | [Criar conta](http/account.http) | `account_management` |
| `PUT` | `/v1/account` | [Atualizar conta](http/account.http) | `account_management` |
| `PUT` | `/v1/account/updatemyaccount` | [Atualizar minha conta](http/account.http) | `basic` |
| `POST` | `/v1/account/dashboard/add` | [Vincular dashboard à conta](http/account.http) | `account_management` |
| `POST` | `/v1/account/dashboard/remove` | [Remover dashboard da conta](http/account.http) | `account_management` |
| `POST` | `/v1/account/profile/add` | [Vincular perfil à conta na empresa](http/account.http) | `account_management` |
| `POST` | `/v1/account/profile/remove` | [Remover perfil da conta na empresa](http/account.http) | `account_management` |
| `DELETE` | `/v1/account/{idAccount}` | [Excluir conta](http/account.http) | `account_management` |
| `GET` | `/v1/company` | [Listar empresas com paginação](http/company.http) | `company_management` |
| `GET` | `/v1/company/all` | [Obter todas as empresas](http/company.http) | `company_management` |
| `GET` | `/v1/company/{idCompany}` | [Obter empresa por ID](http/company.http) | `company_management` |
| `GET` | `/v1/company/document/{companyDocument}` | [Obter empresa por documento](http/company.http) | `company_management` |
| `POST` | `/v1/company` | [Criar empresa](http/company.http) | `company_management` |
| `PUT` | `/v1/company` | [Atualizar empresa](http/company.http) | `company_management` |
| `DELETE` | `/v1/company/{idCompany}` | [Excluir empresa](http/company.http) | `basic` |
| `GET` | `/v1/dashboard` | [Obter dashboards do usuário](http/dashboard.http) | `basic` |
| `POST` | `/v1/dashboard/{idDashboard}` | [Consultar dados do dashboard do usuário](http/dashboard.http) | `basic` |
| `GET` | `/v1/profile` | [Obter todos os perfis](http/profile.http) | `company_management` |
| `GET` | `/v1/version` | [Obter versão da API](http/version.http) | Pública |
