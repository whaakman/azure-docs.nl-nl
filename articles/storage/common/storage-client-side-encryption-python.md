---
title: Versleuteling van de Client-Side met behulp van Python voor opslag op Microsoft Azure | Microsoft Docs
description: De Azure Storage-clientbibliotheek voor Python biedt ondersteuning voor versleuteling aan clientzijde voor een maximale beveiliging voor uw Azure Storage-toepassingen.
services: storage
documentationcenter: python
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: f9bf7981-9948-4f83-8931-b15679a09b8a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: bf6696cfdfe9fc18dd2f000162a4e787a7ca6e21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Versleuteling van de Client-Side met behulp van Python voor opslag op Microsoft Azure
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht
De [Azure Storage-clientbibliotheek voor Python](https://pypi.python.org/pypi/azure-storage) ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat uploaden naar Azure Storage en ontsleutelen van gegevens tijdens het downloaden van de client.

> [!NOTE]
> De Azure Storage Python-bibliotheek is een Preview-versie.
> 
> 

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleuteling en ontsleuteling via de envelop techniek
De processen van coderen en decoderen volgt u de envelop techniek.

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de envelop techniek
Versleuteling via de envelop techniek werkt in de volgende manier:

1. De Azure storage-clientbibliotheek genereert een inhoud versleutelingssleutel (CEK), die een symmetrische sleutel met een eenmalig gebruik is.
2. Gebruikersgegevens worden versleuteld met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de belangrijkste versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en een asymmetrisch sleutelpaar of een symmetrische sleutel, die lokaal wordt beheerd.
   De opslagclientbibliotheek zelf heeft nooit toegang tot de KEK-sleutel. De bibliotheek roept de belangrijkste wrapping algoritme die wordt geleverd door de KEK. Gebruikers kunnen kiezen met aangepaste providers voor sleutel wrapping/uitpakken indien gewenst.
4. De versleutelde gegevens is vervolgens naar de Azure Storage-service geüpload. De ingepakte sleutel samen met enkele aanvullende versleutelingsmetagegevens die is opgeslagen als metagegevens (op een blob) of geïnterpoleerd met de versleutelde gegevens (Wachtrijberichten en tabelentiteiten).

### <a name="decryption-via-the-envelope-technique"></a>Ontsleuteling via de envelop techniek
Ontsleuteling via de envelop techniek werkt in de volgende manier:

1. De clientbibliotheek wordt ervan uitgegaan dat de gebruiker is het beheren van de belangrijkste versleutelingssleutel (KEK-sleutel) lokaal. De gebruiker hoeft niet te weten de specifieke sleutel die is gebruikt voor versleuteling. In plaats daarvan worden een sleutel resolver, die wordt omgezet verschillende sleutel-id's aan sleutels, ingesteld en gebruikt.
2. De clientbibliotheek downloadt de versleutelde gegevens samen met versleuteling materiaal dat is opgeslagen op de service.
3. De verpakte inhoud versleutelingssleutel (CEK) is en niet-ingepakte (ontsleutelde) met behulp van de belangrijkste versleuteling sleutel (KEK-sleutel). Hier opnieuw heeft de clientbibliotheek geen toegang tot de KEK-sleutel. Het aanroept algoritme voor het uitpakken van de aangepaste provider gewoon.
4. De inhoud versleutelingssleutel (CEK) wordt vervolgens gebruikt voor het ontsleutelen van de versleutelde gegevens.

## <a name="encryption-mechanism"></a>Versleuteling mechanisme
Maakt gebruik van de storage-clientbibliotheek [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om versleuteling van gebruikersgegevens. In het bijzonder [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elk service enigszins anders werkt zodat elk van deze hier worden besproken.

### <a name="blobs"></a>Blobs
De clientbibliotheek biedt momenteel ondersteuning voor versleuteling van de hele BLOB's. In het bijzonder versleuteling wordt ondersteund wanneer gebruikers de **maken*** methoden. Voor downloads, zowel volledige als bereik downloads worden ondersteund en garandeert van beide uploaden en downloaden is beschikbaar.

Tijdens het versleutelen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) 32 bytes en envelop versleuteling van het gebruik van deze informatie blobgegevens uitvoeren. De verpakte CEK en een aantal aanvullende metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met de versleutelde blob voor de service.

> [!WARNING]
> Als u bewerkt of uw eigen metagegevens voor de blob uploaden, moet u ervoor te zorgen dat deze metagegevens behouden blijft. Als u nieuwe metagegevens zonder deze metagegevens uploadt, wordt de verpakte CEK, IV en andere metagegevens niet verloren en inhoud van de blob wordt nooit meer worden opgehaald.
> 
> 

Bij het ophalen van de inhoud van het gebruik van de hele blob downloaden van een versleutelde blob omvat de **ophalen*** gemak methoden. De verpakte CEK is al en worden gebruikt in combinatie met de IV (opgeslagen als blobmetagegevens in dit geval) de ontsleutelde gegevens retourneren aan de gebruikers.

Downloaden van een willekeurig bereik (**ophalen*** methoden met bereik parameters doorgegeven) in de versleutelde blob omvat het aanpassen van het bereik dat is opgegeven door gebruikers om op te halen van een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt voor het ontsleutelen van het aangevraagde bereik is.

Blok-blobs en pagina-blobs kunnen alleen worden versleuteld/ontsleuteld met behulp van dit schema. Er is momenteel geen ondersteuning voor het versleutelen van toevoeg-blobs.

### <a name="queues"></a>Wachtrijen
Omdat berichten in wachtrij plaatsen van een willekeurige indeling zijn kunnen, definieert de clientbibliotheek van een aangepaste indeling die de initialisatie van de Vector (IV) en de versleutelde inhoud versleutelingssleutel (CEK) in de berichttekst bevat.

Tijdens het versleutelen kan de clientbibliotheek genereert een willekeurige IV van 16 bytes samen met een willekeurige CEK 32 bytes en envelop versleuteling van de tekst van het wachtrij-bericht met deze informatie wordt uitgevoerd. De verpakte CEK en een aantal aanvullende metagegevens vervolgens worden toegevoegd aan het wachtrijbericht versleutelde. Dit gewijzigde bericht (Zie hieronder) wordt opgeslagen op de service.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

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
3. De verpakte CEK en een aantal aanvullende metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste eigenschap gereserveerd (\_ClientEncryptionMetadata1) is een tekenreekseigenschap die de informatie over IV, versie en verpakte sleutel bevat. De tweede eigenschap gereserveerd (\_ClientEncryptionMetadata2) is een binaire eigenschap met de informatie over de eigenschappen die zijn versleuteld. De informatie in deze tweede eigenschap (\_ClientEncryptionMetadata2) zelf versleuteld.
4. Als gevolg van deze extra gereserveerde vereiste eigenschappen voor versleuteling, kunnen gebruikers nu alleen 250 aangepaste eigenschappen in plaats van 252 hebben. De totale grootte van de entiteit moet minder dan 1MB.
   
   Houd er rekening mee dat alleen de tekenreekseigenschappen van de kunnen worden versleuteld. Als andere typen eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De versleutelde tekenreeksen worden opgeslagen op de service als binaire eigenschappen en ze worden geconverteerd naar tekenreeksen (onbewerkte tekenreeksen, niet EntityProperties met type EdmType.STRING) na de decodering.
   
   Gebruikers moeten de eigenschappen moeten worden gecodeerd opgeven voor tabellen, naast het coderingsbeleid. Dit kunt doen door op te slaan deze eigenschappen in TableEntity-objecten met het type is ingesteld op EdmType.STRING en is ingesteld op true of de encryption_resolver_function instellen op het object tableservice coderen. Een oplossing versleuteling is een functie die ervoor zorgt dat een partitiesleutel, rijsleutel en de naam van eigenschap retourneert een Booleaanse waarde die aangeeft of de eigenschap die moet worden versleuteld. Tijdens het versleutelen gebruik de clientbibliotheek van deze informatie om te bepalen of een eigenschap tijdens het schrijven naar de kabel moet worden versleuteld. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld als X, vervolgens versleutelen eigenschap A; anders versleutelen eigenschappen A en B.) Houd er rekening mee dat het is niet nodig om deze informatie tijdens het lezen of het uitvoeren van query's entiteiten te geven.

### <a name="batch-operations"></a>Batchbewerkingen
Een versleutelingsbeleid geldt voor alle rijen in de batch. De clientbibliotheek wordt intern Genereer een nieuwe willekeurige IV en willekeurige CEK per rij in de batch. Gebruikers kunnen ook voor kiezen voor het versleutelen van de verschillende eigenschappen voor elke bewerking in de batch door dit gedrag definiëren in de conflictoplosser voor versleuteling.
Als een batch is gemaakt als een context manager via de tableservice batch()-methode, wordt de tableservice versleutelingsbeleid automatisch worden toegepast op de batch. Als een batch expliciet gemaakt wordt door het aanroepen van de constructor, moet u het coderingsbeleid doorgegeven als parameter en links gedurende de levensduur van de batch zijn.
Houd er rekening mee dat entiteiten worden gecodeerd als ze worden ingevoegd in de batch met behulp van de batch-versleutelingsbeleid (entiteiten worden niet versleuteld op het moment van het doorvoeren van de batch met behulp van de tableservice versleutelingsbeleid).

### <a name="queries"></a>Query's
Om de querybewerkingen uitvoeren, moet u een sleutel-omzetter kan omzetten van de sleutels in de resultatenset. Als een entiteit die is opgenomen in het resultaat van de query kan niet worden omgezet naar een provider, de clientbibliotheek genereert een fout opgetreden. Voor elke query die wordt uitgevoerd van server side projecties, de clientbibliotheek van de eigenschappen van de metagegevens speciale versleuteling wordt toegevoegd (\_ClientEncryptionMetadata1 en \_ClientEncryptionMetadata2) standaard aan de geselecteerde kolommen.

> [!IMPORTANT]
> Wanneer met behulp van versleuteling aan clientzijde, worden op de hoogte van deze belangrijke punten:
> 
> * Bij het lezen van of schrijven naar een versleutelde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijven naar een versleutelde blob met protocolbewerkingen zoals het blok plaatsen, blokkeringslijst plaatsen, pagina's schrijven of lege pagina's; voorkomen anders u mogelijk de versleutelde blob beschadigd en onleesbaar.
> * Een vergelijkbaar beperking bestaat voor tabellen. Zorg ervoor dat gecodeerde eigenschappen niet bijwerken zonder dat de versleutelingsmetagegevens die worden bijgewerkt.
> * Als u de metagegevens op de versleutelde blob instelt, overschrijft u mogelijk de versleutelingsgerelateerde metagegevens vereist is voor ontsleuteling, omdat de instelling van metagegevens is niet-additieve. Dit geldt ook voor momentopnamen; Vermijd metagegevens tijdens het maken van een momentopname van een versleutelde blob. Als metagegevens moet worden ingesteld, moet u aan te roepen de **get_blob_metadata** methode eerst op te halen de versleutelingsmetagegevens van de huidige gelijktijdige schrijfbewerkingen voorkomen tijdens het instellen van metagegevens.
> * Schakel de **require_encryption** vlag voor het serviceobject voor gebruikers die normaal alleen met gecodeerde gegevens werken gesproken. Zie hieronder voor meer informatie.
> 
> 

De opslagclientbibliotheek wordt verwacht voor de opgegeven KEK en sleutel conflictoplosser voor het implementeren van de volgende interface. [Azure Sleutelkluis](https://azure.microsoft.com/services/key-vault/) ondersteuning voor Python-KEK-management in behandeling is en zal worden geïntegreerd in deze bibliotheek als voltooid.

## <a name="client-api--interface"></a>Client-API / Interface
Nadat een opslagobject van service (dat wil zeggen blockblobservice) is gemaakt, de gebruiker waarden kan toewijzen aan de velden die deel uitmaken van een versleutelingsbeleid: key_encryption_key, key_resolver_function, en require_encryption. Gebruikers kunnen alleen een KEK bieden alleen een resolver of beide. key_encryption_key is het basic sleuteltype die wordt geïdentificeerd met een sleutel-id en de logica en biedt voor het uitpakken van tekstterugloop /. key_resolver_function wordt gebruikt voor het omzetten van een sleutel tijdens de ontsleuteling. Retourneert een geldige KEK een sleutel-id opgegeven. Deze biedt gebruikers de mogelijkheid om te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

De KEK moet de volgende methoden voor het versleutelen van gegevens met succes implementeren:

* wrap_key(cek): loopt van de opgegeven CEK (bytes) die met behulp van een algoritme van de keuze van de gebruiker. Retourneert de ingepakte sleutel.
* get_key_wrap_algorithm(): het algoritme voor het inpakken van sleutels retourneert.
* get_kid(): de id van de tekenreeks voor deze KEK als resultaat geven.
  De KEK moet de volgende manieren is om gegevens te decoderen implementeren:
* unwrap_key (cek, algoritme): Hiermee wordt het niet-ingepakte formulier van de opgegeven CEK met behulp van de algoritme tekenreeks opgegeven.
* get_kid(): retourneert een tekenreeks met sleutel-id voor deze KEK-sleutel.

De belangrijkste resolver moet ten minste een methode die een sleutel-id, krijgt de bijbehorende KEK implementatie van de bovenstaande interface retourneert implementeren. Alleen deze methode is om te worden toegewezen aan de eigenschap key_resolver_function in het service-object.

* De sleutel wordt altijd gebruikt voor versleuteling, en het ontbreken van een sleutel in een fout resulteert.
* Voor ontsleuteling:
  
  * De belangrijkste resolver wordt opgeroepen als ophalen van de sleutel wordt opgegeven. Als de resolver wordt opgegeven, maar geen een toewijzing voor de sleutel-id heeft, wordt er een fout gegenereerd.
  * Als er is geen resolver opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id komt niet overeen met, wordt een fout gegenereerd.
    
    De voorbeelden versleuteling in azure.storage.samples <fix URL>een meer gedetailleerde end-to-end-scenario voor blobs, wachtrijen en tabellen te demonstreren.
      Voorbeeldimplementaties van de KEK-sleutel en de belangrijkste resolver vindt u in de voorbeeldbestanden als KeyWrapper en KeyResolver respectievelijk.

### <a name="requireencryption-mode"></a>RequireEncryption modus
Gebruikers kunnen eventueel inschakelen voor een werkmodus waar alle uploaden en downloaden moeten worden versleuteld. In deze modus, mislukken pogingen tot gegevens zonder een versleutelingsbeleid uploaden of downloaden van gegevens die niet worden versleuteld op de service op de client. De **require_encryption** vlag voor het serviceobject bepaalt dit gedrag.

### <a name="blob-service-encryption"></a>Versleuteling van BLOB-service
Beleidsvelden de versleuteling in te stellen op de blockblobservice-object. Alle andere wordt verwerkt door de clientbibliotheek intern.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload the encrypted contents to the blob.
my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

# Download and decrypt the encrypted contents from the blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Wachtrij-service: versleuteling
De codering velden voor het beleid op het object queueservice instellen. Alle andere wordt verwerkt door de clientbibliotheek intern.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>Tabel-service: versleuteling
Naast het maken van een versleutelingsbeleid voor en op aanvraag opties instellen, moet u ofwel opgeven een **encryption_resolver_function** op de **tableservice**, of stel het kenmerk coderen in de EntityProperty.

### <a name="using-the-resolver"></a>Conflictoplosser

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define the encryption resolver_function.
def my_encryption_resolver(pk, rk, property_name):
    if property_name == 'foo':
        return True
    return False

# Set the KEK and key resolver on the service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Met behulp van kenmerken
Zoals eerder vermeld, kan een eigenschap worden gemarkeerd voor versleuteling door opgeslagen in een object EntityProperty en instellen van het veld versleutelen.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Versleuteling en prestaties
Houd er rekening mee dat de opslag, resulteert in extra prestatieoverhead versleutelt. De inhoudssleutel en IV moet worden gegenereerd, de inhoud zelf moet worden versleuteld en aanvullende metagegevens moet zijn geformatteerd en geüpload. Deze overhead varieert, afhankelijk van de hoeveelheid gegevens wordt versleuteld. Het is raadzaam dat klanten hun toepassingen voor de prestaties tijdens de ontwikkeling altijd testen.

## <a name="next-steps"></a>Volgende stappen
* Download de [Azure Storage-clientbibliotheek voor Java PyPi-pakket](https://pypi.python.org/pypi/azure-storage)
* Download de [Azure Storage-clientbibliotheek voor Python broncode vanuit GitHub](https://github.com/Azure/azure-storage-python)
