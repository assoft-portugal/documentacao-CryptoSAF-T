# Conceito

O CryptoSAF-T, designação dada pela comunidade, consiste no ficheiro SAF-T \(PT\) em que um conjunto de informação sensível sobre os contribuintes é descaracterizada. São [**elementos**](informacao-tecnica/cryptosaf-t/elementos.md) que, ao serem tratados sem quaisquer medidas de proteção, podem expor e colocar em causa a segurança e a privacidade dos contribuintes, bem como daqueles que têm relações comerciais com esses.

## Em que consiste

* Um conjunto [elementos](informacao-tecnica/cryptosaf-t/elementos.md) que compõem o ficheiro SAF-T \(PT\) de contabilidade passam a ser encriptados.
* O CryptoSAF-T só tem efeitos práticos para entrega da IES/DA.
* O processo de encriptação tem por base o algoritmo de cifra [AES-128-CTR](informacao-tecnica/incm/chave-simetrica.md#aes-128-ctr) que retira partido do uso de uma [**chave simétrica**](informacao-tecnica/incm/chave-simetrica.md).
* [Imprensa Nacional Casa da Moeda \(INCM\)](https://www.incm.pt/) intervém no processo através do serviço de geração e armazenamento seguro das chaves.
* Existe apenas uma chave simétrica por contribuinte e por ano, respeitante a cada exercício para qual é submetida a IES.
