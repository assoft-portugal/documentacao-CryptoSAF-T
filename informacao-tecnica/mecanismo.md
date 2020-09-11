# Mecanismo

A encriptação dos [**elementos**](elementos.md) tem por base o algoritmo de cifra [aes-128-ctr](../exemplos/exemplos.md) \(por confirmar\) que retira partido do uso de uma **chave simétrica**. A par da encriptação existe também o desafio da obtenção do checksum do ficheiro SAF-T \(PT\) original.

O CryptoSAF-T pode ser criado a partir do SAF-T \(PT\) original ou, em alternativa, criado após ou em simultâneo com o ficheiro original. Ambas as abordagens são viáveis. Porém, é de considerar que a primeira abordagem pode implicar problemas ao nível do _handling_ de ficheiros XML com grande dimensão.

Analisar um ficheiro XML de grande dimensão pode ser um problema quando este é carregados apenas para a memória:

* [Parsing large XML using iterparse\(\) consumes too much memory. Any alternative?](https://stackoverflow.com/questions/7972823/parsing-large-xml-using-iterparse-consumes-too-much-memory-any-alternative)
* [Best way to parse large XML file in R without reading entire file into memory?](https://stackoverflow.com/questions/45393106/best-way-to-parse-large-xml-file-in-r-without-reading-entire-file-into-memory)
* [Parsing Large XML files, Serially, in Python](http://boscoh.com/programming/reading-xml-serially.html)

A segunda abordagem, de exportar ambos os ficheiros a partir do ERP \(SAF-T e CryptoSAF-T\) pode ser a mais adequada nos casos o resultado sejam ficheiros de grande dimensão. Porém, esta abordagem reforça a necessidade de garantir a integridade estrutural de ambos os ficheiros criados, de modo a não comprometer o checksum.

