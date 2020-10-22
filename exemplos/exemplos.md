# Exemplos

## Encriptação AES-CBC em PowerShell

Crédito de [@ctigeek](https://gist.github.com/ctigeek/2a56648b923d198a6e60)

```text
function Create-AesManagedObject($key, $IV) {
    $aesManaged = New-Object "System.Security.Cryptography.AesManaged"
    $aesManaged.Mode = [System.Security.Cryptography.CipherMode]::CBC
    $aesManaged.Padding = [System.Security.Cryptography.PaddingMode]::Zeros
    $aesManaged.BlockSize = 128
    $aesManaged.KeySize = 256
    if ($IV) {
        if ($IV.getType().Name -eq "String") {
            $aesManaged.IV = [System.Convert]::FromBase64String($IV)
        }
        else {
            $aesManaged.IV = $IV
        }
    }
    if ($key) {
        if ($key.getType().Name -eq "String") {
            $aesManaged.Key = [System.Convert]::FromBase64String($key)
        }
        else {
            $aesManaged.Key = $key
        }
    }
    $aesManaged
}

function Create-AesKey() {
    $aesManaged = Create-AesManagedObject
    $aesManaged.GenerateKey()
    [System.Convert]::ToBase64String($aesManaged.Key)
}

function Encrypt-String($key, $unencryptedString) {
    $bytes = [System.Text.Encoding]::UTF8.GetBytes($unencryptedString)
    $aesManaged = Create-AesManagedObject $key
    $encryptor = $aesManaged.CreateEncryptor()
    $encryptedData = $encryptor.TransformFinalBlock($bytes, 0, $bytes.Length);
    [byte[]] $fullData = $aesManaged.IV + $encryptedData
    $aesManaged.Dispose()
    [System.Convert]::ToBase64String($fullData)
}

function Decrypt-String($key, $encryptedStringWithIV) {
    $bytes = [System.Convert]::FromBase64String($encryptedStringWithIV)
    $IV = $bytes[0..15]
    $aesManaged = Create-AesManagedObject $key $IV
    $decryptor = $aesManaged.CreateDecryptor();
    $unencryptedData = $decryptor.TransformFinalBlock($bytes, 16, $bytes.Length - 16);
    $aesManaged.Dispose()
    [System.Text.Encoding]::UTF8.GetString($unencryptedData).Trim([char]0)
}

$key = Create-AesKey
$key
$unencryptedString = "Mensagem de teste"
$encryptedString = Encrypt-String $key $unencryptedString
$backToPlainText = Decrypt-String $key $encryptedString

if ($unencryptedString -ne $backToPlainText) {
    throw "encryption didn't work!"
}
else {
    Write-Host "Porreiro, funcionou: $backToPlainText "
}
```

## Encriptação AES-CTR em JAVA

{% code title="SaftTestCipher.java" %}
```java
package testeATCifras;

import org.w3c.dom.Document;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import java.io.File;
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
import java.util.Base64;
import java.util.List;

public class SaftTestCipher {

    private static String FILE_NAME = "Exemplo_Facturacao.xml";
    private static String FILE_CIPHER_NAME = "Exemplo_Facturacao_Cipher.xml";
    private static String FILE_DECIPHER_NAME = "Exemplo_Facturacao_Decipher.xml";
	
	//"<Key>8/K97v8vQqbD/ShX5yx+3g==</Key>"
    //"<IV>+KSjwLJcoMXl7W+U1y5VtQ==</IV>"		
	private static String BASE64_KEY = "8/K97v8vQqbD/ShX5yx+3g==";
	private static String BASE64_IV = "+KSjwLJcoMXl7W+U1y5VtQ==";

    private static String FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT_DESCRIPTION = "AccountDescription";
    private static String FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT = "Account";
    private static List<String> FIELDS_CUSTOMER_CLEAR = Arrays.asList("CustomerID", "AccountID", "SelfBillingIndicator");
    private static List<String> FIELDS_CUSTOMER_ADDRESS = Arrays.asList("BillingAddress", "ShipToAddress");
    private static List<String> FIELDS_SUPLIER_CLEAR = Arrays.asList("SuplierID", "AccountID", "SelfBillingIndicator");
    private static List<String> FIELDS_SUPLIER_ADDRESS = Arrays.asList("BillingAddress", "ShipFromAddress");
    private static String FIELDS_TAX_TABLE_ENTRY = "TaxTableEntry";
    private static String FIELDS_TAX_TABLE_DESCRIPTION = "Descritpion";
    private static String FIELDS_GENERAL_LEDGER_ENTRIES_DESCRIPTION = "Description";
    private static String FIELDS_GENERAL_LEDGER_ENTRIES_JOURNAL = "Journal";
    private static String FIELDS_GENERAL_LEDGER_ENTRIES_TRANSACTION = "Transaction";
    private static String FIELDS_GENERAL_LEDGER_ENTRIES_LINES = "Lines";
    private static List<String> FIELDS_GENERAL_LEDGER_ENTRIES_LINES_LINE = Arrays.asList("CreditLine","DebitLine");
    private static List<String> FIELDS_GENERAL_LEDGER_ENTRIES_CIPHER = Arrays.asList("Description", "SourceID");
    private static String FIELDS_PAYMENTS_PAYMENT = "Payment";
    private static String FIELDS_PAYMENTS_SOURCEID = "SourceID";
    private static String FIELDS_PAYMENTS_DESCRIPTION = "Description";
    private static String FIELDS_PAYMENTS_DOCUMENT_STATUS = "DocumentStatus";
    private static String FIELDS_PAYMENTS_LINE = "Line";
    private static String FIELDS_PAYMENTS_SOURCE_DOCUMENT_ID = "SourceDocumentID";
    private static List<String> FIELDS_PAYMENTS_CIPHER = Arrays.asList("Description","SourceID");


    //Struct
    //"GeneralLedgerAccounts"
    // - "AccountDescription"
    //"Customer" todos os campos existentes nesta tabela, exceto os seguintes:
    // - CustomerID
    // - AccountID
    // - SelfBillingIndicator
    //
    //"Supplier", todos os campos existentes nesta tabela, exceto os seguintes:
    // - SupplierID
    // - AccountID
    // - SelfBillingIndicator
    //
    //"TaxTable"
    // i) 2.5.1.4. - Descrição do imposto (Description)
    //
    //"GeneralLedgerEntries"
    //i) 3.4.2. - Descrição do diário (Description);
    //
    //ii) 3.4.3.4. - Código do utilizador que registou o movimento (SourceID);
    //
    //iii) 3.4.3.5. - Descrição do movimento (Description);
    //
    //iv) 3.4.3.11.1.5. - Descrição da linha de documento (Description);
    //
    //v) 3.4.3.11.2.5. - Descrição da linha de documento (Description);
    //
    //f) Na tabela 4.4. - Documentos de recibos emitidos (Payments), quando deva existir, os seguintes campos existentes nesta tabela:
    //
    //i) 4.4.4.7. Descrição do pagamento (Description) - campo não obrigatório na estrutura;
    //
    //ii) 4.4.4.9.4. - Código do utilizador (SourceID);
    //
    //iii) 4.4.4.11. - Código do utilizador (SourceID);
    //
    //iv) 4.4.4.14.2.3. - Descrição da linha (Description) - campo não obrigatório na estrutura.
    //
    //113447153

    public static void main(String[] args) throws Exception {
        //Read File
        //Find where to cipher
        //cipher
        //write to new file

        try {
            System.out.println("Reading Key");
          
            //AES_CTR::
            byte[] keyBytes = Base64.getDecoder().decode(BASE64_KEY);
            SecretKey key = new SecretKeySpec(keyBytes, "AES");
            Cipher cipher = Cipher.getInstance("AES/CTR/NoPadding");
            byte[] iv = Base64.getDecoder().decode(BASE64_IV);
            IvParameterSpec paramSpec = new IvParameterSpec(iv);
            SecretKeySpec keySpec = new SecretKeySpec(key.getEncoded(), "AES");
            cipher.init(Cipher.ENCRYPT_MODE, keySpec, paramSpec);
            System.out.println("Success");
            //Read Document::
            File inputFile = new File(FILE_NAME);
            DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
            Document doc = dBuilder.parse(inputFile);
            doc.getDocumentElement().normalize();
            System.out.println("----------------------------");
            //Initialize cipher:



            //Process File and Cipher Stuff in here:
            //GeneralLedgerAccounts rules::

            NodeList generalLedgerAccountNodeList = doc.getElementsByTagName("GeneralLedgerAccount");
            for (int i = 0; i < generalLedgerAccountNodeList.getLength(); i++) {
                Node generalLedgerAccountNode = generalLedgerAccountNodeList.item(i);
                NodeList generalLedgerAccountNodeChildNodes = generalLedgerAccountNode.getChildNodes();
                for (int j = 0; j < generalLedgerAccountNodeChildNodes.getLength(); j++) {
                    Node generalLedgerAccountChildNode = generalLedgerAccountNodeChildNodes.item(j);
                    if (FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT.equals(generalLedgerAccountChildNode.getNodeName())) {
                        for (int k = 0; k < generalLedgerAccountNodeChildNodes.getLength(); k++) {
                            Node generalLedgerAccountsAccountNode = generalLedgerAccountNodeChildNodes.item(k);
                            if (FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT_DESCRIPTION.equals(generalLedgerAccountsAccountNode.getNodeName())
                                    && generalLedgerAccountsAccountNode.getTextContent() != null && generalLedgerAccountsAccountNode.getNodeType() == 1) {
                                String cipherTextString = cipher(cipher, generalLedgerAccountsAccountNode.getTextContent());
                                generalLedgerAccountsAccountNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }


            //Customer rules::
            NodeList customerList = doc.getElementsByTagName("Customer");
            for (int i = 0; i < customerList.getLength(); i++) {
                Node customerListNode = customerList.item(i);
                NodeList childCustomerList = customerListNode.getChildNodes();
                for (int j = 0; j < childCustomerList.getLength(); j++) {
                    Node childCustomer = childCustomerList.item(j);
                    if (!FIELDS_CUSTOMER_ADDRESS.contains(childCustomer.getNodeName())) {
                        if (childCustomer.getTextContent() != null && childCustomer.getNodeType() == 1 && !FIELDS_CUSTOMER_CLEAR.contains(childCustomer.getNodeName())) {
                            String cipherTextString = cipher(cipher, childCustomer.getTextContent());
                            childCustomer.setTextContent(cipherTextString);
                        }
                    } else {
                        NodeList childChildCustomerList = childCustomer.getChildNodes();
                        for (int k = 0; k < childChildCustomerList.getLength(); k++) {
                            Node childChildCustomer = childChildCustomerList.item(k);
                            if (childChildCustomer.getTextContent() != null && childChildCustomer.getNodeType() == 1) {
                                String cipherTextString = cipher(cipher, childChildCustomer.getTextContent());
                                childChildCustomer.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            //Supplier rules:
            NodeList supplierList = doc.getElementsByTagName("Supplier");
            for (int i = 0; i < supplierList.getLength(); i++) {
                Node supplierListNode = supplierList.item(i);
                NodeList childNodes = supplierListNode.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (!FIELDS_SUPLIER_ADDRESS.contains(childNode.getNodeName())) {
                        if (childNode.getTextContent() != null && childNode.getNodeType() == 1 && !FIELDS_SUPLIER_CLEAR.contains(childNode.getNodeName())) {
                            String cipherTextString = cipher(cipher, childNode.getTextContent());
                            childNode.setTextContent(cipherTextString);
                        }
                    } else {
                        NodeList childChildNodes = childNode.getChildNodes();
                        for (int k = 0; k < childChildNodes.getLength(); k++) {
                            Node childChildNode = childChildNodes.item(k);
                            if (childChildNode.getTextContent() != null && childChildNode.getNodeType() == 1) {
                                String cipherTextString = cipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            //TaxTable rules:
            NodeList taxTableNodeList = doc.getElementsByTagName("TaxTable");
            for (int i = 0; i < taxTableNodeList.getLength(); i++) {
                Node node = taxTableNodeList.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (FIELDS_TAX_TABLE_ENTRY.equals(childNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if (FIELDS_TAX_TABLE_DESCRIPTION.equals(childChildNode.getNodeName())
                                    && childChildNode.getTextContent() != null && childChildNode.getNodeType() == 1) {
                                String cipherTextString = cipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            //"GeneralLedgerEntries rules:
            NodeList generalLedgerEntriesNodeList = doc.getElementsByTagName("GeneralLedgerEntries");
            for (int i = 0; i < generalLedgerEntriesNodeList.getLength(); i++) {
                Node node = generalLedgerEntriesNodeList.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node generalLedgerAccountChildNode = childNodes.item(j);
                    if (FIELDS_GENERAL_LEDGER_ENTRIES_JOURNAL.equals(generalLedgerAccountChildNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if(FIELDS_GENERAL_LEDGER_ENTRIES_DESCRIPTION.equals(childChildNode.getNodeName()) && childChildNode.getTextContent()!=null
                            && childChildNode.getNodeType()==1){
                                String cipherTextString = cipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                            if (FIELDS_GENERAL_LEDGER_ENTRIES_TRANSACTION.equals(childChildNode)) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if (FIELDS_GENERAL_LEDGER_ENTRIES_CIPHER.contains(level3Node.getNodeName()) && level3Node.getTextContent() != null
                                            && level3Node.getNodeType() == 1) {
                                        String cipherTextString = cipher(cipher, level3Node.getTextContent());
                                        level3Node.setTextContent(cipherTextString);
                                    }
                                    if(FIELDS_GENERAL_LEDGER_ENTRIES_LINES.equals(level3Node.getNodeName())){
                                        NodeList level4NodeList = level3Node.getChildNodes();
                                        for(int m = 0; m <level4NodeList.getLength();m++){
                                            Node level4Node = level4NodeList.item(m);
                                            if(FIELDS_GENERAL_LEDGER_ENTRIES_LINES_LINE.contains(level4Node.getNodeName())){
                                                NodeList level5NodeList = level4Node.getChildNodes();
                                                for(int n=0; n< level5NodeList.getLength(); n++){
                                                    Node level5Node = level5NodeList.item(n);
                                                    if(FIELDS_GENERAL_LEDGER_ENTRIES_DESCRIPTION.equals(level5Node.getNodeName())
                                                    && level5Node.getTextContent()!=null && level5Node.getNodeType()==1){
                                                        String cipherTextString = cipher(cipher, level5Node.getTextContent());
                                                        level5Node.setTextContent(cipherTextString);
                                                    }
                                                }
                                            }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }

            //Payment Rules::
            NodeList paymentsNodeList = doc.getElementsByTagName("Payments");
            for (int i = 0; i < paymentsNodeList.getLength(); i++) {
                Node node = paymentsNodeList.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (FIELDS_PAYMENTS_PAYMENT.equals(childNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if(FIELDS_PAYMENTS_CIPHER.equals(childChildNode.getNodeName()) && childChildNode.getTextContent()!=null
                                    && childChildNode.getNodeType()==1){
                                String cipherTextString = cipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                            if (FIELDS_PAYMENTS_DOCUMENT_STATUS.equals(childChildNode.getNodeName())) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if (FIELDS_PAYMENTS_SOURCEID.equals(level3Node.getNodeName()) && level3Node.getTextContent() != null
                                            && level3Node.getNodeType() == 1) {
                                        String cipherTextString = cipher(cipher, level3Node.getTextContent());
                                        level3Node.setTextContent(cipherTextString);
                                    }
                                }
                            }
                            if (FIELDS_PAYMENTS_LINE.equals(childChildNode.getNodeName())) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if(FIELDS_PAYMENTS_SOURCE_DOCUMENT_ID.equals(level3Node.getNodeName())){
                                        NodeList level4NodeList = level3Node.getChildNodes();
                                        for(int m=0; m< level4NodeList.getLength(); m++){
                                            Node level4Node = level4NodeList.item(m);
                                            if(FIELDS_PAYMENTS_DESCRIPTION.equals(level4Node.getNodeName()) && level4Node.getTextContent()!=null
                                            && level4Node.getNodeType()==1){
                                                String cipherTextString = cipher(cipher, level4Node.getTextContent());
                                                level4Node.setTextContent(cipherTextString);
                                            }
                                        }
                                    }
                                }

                            }
                        }
                    }
                }
            }

            //Write to new Document::
            System.out.println("Writing encrypted file");
            TransformerFactory transformerFactory = TransformerFactory.newInstance();
            Transformer transformer = transformerFactory.newTransformer();
            DOMSource source = new DOMSource(doc);
            StreamResult result = new StreamResult(new File(FILE_CIPHER_NAME));
            transformer.transform(source, result);

            //Decipher and write to document
            System.out.println("Decrypting encrypted file");

            //Initialize cipher in decrypt mode
            // Initialize Cipher in DECRYPT_MODE
            cipher = Cipher.getInstance("AES/CTR/NoPadding");
            paramSpec = new IvParameterSpec(iv);
            keySpec = new SecretKeySpec(key.getEncoded(), "AES");
            cipher.init(Cipher.DECRYPT_MODE, keySpec, paramSpec);


            //Process and decipher document

            NodeList generalLedgerAccountNodeListDecipher = doc.getElementsByTagName("GeneralLedgerAccount");
            for (int i = 0; i < generalLedgerAccountNodeListDecipher.getLength(); i++) {
                Node generalLedgerAccountNode = generalLedgerAccountNodeListDecipher.item(i);
                NodeList generalLedgerAccountNodeChildNodes = generalLedgerAccountNode.getChildNodes();
                for (int j = 0; j < generalLedgerAccountNodeChildNodes.getLength(); j++) {
                    Node generalLedgerAccountChildNode = generalLedgerAccountNodeChildNodes.item(j);
                    if (FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT.equals(generalLedgerAccountChildNode.getNodeName())) {
                        for (int k = 0; k < generalLedgerAccountNodeChildNodes.getLength(); k++) {
                            Node generalLedgerAccountsAccountNode = generalLedgerAccountNodeChildNodes.item(k);
                            if (FIELDS_GENERAL_LEDGER_ACCOUNTS_ACCOUNT_DESCRIPTION.equals(generalLedgerAccountsAccountNode.getNodeName())
                                    && generalLedgerAccountsAccountNode.getTextContent() != null && generalLedgerAccountsAccountNode.getNodeType() == 1) {
                                String cipherTextString = decipher(cipher, generalLedgerAccountsAccountNode.getTextContent());
                                generalLedgerAccountsAccountNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            NodeList customerListDecipher = doc.getElementsByTagName("Customer");
            for (int i = 0; i < customerListDecipher.getLength(); i++) {
                Node customerDecipher = customerListDecipher.item(i);
                NodeList customerDecipherChildNodes = customerDecipher.getChildNodes();
                for (int j = 0; j < customerDecipherChildNodes.getLength(); j++) {
                    Node customerDecipherChildNode = customerDecipherChildNodes.item(j);
                    if (!FIELDS_CUSTOMER_ADDRESS.contains(customerDecipherChildNode.getNodeName())) {
                        if (customerDecipherChildNode.getTextContent() != null && customerDecipherChildNode.getNodeType() == 1 && !FIELDS_CUSTOMER_CLEAR.contains(customerDecipherChildNode.getNodeName())) {
                            String decipherTextString = decipher(cipher, customerDecipherChildNode.getTextContent());
                            customerDecipherChildNode.setTextContent(decipherTextString);
                        }
                    } else {
                        NodeList customerDecipherChildNodeChildNodes = customerDecipherChildNode.getChildNodes();
                        for (int k = 0; k < customerDecipherChildNodeChildNodes.getLength(); k++) {
                            Node customerDecipherChildNodeChildNode = customerDecipherChildNodeChildNodes.item(k);
                            if (customerDecipherChildNodeChildNode.getTextContent() != null && customerDecipherChildNodeChildNode.getNodeType() == 1) {
                                String decipherTextString = decipher(cipher, customerDecipherChildNodeChildNode.getTextContent());
                                customerDecipherChildNodeChildNode.setTextContent(decipherTextString);
                            }
                        }
                    }
                }
            }

            NodeList supplierListDecipher = doc.getElementsByTagName("Supplier");
            for (int i = 0; i < supplierListDecipher.getLength(); i++) {
                Node supplierListNode = supplierListDecipher.item(i);
                NodeList childNodes = supplierListNode.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (!FIELDS_SUPLIER_ADDRESS.contains(childNode.getNodeName())) {
                        if (childNode.getTextContent() != null && childNode.getNodeType() == 1 && !FIELDS_SUPLIER_CLEAR.contains(childNode.getNodeName())) {
                            String cipherTextString = decipher(cipher, childNode.getTextContent());
                            childNode.setTextContent(cipherTextString);
                        }
                    } else {
                        NodeList childChildNodes = childNode.getChildNodes();
                        for (int k = 0; k < childChildNodes.getLength(); k++) {
                            Node childChildNode = childChildNodes.item(k);
                            if (childChildNode.getTextContent() != null && childChildNode.getNodeType() == 1) {
                                String cipherTextString = decipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            NodeList taxTableNodeListDecipher = doc.getElementsByTagName("TaxTable");
            for (int i = 0; i < taxTableNodeListDecipher.getLength(); i++) {
                Node node = taxTableNodeListDecipher.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (FIELDS_TAX_TABLE_ENTRY.equals(childNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if (FIELDS_TAX_TABLE_DESCRIPTION.equals(childChildNode.getNodeName())
                                    && childChildNode.getTextContent() != null && childChildNode.getNodeType() == 1) {
                                String cipherTextString = decipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                        }
                    }
                }
            }

            //"GeneralLedgerEntries rules:
            NodeList generalLedgerEntriesNodeListDecipher = doc.getElementsByTagName("GeneralLedgerEntries");
            for (int i = 0; i < generalLedgerEntriesNodeListDecipher.getLength(); i++) {
                Node node = generalLedgerEntriesNodeListDecipher.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node generalLedgerAccountChildNode = childNodes.item(j);
                    if (FIELDS_GENERAL_LEDGER_ENTRIES_JOURNAL.equals(generalLedgerAccountChildNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if(FIELDS_GENERAL_LEDGER_ENTRIES_DESCRIPTION.equals(childChildNode.getNodeName()) && childChildNode.getTextContent()!=null
                                    && childChildNode.getNodeType()==1){
                                String cipherTextString = decipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                            if (FIELDS_GENERAL_LEDGER_ENTRIES_TRANSACTION.equals(childChildNode)) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if (FIELDS_GENERAL_LEDGER_ENTRIES_CIPHER.contains(level3Node.getNodeName()) && level3Node.getTextContent() != null
                                            && level3Node.getNodeType() == 1) {
                                        String cipherTextString = decipher(cipher, level3Node.getTextContent());
                                        level3Node.setTextContent(cipherTextString);
                                    }
                                    if(FIELDS_GENERAL_LEDGER_ENTRIES_LINES.equals(level3Node.getNodeName())){
                                        NodeList level4NodeList = level3Node.getChildNodes();
                                        for(int m = 0; m <level4NodeList.getLength();m++){
                                            Node level4Node = level4NodeList.item(m);
                                            if(FIELDS_GENERAL_LEDGER_ENTRIES_LINES_LINE.contains(level4Node.getNodeName())){
                                                NodeList level5NodeList = level4Node.getChildNodes();
                                                for(int n=0; n< level5NodeList.getLength(); n++){
                                                    Node level5Node = level5NodeList.item(n);
                                                    if(FIELDS_GENERAL_LEDGER_ENTRIES_DESCRIPTION.equals(level5Node.getNodeName())
                                                            && level5Node.getTextContent()!=null && level5Node.getNodeType()==1){
                                                        String cipherTextString = decipher(cipher, level5Node.getTextContent());
                                                        level5Node.setTextContent(cipherTextString);
                                                    }
                                                }
                                            }
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }

            //Payment Rules::
            NodeList paymentsNodeListDecipher = doc.getElementsByTagName("Payments");
            for (int i = 0; i < paymentsNodeListDecipher.getLength(); i++) {
                Node node = paymentsNodeListDecipher.item(i);
                NodeList childNodes = node.getChildNodes();
                for (int j = 0; j < childNodes.getLength(); j++) {
                    Node childNode = childNodes.item(j);
                    if (FIELDS_PAYMENTS_PAYMENT.equals(childNode.getNodeName())) {
                        for (int k = 0; k < childNodes.getLength(); k++) {
                            Node childChildNode = childNodes.item(k);
                            if(FIELDS_PAYMENTS_CIPHER.equals(childChildNode.getNodeName()) && childChildNode.getTextContent()!=null
                                    && childChildNode.getNodeType()==1){
                                String cipherTextString = decipher(cipher, childChildNode.getTextContent());
                                childChildNode.setTextContent(cipherTextString);
                            }
                            if (FIELDS_PAYMENTS_DOCUMENT_STATUS.equals(childChildNode.getNodeName())) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if (FIELDS_PAYMENTS_SOURCEID.equals(level3Node.getNodeName()) && level3Node.getTextContent() != null
                                            && level3Node.getNodeType() == 1) {
                                        String cipherTextString = decipher(cipher, level3Node.getTextContent());
                                        level3Node.setTextContent(cipherTextString);
                                    }
                                }
                            }
                            if (FIELDS_PAYMENTS_LINE.equals(childChildNode.getNodeName())) {
                                NodeList level3NodeList = childChildNode.getChildNodes();
                                for (int l = 0; l < level3NodeList.getLength(); l++) {
                                    Node level3Node = level3NodeList.item(l);
                                    if(FIELDS_PAYMENTS_SOURCE_DOCUMENT_ID.equals(level3Node.getNodeName())){
                                        NodeList level4NodeList = level3Node.getChildNodes();
                                        for(int m=0; m< level4NodeList.getLength(); m++){
                                            Node level4Node = level4NodeList.item(m);
                                            if(FIELDS_PAYMENTS_DESCRIPTION.equals(level4Node.getNodeName()) && level4Node.getTextContent()!=null
                                                    && level4Node.getNodeType()==1){
                                                String cipherTextString = decipher(cipher, level4Node.getTextContent());
                                                level4Node.setTextContent(cipherTextString);
                                            }
                                        }
                                    }
                                }

                            }
                        }
                    }
                }
            }

            //Write to new Document::
            System.out.println("Writing decipher");
            TransformerFactory transformerFactoryDecipher = TransformerFactory.newInstance();
            Transformer transformerDecipher = transformerFactory.newTransformer();
            DOMSource sourceDecipher = new DOMSource(doc);
            StreamResult resultDecipher = new StreamResult(new File(FILE_DECIPHER_NAME));
            transformerDecipher.transform(sourceDecipher, resultDecipher);

            System.out.println("That's All folks!!!");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static String cipher(Cipher cipher, String text) throws UnsupportedEncodingException {
        byte[] cipherTextBytes = cipher.update(text.getBytes("CP1252"));
        String cipherTextString = Base64.getEncoder().encodeToString(cipherTextBytes);
        return cipherTextString;
    }

    private static String decipher(Cipher cipher, String text) throws UnsupportedEncodingException {
        byte[] cipherTextBytes = Base64.getDecoder().decode(text);
        byte[] decipheredBytes = cipher.update(cipherTextBytes);
        String decipherTextString = new String(decipheredBytes, "CP1252");
        return decipherTextString;
    }
}

```
{% endcode %}



