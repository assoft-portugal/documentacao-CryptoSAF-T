# Conceito

O CryptoSAF-T, designação dada pela comunidade, consiste no ficheiro SAF-T \(PT\) em que um conjunto de informação sensível sobre os contribuintes é descaracterizada. São [**elementos**](informacao-tecnica/elementos.md) que, ao serem tratados sem quaisquer medidas de proteção, podem expor e colocar em causa a segurança e a privacidade dos contribuintes, bem como daqueles que têm relações comerciais com esses.

## Em que consiste

* Os programas de contabilidade têm de criar um ficheiro SAF-T \(PT\) de contabilidade com parte dos [elementos encriptados \(CryptoSAF-T\)](informacao-tecnica/elementos.md).
* O CryptoSAF-T **só tem efeitos para a entrega da IES/DA**.
* O processo de encriptação tem \(por confirmar\) por base o algoritmo de cifra [AES-GCM authenticated encryption,](https://en.wikipedia.org/wiki/Galois/Counter_Mode) que retira partido do uso de uma **chave simétrica**.
* [Imprensa Nacional Casa da Moeda \(INCM\)](https://www.incm.pt/) intervém no processo através da disponibilização do serviço de geração e armazenamento seguro das chaves.
* Existe apenas uma chave simétrica por contribuinte e por ano, respeitante a cada exercício para qual é submetida a IES.

## Estrutura do SAF-T \(PT\) de contabilidade

A estrutura encontra-se regulada pela Portaria 302/2016 de 2 de dezembro \( [**PT**](https://info.portaldasfinancas.gov.pt/pt/informacao_fiscal/legislacao/diplomas_legislativos/Documents/Portaria_302_2016.pdf) \| [**EN**](https://info.portaldasfinancas.gov.pt/pt/docs/Portug_tax_system/Documents/Ordinance_No_302_2016_of_the_2nd_December.pdf) \) e o respetivo schema XSD pode ser descarregado [**aqui**](https://info.portaldasfinancas.gov.pt/apps/saft-pt04/saftpt1.04_01.xsd). Para mais informação pode aceder à área destinada ao SAF-T \(PT\) no [Portal das Finanças](https://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/SAFT_PT/Paginas/news-saf-t-pt.aspx).

| Índice | Elemento |
| :--- | :--- |
| 1. | Cabeçalho \(Header\) |
| 2.1. | Tabela de código de contas \([GeneralLedgerAccounts](informacao-tecnica/elementos.md#2-1-tabela-de-codigos-de-contas-generalledgeraccounts)\) |
| 2.2. | Tabela de clientes \([Customer](informacao-tecnica/elementos.md#2-2-tabela-de-clientes-customer)\) |
| 2.3. | Tabela de fornecedores \([Supplier](informacao-tecnica/elementos.md#2-3-tabela-de-fornecedores-supplier)\) |
| 2.5. | Tabela de impostos \([TaxTable](informacao-tecnica/elementos.md#2-5-tabela-de-impostos-taxtable)\) |
| 3. | Movimentos contabilísticos \([GeneralLedgerEntries](informacao-tecnica/elementos.md#3-tabela-de-movimentos-contabilisticos-generalledgerentries)\) |
| 4.4. | Documentos de recibos emitidos \([Payments](informacao-tecnica/elementos.md#4-4-tabela-de-documentos-de-recibos-emitidos-payments)\) |

