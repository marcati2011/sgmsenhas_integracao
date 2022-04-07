# SGM Controle

| PROTOCOL | ROUTE        | DESC                                                                          |
| -------- | ------------ | ----------------------------------------------------------------------------- |
| POST     | /ServRotinas | Todas as requisições serão `Servlet` mudando somente os parametros utilizados |

# Parametros

Toda a requisição opera por parametros na url e para o acesso ao controle contem a seguinte estrutura.

| Chave      | Valor                   | Descrição                                                                                      |
| ---------- | ----------------------- | ---------------------------------------------------------------------------------------------- |
| token      | -                       | o token adquirido ao realizar a autenticação como mostrado na [sessão inicial](../README.md) . |
| operacao   | backing_bean            | -                                                                                              |
| classe     | sgm.server.api.Controle | -                                                                                              |
| metodo     | -                       | [confira logo a baixo](#metodos)                                                               |
| parametros | -                       | [confira logo a baixo](#parametros)                                                            |

Cada método possui seus próprios parametros em formato `json`.

## Métodos

Os métodos são as operações dentro do `endpoint` que podem ser utilizados, todos eles recebem parametros especificos.

| Método     | Descrição                                                         |
| ---------- | ----------------------------------------------------------------- |
| getGuiches | Retorna um `array` com os dados dos guichês disponiveis na sessão |
| proximo    | para chamar a próxima senha pelo controle                         |

#### getGuiches

```http
POST /AutoAtendimento/ServRotinas?operacao=backing_bean&classe=sgm.server.api.Controle&metodo=getGuiches&parametros={}&token=TOKEN
Content-Type: application/json; charset=utf-8

# [{ value: idGuiche, description: nomeGuiche }, ...]
```

> O `idGuiche` é o dado que vai ser utilizado para as demais operações e por isto deve ser guardado.

#### proximo

```http
# $ID_GUICHE é o valor armazenado de um dos objetos retornados na requisição "getGuiches"

POST /AutoAtendimento/ServRotinas?operacao=backing_bean&classe=sgm.server.api.Controle&metodo=proximo&parametros={"idGuiche":$ID_GUICHE}&token=TOKEN
Content-Type: application/json; charset=utf-8

# verificar ainda, backing_bean incompleto

```