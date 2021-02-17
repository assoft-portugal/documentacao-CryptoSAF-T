# CryptoSAF-T

## Aspetos chave do CryptoSAF-T

* Ficheiros SAF-T \(PT\) têm que obedecer **integralmente às regras de estrutura** do ficheiro previstas na [Portaria n.º 321-A/2007](https://dre.pt/web/guest/pesquisa/-/search/664305/details/normal?l=1), de 26 de março, na sua redação [atual](https://info.portaldasfinancas.gov.pt/pt/apoio_contribuinte/SAFT_PT/Paginas/news-saf-t-pt.aspx).
* É necessário obter uma **soma de verificação \(**[**checksum**](informacao-tecnica/saf-t-pt/checksum.md)**\)** do ficheiro SAF-T \(PT\) original antes da encriptação dos elementos.
* O checksum é enviado em conjunto com o CryptoSAF-T no momento da submissão da IES/DA.
* A encriptação dos [**elementos**](informacao-tecnica/cryptosaf-t/elementos.md) do SAF-T \(PT\) ocorre com sucesso se assegurada a reversão do processo usando a mesma chave [simétrica](conceito.md#o-que-vai-acontecer), através da qual se obtém o ficheiro original, **estruturalmente sem erros**.
* O checksum é usado para validar que o ficheiro baseado no CryptoSAF-T que é submetido para o efeito da entrega da IES/DA é igual ao original.

