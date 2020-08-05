# Obrigações

Para o efeito da submissão da IES/DA os contribuintes passam a ter que usar **obrigatoriamente** o CryptoSAF-T. Essa obrigação reverte-se igualmente nas aplicações de gestão de contabilidade, que passam a ter que o suportar.

## Aspetos chave do CryptoSAF-T

* Ficheiros SAF-T \(PT\) têm que obedecer **integralmente às regras de estrutura** do ficheiro previstas na [Portaria n.º 321-A/2007](https://dre.pt/web/guest/pesquisa/-/search/664305/details/normal?l=1), de 26 de março, na sua redação [atual](https://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/SAFT_PT/Paginas/news-saf-t-pt.aspx).
* É necessário obter uma **soma de verificação \(checksum\)** do ficheiro SAF-T \(PT\) original antes da encriptação dos elementos.
* O checksum é enviado em conjunto com o CryptoSAF-T no momento da submissão da IES/DA.
* A encriptação dos [**elementos**](informacao-tecnica/elementos.md) do SAF-T \(PT\) ocorre com sucesso se assegurada a reversão do processo usando a mesma chave [simétrica](conceito.md#o-que-vai-acontecer), através da qual se obtém o ficheiro original, **estruturalmente sem erros**.
* O checksum é usado para validar que o ficheiro submetido para a entrega da IES/DA está em conformidade com original.

### Segurança da chave simétrica

O [Decreto-Lei Decreto-Lei n.º 48/2020 de 3 de agosto](https://data.dre.pt/eli/dec-lei/48/2020/08/03/p/dre) estabelece na alínea b\) do Artigo 3.º que a chave simétrica tem de ser mantida em sigilo **e não pode ser usada para a encriptação de outro ficheiro relativo o outro exercício ou outro contribuinte \(NIF\), como uma obrigação do fabricante de software**.

A interpretação à letra da legislação pode dar a entender será necessário um mecanismo de custódia da chave simétrica pelo software. Não é isso que se pede.

A responsabilidade da segurança da chave é em primeira instância do contribuinte para quem foi emitida.

Porém, não impede que os produtos de software possam partilhar essa responsabilidade através de um mecanismo de custódia que faça a gestão das chaves dos contribuintes. A centralização de processos relacionados com a chave simétrica através de mecanismos internos de custódia contribuem para o aumento da segurança e para o sucesso das operações.

#### Sobre este aspeto é portanto necessário

* Usar a chave de forma segura nas comunicações com os serviços em que esta é requerida.
* Garantir que é usada a chave correta para a encriptação do ficheiro do contribuinte.
* Garantir que a chave não é usada para encriptar outro ficheiro do mesmo contribuinte relativo a outro exercício ou outro ficheiro de um contribuinte diferente.
* Garantir que a chave não é usada para outro fim que não aquele que está estabelecido na legislação.

