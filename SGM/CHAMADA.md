# SGM Controle

| PROTOCOL | ROUTE                  | DESC                                                                                                           |
| -------- | ---------------------- | -------------------------------------------------------------------------------------------------------------- |
| POST     | /SrvImpressoraIpRemota | Todas as requisições para pegar os dados irão ocorrer neste `Servlet` mudando somente os parametros utilizados |

## Request

O query params `parametros` passado, é um json com a seguinte estrutura:

```ts
interface Parametros {
  // id do registro do ponto de chamada que referencia o ponto de atendimento
  idImpressora: number;
  // veja na tabela a baixo
  idBotao: 1 | 2 | 3;
  operacao: "CHAMA_SENHA";
  idLoja: 1;
}
```

### Tabela de botões

| Ação           | idBotao |
| -------------- | ------- |
| Próxima senha  | 1       |
| Repetir senha  | 2       |
| Ausentar senha | 3       |
| -              | -       |

### Exemplo de requisição `http`

```http
POST /AutoAtendimento/SrvImpressoraIpRemota?parametros={"idImpressora":1,"idBotao":1,"operacao":"CHAMA_SENHA","idLoja":1}
Content-Type: application/json; charset=utf-8

{ ... } # retorno
```
