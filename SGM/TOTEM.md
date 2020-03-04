# SGM Totem

| PROTOCOL | ROUTE        | DESC                                                                                      |
| -------- | ------------ | ----------------------------------------------------------------------------------------- |
| POST     | /ServRotinas | Todas as requisições vão ocorrer neste `Servlet` mudando somente os parametros utilizados |

## Tópicos

- [Query parameters](#query-parameters)
- [Retornos por método](#retornos-por-método)
- [Requisição HTTP](#requisição-http)

## Query parameters

A requisição recebe 4 `query parameters`:

| KEY        | VALUE                                                     | DESC                                           |
| ---------- | --------------------------------------------------------- | ---------------------------------------------- |
| classe     | autoatendimento.backingbean.RecepcoesBacking              | Valor utilizado em todas as requisições.       |
| metodo     | `getRecepcoes`, `getRecepcoesBotoes`, `getPeriodosAtivos` | Valores aceitos. Veja a descrição logo abaixo. |
| operacao   | backing_bean                                              | Valor utilizado em todas as requisições.       |
| parametros | `{}`                                                      | Deve ser enviado um json vazio                 |

> `getRecepcoes` - Recebe a lista com todas as recepções cadastradas no sistema.
>
> `getRecepcoesBotoes` - Recebe uma lista com todos os botões cadastrados na recepção.
>
> `getPeriodosAtivos` - Recebe uma lista dos periodos ativos dos botões cadastrados.

## Retornos por método

#### getRecepcoes

> ATENÇÃO: O retorno não é sofre um parse, então todos os campos retornam como string.

| KEY               | VALUES                                                                                       | DESC                                                                      |
| ----------------- | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| botoesAlinhamento | 'left' / 'center' / 'right'                                                                  | Alinhamento das colunas                                                   |
| botoesColunas     | number                                                                                       | Quantidade de colunas na tela                                             |
| botoesEspacamento | number                                                                                       | Espaçamento entre os botões                                               |
| classificarSenha  | number                                                                                       | 0 - não classificar, outros seria o id da recepção do tipo Classificação  |
| descricao         | string                                                                                       | Nome da recepção                                                          |
| extensao          | string                                                                                       | Extenção da imagem de fundo                                               |
| folder            | string                                                                                       | Pasta da imagem de fundo                                                  |
| id                | number                                                                                       | ID do registro                                                            |
| idArquivo         | number                                                                                       | ID da imagem de fundo                                                     |
| status            | 0/1                                                                                          | 0 - inativa, 1 - ativa                                                    |
| timeout           | number                                                                                       | Timeout de retorno para esta tela                                         |
| tipoRecepcao      | 'RECEPCAO' / 'CLASSIFICACAO' / 'GRUPO_GUICHES' / 'IDENTIFICACAO' / 'IDENTIFICACAO_BIOMETRIA' | Tipo da recepção                                                          |
| titulo            | string / null                                                                                | Campo opcional, cada tela de recepção pode ter uma mensagem personalizada |
| tituloCor         | string / null                                                                                | Cor do título                                                             |
| tituloFontSize    | string / null                                                                                | Tamanho da fonte do título                                                |
| viasImprimir      | number                                                                                       | Quantidade de vias a serem impressas                                      |

> caminho da imagem: /AutoAtendimento/images/{folder}/{idArquivo}.{extensao}

```ts
interface Recepcao {
  botoesAlinhamento: "left" | "center" | "right";
  botoesColunas: number;
  botoesEspacamento: number;
  classificarSenha: number;
  descricao: string;
  extensao: string;
  folder: string;
  id: number;
  idArquivo: number;
  status: 0 | 1;
  timeout: number;
  tipoRecepcao:
    | "RECEPCAO"
    | "CLASSIFICACAO"
    | "GRUPO_GUICHES"
    | "IDENTIFICACAO"
    | "IDENTIFICACAO_BIOMETRIA";
  titulo: string | null;
  tituloCor: string | null;
  tituloFontSize: string | null;
  viasImprimir: number;
}

// Exemplo de retorno

type result = Recepcao[];
```

#### getRecepcoesBotoes

> ATENÇÃO: O retorno não é sofre um parse, então todos os campos retornam como string.

| KEY                     | VALUES          | DESC                                                                            |
| ----------------------- | --------------- | ------------------------------------------------------------------------------- |
| botaoAlinhamentoTexto   | string          | Alinhamento do botão no texto                                                   |
| botaoAltura             | number          | Altura do botão                                                                 |
| botaoCor                | string          | Cor de fundo do botão                                                           |
| botaoCorTexto           | string          | Cor do texto no botão                                                           |
| botaoFonteTamanho       | number          | Tamanho da fonte utilizada no botão                                             |
| botaoFonteTexto         | string          | Fonte utilizada no botão                                                        |
| botaoLargura            | number          | Largura do botão                                                                |
| classificarSenha        | number          | Referencia ao retorno `getRecepcoes` com `tipoRecepcao` 'CLASSIFICACAO'         |
| codigoServico           | number          | ID da espera                                                                    |
| corFundoTooltip         | string          | cor de fundo da legenda/tooltip                                                 |
| corTextoTooltip         | string          | cor do texto da legenda/tooltip                                                 |
| descricao               | string          | Legenda/Tooltip do botão                                                        |
| id                      | number          | ID do registro                                                                  |
| idEtiqueta              | number          | ID da etiqueta utilizada                                                        |
| idRecepcao              | number          | Referencia ao retorno `getRecepcoes`                                            |
| inserirComplemento      | number          | se ao clicar neste botão ele precisa inserir um complemento ao botão            |
| label                   | string          | Nome do botão                                                                   |
| ordem                   | number          | Ordem do botão na tela                                                          |
| permiteRedirecionamento | number          | Referencia ao retorno `getRecepcoes` com `tipoRecepcao` 'GRUPO_GUICHES'         |
| status                  | 0/1             | se esta ativo ou não                                                            |
| tipoServico             | ESPERA/RECEPCAO | Tipo de botão. 'ESPERA' emite a impressão, 'RECEPCAO' direciona para outra tela |
| tooltipFonteTamanho     | number          | tamanho da fonte utilizada na legenda/tooltip                                   |

```ts
// O retorno será um array de RecepcoesBotoes

interface RecepcoesBotoes {
  botaoAlinhamentoTexto: string;
  botaoAltura: number;
  botaoCor: string;
  botaoCorTexto: string;
  botaoFonteTamanho: number;
  botaoFonteTexto: string;
  botaoLargura: number;
  classificarSenha: number;
  codigoServico: number;
  corFundoTooltip: string;
  corTextoTooltip: string;
  descricao: string;
  id: number;
  idEtiqueta: number;
  idRecepcao: number;
  inserirComplemento: number;
  label: string;
  ordem: number;
  permiteRedirecionamento: number;
  status: 0 | 1;
  tipoServico: "ESPERA" | "RECEPCAO";
  tooltipFonteTamanho: number;
}

// Exemplo de retorno

type result = RecepcoesBotoes[];
```

#### getPeriodosAtivos

> ATENÇÃO: O retorno não é sofre um parse, então todos os campos retornam como string.

Cada botão pode conter varios periodos cadastrados.

| KEY              | VALUES        | DESC                                      |
| ---------------- | ------------- | ----------------------------------------- |
| diaSemana        | 0/1/2/3/4/5/6 | dia da semana em número: domingo - sábado |
| fim              | string        | hora do fim de exibição do botão          |
| id               | number        | ID do registro                            |
| idRecepcaoEspera | number        | referencia ao retorno getRecepcoesBotoes  |
| inicio           | string        | hora de inicio de exibição do botão       |

```ts
interface PeriodoAtivo {
  diaSemana: 0 | 1 | 2 | 3 | 4 | 5 | 6;
  fim: string;
  id: number;
  idRecepcaoEspera: number;
  inicio: string;
}

// Exemplo de retorno

type result = PeriodoAtivo[];
```

## Requisição HTTP

```http
POST /AutoAtendimento/ServRotinas?classe=autoatendimento.backingbean.RecepcoesBacking&operacao=backing_bean&metodo=getRecepcoes&parametros=%7B%7D
```
