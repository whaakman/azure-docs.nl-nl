---
title: Soft verwijderen voor Azure Storage-blobs (Preview) | Microsoft Docs
description: Azure Storage biedt nu voorlopig verwijderen (Preview) voor objecten van de blob zodat u uw gegevens eenvoudig kunt herstellen kunt wanneer ten onrechte wordt gewijzigd of verwijderd door een toepassing of een andere gebruiker storage-account.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 03/21/2018
ms.author: mihauss
ms.openlocfilehash: 649838af1d4c753ac1d82a66c855ef313f14e85b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="soft-delete-for-azure-storage-blobs-preview"></a>Voorlopig verwijderen voor Azure Storage-blobs (Preview)

## <a name="overview"></a>Overzicht

Azure Storage biedt nu voorlopig verwijderen (Preview) voor objecten van de blob zodat u uw gegevens eenvoudig kunt herstellen kunt wanneer ten onrechte wordt gewijzigd of verwijderd door een toepassing of een andere gebruiker storage-account.

## <a name="how-does-it-work"></a>Hoe werkt het?

Wanneer ingeschakeld, wordt er voorlopig verwijderen kunt u opslaan en herstellen van uw gegevens als blobs of blob momentopnamen worden verwijderd. Deze beveiliging wordt uitgebreid naar blob-gegevens worden gewist als gevolg van een overschrijven.

Wanneer gegevens worden verwijderd, verandert deze in een voorlopig verwijderde staat bevindt in plaats van permanent wordt gewist. Wanneer voorlopig verwijderen ingeschakeld is en u gegevens overschrijft, moet een voorlopig verwijderde momentopname is gegenereerd voor het opslaan van de status van de gegevens overschreven. Voorlopig verwijderde objecten zijn onzichtbaar tenzij expliciet wordt vermeld.
U kunt de hoeveelheid tijd voorlopig verwijderde gegevens kunnen hersteld worden voordat het permanent is verlopen.

