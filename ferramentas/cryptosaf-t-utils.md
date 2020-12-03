# SAF-T: Utils

\*\*\*\*[**CryptoSAF-T: SAF-T Utils**](https://github.com/assoft-portugal/CryptoSAF-T-SAF-T-Utils): é um ambiente escrito em JAVA que permite testar o método de encriptação do ficheiro SAF-T \(PT\) usando o algoritmo AES-128-CTR \(FastSaftEncrypt\), assim, como a canonização do ficheiro SAF-T \(PT\) original \(FastHashCannon\).

{% tabs %}
{% tab title="FastHashCannon" %}
### Execução

`pt.cryptosaft.demo.FastHashCannon [inputXml] [outputXml]`

| Argumento | Descrição | Valores |
| :--- | :--- | :--- |
| inputXml | Ficheiro SAF-T de entrada | Ex: `saft.xml` |
| outputXml | Ficheiro SAF-T canonizado | Ex: `saft_can.xml` |

### **JAR**

`java -jar jar/FastHashCannon.jar-jar-with-dependencies.jar src/main/resources/Exemplo_Facturacao.xml`
{% endtab %}

{% tab title="FastSaftEncrypt" %}
### Execução

`pt.cryptosaft.demo.FastSaftEncrypt [modo] [inputXml] [outputXml] [chave] [iv]`

| Argumento | Descrição | Valores |
| :--- | :--- | :--- |
| modo | Modo de operação | `E` - Descaracterização \(Encrypt\); `D` - Reversão \(Decrypt\); |
| inputXml | Ficheiro SAF-T de entrada | Ex: `saft.xml` |
| outputXml | Ficheiro SAF-T de saída | Ex: `saft_desc.xml` |
| chave | Chave simétrica em formato Base64 | Ex: `8/K97v8vQqbD/ShX5yx+3g==` |
| iv | Vetor de inicialização em formato Base64 | Ex: `+KSjwLJcoMXl7W+U1y5VtQ==` |

### **JAR**

`java -jar jar/FastSaftEncrypt.jar-jar-with-dependencies.jar E src/main/resources/Exemplo_Facturacao.xml /src/main/resources/CryptoSAFT-Exemplo_Facturacao.xml 8/K97v8vQqbD/ShX5yx+3g== +KSjwLJcoMXl7W+U1y5VtQ==`
{% endtab %}
{% endtabs %}

