# Conceito

O CryptoSAF-T, designação dada pela comunidade, consiste no ficheiro SAF-T \(PT\) em que um conjunto de informação sensível sobre os contribuintes é descaracterizada. São [**elementos**](informacao-tecnica/cryptosaf-t/elementos.md) que, ao serem tratados sem quaisquer medidas de proteção, podem expor e colocar em causa a segurança e a privacidade dos contribuintes, bem como daqueles que têm relações comerciais com esses.

## Em que consiste

* Os programas de contabilidade têm de criar um ficheiro SAF-T \(PT\) de contabilidade com parte dos [elementos encriptados \(CryptoSAF-T\)](informacao-tecnica/cryptosaf-t/elementos.md).
* O CryptoSAF-T **só tem efeitos para a entrega da IES/DA**.
* O processo de encriptação tem por base o algoritmo de cifra [AES-128-CTR](informacao-tecnica/incm/chave-simetrica.md#aes-128-ctr) que retira partido do uso de uma **chave simétrica**.
* [Imprensa Nacional Casa da Moeda \(INCM\)](https://www.incm.pt/) intervém no processo através da disponibilização do serviço de geração e armazenamento seguro das chaves.
* Existe apenas uma chave simétrica por contribuinte e por ano, respeitante a cada exercício para qual é submetida a IES.

