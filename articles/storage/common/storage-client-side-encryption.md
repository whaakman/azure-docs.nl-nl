---
title: Client-Side-versleuteling voor Microsoft Azure Storage met .NET | Microsoft Docs
description: De Azure Storage-clientbibliotheek voor .NET ondersteunt clientzijde versleuteling en integratie met Azure Key Vault voor een maximale beveiliging voor uw Azure Storage-toepassingen.
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: becfccca-510a-479e-a798-2044becd9a64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: tamram
ms.openlocfilehash: fe8023729bd1294dedd2a4e4723a8be0976731d6
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2017
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Client-Side-versleuteling en Azure Sleutelkluis voor Microsoft Azure Storage
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht
De [Azure Storage-clientbibliotheek voor .NET-Nuget-pakket](https://www.nuget.org/packages/WindowsAzure.Storage) ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor sleutelbeheer storage-account.

Zie voor een stapsgewijze zelfstudie waarin u door het proces leidt van het versleutelen van blobs met client-side '-versleuteling en Azure Sleutelkluis, [blobs versleutelen en ontsleutelen in Microsoft Azure Storage met Azure Key Vault](../blobs/storage-encrypt-decrypt-blobs-key-vault.md).

Zie voor de versleuteling van de clientzijde met Java, [versleuteling met Java voor Microsoft Azure Storage Client-Side](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleuteling en ontsleuteling via de envelop techniek
De processen van coderen en decoderen volgt u de envelop techniek.

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de envelop techniek
Versleuteling via de envelop techniek werkt in de volgende manier:

1. De Azure storage-clientbibliotheek genereert een inhoud versleutelingssleutel (CEK), die een symmetrische sleutel met een eenmalig gebruik is.
2. Gebruikersgegevens worden versleuteld met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de belangrijkste versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel en kan worden beheerd lokaal of opgeslagen in Azure sleutel kluizen.
   
    De opslagclientbibliotheek zelf heeft nooit toegang tot de KEK-sleutel. De bibliotheek roept de belangrijkste wrapping algoritme die wordt geleverd door de Sleutelkluis. Gebruikers kunnen kiezen met aangepaste providers voor sleutel wrapping/uitpakken indien gewenst.

4. De versleutelde gegevens is vervolgens naar de Azure Storage-service geüpload. De ingepakte sleutel samen met enkele aanvullende versleutelingsmetagegevens die is opgeslagen als metagegevens (op een blob) of geïnterpoleerd met de versleutelde gegevens (Wachtrijberichten en tabelentiteiten).

### <a name="decryption-via-the-envelope-technique"></a>Ontsleuteling via de envelop techniek
Ontsleuteling via de envelop techniek werkt in de volgende manier:

1. De clientbibliotheek wordt ervan uitgegaan dat de gebruiker wordt beheerd door de sleutel versleutelingssleutel (KEK-sleutel) lokaal of in kluizen van Azure-sleutel. De gebruiker hoeft niet te weten de specifieke sleutel die is gebruikt voor versleuteling. In plaats daarvan worden een sleutel resolver die verschillende sleutel-id's worden omgezet in sleutels ingesteld en gebruikt.
2. De clientbibliotheek downloadt de versleutelde gegevens samen met versleuteling materiaal dat is opgeslagen op de service.
3. De verpakte inhoud versleutelingssleutel (CEK) is en niet-ingepakte (ontsleutelde) met behulp van de belangrijkste versleuteling sleutel (KEK-sleutel). Hier opnieuw heeft de clientbibliotheek geen toegang tot de KEK-sleutel. Gewoon Hiermee wordt de aangepaste of uitpakken van de provider van de Sleutelkluis-algoritme.
4. De inhoud versleutelingssleutel (CEK) wordt vervolgens gebruikt voor het ontsleutelen van de versleutelde gegevens.

## <a name="encryption-mechanism"></a>Versleuteling mechanisme
Maakt gebruik van de storage-clientbibliotheek [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om versleuteling van gebruikersgegevens. In het bijzonder [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elk service enigszins anders werkt zodat elk van deze hier worden besproken.

### <a name="blobs"></a>Blobs
De clientbibliotheek biedt momenteel ondersteuning voor versleuteling van de hele BLOB's. In het bijzonder versleuteling wordt ondersteund wanneer gebruikers de **UploadFrom*** methoden of de **OpenWrite** methode. Voor downloads, zowel volledige als bereik downloads worden ondersteund.

Tijdens het versleutelen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes en envelop versleuteling van het gebruik van deze informatie blobgegevens uitvoeren. De verpakte CEK en een aantal aanvullende metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met de versleutelde blob voor de service.

> [!WARNING]
> Als u bewerkt of uw eigen metagegevens voor de blob uploaden, moet u ervoor te zorgen dat deze metagegevens behouden blijft. Als u uploadt is nieuwe metagegevens zonder deze metagegevens, de verpakte CEK IV en andere metagegevens verloren en inhoud van de blob wordt nooit meer worden opgehaald.
> 
> 

Bij het ophalen van de inhoud van het gebruik van de hele blob downloaden van een versleutelde blob omvat de **DownloadTo***/**BlobReadStream** gemak methoden. De verpakte CEK is al en worden gebruikt in combinatie met de IV (opgeslagen als blobmetagegevens in dit geval) de ontsleutelde gegevens retourneren aan de gebruikers.

Downloaden van een willekeurig bereik (**DownloadRange*** methoden) in de versleutelde blob omvat het aanpassen van het bereik dat is opgegeven door gebruikers om op te halen van een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt voor het ontsleutelen van het aangevraagde bereik is.

Alle blob-typen (blok-blobs, pagina-blobs en toevoeg-blobs) kunnen worden versleuteld/ontsleuteld met behulp van dit schema.

### <a name="queues"></a>Wachtrijen
Omdat berichten in wachtrij plaatsen van een willekeurige indeling zijn kunnen, definieert de clientbibliotheek van een aangepaste indeling die de initialisatie van de Vector (IV) en de versleutelde inhoud versleutelingssleutel (CEK) in de berichttekst bevat.

Tijdens het versleutelen kan de clientbibliotheek genereert een willekeurige IV van 16 bytes samen met een willekeurige CEK 32 bytes en envelop versleuteling van de tekst van het wachtrij-bericht met deze informatie wordt uitgevoerd. De verpakte CEK en een aantal aanvullende metagegevens vervolgens worden toegevoegd aan het wachtrijbericht versleutelde. Dit gewijzigde bericht (Zie hieronder) wordt opgeslagen op de service.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Tijdens het ontsleutelen, wordt de ingepakte sleutel opgehaald uit bericht uit de wachtrij en al. De IV is ook opgehaald uit bericht uit de wachtrij en gebruikt samen met de niet-ingepakte sleutel om de wachtrij berichtgegevens te ontsleutelen. Houd er rekening mee dat de versleutelingsmetagegevens van de klein (onder 500 bytes), is zodat terwijl deze telt mee voor de limiet van 64KB voor een wachtrijbericht, de impact beheerd worden moet.

### <a name="tables"></a>Tabellen
De clientbibliotheek biedt ondersteuning voor versleuteling van entiteitseigenschappen voor invoegen en vervang bewerkingen.

> [!NOTE]
> Samenvoegen wordt momenteel niet ondersteund. Omdat een subset van eigenschappen mogelijk zijn versleuteld eerder met een andere sleutel, leidt gewoon de nieuwe eigenschappen samenvoegen en bijwerken van de metagegevens tot verlies van gegevens. Samenvoegen van een vereist extra service aanroepen om te lezen van de vooraf bestaande entiteit van de service of met een nieuwe sleutel per eigenschap, die beide zijn niet geschikt voor betere prestaties.
> 
> 

Tabel gegevensversleuteling werkt als volgt:  

1. Gebruikers opgeven welke eigenschappen u moet worden versleuteld.
2. De clientbibliotheek genereert een willekeurige initialisatie Vector (IV) van 16 bytes samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes voor elke entiteit en envelop versleuteling uitgevoerd op de afzonderlijke eigenschappen moeten worden gecodeerd door een nieuwe IV per eigenschap af te leiden. De gecodeerde eigenschap wordt als binaire gegevens opgeslagen.
3. De verpakte CEK en een aantal aanvullende metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerde eigenschap (_ClientEncryptionMetadata1) is een tekenreekseigenschap die de informatie over IV, versie en verpakte sleutel bevat. De tweede gereserveerde eigenschap (_ClientEncryptionMetadata2) is een binaire eigenschap met de informatie over de eigenschappen die zijn versleuteld. De informatie in deze tweede eigenschap (_ClientEncryptionMetadata2) is versleuteld.
4. Als gevolg van deze extra gereserveerde vereiste eigenschappen voor versleuteling, kunnen gebruikers nu alleen 250 aangepaste eigenschappen in plaats van 252 hebben. De totale grootte van de entiteit moet minder dan 1 MB.

Houd er rekening mee dat alleen de tekenreekseigenschappen van de kunnen worden versleuteld. Als andere typen eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De versleutelde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen na de decodering.

Gebruikers moeten de eigenschappen moeten worden gecodeerd opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door een kenmerk [EncryptProperty] (voor POCO-entiteiten die zijn afgeleid van TableEntity) of een omzetter versleuteling in de aanvraag-opties. Een oplossing versleuteling is een gemachtigde die ervoor zorgt dat een partitiesleutel, rijsleutel en de naam van eigenschap retourneert een Booleaanse waarde die aangeeft of de eigenschap die moet worden versleuteld. Tijdens het versleutelen gebruik de clientbibliotheek van deze informatie om te bepalen of een eigenschap tijdens het schrijven naar de kabel moet worden versleuteld. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld als X, vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en B.) Houd er rekening mee dat het is niet nodig om deze informatie tijdens het lezen of het uitvoeren van query's entiteiten te geven.

### <a name="batch-operations"></a>Batchbewerkingen
In de batchbewerkingen, wordt de dezelfde KEK gebruikt in een rij in de desbetreffende batchbewerking omdat de clientbibliotheek slechts staat één options-object (en daarom een beleid/KEK) per batchbewerking. Echter, de clientbibliotheek wordt intern Genereer een nieuwe willekeurige IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook voor kiezen voor het versleutelen van de verschillende eigenschappen voor elke bewerking in de batch door dit gedrag definiëren in de conflictoplosser voor versleuteling.

### <a name="queries"></a>Query's
Om de querybewerkingen uitvoeren, moet u een sleutel-omzetter kan omzetten van de sleutels in de resultatenset. Als een entiteit die is opgenomen in het resultaat van de query kan niet worden omgezet naar een provider, de clientbibliotheek genereert een fout opgetreden. Voor elke query die serverzijde projecties uitvoert, wordt de clientbibliotheek van de eigenschappen van de metagegevens speciale codering (_ClientEncryptionMetadata1 en _ClientEncryptionMetadata2) standaard toegevoegd aan de geselecteerde kolommen.

## <a name="azure-key-vault"></a>Azure Key Vault
Met Azure Key Vault kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Met Azure Sleutelkluis gebruikers kunnen sleutels en geheimen versleutelen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels. PFX-bestanden en wachtwoorden) met behulp van de sleutels die worden beveiligd door hardware security modules (HSM's). Zie voor meer informatie [wat is Azure Sleutelkluis?](../../key-vault/key-vault-whatis.md).

De opslagclientbibliotheek maakt gebruik van de Sleutelkluis-kernbibliotheek om te voorzien van een gemeenschappelijk framework in Azure voor het beheren van sleutels. Gebruikers krijgen ook extra voordeel van het gebruik van de extensies Sleutelkluis bibliotheek. De bibliotheek extensies biedt nuttige functionaliteit om eenvoudig en naadloze Symmetric/RSA lokale en cloud sleutel providers en met aggregatie en opslaan in cache.

### <a name="interface-and-dependencies"></a>Interface en afhankelijkheden
Er zijn drie Sleutelkluis-pakketten:

* Microsoft.Azure.KeyVault.Core bevat de IKey en IKeyResolver. Er is een klein pakket zonder afhankelijkheden. De storage-clientbibliotheek voor .NET wordt gedefinieerd als een afhankelijkheid.
* Microsoft.Azure.KeyVault bevat de Sleutelkluis REST-client.
* Microsoft.Azure.KeyVault.Extensions bevat extensiecode met implementaties van cryptografische algoritmen en een RSAKey en een SymmetricKey. Dit is afhankelijk van de Core en KeyVault-naamruimten en biedt functionaliteit om een cumulatieve resolver (wanneer gebruikers gebruiken van meerdere sleutels providers wilt) en een cache in belangrijke resolver te definiëren. Hoewel de storage-clientbibliotheek niet rechtstreeks afhankelijk van dit pakket is als gebruikers gebruik Azure Key Vault voor het opslaan van de sleutels of de extensies Sleutelkluis gebruiken om te gebruiken van de lokale en cloud cryptografische providers wilt maken, moeten ze dit pakket.

Sleutelkluis is ontworpen voor hoge waarde hoofdsleutels en bandbreedteregeling limieten per Sleutelkluis zijn ontworpen met dit in gedachten. Bij het uitvoeren van versleuteling aan clientzijde met Sleutelkluis, wordt de voorkeur model wordt lokaal met symmetrische hoofdsleutels opgeslagen als geheimen in de Sleutelkluis en in de cache opgeslagen. Gebruikers moeten het volgende doen:

1. Een geheim offline maken en uploaden naar de Sleutelkluis.
2. Gebruik van het geheim base-id als een parameter los van de huidige versie van het geheim voor versleuteling en deze informatie lokaal in de cache. CachingKeyResolver gebruiken voor het opslaan in cache; gebruikers wordt niet verwacht voor het implementeren van hun eigen caching logica.
3. Gebruik de caching resolver als invoer bij het maken van het versleutelingsbeleid voor.

Meer informatie over het gebruik van de Sleutelkluis vindt u in de [versleuteling codevoorbeelden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>Aanbevolen procedures
Ondersteuning voor codering is alleen beschikbaar in de storage-clientbibliotheek voor .NET. Windows Phone en Windows Runtime ondersteunen momenteel geen versleuteling.

> [!IMPORTANT]
> Wanneer met behulp van versleuteling aan clientzijde, worden op de hoogte van deze belangrijke punten:
> 
> * Bij het lezen van of schrijven naar een versleutelde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijven naar een versleutelde blob met protocolbewerkingen zoals het blok plaatsen, blokkeringslijst plaatsen, pagina's schrijven, wissen's of toevoeg-blok; voorkomen anders u mogelijk de versleutelde blob beschadigd en onleesbaar.
> * Een vergelijkbaar beperking bestaat voor tabellen. Zorg ervoor dat gecodeerde eigenschappen niet bijwerken zonder dat de versleutelingsmetagegevens die worden bijgewerkt.
> * Als u de metagegevens op de versleutelde blob instelt, overschrijft u mogelijk de versleutelingsgerelateerde metagegevens vereist is voor ontsleuteling, omdat de instelling van metagegevens is niet-additieve. Dit geldt ook voor momentopnamen; Vermijd metagegevens tijdens het maken van een momentopname van een versleutelde blob. Als metagegevens moet worden ingesteld, moet u aan te roepen de **FetchAttributes** methode eerst op te halen de versleutelingsmetagegevens van de huidige gelijktijdige schrijfbewerkingen voorkomen tijdens het instellen van metagegevens.
> * Schakel de **RequireEncryption** eigenschap in de aanvraag standaardopties voor gebruikers die normaal alleen met werken gesproken gecodeerde gegevens. Zie hieronder voor meer informatie.
> 
> 

## <a name="client-api--interface"></a>Client-API / Interface
Tijdens het maken van een object EncryptionPolicy gebruikers alleen een sleutel (IKey implementeren), krijgt alleen een resolver (IKeyResolver implementeren) of beide. IKey is het basic sleuteltype die wordt geïdentificeerd met een sleutel-id en de logica en biedt voor het uitpakken van tekstterugloop /. IKeyResolver wordt gebruikt voor het omzetten van een sleutel tijdens de ontsleuteling. Hiermee definieert u een methode ResolveKey die als resultaat een IKey een sleutel-id opgegeven geeft. Deze biedt gebruikers de mogelijkheid om te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

* De sleutel wordt altijd gebruikt voor versleuteling, en het ontbreken van een sleutel in een fout resulteert.
* Voor ontsleuteling:
  * De belangrijkste resolver wordt opgeroepen als ophalen van de sleutel wordt opgegeven. Als de resolver wordt opgegeven, maar geen een toewijzing voor de sleutel-id heeft, wordt er een fout gegenereerd.
  * Als er is geen resolver opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id komt niet overeen met, wordt een fout gegenereerd.

De codevoorbeelden in dit artikel demonstreren instellen van een versleutelingsbeleid voor van en werken met de versleutelde gegevens, maar niet laten zien voor het werken met Azure Sleutelkluis. De [versleuteling voorbeelden](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) op GitHub demonstreren een meer gedetailleerde end-to-end-scenario voor blobs, wachtrijen en tabellen, samen met Sleutelkluis-integratie.

### <a name="requireencryption-mode"></a>RequireEncryption modus
Gebruikers kunnen eventueel inschakelen voor een werkmodus waar alle uploaden en downloaden moeten worden versleuteld. In deze modus, mislukken pogingen tot gegevens zonder een versleutelingsbeleid uploaden of downloaden van gegevens die niet worden versleuteld op de service op de client. De **RequireEncryption** eigenschap van het Aanvraagobject opties bepaalt dit gedrag. Als uw toepassing, worden alle objecten die zijn opgeslagen in Azure Storage gecodeerd, dan kunt u instellen de **RequireEncryption** -eigenschap op de standaardopties voor de aanvraag voor het object van de client service. Bijvoorbeeld ingesteld **CloudBlobClient.DefaultRequestOptions.RequireEncryption** naar **true** voor versleuteling vereisen voor alle bewerkingen die worden uitgevoerd door middel van dat clientobject blob.


### <a name="blob-service-encryption"></a>Versleuteling van BLOB-service
Maak een **BlobEncryptionPolicy** object en stel deze in de opties voor aanvraag (per API of op een client met behulp van **DefaultRequestOptions**). Alle andere wordt verwerkt door de clientbibliotheek intern.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

 // Set the encryption policy on the request options.
 BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

 // Upload the encrypted contents to the blob.
 blob.UploadFromStream(stream, size, null, options, null);

 // Download and decrypt the encrypted contents from the blob.
 MemoryStream outputStream = new MemoryStream();
 blob.DownloadToStream(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Wachtrij-service: versleuteling
Maak een **QueueEncryptionPolicy** object en stel deze in de opties voor aanvraag (per API of op een client met behulp van **DefaultRequestOptions**). Alle andere wordt verwerkt door de clientbibliotheek intern.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

### <a name="table-service-encryption"></a>Tabel-service: versleuteling
Naast het maken van een versleutelingsbeleid voor en op aanvraag opties instellen, moet u ofwel opgeven een **EncryptionResolver** in **TableRequestOptions**, of stel het kenmerk [EncryptProperty] in de entiteit.

#### <a name="using-the-resolver"></a>Conflictoplosser

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need to specify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Met behulp van kenmerken
Zoals eerder vermeld, als de entiteit TableEntity implementeert, klikt u vervolgens de eigenschappen kunnen gedecoreerd worden met het kenmerk [EncryptProperty] in plaats van de **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Versleuteling en prestaties
Houd er rekening mee dat de opslag, resulteert in extra prestatieoverhead versleutelt. De inhoudssleutel en IV moet worden gegenereerd, de inhoud zelf moet worden versleuteld en aanvullende metagegevens moet zijn geformatteerd en geüpload. Deze overhead varieert, afhankelijk van de hoeveelheid gegevens wordt versleuteld. Het is raadzaam dat klanten hun toepassingen voor de prestaties tijdens de ontwikkeling altijd testen.

## <a name="next-steps"></a>Volgende stappen
* [Zelfstudie: Blobs versleutelen en ontsleutelen in Microsoft Azure Storage met Azure Sleutelkluis](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
* Download de [Azure Storage-clientbibliotheek voor .NET-NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.Storage)
* Download de Azure Key Vault NuGet [Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [Client](http://www.nuget.org/packages/Microsoft.Azure.KeyVault/), en [extensies](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) pakketten  
* Ga naar de [documentatie van Azure Sleutelkluis](../../key-vault/key-vault-whatis.md)