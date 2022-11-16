# M5 Chamada de pedidos

# Estrutura dos dados

```ts
declare type Status =
  | "produto-em-preparo" // produto na cascata de em preparo
  | "produto-finalizado" // produto na cascata de finalizados
  | "finalizado"; // removido das cascatas. produto já foi entregue para o cliente

declare type Pedido = {
  id: number;
  id_loja: number;
  // layout que esta sendo utilizado na tv
  id_layout: number;
  // código do pedido
  dado: string;
  status: Status;
  complemento: {
    // esta informação vai aparecer na tv
    loja: string;
  };
  created_at: string;
  updated_at: string;
};
```

# Chamada de pedidos

| PROTOCOL | ROUTE        | DESC                                                                                         |
| -------- | ------------ | -------------------------------------------------------------------------------------------- |
| POST     | /ServRotinas | Todas as requisições serão feitas neste `end-point` mudando somente os parametros utilizados |

# Parametros

Toda a requisição opera por parametros na url e para o acesso ao controle contem a seguinte estrutura.

| Chave      | Valor                                     | Descrição                                                                                      |
| ---------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------- |
| token      | -                                         | o token adquirido ao realizar a autenticação como mostrado na [sessão inicial](../README.md) . |
| operacao   | backing_bean                              | -                                                                                              |
| classe     | br.marcars.m5.backingbean.AnunciosBacking | -                                                                                              |
| metodo     | -                                         | [confira logo a baixo](#metodos)                                                               |
| parametros | -                                         | [confira logo a baixo](#parametros)                                                            |

Cada método possui seus próprios parametros em formato `json`.

## Métodos

Os métodos são as operações dentro do `endpoint` que podem ser utilizados, todos eles recebem parametros especificos.

| Método       | Descrição                                             |
| ------------ | ----------------------------------------------------- |
| listPedidos  | Retorna um `array` com os dados de pedidos por página |
| salvarPedido | salva o pedido e executa a chamada dele nos players   |

#### listPedidos

```http
# id_layout: é o que define de qual TV vai ser retornado os pedidos que não foram removidos ainda pelo usuário
# page: a paginação dos pedidos. Retorna em pilhas de 20 registros.

POST /M5/ServRotinas?operacao=backing_bean&classe=br.marcars.m5.backingbean.AnunciosBacking&metodo=listPedidos&parametros={"id_layout":0,page:1}&token=TOKEN
Content-Type: application/json; charset=utf-8

# o retorno vai ser nos moldes da estrutura Pedidos acima des
# [...]
```

#### salvarPedido

```http
# id_layout: é o que define de qual TV vai ser retornado os pedidos que não foram removidos ainda pelo usuário
# page: a paginação dos pedidos. Retorna em pilhas de 20 registros.

POST /M5/ServRotinas?operacao=backing_bean&classe=br.marcars.m5.backingbean.AnunciosBacking&metodo=listPedidos&parametros={"id_layout":0,"id":0,"dado":"123","status":"produto-finalizado","complemento":{"loja":"TEXTO AQUI"}}&token=TOKEN
Content-Type: application/json; charset=utf-8

# {"id":0,"id_loja":0,"id_layout":0,"dado":"123","status":"produto-finalizado","complemento":{"loja":"TEXTO AQUI"},"created_at":"2001-01-01 12:00:00.0","updated_at":"2001-01-01 12:00:00.0"}
```
