# MARCARS INTEGRAÇÕES

## SGM

### Autenticação

| PROTOCOL | ROUTE       | DESC                           |
| -------- | ----------- | ------------------------------ |
| POST     | /ServSessao | Rota para autenticar o usuário |

#### Query parameters

A requisição recebe 3 `query parameters`:

| KEY      | VALUE | DESC                           |
| -------- | ----- | ------------------------------ |
| operacao | LOGIN | Operação que vai ser executada |
| LOGIN    | ?     | Login do usuário               |
| SENHA    | ?     | Senha do usuário               |

```typescript
interface ServSessaoRequest {
  operacao: "LOGIN";
  LOGIN: string;
  SENHA: string;
}
```

#### Requisição HTTP para coletar os dados

```http
POST http://localhost:8080/AutoAtendimento/ServSessao?operacao=LOGIN&LOGIN=user_login&SENHA=user_password
Content-Type: application/json; charset=utf-8

[user, { token } ] # retorno
```

Tera como retorno o seguinte dado:

```typescript
interface ServSessaoUser {
  id: number
  login: string
  email: string
  [x: string]: any
}

interface ServSessaoToken {
  token: string
}

type ServSessaoResult = [ServSessaoUser, ServSessaoToken]
```

### Tipos de integração

- [Totem](SGM/TOTEM.md)
- [Chamada](SGM/CHAMADA.md)
```
