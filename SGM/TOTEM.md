# SGM Totem

| PROTOCOL | ROUTE        | DESC                                                                                                           |
| -------- | ------------ | -------------------------------------------------------------------------------------------------------------- |
| POST     | /ServRotinas | Todas as requisições para pegar os dados irão ocorrer neste `Servlet` mudando somente os parametros utilizados |

## Tópicos

- [Query parameters](#query-parameters)
- [Retornos por método](#retornos-por-método)
- [Requisição HTTP](#requisição-http)

## Query parameters

A requisição recebe 4 `query parameters`:

| KEY        | VALUE                                        | DESC                                           |
| ---------- | -------------------------------------------- | ---------------------------------------------- |
| classe     | autoatendimento.backingbean.RecepcoesBacking | Valor utilizado em todas as requisições.       |
| metodo     | `getRecepcoes`, `getRecepcoesBotoes`         | Valores aceitos. Veja a descrição logo abaixo. |
| operacao   | backing_bean                                 | Valor utilizado em todas as requisições.       |
| parametros | `{}`                                         | Deve ser enviado um json vazio                 |

> `getRecepcoes` - Recebe a lista com todas as recepções cadastradas no sistema.
>
> `getRecepcoesBotoes` - Recebe uma lista com todos os botões cadastrados na recepção.

## Retornos por método

#### getRecepcoes

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
interface Arquivo {
  descricao: string;
  dt_criacao: string;
  extensao: string;
  folder: string;
  id: number;
  id_loja: number;
  id_usuario: number;
}

interface Recepcao {
  botoes_alinhamento: "left" | "center" | "right";
  botoes_colunas: number;
  botoes_espacamento: number;
  descricao: string;
  extensao: string;
  folder: string;
  id: number;
  idArquivo: number;
  status: 0 | 1;
  timeout: number;
  nome_impressora: string;
  tipo_recepcao:
    | "RECEPCAO"
    | "CLASSIFICACAO"
    | "GRUPO_GUICHES"
    | "IDENTIFICACAO"
    | "IDENTIFICACAO_BIOMETRIA";
  titulo: string | null;
  titulo_cor: string | null;
  titulo_font_size: string | null;
  vias_imprimir: number;
  json: {
    title: {
      text: string;
      color: string;
      font_size: number;
    };
    buttons: {
      align: "left" | "center" | "right";
      gap: number;
      columns: number;
    };
  };
  arquivos: Arquivo[];
}

// Exemplo de retorno

type result = Recepcao[];
```

#### getRecepcoesBotoes

| KEY                      | VALUES          | DESC                                                                            |
| ------------------------ | --------------- | ------------------------------------------------------------------------------- |
| botao_alinhamento_texto  | string          | Alinhamento do botão no texto                                                   |
| botao_altura             | number          | Altura do botão                                                                 |
| botao_cor                | string          | Cor de fundo do botão                                                           |
| botao_cor_texto          | string          | Cor do texto no botão                                                           |
| botao_fonte_tamanho      | number          | Tamanho da fonte utilizada no botão                                             |
| botao_fonte_texto        | string          | Fonte utilizada no botão                                                        |
| botao_largura            | number          | Largura do botão                                                                |
| classificar_senha        | number          | Referencia ao retorno `getRecepcoes` com `tipoRecepcao` 'CLASSIFICACAO'         |
| codigo_servico           | number          | ID da espera                                                                    |
| cor_fundo_tooltip        | string          | cor de fundo da legenda/tooltip                                                 |
| cor_texto_tooltip        | string          | cor do texto da legenda/tooltip                                                 |
| descricao                | string          | Legenda/Tooltip do botão                                                        |
| id                       | number          | ID do registro                                                                  |
| id_etiqueta              | number          | ID da etiqueta utilizada                                                        |
| id_recepcao              | number          | Referencia ao retorno `getRecepcoes`                                            |
| inserir_complemento      | number          | se ao clicar neste botão ele precisa inserir um complemento ao botão            |
| label                    | string          | Nome do botão                                                                   |
| ordem                    | number          | Ordem do botão na tela                                                          |
| permite_redirecionamento | number          | Referencia ao retorno `getRecepcoes` com `tipoRecepcao` 'GRUPO_GUICHES'         |
| status                   | 0/1             | se esta ativo ou não                                                            |
| tipo_servico             | ESPERA/RECEPCAO | Tipo de botão. 'ESPERA' emite a impressão, 'RECEPCAO' direciona para outra tela |
| tooltip_fonte_tamanho    | number          | tamanho da fonte utilizada na legenda/tooltip                                   |

Cada botão pode conter varios periodos cadastrados.

| KEY                | VALUES        | DESC                                      |
| ------------------ | ------------- | ----------------------------------------- |
| dia_semana         | 0/1/2/3/4/5/6 | dia da semana em número: domingo - sábado |
| fim                | string        | hora do fim de exibição do botão          |
| id                 | number        | ID do registro                            |
| id_recepcao_espera | number        | referencia ao retorno getRecepcoesBotoes  |
| inicio             | string        | hora de inicio de exibição do botão       |

```ts
// O retorno será um array de RecepcoesBotoes

interface RecepcaoEsperasPeriodoAtivo {
  id: number
  id_recepcao_espera: number
  dia_semana: number
  inicio: string
  fim: string
}

interface Espera {
  id: number
  label: string
  prefixo: string
}

interface RecepcoesBotoes {
  botao_alinhamento_texto: "left" | "center" | "right";
  botao_altura: number;
  botao_cor: string;
  botao_cor_texto: string;
  botao_fonte_tamanho: number;
  botao_fonte_texto: string;
  botao_largura: number;
  classificar_senha: number;
  codigo_servico: number;
  cor_fundo_tooltip: string;
  cor_texto_tooltip: string;
  id: number;
  id_etiqueta: number;
  id_recepcao: number;
  inserir_complemento: number;
  json: {
    enableEcoPrint: false;
    message: null;
  };
  espera: Espera
  label: string;
  ordem: number;
  permite_redirecionamento: number;
  status: number;
  tipo_servico: "ESPERA" | 'RECEPCAO;
  titulo_complemento: string;
  tooltip_fonte_tamanho: number;
  recepcao_esperas_periodo_ativo: RecepcaoEsperasPeriodoAtivo[];
}

// Exemplo de retorno

type result = RecepcoesBotoes[];
```

## Requisição HTTP para coletar os dados

#### Método getRecepcoes

[Informações do retorno](#getrecepcoes)

```http
POST /AutoAtendimento/ServRotinas?classe=autoatendimento.backingbeans.APIBacking&operacao=backing_bean&metodo=getRecepcoes&parametros={}&token=SEU_TOKEN

{ ... } # retorno
```

#### Método getRecepcoesBotoes

[Informações do retorno](#getrecepcoesbotoes)

```http
POST /AutoAtendimento/ServRotinas?classe=autoatendimento.backingbeans.APIBacking&operacao=backing_bean&metodo=getRecepcaoBotoes&parametros={"idRecepcao":ID_RECEPCAO}&token=SEU_TOKEN
Content-Type: application/x-www-form-urlencoded; charset=utf-8

{ ... } # retorno
```

## Requisição HTTP de impressão

| KEY                | VALUES                                  | DESC                                |
| ------------------ | --------------------------------------- | ----------------------------------- |
| classe             | autoatendimento.backingbeans.APIBacking |                                     |
| operacao           | backing_bean                            |                                     |
| metodo             | novaSenha                               |                                     |
| parametros         | {}                                      |                                     |
| sequencia          | 0                                       |                                     |
| idRecepcaoOriginal | number                                  | id da recepção onde esta o botão    |
| idRecepcao         | number                                  | id da recepção onde esta o botão    |
| somenteGerar       | boolean                                 | true (não imprime), false (imprime) |
| idBotao            | number                                  | id do botão a ser gerado            |
| idEspera           | number                                  | id da espera do botão               |

```ts
interface EsperaGrupo {
  id: number;
  descricao: string;
}

interface PrintObject {
  id: number;
  numeroSenha: number;
  retirada: string;
  idLoja: number;
  fichaChamada: false;
  tempoDecorridoEspera: number;
  status: {
    id: number;
    descricao: string;
  };
  espera: {
    id: number;
    prefixo: string;
    label: string;
    corCodigo: string;
    esperaAmarelo: number;
    esperaVerde: number;
    esperaVermelho: number;
  };
  corCodigo: string;
  esperaGrupo: EsperaGrupo[];
  classificacoes: [];
}
```

```http
POST /AutoAtendimento/SrvlRecepcao?classe=autoatendimento.backingbeans.APIBacking&operacao=backing_bean&metodo=imprimirSenha&parametros=%7B%7D&idRecepcaoOriginal=ID_RECEPCAO&idRecepcao=ID_RECEPCAO&somenteGerar=false&sequencia=0&idBotao=ID_RECEPCAO_ESPERA&idEspera=ID_ESPERA&token=SEU_TOKEN
Content-Type: application/json; charset=utf-8

{ ... } # retorno
```
