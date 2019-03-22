---
title: Client-Side-versleuteling voor Microsoft Azure Storage met behulp van Java | Microsoft Docs
description: De Azure Storage-clientbibliotheek voor Java biedt ondersteuning voor versleuteling op de client en de integratie met Azure Key Vault voor een maximale beveiliging voor uw Azure Storage-toepassingen.
services: storage
author: lakasa
ms.service: storage
ms.devlang: java
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.subservice: common
ms.openlocfilehash: 0a2088e603828a7850cb250c1874008d63fe9c89
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57992457"
---
# <a name="client-side-encryption-and-azure-key-vault-with-java-for-microsoft-azure-storage"></a>Client-Side-versleuteling en Azure Key Vault met behulp van Java voor Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht
De [Azure Storage-clientbibliotheek voor Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat u uploadt naar Azure Storage en de ontsleuteling van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor sleutelbeheer voor storage-account.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleuteling en ontsleuteling via de techniek envelop
De techniek envelop volgt u de processen van versleuteling en ontsleuteling.  

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de techniek envelop
Versleuteling via de techniek envelop werkt in de volgende manier:  

1. De Azure storage-clientbibliotheek genereert een sleutel met versleuteling van inhoud (CEK), dit een symmetrische sleutel van één permanente gebruiken is.  
2. Gebruikersgegevens worden versleuteld met behulp van deze CEK.   
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de sleutel van versleutelingssleutel (KEK-sleutel). De KEK kan wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden lokaal beheerd of die zijn opgeslagen in de Sleutelkluizen van Azure.  
   De opslagclientbibliotheek zelf heeft nooit toegang tot de KEK-sleutel. De bibliotheek roept de belangrijkste wrapping-algoritme die wordt geleverd door Key Vault. Gebruikers kunnen kiezen om te gebruiken van aangepaste providers voor sleutel verpakken/uitpakken, indien gewenst.  
4. De versleutelde gegevens wordt vervolgens naar de Azure Storage-service geüpload. De ingepakte sleutel samen met enkele aanvullende metagegevens is opgeslagen als metagegevens (voor een blob) of geïnterpoleerde met de versleutelde gegevens (berichten in wachtrij plaatsen en tabelitems).

### <a name="decryption-via-the-envelope-technique"></a>Ontsleuteling via de techniek envelop
Ontsleuteling via de techniek envelop werkt in de volgende manier:  

1. De clientbibliotheek wordt ervan uitgegaan dat de gebruiker wordt beheerd met de sleutel van versleutelingssleutel (KEK) lokaal of in de Sleutelkluizen van Azure. De gebruiker hoeft niet te weten de specifieke sleutel die is gebruikt voor versleuteling. In plaats daarvan worden een sleutel resolver die verschillende sleutel-id's worden omgezet in sleutels ingesteld en gebruikt.  
2. De clientbibliotheek downloadt de versleutelde gegevens, samen met versleuteling materiaal dat is opgeslagen op de service.  
3. De verpakte inhoud versleutelingssleutel (CEK) is niet-ingepakte (ontsleutelde) met behulp van de key-versleuteling (KEK)-sleutel. Hier weer heeft de clientbibliotheek geen toegang tot de KEK-sleutel. Het gewoon roept de aangepaste of uitpakken van de provider van de Key Vault-algoritme.  
4. De sleutel voor versleuteling van inhoud (CEK) wordt vervolgens gebruikt voor het ontsleutelen van de versleutelde gegevens.

## <a name="encryption-mechanism"></a>-Versleutelingsmechanisme
Maakt gebruik van de storage-clientbibliotheek [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om gebruikersgegevens te versleutelen. Met name [Cipher Block Chaining (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elke service enigszins anders werkt, zodat we elk van deze hier wordt besproken.

### <a name="blobs"></a>Blobs
De clientbibliotheek biedt momenteel ondersteuning voor versleuteling van hele BLOB's. Specifiek, versleuteling wordt ondersteund wanneer gebruikers de **uploaden*** methoden of de **openOutputStream** methode. Voor downloads, zowel volledige als bereik downloads worden ondersteund.  

Tijdens het versleutelen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) van 32 bytes en envelop versleuteling van de blob-gegevens met behulp van deze gegevens uitvoeren. De verpakte CEK en enkele aanvullende metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met de versleutelde blob voor de service.

> [!WARNING]
> Als u bewerken of uw eigen metagegevens voor de blob uploaden, moet u om ervoor te zorgen dat deze metagegevens behouden blijft. Als u nieuwe metagegevens zonder deze metagegevens uploaden, de verpakte CEK, IV en andere metagegevens, gaan verloren en de blobinhoud wordt nooit meer worden opgehaald.
> 
> 

Bij het ophalen van de inhoud van het gebruik van de gehele blob downloaden van een versleutelde blob omvat de **downloaden**/**openInputStream** methoden voor gebruiksgemak. De verpakte CEK is uitgepakt en gebruikt in combinatie met de IV (opgeslagen als blobmetagegevens in dit geval) de ontsleutelde om gegevens te retourneren aan de gebruikers.

Downloaden van een willekeurige adresbereik (**downloadRange** methoden) in de versleutelde blob omvat het aanpassen van het bereik dat is opgegeven door gebruikers om op te halen van een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt voor het ontsleutelen van is de aangevraagde het bereik.  

Alle blob-typen (blok-blobs, pagina-blobs en toevoeg-blobs) kan worden versleuteld/ontsleuteld met behulp van dit schema.

### <a name="queues"></a>Wachtrijen
Omdat berichten in wachtrij plaatsen van elke indeling zijn kunnen, definieert de clientbibliotheek van een aangepaste indeling die de initialisatie van Vector (IV) en de versleutelde inhoud versleutelingssleutel (CEK) in de berichttekst bevat.  

Tijdens het versleutelen de clientbibliotheek genereert een willekeurige IV van 16 bytes, samen met een willekeurige CEK 32 bytes en envelop versleuteling van de tekst van het wachtrij-bericht met deze informatie wordt uitgevoerd. De verpakte CEK en enkele aanvullende metagegevens vervolgens worden toegevoegd aan de gecodeerde wachtrijbericht. Deze gewijzigde bericht (Zie hieronder) wordt opgeslagen op de service.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Tijdens het ontsleutelen, is de ingepakte sleutel opgehaald uit het wachtrijbericht en uitgepakt. De IV is ook geëxtraheerd uit het wachtrijbericht en gebruikt, samen met de niet-ingepakte sleutel om de wachtrij berichtgegevens te ontsleutelen. Houd er rekening mee dat de metagegevens van de versleuteling klein (onder 500 bytes), is dus terwijl deze mee voor de limiet van 64KB voor een wachtrijbericht tellen, de impact beheerd worden moet.

### <a name="tables"></a>Tabellen
De clientbibliotheek biedt ondersteuning voor versleuteling van entiteitseigenschappen voor invoegen en vervangbewerkingen.

> [!NOTE]
> Samenvoegen wordt momenteel niet ondersteund. Omdat een subset met eigenschappen kan zijn gecodeerd eerder met behulp van een andere sleutel, leidt gewoon samenvoegen van de nieuwe eigenschappen en het bijwerken van de metagegevens tot verlies van gegevens. Samenvoegen van een vereist extra service aanroepen om te lezen de reeds bestaande entiteit van de service of met behulp van een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.
> 
> 

Tabel gegevensversleuteling werkt als volgt:  

1. Gebruikers opgeven de eigenschappen moeten worden versleuteld.  
2. De clientbibliotheek genereert een willekeurige initialisatie Vector (IV) van 16 bytes, samen met een willekeurige versleuteling van de inhoud-sleutel (CEK) van 32 bytes voor elke entiteit en envelop-versleuteling uitvoert op de afzonderlijke eigenschappen te laten versleutelen door een nieuwe IV per eigenschap die is afgeleid. De gecodeerde eigenschap wordt opgeslagen als binaire gegevens.  
3. De verpakte CEK en enkele aanvullende metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerde eigenschap (_ClientEncryptionMetadata1) is een string-eigenschap die de informatie over IV, versie en ingepakte sleutel bevat. De tweede gereserveerde eigenschap (_ClientEncryptionMetadata2) is een binaire eigenschap waarin de informatie over de eigenschappen die zijn versleuteld. De informatie in dit tweede eigenschap (_ClientEncryptionMetadata2) is versleuteld.  
4. Vanwege deze extra gereserveerde eigenschappen vereist zijn voor versleuteling, kunnen gebruikers nu alleen 250 aangepaste eigenschappen in plaats van 252 hebben. De totale grootte van de entiteit moet minder dan 1MB.  
   
   Houd er rekening mee dat alleen de tekenreekseigenschappen van de kunnen worden versleuteld. Als andere typen eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De gecodeerde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na ontsleuteling.
   
   Gebruikers moeten de eigenschappen moeten worden versleuteld opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een kenmerk [versleutelen] \(voor POCO-entiteiten die zijn afgeleid van TableEntity) of een omzetter versleuteling in de aanvraag-opties. Een oplossing versleuteling is een gemachtigde die ervoor zorgt dat een partitiesleutel, een rijsleutel en een naam van de eigenschap retourneert een Booleaanse waarde die aangeeft of deze eigenschap moet worden versleuteld. Tijdens het versleutelen, wordt de clientbibliotheek van deze informatie gebruiken om te bepalen of een eigenschap moet worden versleuteld tijdens het schrijven naar de kabel. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld, als X, vervolgens versleutelen eigenschap A; anders eigenschappen A en B. te versleutelen) Houd er rekening mee dat het is niet nodig om deze informatie tijdens het lezen of uitvoeren van query's entiteiten te geven.

### <a name="batch-operations"></a>Batchbewerkingen
In batchbewerkingen, wordt de dezelfde KEK-sleutel gebruikt in alle tabelrijen in die batchbewerking omdat de clientbibliotheek slechts staat één options-object (en daarom een beleid/KEK) per batchbewerking. Echter, de clientbibliotheek intern genereert een nieuwe willekeurige IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook kiezen voor het versleutelen van verschillende eigenschappen voor elke bewerking in de batch met het definiëren van dit gedrag in de conflictoplosser voor versleuteling.

### <a name="queries"></a>Query's
> [!NOTE]
> Omdat de entiteiten zijn versleuteld, kunt u query's die filteren niet uitvoeren op een gecodeerde eigenschap.  Als u probeert, is resultaten onjuist, omdat de service probeert zou om versleutelde gegevens met niet-versleutelde gegevens te vergelijken.
> 
> 
> Als u wilt querybewerkingen uitvoeren, moet u een sleutel-omzetter kan omzetten van de sleutels in de resultatenset. Als een entiteit die deel uitmaken van het queryresultaat kan niet worden omgezet naar een provider, genereert de clientbibliotheek een fout. Voor elke query die server side projecties uitvoert, wordt de clientbibliotheek van de eigenschappen van de metagegevens speciale versleuteling (_ClientEncryptionMetadata1 en _ClientEncryptionMetadata2) standaard toegevoegd aan de geselecteerde kolommen.

## <a name="azure-key-vault"></a>Azure Key Vault
Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Met behulp van Azure Key Vault, kunnen gebruikers versleutelen van sleutels en geheimen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels. PFX-bestanden en wachtwoorden) met behulp van sleutels die worden beveiligd door hardware security modules (HSM's). Zie voor meer informatie, [wat is Azure Key Vault?](../../key-vault/key-vault-whatis.md).

De storage-clientbibliotheek maakt gebruik van de Key Vault-kernbibliotheek om te voorzien van een gemeenschappelijk framework in Azure voor het beheren van sleutels. Gebruikers krijgen ook het bijkomende voordeel van het gebruik van de Key Vault-extensies-bibliotheek. De extensies-bibliotheek biedt functionaliteit om eenvoudig en naadloze Symmetric/RSA lokale en belangrijke cloudproviders en met aggregatie en caching.

### <a name="interface-and-dependencies"></a>-Interface en afhankelijkheden
Er zijn drie Key Vault-pakketten:  

* Basisversie van Azure Key Vault bevat de IKey en IKeyResolver. Er is een klein pakket zonder afhankelijkheden. De opslagclientbibliotheek voor Java gedefinieerd als een afhankelijkheid.
* Azure keyvault bevat de Key Vault REST-client.  
* extensies van Azure Key Vault bevat de extensiecode die bevat implementaties van cryptografische algoritmen en een RSAKey en een SymmetricKey. Het is afhankelijk van de naamruimten Core en Key Vault en voorziet in functionaliteit voor het definiëren van een statistische conflictoplosser (wanneer gebruikers meerdere sleutel-providers gebruiken) en een cachebewerkingen conflictoplosser voor de sleutel. Hoewel de storage-clientbibliotheek niet rechtstreeks afhankelijk van dit pakket is als gebruikers willen Azure Key Vault gebruiken voor het opslaan van hun sleutels of de Key Vault-extensies gebruiken om te gebruiken van de lokale en cryptografische providers in de cloud, moeten ze dit pakket.  
  
  Key Vault is ontworpen voor hoge waarde hoofdsleutels en bandbreedtebeperking limieten per Key Vault zijn ontworpen met dit in gedachten. Bij het uitvoeren van versleuteling aan clientzijde met Key Vault, is het beste model symmetrische hoofdsleutels opgeslagen als geheimen in Key Vault en in de cache opgeslagen lokaal gebruiken. Gebruikers moeten het volgende doen:  

1. Een geheim offline maken en uploaden naar de Key Vault.  
2. Gebruik van het geheim base-id als parameter kunt oplossen door de huidige versie van het geheim voor het versleutelen en deze informatie lokaal in de cache. CachingKeyResolver gebruiken voor het opslaan in cache; gebruikers wordt niet verwacht voor het implementeren van hun eigen logica caching.  
3. De cachebewerkingen conflictoplosser gebruiken als invoer tijdens het maken van het versleutelingsbeleid voor.
   Meer informatie over het gebruik van Key Vault vindt u in de codevoorbeelden voor versleuteling. <fix URL>  

## <a name="best-practices"></a>Aanbevolen procedures
Ondersteuning voor versleuteling is alleen beschikbaar in de storage-clientbibliotheek voor Java.

> [!IMPORTANT]
> Houd rekening met deze belangrijke punten bij het gebruik van versleuteling op de client:
> 
> * Bij het lezen van of schrijven naar een versleutelde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijven naar een versleutelde blob protocol bewerkingen zoals Put-blokken, blokkeringslijst plaatsen, pagina's schrijven, wissen pagina's of toevoegen blokkeren; voorkomen anders u mogelijk de versleutelde blob beschadigd en onleesbaar.
> * Een dergelijke beperking bestaat voor tabellen. Zorg ervoor dat versleutelde eigenschappen niet bijwerken zonder versleutelingsgerelateerde metagegevens worden bijgewerkt.  
> * Als u de metagegevens op de versleutelde blob hebt ingesteld, overschrijft u de versleutelingsgerelateerde metagegevens vereist voor ontsleuteling, omdat de instelling van metagegevens is niet-additieve. Dit geldt ook voor momentopnamen; Vermijd metagegevens op te geven tijdens het maken van een momentopname van een versleutelde blob. Als de metagegevens van moet zijn ingesteld, moet u aanroepen de **downloadAttributes** methode om de metagegevens van de huidige versleuteling ophalen, en te voorkomen dat gelijktijdige schrijfbewerkingen terwijl metagegevens wordt ingesteld.  
> * Schakel de **requireEncryption** vlag in de standaardopties voor de aanvraag voor gebruikers die alleen met de versleutelde gegevens werken moeten. Zie hieronder voor meer informatie.  
> 
> 

## <a name="client-api--interface"></a>Client-API / Interface
Tijdens het maken van een object EncryptionPolicy, kunnen gebruikers alleen een sleutel (IKey implementeren), opgeven. alleen een resolver (IKeyResolver implementeren), of beide. IKey is de standaard type sleutel dat wordt aangeduid met behulp van een sleutel-id en waarmee de logica voor het verpakken/uitpakken. IKeyResolver wordt gebruikt voor het oplossen van een sleutel tijdens het ontsleutelen. Het definieert een ResolveKey-methode een IKey een sleutel-id opgegeven retourneert. Deze biedt gebruikers de mogelijkheid om te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

* De sleutel wordt altijd gebruikt voor versleuteling, en het ontbreken van een sleutel zal leiden tot een fout.  
* Voor ontsleuteling:  
  
  * De belangrijkste resolver wordt aangeroepen als voor het ophalen van de sleutel opgegeven. Als de conflictoplosser is opgegeven, maar beschikt niet over een toewijzing voor de sleutel-id, wordt er een fout opgetreden.  
  * Als conflictoplosser niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id komt niet overeen met, wordt er een fout opgetreden.  
    
    De [versleuteling voorbeelden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>demonstreren een meer gedetailleerde end-to-end-scenario voor blobs, wachtrijen en tabellen, samen met Key Vault-integratie.

### <a name="requireencryption-mode"></a>RequireEncryption modus
Gebruikers kunnen eventueel inschakelen voor een modus waarin alle uploaden en downloaden moeten worden versleuteld. In deze modus mislukken pogingen tot gegevens zonder een versleutelingsbeleid uploaden of downloaden van gegevens die niet worden versleuteld op de service op de client. De **requireEncryption** vlag van de aanvraag options-object bepaalt dit gedrag. Als uw toepassing u alle objecten die zijn opgeslagen in Azure Storage versleutelt, dan kunt u instellen de **requireEncryption** eigenschap op de standaardopties voor de aanvraag voor het service-client-object.   

Gebruik bijvoorbeeld **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** voor versleuteling vereisen voor alle blob-bewerkingen die worden uitgevoerd via deze clientobject.

### <a name="blob-service-encryption"></a>BLOB-service-versleuteling
Maak een **BlobEncryptionPolicy** object en stel deze in de Aanvraagopties voor (per API of op het clientniveau van een met behulp van **DefaultRequestOptions**). Alle andere wordt afgehandeld door de clientbibliotheek intern.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

// Set the encryption policy on the request options.
BlobRequestOptions options = new BlobRequestOptions();
options.setEncryptionPolicy(policy);

// Upload the encrypted contents to the blob.
blob.upload(stream, size, null, options, null);

// Download and decrypt the encrypted contents from the blob.
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
blob.download(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Versleuteling van de wachtrij
Maak een **QueueEncryptionPolicy** object en stel deze in de Aanvraagopties voor (per API of op het clientniveau van een met behulp van **DefaultRequestOptions**). Alle andere wordt afgehandeld door de clientbibliotheek intern.

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

// Add message
QueueRequestOptions options = new QueueRequestOptions();
options.setEncryptionPolicy(policy);

queue.addMessage(message, 0, 0, options, null);

// Retrieve message
CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);
```

### <a name="table-service-encryption"></a>Versleuteling van de tabel
Naast het maken van een versleutelingsbeleid voor en leert u deze op verzoek-opties, u moet opgeven een **EncryptionResolver** in **TableRequestOptions**, of stel het kenmerk [versleutelen] op van de entiteit getter en setter.

### <a name="using-the-resolver"></a>Met behulp van de conflictoplosser

```java
// Create the IKey used for encryption.
RsaKey key = new RsaKey("private:key1" /* key identifier */);

// Create the encryption policy to be used for upload and download.
TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

TableRequestOptions options = new TableRequestOptions()
options.setEncryptionPolicy(policy);
options.setEncryptionResolver(new EncryptionResolver() {
    public boolean encryptionResolver(String pk, String rk, String key) {
        if (key == "foo")
        {
            return true;
        }
        return false;
    }
});

// Insert Entity
currentTable.execute(TableOperation.insert(ent), options, null);

// Retrieve Entity
// No need to specify an encryption resolver for retrieve
TableRequestOptions retrieveOptions = new TableRequestOptions()
retrieveOptions.setEncryptionPolicy(policy);

TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
TableResult result = currentTable.execute(operation, retrieveOptions, null);
```

### <a name="using-attributes"></a>Met behulp van kenmerken
Zoals hierboven vermeld als de entiteit TableEntity implementeert, klikt u vervolgens de eigenschappen-getter en setter kunnen worden voorzien van het kenmerk [versleutelen] in plaats van op te geven de **EncryptionResolver**.

```java
private string encryptedProperty1;

@Encrypt
public String getEncryptedProperty1 () {
    return this.encryptedProperty1;
}

@Encrypt
public void setEncryptedProperty1(final String encryptedProperty1) {
    this.encryptedProperty1 = encryptedProperty1;
}
```

## <a name="encryption-and-performance"></a>Versleuteling en prestaties
Houd er rekening mee dat uw opslag, resulteert in extra prestatieoverhead versleutelt. De inhoudssleutel en IV moet worden gegenereerd, de inhoud zelf moet worden versleuteld en aanvullende metagegevens moeten worden geformatteerd en geüpload. Deze overhead varieert, afhankelijk van de hoeveelheid gegevens wordt versleuteld. Het is raadzaam dat klanten altijd hun toepassingen voor de prestaties tijdens de ontwikkeling testen.

## <a name="next-steps"></a>Volgende stappen
* Download de [Azure Storage-clientbibliotheek voor Java-Maven-pakket](https://mvnrepository.com/artifact/com.microsoft.azure/azure-storage)  
* Download de [Azure Storage Client Library voor Java-broncode van GitHub](https://github.com/Azure/azure-storage-java)   
* Download de Azure Key Vault Maven-bibliotheek voor Java-Maven-pakketten:
  * [Core](https://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core) pakket
  * [Client](https://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) pakket
* Ga naar de [documentatie voor Azure Key Vault](../../key-vault/key-vault-whatis.md)
