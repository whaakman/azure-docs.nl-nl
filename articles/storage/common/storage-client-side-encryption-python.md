---
title: Client-Side-versleuteling voor Microsoft Azure Storage met Python | Microsoft Docs
description: De Azure Storage-clientbibliotheek voor Python ondersteunt versleuteling aan clientzijde voor een maximale beveiliging voor uw Azure Storage-toepassingen.
services: storage
author: lakasa
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.component: common
ms.openlocfilehash: 6a6508393fe935b456cde815d35f2fd4447cd2d4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528119"
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Client-Side-versleuteling voor Microsoft Azure Storage met Python
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Overzicht
De [Azure Storage-clientbibliotheek voor Python](https://pypi.python.org/pypi/azure-storage) ondersteunt de versleuteling van gegevens binnen clienttoepassingen voordat u uploadt naar Azure Storage en de ontsleuteling van gegevens tijdens het downloaden van de client.

> [!NOTE]
> De Azure Storage Python-bibliotheek is in preview.
> 
> 

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Versleuteling en ontsleuteling via de techniek envelop
De techniek envelop volgt u de processen van versleuteling en ontsleuteling.

### <a name="encryption-via-the-envelope-technique"></a>Versleuteling via de techniek envelop
Versleuteling via de techniek envelop werkt in de volgende manier:

1. De Azure storage-clientbibliotheek genereert een sleutel met versleuteling van inhoud (CEK), dit een symmetrische sleutel van één permanente gebruiken is.
2. Gebruikersgegevens worden versleuteld met behulp van deze CEK.
3. De CEK wordt vervolgens verpakt (versleuteld) met behulp van de sleutel van versleutelingssleutel (KEK-sleutel). De KEK wordt aangeduid met een sleutel-id en mag bestaan uit een asymmetrisch sleutelpaar of een symmetrische sleutel, die lokaal wordt beheerd.
   De opslagclientbibliotheek zelf heeft nooit toegang tot de KEK-sleutel. De bibliotheek roept de belangrijkste wrapping-algoritme die wordt geleverd door de KEK-sleutel. Gebruikers kunnen kiezen om te gebruiken van aangepaste providers voor sleutel verpakken/uitpakken, indien gewenst.
4. De versleutelde gegevens wordt vervolgens naar de Azure Storage-service geüpload. De ingepakte sleutel samen met enkele aanvullende metagegevens is opgeslagen als metagegevens (voor een blob) of geïnterpoleerde met de versleutelde gegevens (berichten in wachtrij plaatsen en tabelitems).

### <a name="decryption-via-the-envelope-technique"></a>Ontsleuteling via de techniek envelop
Ontsleuteling via de techniek envelop werkt in de volgende manier:

1. De clientbibliotheek wordt ervan uitgegaan dat de gebruiker is het beheren van de sleutel van versleutelingssleutel (KEK) lokaal. De gebruiker hoeft niet te weten de specifieke sleutel die is gebruikt voor versleuteling. In plaats daarvan worden een sleutel resolver, waarmee u verschillende sleutel-id's voor sleutels, ingesteld en gebruikt.
2. De clientbibliotheek downloadt de versleutelde gegevens, samen met versleuteling materiaal dat is opgeslagen op de service.
3. De verpakte inhoud versleutelingssleutel (CEK) is niet-ingepakte (ontsleutelde) met behulp van de key-versleuteling (KEK)-sleutel. Hier weer heeft de clientbibliotheek geen toegang tot de KEK-sleutel. Het gewoon aanroept uitpakken algoritme van de aangepaste provider.
4. De sleutel voor versleuteling van inhoud (CEK) wordt vervolgens gebruikt voor het ontsleutelen van de versleutelde gegevens.

## <a name="encryption-mechanism"></a>-Versleutelingsmechanisme
Maakt gebruik van de storage-clientbibliotheek [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) om gebruikersgegevens te versleutelen. Met name [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modus met AES. Elke service enigszins anders werkt, zodat we elk van deze hier wordt besproken.

### <a name="blobs"></a>Blobs
De clientbibliotheek biedt momenteel ondersteuning voor versleuteling van hele BLOB's. Specifiek, versleuteling wordt ondersteund wanneer gebruikers de **maken*** methoden. Voor downloads, zowel volledige als bereik downloads worden ondersteund, en parallelle uitvoering van beide uploaden en downloaden is beschikbaar.

Tijdens het versleutelen, wordt de clientbibliotheek genereren van een willekeurige initialisatie Vector (IV) van 16 bytes, samen met een willekeurige inhoud versleutelingssleutel (CEK) van 32 bytes en envelop versleuteling van de blob-gegevens met behulp van deze gegevens uitvoeren. De verpakte CEK en enkele aanvullende metagegevens worden vervolgens opgeslagen als blob-metagegevens samen met de versleutelde blob voor de service.

> [!WARNING]
> Als u bewerken of uw eigen metagegevens voor de blob uploaden, moet u om ervoor te zorgen dat deze metagegevens behouden blijft. Als u nieuwe metagegevens zonder deze metagegevens uploaden, de verpakte CEK, IV en andere metagegevens, gaan verloren en de blobinhoud wordt nooit meer worden opgehaald.
> 
> 

Bij het ophalen van de inhoud van het gebruik van de gehele blob downloaden van een versleutelde blob omvat de **ophalen*** methoden voor gebruiksgemak. De verpakte CEK is uitgepakt en gebruikt in combinatie met de IV (opgeslagen als blobmetagegevens in dit geval) de ontsleutelde om gegevens te retourneren aan de gebruikers.

Downloaden van een willekeurige adresbereik (**ophalen*** methoden met bereik parameters doorgegeven) omvat het aanpassen van het bereik dat is opgegeven door gebruikers om op te halen van een kleine hoeveelheid aanvullende gegevens die kunnen worden gebruikt om met succes in de versleutelde blob het gevraagde bereik ontsleutelen.

Blok-blobs en pagina-blobs kunnen alleen worden versleuteld/ontsleuteld met behulp van dit schema. Er is momenteel geen ondersteuning voor het versleutelen van toevoeg-blobs.

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
3. De verpakte CEK en enkele aanvullende metagegevens worden vervolgens opgeslagen als twee extra gereserveerde eigenschappen. De eerste gereserveerde eigenschap (\_ClientEncryptionMetadata1) is een string-eigenschap die de informatie over IV, versie en ingepakte sleutel bevat. De tweede gereserveerde eigenschap (\_ClientEncryptionMetadata2) is een binaire eigenschap waarin de informatie over de eigenschappen die zijn versleuteld. De informatie in dit tweede eigenschap (\_ClientEncryptionMetadata2) is versleuteld.
4. Vanwege deze extra gereserveerde eigenschappen vereist zijn voor versleuteling, kunnen gebruikers nu alleen 250 aangepaste eigenschappen in plaats van 252 hebben. De totale grootte van de entiteit moet minder dan 1MB.
   
   Houd er rekening mee dat alleen de tekenreekseigenschappen van de kunnen worden versleuteld. Als andere typen eigenschappen moeten worden versleuteld, moeten ze worden geconverteerd naar tekenreeksen. De gecodeerde tekenreeksen worden opgeslagen op de service als binaire eigenschappen, en ze worden geconverteerd naar tekenreeksen (onbewerkte tekenreeksen, niet EntityProperties met het type EdmType.STRING) na ontsleuteling.
   
   Gebruikers moeten de eigenschappen moeten worden versleuteld opgeven voor tabellen, naast het coderingsbeleid. Dit kan worden gedaan door op te slaan deze eigenschappen in TableEntity objecten met het type is ingesteld op EdmType.STRING en is ingesteld op true of de encryption_resolver_function instellen op het object tableservice coderen. Een oplossing versleuteling is een functie die u neemt een partitiesleutel, een rijsleutel en een eigenschapsnaam en retourneert een Booleaanse waarde die aangeeft of deze eigenschap moet worden versleuteld. Tijdens het versleutelen, wordt de clientbibliotheek van deze informatie gebruiken om te bepalen of een eigenschap moet worden versleuteld tijdens het schrijven naar de kabel. De gemachtigde biedt ook de mogelijkheid van logica over hoe eigenschappen zijn gecodeerd. (Bijvoorbeeld, als X, vervolgens versleutelen eigenschap A; anders eigenschappen A en B. te versleutelen) Houd er rekening mee dat het is niet nodig om deze informatie tijdens het lezen of uitvoeren van query's entiteiten te geven.

### <a name="batch-operations"></a>Batchbewerkingen
Een versleutelingsbeleid voor is van toepassing op alle rijen in de batch. De clientbibliotheek wordt een nieuwe willekeurige IV en willekeurige CEK per rij in de batch intern gegenereerd. Gebruikers kunnen ook kiezen voor het versleutelen van verschillende eigenschappen voor elke bewerking in de batch met het definiëren van dit gedrag in de conflictoplosser voor versleuteling.
Als een batch is gemaakt als een context manager via de methode van de batch() tableservice, is het versleutelingsbeleid van de tableservice wordt automatisch toegepast op de batch. Als een batch expliciet door het aanroepen van de constructor is gemaakt, moet het versleutelingsbeleid voor worden doorgegeven als parameter en links ongewijzigd gedurende de levensduur van de batch.
Houd er rekening mee dat de entiteiten worden versleuteld terwijl ze worden ingevoegd in de batch met behulp van de batch-versleutelingsbeleid (entiteiten zijn niet versleuteld op het moment van het doorvoeren van de batch met behulp van de tableservice versleutelingsbeleid).

### <a name="queries"></a>Query's
> [!NOTE]
> Omdat de entiteiten zijn versleuteld, kunt u query's die filteren niet uitvoeren op een gecodeerde eigenschap.  Als u probeert, is resultaten onjuist, omdat de service probeert zou om versleutelde gegevens met niet-versleutelde gegevens te vergelijken.
> 
>
Als u wilt querybewerkingen uitvoeren, moet u een sleutel-omzetter kan omzetten van de sleutels in de resultatenset. Als een entiteit die deel uitmaken van het queryresultaat kan niet worden omgezet naar een provider, genereert de clientbibliotheek een fout. Voor elke query die wordt uitgevoerd van server side projecties, de clientbibliotheek van de eigenschappen van speciale versleuteling metagegevens wordt toegevoegd (\_ClientEncryptionMetadata1 en \_ClientEncryptionMetadata2) standaard aan de geselecteerde kolommen.

> [!IMPORTANT]
> Houd rekening met deze belangrijke punten bij het gebruik van versleuteling op de client:
> 
> * Bij het lezen van of schrijven naar een versleutelde blob, hele blob uploaden opdrachten en bereik/geheel blob downloaden opdrachten gebruiken. Schrijf naar een versleutelde blob protocol bewerkingen zoals Put-blokken, blokkeringslijst plaatsen, pagina's schrijven of wissen pagina's. anders u mogelijk de versleutelde blob beschadigd en onleesbaar.
> * Een dergelijke beperking bestaat voor tabellen. Zorg ervoor dat versleutelde eigenschappen niet bijwerken zonder versleutelingsgerelateerde metagegevens worden bijgewerkt.
> * Als u de metagegevens op de versleutelde blob hebt ingesteld, overschrijft u de versleutelingsgerelateerde metagegevens vereist voor ontsleuteling, omdat de instelling van metagegevens is niet-additieve. Dit geldt ook voor momentopnamen; Vermijd metagegevens op te geven tijdens het maken van een momentopname van een versleutelde blob. Als de metagegevens van moet zijn ingesteld, moet u aanroepen de **get_blob_metadata** methode om de metagegevens van de huidige versleuteling ophalen, en te voorkomen dat gelijktijdige schrijfbewerkingen terwijl metagegevens wordt ingesteld.
> * Schakel de **require_encryption** vlag aan het service-object voor gebruikers die alleen met de versleutelde gegevens werken moeten. Zie hieronder voor meer informatie.
> 
> 

De opslagclientbibliotheek wordt verwacht dat de KEK-sleutel opgegeven en de sleutel conflictoplosser voor het implementeren van de volgende interface. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ondersteuning voor Python-KEK-management in behandeling is en wordt geïntegreerd in deze bibliotheek wanneer dit is voltooid.

## <a name="client-api--interface"></a>Client-API / Interface
Nadat een opslagobject van service (dat wil zeggen blockblobservice) is gemaakt, de gebruiker waarden kan toewijzen aan de velden die deel uitmaken van een versleutelingsbeleid: key_encryption_key, key_resolver_function, en require_encryption. Gebruikers kunnen alleen een KEK-sleutel, bieden alleen een resolver, of beide. key_encryption_key is de standaard type sleutel dat wordt aangeduid met behulp van een sleutel-id en waarmee de logica voor het verpakken/uitpakken. key_resolver_function wordt gebruikt voor het oplossen van een sleutel tijdens het ontsleutelen. Retourneert een geldige KEK-sleutel een sleutel-id opgegeven. Deze biedt gebruikers de mogelijkheid om te kiezen tussen meerdere sleutels die worden beheerd op meerdere locaties.

De KEK-sleutel moet de volgende methoden voor het versleutelen van gegevens met succes implementeren:

* wrap_key(cek): loopt van de opgegeven CEK (bytes) die gebruikmaakt van een algoritme van de keuze van de gebruiker. Retourneert de ingepakte sleutel.
* get_key_wrap_algorithm(): retourneert het algoritme voor het verpakken van sleutels.
* get_kid(): retourneert de tekenreeks met sleutel-id voor deze KEK-sleutel.
  De KEK-sleutel moet de volgende methoden voor het ontsleutelen van gegevens met succes implementeren:
* unwrap_key (cek, algoritme): Hiermee wordt het niet-ingepakte formulier van de opgegeven CEK met behulp van de tekenreeks opgegeven algoritme.
* get_kid(): retourneert een tekenreeks met sleutel-id voor deze KEK-sleutel.

De belangrijkste resolver moet ten minste een methode die gegeven van een sleutel-id, retourneert de bijbehorende KEK implementatie van de bovenstaande interface implementeren. Alleen deze methode is om te worden toegewezen aan de eigenschap key_resolver_function op de serviceobject.

* De sleutel wordt altijd gebruikt voor versleuteling, en het ontbreken van een sleutel zal leiden tot een fout.
* Voor ontsleuteling:
  
  * De belangrijkste resolver wordt aangeroepen als voor het ophalen van de sleutel opgegeven. Als de conflictoplosser is opgegeven, maar beschikt niet over een toewijzing voor de sleutel-id, wordt er een fout opgetreden.
  * Als conflictoplosser niet is opgegeven, maar een sleutel is opgegeven, wordt de sleutel wordt gebruikt als de id overeenkomt met de vereiste sleutel-id. Als de id komt niet overeen met, wordt er een fout opgetreden.
    
    De voorbeelden versleuteling in azure.storage.samples <fix URL>demonstreren een meer gedetailleerde end-to-end-scenario voor blobs, wachtrijen en tabellen.
      Voorbeeldimplementaties van de KEK-sleutel en de sleutel conflictoplosser vindt u in de voorbeeldbestanden KeyWrapper en KeyResolver respectievelijk.

### <a name="requireencryption-mode"></a>RequireEncryption modus
Gebruikers kunnen eventueel inschakelen voor een modus waarin alle uploaden en downloaden moeten worden versleuteld. In deze modus mislukken pogingen tot gegevens zonder een versleutelingsbeleid uploaden of downloaden van gegevens die niet worden versleuteld op de service op de client. De **require_encryption** vlag aan het service-object bepaalt dit gedrag.

### <a name="blob-service-encryption"></a>BLOB-service-versleuteling
Beleidsvelden voor de versleuteling in te stellen op de blockblobservice-object. Alle andere wordt afgehandeld door de clientbibliotheek intern.

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

### <a name="queue-service-encryption"></a>Versleuteling van de wachtrij
Instellen dat versleuteling alleen de velden voor het beleid voor het object queueservice. Alle andere wordt afgehandeld door de clientbibliotheek intern.

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

### <a name="table-service-encryption"></a>Versleuteling van de tabel
Naast het maken van een versleutelingsbeleid voor en leert u deze op verzoek-opties, u moet opgeven een **encryption_resolver_function** op de **tableservice**, of het coderen-kenmerk ingesteld op de EntityProperty.

### <a name="using-the-resolver"></a>Met behulp van de conflictoplosser

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
Zoals eerder vermeld, kan een eigenschap worden gemarkeerd voor versleuteling door in een EntityProperty-object op te slaan en het instellen van het veld coderen.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Versleuteling en prestaties
Houd er rekening mee dat uw opslag, resulteert in extra prestatieoverhead versleutelt. De inhoudssleutel en IV moet worden gegenereerd, de inhoud zelf moet worden versleuteld en aanvullende metagegevens moet worden geformatteerd en geüpload. Deze overhead varieert, afhankelijk van de hoeveelheid gegevens wordt versleuteld. Het is raadzaam dat klanten altijd hun toepassingen voor de prestaties tijdens de ontwikkeling testen.

## <a name="next-steps"></a>Volgende stappen
* Download de [Azure Storage-clientbibliotheek voor Java PyPi pakket](https://pypi.python.org/pypi/azure-storage)
* Download de [Azure Storage-clientbibliotheek voor Python broncode van GitHub](https://github.com/Azure/azure-storage-python)
