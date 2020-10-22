# Webservice

## **Descrição**

O pedido da chave e do respetivo IV pode ser feito através de webservice cujas especificações assentam num WSDL standard.

Nesta página encontra os métodos que o webservice disponibiliza para todos os utilizadores. Este webservice estará \(quando for lançado\) exposto ao publico sem qualquer autenticação e permitirá invocar as operações de pedido e levantamento de chave.

{% file src="../../.gitbook/assets/public \(1\).wsdl" caption="public.wdsl \(testes\)" %}

## Pedido de Chave \(KeyRequest\) 

Serviço de pedido de chave que associa uma chave a um par `Ano fiscal/NIF` e despoleta o envio de uma notificação ViaCTT para o contribuinte com o respetivo código de levantamento. 

### Parâmetros de entrada 

* `KeyRequest.FiscalYear` \(Obrigatório, Valor numérico entre 2020 e 9999\) - Ano fiscal a que se refere o pedido de chave 
* `KeyRequest.VatNumber` \(Obrigatório, Valor numérico entre 100000000 e 999999999\) - Número de identificação fiscal a que se refere o pedido de chave 

### Parâmetros de saída 

* `KeyRequestResponse.Response` \(Obrigatório\) - Ver Response mais abaixo 

## Levantamento de Chave \(KeyRetrieve\) 

Serviço de pedido de levantamento de chave que devolve a chave associada a um par `Ano fiscal/NIF`, validando o código de autenticação. 

### Parâmetros de entrada 

* `KeyRetrieve.FiscalYear` \(Obrigatório, Valor numérico entre 2020 e 9999\) - Ano fiscal a que se refere o levantamento de chave 
* `KeyRetrieve.VatNumber` \(Obrigatório, Valor numérico entre 100000000 e 999999999\) - Número de identificação fiscal a que se refere o levantamento de chave 
* `KeyRetrieve.RetrieveCode` \(Obrigatório, Valor alfanumérico com tamanho 10\) - Código de levantamento enviado ao contribuinte pelo ViaCTT. 

### Parâmetros de saída 

* `KeyRetrieveResponse.Key` - \(Opcional\) Chave produzida pelo serviço correspondente ao Ano Fiscal/Nif \(Base64\) 
* `KeyRetrieveResponse.IV` - \(Opcional\) Vector de inicialização \(IV\) produzida pelo serviço correspondente ao Ano Fiscal/Nif \(Base64\) 
* `KeyRetrieveResponse.Response` \(Obrigatório\) - Ver Response mais abaixo 

### Response 

* `Response.ResponseCode` \(Obrigatório, Valor numérico\) - Código de resposta que indica o sucesso ou erro da mesma \(ver tabela de Códigos de erro\) 
* `Response.Error` \(Opcional, Valor alfanumérico\) - Breve descrição do erro 

## Códigos de erro 

| ResponseCode  | Descrição  |
| :--- | :--- |
| 0  | Sucesso  |
| 1  | Ano fiscal inválido  |
| 2  | NIF inválido  |
| 3  | Código de levantamento inválido  |
| 4  | Ano fiscal/NIF bloqueado  |
| 5  | Aviso - Canal ViaCTT não ativo |

