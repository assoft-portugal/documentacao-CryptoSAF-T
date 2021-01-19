# Chave simétrica

A INCM é responsável pela disponibilização e manutenção do serviço de geração e armazenamento de chaves. Estas chaves são guardadas pelo período de conservação estabelecido do decreto, de 15 anos. No regulamento da INCM \(ainda por publicar\) constam os termos de adesão ao serviço, bem como as regras técnicas para a sua utilização.

## AES

Consiste numa cifra de blocos que foi especificada pelo NIST em 2001. É conhecida por ser uma cifra de alta performance e baixa memória tendo implementações em todas as principais linguagens de programação e ferramentas \(Java, C++, C\#, OpenSSL\) Não se conhecem ataques práticos a esta cifra principalmente quando são usadas com chaves de 128 bits ou superior.

### AES-128-CTR

No modo CTR os blocos cifrados não dependem do anterior mas sim de um contador que é incrementado a cada bloco. Este modo de operação faz com que para valores de entrada idênticos não sejam obtidos resultados idênticos, que poderiam levar a uma fácil identificação do conteúdo.

## Pedido de chave simétrica

1. O pedido de chaves é efetuado pelas aplicações através da invocação de um `webservice` fornecido pela INCM.
2. No processo de invocação é necessário informar: - `NIF` do Contribuinte - `Ano` a que respeita a declaração da IES/DA
3. Posteriormente, a INCM envia a senha de autenticação para o endereço viaCTT do contribuinte. Este, terá que a fornecer à aplicação de contabilidade.
4. A aplicação invoca de novo o `webservice` da INCM, mas, nesta fase, indicando o `NIF`, o `Ano` e a `senha do contribuinte`, para  obter, finalmente, a chave simétrica.

> ### Este processo pode ser repetido, permitindo recuperar a chave caso seja necessário.

Consulte a página de [**Pedido de chave**](pedido-de-chave.md) para saber mais acerca deste processo.

