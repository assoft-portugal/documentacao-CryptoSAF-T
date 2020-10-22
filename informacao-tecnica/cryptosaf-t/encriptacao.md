# Encriptação

## Remover do XML informação desnecessária

Para assegurar a verificação do [**checksum**](../saf-t-pt/checksum.md) após a desencriptação do CryptoSAF-T, no ficheiro SAF-T \(PT\) original é, em primeiro lugar, necessário retirar toda a informação desnecessária que possa estar a mais. Este procedimento é feito através do método Canonical XML \[[XML-C14N](https://www.w3.org/TR/xml-exc-c14n/#ref-XML-C14N)\].

### Algumas ferramentas

* .NET [XmlDsigExcC14NTransform Class](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.xml.xmldsigexcc14ntransform?view=dotnet-plat-ext-3.1)
* Windows / Linux / Unix: [libxml2](https://www.aleksey.com/xmlsec/c14n.html)\*\*\*\*
* JAVA: [XOM](https://github.com/elharo/xom/), [Santuario Class Canonicalizer](http://santuario.apache.org/Java/api/org/apache/xml/security/c14n/Canonicalizer.html)

## Handling do XML

Para garantir a integridade da informação contida no ficheiro, o **CryptoSAF-T tem de ser criado a partir do SAF-T \(PT\) original**. Está previsto a criação de um documento técnico com todas as regras que vão estar na base do processo de encriptação. 

Processar um ficheiro XML de grande dimensão pode ser um problema quando este é carregado apenas para a memória:

* [Parsing large XML using iterparse\(\) consumes too much memory. Any alternative?](https://stackoverflow.com/questions/7972823/parsing-large-xml-using-iterparse-consumes-too-much-memory-any-alternative)
* [Best way to parse large XML file in R without reading entire file into memory?](https://stackoverflow.com/questions/45393106/best-way-to-parse-large-xml-file-in-r-without-reading-entire-file-into-memory)
* [Parsing Large XML files, Serially, in Python](http://boscoh.com/programming/reading-xml-serially.html)