Voorlopig verwijderen is achterwaarts compatibel; u moet geen wijzigingen aanbrengen in uw toepassingen om te profiteren van deze functie biedt bescherming. Echter, [gegevensherstel](#recovery) introduceert een nieuwe **ongedaan Blob** API.

> [!NOTE]
> Tijdens de openbare Preview aanroepen van Blob-laag ingesteld met een blob met momentopnamen is niet toegestaan.
Voorlopige verwijderingen genereert momentopnamen van uw gegevens beveiligen wanneer deze wordt overschreven. Er wordt gewerkt aan een oplossing voor het inschakelen van BLOB's met momentopnamen lagen.

### <a name="configuration-settings"></a>Configuratie-instellingen

Wanneer u een nieuw account maakt, is voorlopig verwijderen standaard uitgeschakeld. Voorlopig verwijderen is ook standaard uitgeschakeld voor de bestaande opslagaccounts. U kunt de functie in of uit kunt schakelen op elk gewenst moment tijdens de levensduur van een opslagaccount.

U wordt nog steeds toegang tot en voorlopig verwijderde gegevens herstellen als de functie is uitgeschakeld, ervan uitgaande dat voorlopig verwijderde gegevens is opgeslagen als de functie eerder is ingeschakeld. Wanneer u voorlopig verwijderen inschakelt, moet u ook de bewaarperiode configureren.

De bewaarperiode geeft de hoeveelheid tijd die voorlopig verwijderde gegevens opgeslagen en beschikbaar voor herstel worden. De bewaarperiode kloksnelheid voor blobs en blob momentopnamen die expliciet worden verwijderd, wordt gestart wanneer de gegevens worden verwijderd. Voor voorlopig verwijderde momentopnamen gegenereerd door de functie voorlopig verwijderen wanneer gegevens worden overschreven, begint de klok wanneer de momentopname is gegenereerd. Momenteel kunt u voorlopig verwijderde gegevens voor tussen 1 en 365 dagen bewaren.

U kunt de bewaarperiode voor voorlopige verwijderingen op elk gewenst moment wijzigen. Een bijgewerkte bewaarperiode gelden alleen voor nieuwe verwijderde gegevens. Eerder verloopt verwijderde gegevens op basis van de bewaarperiode op die is geconfigureerd als deze gegevens is verwijderd.

### <a name="saving-deleted-data"></a>Verwijderde gegevens opslaan

Voorlopige verwijderingen behoudt uw gegevens in veel gevallen waarbij blobs of blob momentopnamen zijn verwijderd of overschreven.

Wanneer een blob wordt overschreven met **Blob plaatsen**, **blok plaatsen**, **blokkeringslijst plaatsen** of **Blob kopiëren** een momentopname van de status van de blob voorafgaand aan de schrijven naar de bewerking wordt automatisch gegenereerd. Deze momentopname is een voorlopig verwijderde momentopname; Dit is onzichtbaar tenzij expliciet voorlopig verwijderde objecten worden genoemd. Zie de [herstel](#recovery) sectie voor informatie over het voorlopig verwijderde objecten weergeven.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens weergegeven onder oudere gegevens. Wanneer B0 wordt overschreven met B1, wordt een voorlopig verwijderde momentopname van B0 gegenereerd. Wanneer B1 wordt overschreven met B2, wordt een voorlopig verwijderde momentopname van B1 gegenereerd.*

> [!NOTE]
> Voorlopige verwijderingen alleen biedt beveiliging voor het kopiëren van overschreven wanneer deze is ingeschakeld voor de bestemmings-blob-account.

> [!NOTE]
> Voorlopige verwijderingen komt niet toestaan van beveiliging voor blobs in de laag archief overschrijven. Als een blob in het archief met een nieuwe blob in elke categorie wordt overschreven, wordt de overschreven blob permanent verlopen.

Wanneer **Blob verwijderen** wordt aangeroepen op een momentopname die momentopname is gemarkeerd als voorlopig verwijderd. Een nieuwe momentopname is niet gegenereerd.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens weergegeven onder oudere gegevens. Wanneer **momentopname Blob** B0 aangeroepen, wordt een momentopname en B1 is de actieve status van de blob. Wanneer de momentopname B0 wordt verwijderd, deze is gemarkeerd als voorlopig verwijderd.*

Wanneer **Blob verwijderen** is aangeroepen voor een basis-blob (een blob is niet zelf een momentopname), blob is gemarkeerd als voorlopig verwijderd. Consistent zijn met eerdere gedrag aanroepen **Blob verwijderen** op een blob met actieve momentopnamen wordt een fout geretourneerd. Het aanroepen van **Blob verwijderen** op een blob met voorlopig verwijderde momentopnamen foutbericht niet ophalen. U kunt nog steeds een blob en alle bijbehorende momentopnamen in één bewerking verwijderen wanneer voorlopig verwijderen is ingeschakeld. In dat geval de base blob markeert en momentopnamen als voorlopig verwijderd.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens weergegeven onder oudere gegevens. Hier een **Blob verwijderen** aanroep B2 en alle bijbehorende momentopnamen te verwijderen. De actieve blob, B2 en alle bijbehorende momentopnamen zijn gemarkeerd als voorlopig verwijderd.*

> [!NOTE]
> Wanneer een voorlopig verwijderde blob wordt overschreven, wordt een voorlopig verwijderde momentopname van de status van de blob voordat de schrijfbewerking wordt automatisch gegenereerd. De nieuwe blob neemt over van de laag van de blob overschreven.

Voorlopig verwijderen niet uw gegevens opslaan in geval van een container of account verwijderingen niet als blob-metagegevens en de blob-eigenschappen worden overschreven. Als u wilt voorkomen dat een opslagaccount foutieve verwijderd, kunt u een vergrendeling met de Azure Resource Manager. Zie het artikel Azure Resource Manager [Resources vergrendelen om te voorkomen dat onverwachte wijzigingen](../../azure-resource-manager/resource-group-lock-resources.md) voor meer informatie.

De volgende tabel worden verwacht gedrag wanneer voorlopig verwijderen is ingeschakeld:

| REST-API-bewerking                                                                                                | Resourcetype                 | Beschrijving                                                                                                 | In het gedrag wijzigen                                                                                                                                                                                                                                                                                                                                                   |
|-------------------------------------------------------------------------------------------------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Verwijderen](/rest/api/storagerp/StorageAccounts/Delete)              | Account                       | Hiermee verwijdert u het opslagaccount, met inbegrip van alle containers en blobs die deze bevat.                           | Geen wijziging. Containers en blobs in de verwijderde account zijn niet hersteld.                                                                                                                                                                                                                                                                                          |
| [Verwijderen van Container](/rest/api/storageservices/fileservices/delete-container)       | Container                     | Hiermee verwijdert u de container, inclusief alle blobs die deze bevat.                                                | Geen wijziging. BLOBs in de container verwijderde zijn niet hersteld.                                                                                                                                                                                                                                                                                                       |
| [Blob plaatsen](/rest/api/storageservices/fileservices/put-blob)                       | Blok toevoegen en pagina-Blobs | Hiermee maakt u een nieuwe blob of een bestaande blob zijn binnen een container vervangen                                          | Als dit wordt gebruikt ter vervanging van een bestaande blob zijn, wordt een momentopname van de status van de blob voorafgaand aan de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als deze is vervangen door een blob van hetzelfde type (blok, toevoegen of pagina). Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens permanent verlopen. |
| [Verwijderen van Blob](/rest/api/storageservices/fileservices/delete-blob)                 | Blok toevoegen en pagina-Blobs | Een blob of een momentopname van de blob voor verwijdering gemarkeerd. De blob of een momentopname wordt later verwijderd tijdens de garbagecollection | Als gebruikt voor wordt een momentopname van een blob, momentopname is gemarkeerd als voorlopig verwijderen verwijderd. Als gebruikt voor het verwijderen van een blob, blob is gemarkeerd als voorlopig verwijderd.                                                                                                                                                                                                                           |
| [Blob kopiëren](/rest/api/storageservices/fileservices/copy-blob)                     | Blok toevoegen en pagina-Blobs | Een bron-blob kopieert naar een bestemmings-blob in hetzelfde opslagaccount of in een ander opslagaccount.       | Als dit wordt gebruikt ter vervanging van een bestaande blob zijn, wordt een momentopname van de status van de blob voorafgaand aan de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als deze is vervangen door een blob van hetzelfde type (blok, toevoegen of pagina). Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens permanent verlopen. |
| [Blok plaatsen](/rest/api/storageservices/put-block)                                  | Blok-blobs                   | Maakt een nieuw blok om te worden vastgelegd als onderdeel van een blok-blob.                                                | Er is geen wijziging als gebruikt voor het doorvoeren van een blok naar een blob die actief is. Als gebruikt voor het doorvoeren van een blok naar een blob die is voorlopig verwijderd, wordt een nieuwe blob gemaakt en een momentopname is automatisch gegenereerd voor het vastleggen van de status van de voorlopig verwijderde blob.                                                                                                                      |
| [Lijst met geblokkeerde websites plaatsen](/rest/api/storageservices/fileservices/put-block-list)           | Blok-blobs                   | Voert een blob door te geven van de set blok-id's die bestaan uit de blok-blob.                             | Als dit wordt gebruikt ter vervanging van een bestaande blob zijn, wordt een momentopname van de status van de blob voorafgaand aan de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als het een blok-Blob is. Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens permanent verlopen.                                                |
| [Pagina plaatsen](/rest/api/storageservices/fileservices/put-page)                       | Pagina-blobs                    | Schrijft een bereik van pagina's naar een pagina-Blob.                                                                     | Geen wijziging. Pagina-Blob-gegevens die wordt overschreven in- of uitgeschakeld met behulp van deze bewerking wordt niet opgeslagen en kan niet worden hersteld.                                                                                                                                                                                                                                                   |
| [Blok toevoegen](/rest/api/storageservices/fileservices/append-block)               | Toevoeg-Blobs                  | Een blok gegevens schrijft naar het einde van een toevoeg-Blob                                                         | Geen wijziging.                                                                                                                                                                                                                                                                                                                                                           |
| [Blob-eigenschappen instellen](/rest/api/storageservices/fileservices/set-blob-properties) | Blok toevoegen en pagina-Blobs | Hiermee stelt u waarden voor de eigenschappen gedefinieerd voor een blob.                                                       | Geen wijziging. Overschreven blob-eigenschappen zijn niet hersteld.                                                                                                                                                                                                                                                                                                          |
| [Blobmetagegevens instellen](/rest/api/storageservices/fileservices/set-blob-metadata)     | Blok toevoegen en pagina-Blobs | Sets gebruiker gedefinieerde metagegevens voor de opgegeven blob als een of meer naam / waarde-paren.                          | Geen wijziging. Overschreven blobmetagegevens kan niet worden hersteld.                                                                                                                                                                                                                                                                                                             |

Het is belangrijk dat u ziet dat het aanroepen van 'Pagina plaatsen' om te overschrijven of bereiken van een pagina-Blob wissen niet automatisch momentopnamen genereren wordt. Schijven voor virtuele machine worden ondersteund door de pagina-Blobs en gebruik **pagina plaatsen** om gegevens te schrijven.

### <a name="recovery"></a>Herstel

We hebben een nieuwe API voor de 'Blob verwijderen ongedaan maken' geïntroduceerd verwijderde gegevens te herstellen te vereenvoudigen. Het aanroepen van de API verwijderen ongedaan maken op een voorlopig verwijderde base blob te herstellen en alle bijbehorende voorlopig verwijderde momentopnamen als actief. Aanroepen van de API verwijderen ongedaan maken op een actieve base blob herstelacties alle voorlopig verwijderde momentopnamen als actief worden gekoppeld. Wanneer momentopnamen worden teruggezet als actief, eruitzien ze als de gebruiker gegenereerde momentopnamen; ze overschrijven de base blob niet.

Herstellen van een blob naar een specifieke voorlopig verwijderde momentopname kunt u bellen **Blob ongedaan** op de grondtal blob. Kopieer vervolgens de momentopname via de blob nu actief. U kunt ook de momentopname kopiëren naar een nieuwe blob.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens weergegeven onder oudere gegevens. Hier **ongedaan Blob** is aangeroepen voor blob B, waardoor herstellen van de basis blob, B1 en alle bijbehorende momentopnamen, hier alleen B0, als actief. In de tweede stap wordt B0 gekopieerd via de base blob. Deze kopieerbewerking genereert een voorlopig verwijderde momentopname van B1.*

Om te bekijken voorlopig verwijderde blobs en momentopnamen blob, kunt u verwijderde gegevens in **lijst Blobs**. U kunt kiezen om alleen voorlopig verwijderde base blobs weer te geven of om op te nemen ook voorlopig verwijderde blob-momentopnamen. Voor alle voorlopig verwijderde gegevens, kunt u de tijd wanneer de gegevens zijn verwijderd en het aantal dagen voordat de gegevens worden definitief verlopen weergeven.

### <a name="example"></a>Voorbeeld

Hieronder volgt de uitvoer van de console van een .NET-script dat wordt geüpload, worden overschreven, momentopnamen, verwijderingen en Hiermee herstelt u een blob met de naam "Hallowereld" voorlopig verwijderen is ingeschakeld:

```bash
Upload:
- HelloWorld (is soft deleted: False, is snapshot: False)

Overwrite:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Snapshot:
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Delete (including snapshots):
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: False)

Undelete:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)

Copy a snapshot over the base blob:
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: True)
- HelloWorld (is soft deleted: True, is snapshot: True)
- HelloWorld (is soft deleted: False, is snapshot: False)
```

Zie de [Vervolgstappen](#Next steps) sectie voor een verwijzing naar de toepassing die deze uitvoer geproduceerd.

## <a name="pricing-and-billing"></a>Prijzen en facturering

Alle voorlopig verwijderde gegevens wordt gefactureerd op dezelfde snelheid als actieve gegevens. U wordt niet in rekening gebracht voor gegevens die wordt definitief verwijderd na de geconfigureerde bewaarperiode. Zie voor meer informatie naar momentopnamen en hoe ze kosten doorlopen [begrijpen hoe de kosten voor het samenvoegen van momentopnamen](storage-blob-snapshots.md).

U wordt niet gefactureerd voor de transacties die betrekking hebben op de automatische generatie van momentopnamen. U wordt gefactureerd voor **ongedaan Blob** transacties 'Schrijven Operations' snelheid.

Bekijk voor meer informatie over prijzen voor Azure Blob Storage in het algemeen de [Azure Blob Storage pagina met prijzen](https://azure.microsoft.com/pricing/details/storage/blobs/).

Wanneer u in eerste instantie voorlopig verwijderen inschakelt, wordt u aangeraden een kleine bewaarperiode om beter te begrijpen hoe de functie invloed is op uw factuur.

## <a name="quickstart"></a>Snelstartgids

### <a name="azure-portal"></a>Azure Portal
Om in te schakelen voorlopig verwijderen, gaat u naar de **voorlopig verwijderen** onder de optie **Blob-Service**. Klik vervolgens op **ingeschakeld** en voer het aantal dagen dat u wilt voorlopig verwijderde gegevens wilt behouden.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Als u wilt weergeven voorlopig verwijderde blobs, selecteer de **verwijderde blobs weergeven** selectievakje.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Voorlopig verwijderde momentopnamen voor een opgegeven blob kunnen weergeven, selecteert u de blob en klik vervolgens op **momentopnamen weergeven**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Zorg ervoor dat de **verwijderde momentopnamen weergeven** selectievakje is ingeschakeld.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Wanneer u op een voorlopig verwijderde blob of een momentopname klikt, zult u de eigenschappen van nieuwe blob. Ze geven aan wanneer het object is verwijderd en hoeveel dagen worden gehandhaafd tot de blob of een momentopname van de blob permanent is verlopen. Als het voorlopig verwijderde object geen momentopname is, heeft u ook de optie voor het ongedaan maken.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Houd er rekening mee dat ongedaan maken van een blob wordt ook verwijdering alle bijbehorende momentopnamen. Als u wilt verwijderen ongedaan maken voorlopig verwijderde momentopnamen voor een actieve blob, klikt u op de blob en selecteer **alle momentopnamen verwijderen ongedaan maken**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Wanneer u een blob momentopnamen verwijderen ongedaan maken, klikt u op **opwaarderen** kopiëren van een momentopname via de hoofdmap blob, waardoor het herstellen van de blob naar de momentopname.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

### <a name="powershell"></a>PowerShell
Bijwerken van de client van een blob-service-eigenschappen zodat voorlopig verwijderen. Het volgende voorbeeld wordt een voorlopig verwijderen voor een subset van accounts in een abonnement:

```powershell
Set-AzureRmContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzureRMStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzureStorageDeleteRetentionPolicy -RetentionDays 7
```

Als u wilt herstellen blobs die per ongeluk zijn verwijderd, verwijderen ongedaan maken voor deze blobs niet aanroepen. Houd er rekening mee dat aanroepen **ongedaan Blob**, zowel op actieve en voorlopig verwijderde blobs, alle gekoppelde voorlopig verwijderde momentopnamen als actief worden hersteld. Verwijderen ongedaan maken in het volgende voorbeeld aanroepen op alle voorlopig verwijderde en actieve blobs in een container:
```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzureStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
### <a name="python-client-library"></a>Python-clientbibliotheek

Bijwerken van de client van een blob-service-eigenschappen zodat voorlopig verwijderen:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(delete_retention_policy = DeleteRetentionPolicy(enabled = True, days = 7))
```

### <a name="net-client-library"></a>.NET-clientbibliotheek

Bijwerken van de client van een blob-service-eigenschappen zodat voorlopig verwijderen:

```csharp
// Get the blob client’s service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client’s service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Als u wilt herstellen blobs die per ongeluk zijn verwijderd, verwijderen ongedaan maken voor deze blobs niet aanroepen.
Houd er rekening mee dat aanroepen **ongedaan Blob**, zowel op actieve en voorlopig verwijderde blobs, alle gekoppelde voorlopig verwijderde momentopnamen als actief worden hersteld. Verwijderen ongedaan maken in het volgende voorbeeld aanroepen op alle voorlopig verwijderde en actieve blobs in een container:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Als u wilt herstellen naar een specifieke blob-versie, eerste aanroep verwijderen ongedaan maken van een blob vervolgens de gewenste momentopname kopiëren via de blob. Het volgende voorbeeld wordt een blok-blob naar de meest recent gegenereerde momentopname hersteld:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```

## <a name="should-i-use-soft-delete"></a>Moet ik voorlopig verwijderen gebruiken?

Als er een kans is dat uw gegevens per ongeluk wordt gewijzigd of verwijderd door een toepassing of een andere gebruiker van de storage-account, raden wij voorlopig verwijderen inschakelt.
Voorlopige verwijderingen maakt deel uit van een strategie voor gegevensbescherming en helpt onbedoeld gegevensverlies voorkomen.

## <a name="faq"></a>Veelgestelde vragen

**Voor welke opslagtypen kan ik voorlopig verwijderen gebruiken?**

Voorlopig verwijderen is momenteel alleen beschikbaar voor blobopslag (object).

**Is voorlopig verwijderen beschikbaar voor alle opslagaccounttypen?**

Ja, voorlopig verwijderen is beschikbaar voor blob storage-accounts ook als blobs in opslagaccounts voor. Dit geldt voor standard en premium-accounts. Voorlopig verwijderen is niet beschikbaar voor beheerde schijven.

**Is voorlopig verwijderen beschikbaar voor alle opslaglagen?**

Ja, voorlopig verwijderen is beschikbaar voor alle opslaglagen, met inbegrip van hot, cool en te archiveren. Echter voorlopig verwijderen niet van bieden bescherming voor blobs in de laag archief overschrijven.

**Premium-opslagaccounts hebben een blob momentopnamelimiet van 100 per. Voorlopig verwijderde momentopnamen meetellen voor deze limiet?**

Nee, voorlopig verwijderd momentopnamen niet meetellen voor deze limiet.

**Kan ik voorlopig verwijderen voor bestaande opslagaccounts inschakelen?**

Ja, voorlopig verwijderen is geconfigureerd voor bestaande en nieuwe storage-accounts.

**Als ik een hele account of een container met voorlopig verwijderen ingeschakeld verwijdert, worden alle bijbehorende blobs worden opgeslagen?**

Nee, als u een hele account of de container verwijdert, alle bijbehorende blobs definitief verwijderd. Als u wilt weten hoe u een opslagaccount beveiligen tegen onopzettelijk verwijderen, Zie het artikel Azure Resource Manager [Resources vergrendelen om te voorkomen dat onverwachte wijzigingen](/azure-resource-manager/resource-group-lock-resources.md).

**Kan ik capaciteitsmetrieken voor verwijderde gegevens bekijken?**

Voorlopig verwijderde gegevens zijn opgenomen als onderdeel van de capaciteit van de totale opslagruimte. Zie voor meer informatie over bewaking opslagcapaciteit en bij te houden, de [Opslaganalyse](../common/storage-analytics.md) artikel.

**Als ik voorlopige verwijderingen uitschakelt, wordt ik nog steeds toegang kunnen krijgen tot voorlopig verwijderde gegevens?**

Ja, nog wel openen en lopende voorlopig verwijderde gegevens herstellen wanneer voorlopig verwijderen is uitgeschakeld.

**Kan ik lezen en voorlopig verwijderde momentopnamen van mijn blob kopiëren?**

Ja, maar u moet eerst verwijderen ongedaan maken op de blob aanroepen.

**Voorlopige verwijderingen beschikbaar is voor alle typen blob?**

Ja, voorlopig verwijderen is beschikbaar voor blok-Blobs, toevoeg-Blobs en pagina-Blobs.

**Is voorlopig verwijderen beschikbaar voor virtuele-machineschijven?**

Voorlopig verwijderen is beschikbaar voor premium en standard niet-beheerde schijven. Voorlopig verwijderen wordt helpen u alleen gegevens die zijn verwijderd door herstellen **Blob verwijderen**, **Blob plaatsen**, **blokkeringslijst plaatsen**, **plaatsen blok** en **Blob kopiëren**. Gegevens overschreven door een aanroep naar **pagina plaatsen** kan niet worden hersteld.

**Moet ik mijn bestaande toepassingen voorlopig verwijderen gebruiken wijzigen?**

Het is mogelijk om te profiteren van de voorlopige verwijderingen ongeacht de API-versie die u gebruikt. Echter, als u wilt weergeven en voorlopig verwijderde blobs en momentopnamen van de blob herstellen, moet u versie 2017-07-29 van de [REST-API voor Storage Services](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) of hoger. In het algemeen altijd wordt aangeraden de nieuwste versie, ongeacht of het gebruik van deze functie.

## <a name="next-steps"></a>Volgende stappen

* [Voorbeeldcode voor .NET](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)

* [Blob Service REST API](/rest/api/storageservices/fileservices/blob-service-rest-api)

* [Azure Storage-replicatie](../common/storage-redundancy.md)

* [Maximaal beschikbare toepassingen met behulp van RA-GRS ontwerpen](../common/storage-designing-ha-apps-with-ragrs.md)

* [Wat te doen als een Azure Storage-storing optreedt](../common/storage-disaster-recovery-guidance.md)
