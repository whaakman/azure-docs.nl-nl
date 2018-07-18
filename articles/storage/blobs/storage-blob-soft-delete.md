---
title: Voorlopig verwijderen voor Azure Storage-blobs (Preview) | Microsoft Docs
description: Azure Storage biedt nu voorlopig verwijderen (Preview) voor blob-objecten, zodat u uw gegevens eenvoudig herstellen kunt wanneer deze per ongeluk wordt gewijzigd of verwijderd door een toepassing of de gebruiker van andere storage-account.
services: storage
author: MichaelHauss
manager: vamshik
ms.service: storage
ms.topic: article
ms.date: 07/15/2018
ms.author: mihauss
ms.openlocfilehash: 408e2167e60cbdfa2b4eee136bf3ac4321ae8121
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39091728"
---
# <a name="soft-delete-for-azure-storage-blobs"></a>Voorlopig verwijderen voor Azure Storage-blobs
Azure Storage biedt nu voorlopig verwijderen voor blob-objecten, zodat u uw gegevens eenvoudig herstellen kunt wanneer deze per ongeluk wordt gewijzigd of verwijderd door een toepassing of de gebruiker van andere storage-account.

## <a name="how-does-it-work"></a>Hoe werkt het?
Wanneer ingeschakeld, kunt u opslaan en uw gegevens herstellen wanneer blobs of blobschermopnamen zijn verwijderd met voorlopig verwijderen. Deze beveiliging heeft ook betrekking op blobgegevens die wordt gewist als gevolg van een overschrijven.

Wanneer gegevens worden verwijderd, verandert deze in een voorlopig verwijderde status in plaats van permanent worden gewist. Wanneer voorlopig verwijderen ingeschakeld is en u kunt gegevens overschrijven, wordt een voorlopig verwijderde momentopname is gegenereerd om op te slaan de status van de gegevens overschreven. Voorlopig verwijderde objecten zijn onzichtbaar tenzij uitdrukkelijk vermeld. U kunt de hoeveelheid tijd voorlopig verwijderde gegevens kunnen hersteld worden voordat het permanent is verlopen.

Voorlopig verwijderen is achterwaarts compatibel zijn. u hebt geen wijzigingen aanbrengen in uw toepassingen om te profiteren van deze functie biedt bescherming. Echter, [gegevensherstel](#recovery) introduceert een nieuwe **verwijdering Blob** API.

### <a name="configuration-settings"></a>Configuratie-instellingen
Wanneer u een nieuw account maakt, is voorlopig verwijderen standaard uitgeschakeld. Voorlopig verwijderen is ook standaard uitgeschakeld voor bestaande storage-accounts. U kunt de functie in of uit kunt schakelen op elk gewenst moment gedurende de levensduur van een storage-account.

U wordt nog steeds openen en herstellen van voorlopig verwijderde gegevens wanneer de functie is uitgeschakeld, ervan uitgaande dat voorlopig verwijderde gegevens is opgeslagen wanneer de functie eerder is ingeschakeld. Wanneer u voorlopig verwijderen inschakelt, moet u ook het configureren van de bewaarperiode.

De bewaarperiode geeft de hoeveelheid tijd die voorlopig verwijderde gegevens opgeslagen en beschikbaar voor herstel worden. De bewaarperiode klok voor blobs en blob-momentopnamen die expliciet zijn verwijderd, wordt gestart wanneer de gegevens worden verwijderd. Voor het voorlopig verwijderde momentopnamen die worden gegenereerd door de functie voor voorlopig verwijderen wanneer gegevens worden overschreven, wordt de klok gestart wanneer de momentopname is gegenereerd. Op dit moment kunt u voorlopig verwijderde gegevens voor tussen 1 en 365 dagen bewaren.

U kunt de functie voor voorlopig verwijderen bewaarperiode op elk gewenst moment wijzigen. Een bijgewerkte bewaarperiode wordt alleen toegepast op onlangs verwijderde gegevens. Eerder verloopt verwijderde gegevens op basis van de bewaarperiode liggen die is geconfigureerd wanneer deze gegevens is verwijderd. Bij het verwijderen van een voorlopig verwijderde object heeft geen invloed op de vervaltijd.

### <a name="saving-deleted-data"></a>Bezig met opslaan van de verwijderde gegevens
Voorlopig verwijderen behoudt uw gegevens in veel gevallen waarbij blobs of blobschermopnamen zijn verwijderd of overschreven.

Wanneer een blob wordt overschreven met behulp van **Put Blob**, **Put-blokken**, **blokkeringslijst plaatsen** of **kopiëren van de Blob** een momentopname van de status van de blob voorafgaand aan de schrijven bewerking automatisch wordt gegenereerd. Deze momentopname is een momentopname van een voorlopig verwijderde; het is onzichtbaar, tenzij expliciet voorlopig verwijderde objecten worden genoemd. Zie de [Recovery](#recovery) sectie voor meer informatie over om voorlopig verwijderde objecten weer te geven.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-overwrite.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens worden weergegeven onder de oudere gegevens. Wanneer B0 wordt overschreven met B1, wordt een momentopname van een voorlopig verwijderde van B0 gegenereerd. Wanneer B1 wordt overschreven met B2, wordt een momentopname van een voorlopig verwijderde van B1 gegenereerd.*

> [!NOTE]  
> Voorlopig verwijderen alleen biedt beveiliging voor kopieerbewerkingen wordt overschreven wanneer deze is ingeschakeld voor de bestemmings-blob-account.

> [!NOTE]  
> Voorlopig verwijderen is niet zomaar beveiliging voor blobs in de archieflaag overschrijven. Als een blob in archief met een nieuwe blob in elke laag wordt overschreven, wordt de overschreven blob permanent is verlopen.

Wanneer **Blob verwijderen** wordt aangeroepen op een momentopname die momentopname is gemarkeerd als zachte verwijderd. Een nieuwe momentopname is niet gegenereerd.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-delete-snapshot.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens worden weergegeven onder de oudere gegevens. Wanneer **momentopname maken van Blob** B0 aangeroepen, wordt een momentopname en B1 is de actieve status van de blob. Als de momentopname B0 wordt verwijderd, wordt deze gemarkeerd als zachte verwijderd.*

Wanneer **Blob verwijderen** wordt aangeroepen voor een basis-blob (een blob-dat wil zeggen niet zelf een momentopname), die blob is gemarkeerd als zachte verwijderd. Consistent zijn met het gedrag van het vorige, aanroepen van **Blob verwijderen** voor een blob die active-momentopnamen wordt een fout geretourneerd. Aanroepen van **Blob verwijderen** voor een blob met voorlopig verwijderde momentopnamen geen een fout geretourneerd. U kunt nog steeds een blob en alle bijbehorende momentopnamen in één bewerking verwijderen wanneer de functie voor voorlopig verwijderen is ingeschakeld. In dat geval wordt de basis blob gemarkeerd en momentopnamen als zachte verwijderd.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-explicit-include.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens worden weergegeven onder de oudere gegevens. Hier een **Blob verwijderen** wordt een aanroep uitgevoerd om B2 en alle bijbehorende momentopnamen te verwijderen. De actieve blob, B2 en alle bijbehorende momentopnamen zijn gemarkeerd als zachte verwijderd.*

> [!NOTE]  
> Wanneer een voorlopig verwijderde blob wordt overschreven, wordt een voorlopig verwijderde momentopname van de status van de blob voordat de schrijfbewerking wordt automatisch gegenereerd. De nieuwe blob neemt over van de laag van de overschreven blob.

Voorlopig verwijderen niet de gegevens opslaan in geval van een container of account verwijderd of wanneer blob-metagegevens en blob-eigenschappen worden overschreven. Als u wilt een storage-account beveiligen tegen onjuiste verwijderen, kunt u een vergrendeling met de Azure Resource Manager configureren. Zie het artikel Azure Resource Manager [Resources vergrendelen om onverwachte wijzigingen te voorkomen dat te](../../azure-resource-manager/resource-group-lock-resources.md) voor meer informatie.

De volgende tabel worden verwacht gedrag wanneer voorlopig verwijderen is ingeschakeld:

| REST-API-bewerking | Resourcetype | Beschrijving | Wijziging in gedrag |
|--------------------|---------------|-------------|--------------------|
| [Verwijderen](/rest/api/storagerp/StorageAccounts/Delete) | Account | Hiermee verwijdert u het opslagaccount, met inbegrip van alle containers en blobs die deze bevat.                           | Er is geen wijziging. Containers en blobs in de verwijderde account zijn niet hersteld. |
| [Container verwijderen](/rest/api/storageservices/delete-container) | Container | Hiermee verwijdert u de container, inclusief alle blobs die deze bevat. | Er is geen wijziging. BLOBs in de verwijderde container zijn niet hersteld. |
| [Blob plaatsen](/rest/api/storageservices/put-blob) | Blok toevoegen en pagina-Blobs | Hiermee maakt u een nieuwe blob of vervangen van een bestaande blob in een container | Als dit wordt gebruikt ter vervanging van een bestaande blob, wordt een momentopname van de status van de blob voordat de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als deze is vervangen door een blob van hetzelfde type (blok, toevoegen of pagina). Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens definitief verlopen. |
| [Blob verwijderen](/rest/api/storageservices/delete-blob) | Blok toevoegen en pagina-Blobs | Een blob of een blob-momentopname voor verwijdering worden gemarkeerd. De blob of een momentopname wordt later verwijderd tijdens de garbagecollection | Als u gebruikt voor het wordt een blob-momentopname die momentopname is gemarkeerd als voorlopig verwijderen verwijderd. Als u gebruikt voor het verwijderen van een blob, die blob is gemarkeerd als zachte verwijderd. |
| [Blob kopiëren](/rest/api/storageservices/copy-blob) | Blok toevoegen en pagina-Blobs | Een bron-blob kopieert naar een bestemmings-blob in hetzelfde opslagaccount of in een ander opslagaccount. | Als dit wordt gebruikt ter vervanging van een bestaande blob, wordt een momentopname van de status van de blob voordat de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als deze is vervangen door een blob van hetzelfde type (blok, toevoegen of pagina). Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens definitief verlopen. |
| [Blok plaatsen](/rest/api/storageservices/put-block) | Blok-blobs | Hiermee maakt u een nieuw blok om te worden doorgevoerd als onderdeel van een blok-blob. | Er is geen wijziging als gebruikt voor het doorvoeren van een blok naar een blob die actief is. Als dit wordt gebruikt om door te voeren van een blok naar een blob die is voorlopig verwijderde, een nieuwe blob wordt gemaakt en een momentopname is automatisch gegenereerd om vast te leggen de status van de voorlopig verwijderde blob. |
| [Lijst met geblokkeerde websites plaatsen](/rest/api/storageservices/put-block-list) | Blok-blobs | Hiermee wordt een blob doorgevoerd door de set blok-id's die deel uitmaken van de blok-blob op te geven. | Als dit wordt gebruikt ter vervanging van een bestaande blob, wordt een momentopname van de status van de blob voordat de aanroep wordt automatisch gegenereerd. Dit geldt ook voor een eerder voorlopig verwijderde blob en alleen als het is een blok-Blob. Als deze is vervangen door een blob van een ander type, worden alle bestaande voorlopig verwijderde gegevens definitief verlopen. |
| [Pagina plaatsen](/rest/api/storageservices/put-page) | Pagina-blobs | Schrijft een scala aan pagina's naar een pagina-Blob. | Er is geen wijziging. Pagina-Blob-gegevens die worden overschreven in- of uitgeschakeld met deze bewerking wordt niet opgeslagen en kan niet worden hersteld. |
| [Blok toevoegen](/rest/api/storageservices/append-block) | Toevoeg-Blobs | Een blok gegevens schrijft naar het einde van een toevoeg-Blob | Er is geen wijziging. |
| [Blobeigenschappen instellen](/rest/api/storageservices/set-blob-properties) | Blok toevoegen en pagina-Blobs | Hiermee stelt u waarden voor de eigenschappen gedefinieerd voor een blob. | Er is geen wijziging. Overschreven blob-eigenschappen kunnen niet worden hersteld. |
| [Instellen van de metagegevens van de Blob](/rest/api/storageservices/set-blob-metadata) | Blok toevoegen en pagina-Blobs | Sets gebruiker gedefinieerde metagegevens voor de opgegeven blob als een of meer naam / waarde-paren. | Er is geen wijziging. Metagegevens van de overschreven blob kan niet worden hersteld. |

Het is belangrijk dat u ziet dat aanroepen van "Page plaatsen" om te overschrijven of wissen van de bereiken van een pagina-Blob wordt niet automatisch momentopnamen genereren. Schijven van virtuele machines worden ondersteund door de pagina-Blobs en gebruik **pagina plaatsen** om gegevens te schrijven.

### <a name="recovery"></a>Herstel
Als u wilt maken het gemakkelijker om verwijderde gegevens te herstellen, hebben we een nieuwe 'Blob ongedaan maken'-API geïntroduceerd. Aanroepen van de API ongedaan maken voor een voorlopig verwijderde base blob te herstellen en alle bijbehorende voorlopig verwijderde momentopnamen als actief. Aanroepen van de API ongedaan maken op een actieve base blob Hiermee herstelt u alle voorlopig verwijderde momentopnamen als actief worden gekoppeld. Wanneer momentopnamen worden hersteld als actieve, eruit ze door gebruikers gegenereerde momentopnamen; de base blob ze niet worden overschreven.

Om terug te zetten van een blob naar een specifieke voorlopig verwijderde momentopname kunt u bellen **verwijdering Blob** op de basis-blob. Kopieer vervolgens de momentopname van de BLOB nu actief. U kunt ook de momentopname van het kopiëren naar een nieuwe blob.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-recover.png)

*Voorlopig verwijderde gegevens is grijs, terwijl de actieve gegevens blauw is. Meer onlangs geschreven gegevens worden weergegeven onder de oudere gegevens. Hier **verwijdering Blob** wordt aangeroepen voor blob B, waardoor herstellen van de basis-blob, B1, en alle bijbehorende momentopnamen, hier alleen B0, als actief. In de tweede stap B0 de base blob gekopieerd. Deze kopieerbewerking genereert een voorlopig verwijderde momentopname van B1.*

Voorlopig verwijderde blobs weergeven en blob-momentopnamen, kunt u kiezen om op te nemen van verwijderde gegevens in **Blobs weergeven**. U kunt kiezen om alleen voorlopig verwijderde base blobs weer te geven of om op te nemen ook voorlopig verwijderde blob-momentopnamen. Voor alle voorlopig verwijderde gegevens, kunt u het tijdstip waarop de gegevens is verwijderd en het aantal dagen voordat de gegevens definitief verloopt weergeven.

### <a name="example"></a>Voorbeeld
Hier volgt de uitvoer van de console van een .NET-script dat wordt geüpload, worden overschreven, momentopnamen, verwijderingen en Hiermee herstelt u een blob met de naam "Hallowereld" wanneer voorlopig verwijderen is ingeschakeld:

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

Zie de [Vervolgstappen](#next-steps) sectie voor een verwijzing naar de toepassing die deze uitvoer heeft geproduceerd.

## <a name="pricing-and-billing"></a>Prijzen en facturering
Alle voorlopig verwijderde gegevens gefactureerd tegen hetzelfde tarief als actieve gegevens. U, wordt niet in rekening gebracht voor gegevens die wordt definitief verwijderd na de geconfigureerde bewaarperiode. Zie voor meer informatie over momentopnamen en hoe deze kosten toenemen, [inzicht in hoe de kosten in rekening gebracht voor het samenvoegen van momentopnamen](storage-blob-snapshots.md).

U, wordt niet in rekening gebracht voor de transacties die betrekking hebben op de automatische generatie van momentopnamen. U wordt gefactureerd voor **verwijdering Blob** transacties tegen het tarief "Schrijfbewerkingen".

Bekijk voor meer informatie over prijzen voor Azure Blob Storage in het algemeen de [Azure Blob Storage pagina met prijzen van](https://azure.microsoft.com/pricing/details/storage/blobs/).

Wanneer u in eerste instantie voorlopig verwijderen inschakelt, wordt u aangeraden een kleine bewaarperiode om beter te begrijpen hoe de functie invloed is op uw factuur.

## <a name="quickstart"></a>Snelstartgids
### <a name="azure-portal"></a>Azure Portal
Om in te schakelen voorlopig verwijderen, gaat u naar de **voorlopig verwijderen** onder de optie **Blob-Service**. Klik vervolgens op **ingeschakeld** en voer het aantal dagen dat u wilt om voorlopig verwijderde gegevens te behouden.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-configuration.png)

Als u wilt voorlopig verwijderde blobs weergeven, selecteert u de **verwijderde blobs weergeven** selectievakje.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted.png)

Als u wilt weergeven van voorlopig verwijderde momentopnamen voor een bepaalde blob, selecteert u de blob en klik vervolgens op **momentopnamen weergeven**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Zorg ervoor dat de **verwijderde momentopnamen weergeven** selectievakje is ingeschakeld.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Wanneer u op een voorlopig verwijderde blob of een momentopname klikt, ziet u de nieuwe blobeigenschappen. Ze geven aan wanneer het object is verwijderd en het aantal dagen tot de blob of de blob-momentopname permanent is verlopen. Als het voorlopig verwijderde object niet een momentopname is, heeft u ook de optie voor het ongedaan maken.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-properties.png)

Houd er rekening mee dat ongedaan maken van een blob wordt ook verwijdering alle bijbehorende momentopnamen. Voor het voorlopig verwijderde momentopnamen voor een actieve blob verwijdering, klikt u op de blob en selecteert u **ongedaan maken van alle momentopnamen**.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Nadat u de verwijdering ongedaan van een blob-momentopnamen maken, klikt u op **opwaarderen** om te kopiëren van een momentopname van een het root-BLOB, waardoor de blob herstellen naar de momentopname.

![](media/storage-blob-soft-delete/storage-blob-soft-delete-portal-promote-snapshot.png)

### <a name="powershell"></a>PowerShell
Om in te schakelen voorlopig verwijderen, bijwerken van de blobclient van een service-eigenschappen. Het volgende voorbeeld wordt de functie voor voorlopig verwijderen voor een subset van accounts in een abonnement:

```powershell
Set-AzureRmContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzureRMStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzureStorageDeleteRetentionPolicy -RetentionDays 7
```
U kunt controleren of dat deze functie voor voorlopig verwijderen is ingeschakeld met behulp van de volgende opdracht uit:

```powershell
$MatchingAccounts | Get-AzureStorageServiceProperty -ServiceType Blob
```

Als u wilt herstellen blobs die per ongeluk zijn verwijderd, verwijderen ongedaan maken van de blobs niet aanroepen. Houd er rekening mee dat aanroepen **verwijdering Blob**, zowel voor actieve en voorlopig verwijderde blobs herstellen door alle gekoppelde voorlopig verwijderde momentopnamen als actief. Verwijderen ongedaan maken in het volgende voorbeeld wordt op alle voorlopig verwijderde en actieve blobs in een container:
```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzureStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Als u zoekt het bewaarbeleid van currrent voorlopig verwijderen, gebruik de volgende opdracht:

```azurepowershell-interactive
   $account = Get-AzureRmStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzureStorageServiceProperty -ServiceType Blob -Context $account.Context
```

### <a name="azure-cli"></a>Azure-CLI 
Om in te schakelen voorlopig verwijderen, bijwerken van de blobclient van een service-eigenschappen:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Om te controleren of voorlopig verwijderen is ingeschakeld, gebruikt u de volgende opdracht uit: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

### <a name="python-client-library"></a>Python-clientbibliotheek
Om in te schakelen voorlopig verwijderen, bijwerken van de blobclient van een service-eigenschappen:

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
Om in te schakelen voorlopig verwijderen, bijwerken van de blobclient van een service-eigenschappen:

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Als u wilt herstellen blobs die per ongeluk zijn verwijderd, verwijderen ongedaan maken van de blobs niet aanroepen. Houd er rekening mee dat aanroepen **verwijdering Blob**, zowel voor actieve en voorlopig verwijderde blobs herstellen door alle gekoppelde voorlopig verwijderde momentopnamen als actief. Verwijderen ongedaan maken in het volgende voorbeeld wordt op alle voorlopig verwijderde en actieve blobs in een container:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Als u wilt herstellen naar een specifieke blob-versie, roept u eerst verwijderen ongedaan maken voor een blob en vervolgens de gewenste momentopname kopiëren via de blob. Het volgende voorbeeld wordt een blok-blob met de momentopname van de meest recent gegenereerde hersteld:

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
Als er een kans is dat uw gegevens per ongeluk wordt gewijzigd of verwijderd door een toepassing of een andere gebruiker van de storage-account, raden wij inschakelen van voorlopig verwijderen. Voorlopig verwijderen maakt deel uit van een strategie voor gegevensbescherming en onbedoeld gegevensverlies kunt voorkomen.

## <a name="faq"></a>Veelgestelde vragen
**Voor welke typen gegevensopslag kan ik voorlopig verwijderen gebruiken?**  
Voorlopig verwijderen is momenteel alleen beschikbaar voor blobopslag (object).

**Voorlopig verwijderen voor alle storage-accounttypen beschikbaar is?**  
Ja, voorlopig verwijderen is beschikbaar voor blob storage-accounts en als die voor de blobs in algemene opslagaccounts (GPv1 en GPv2). Dit geldt voor zowel standard als premium-accounts. Voorlopig verwijderen is niet beschikbaar voor beheerde schijven.

**Voorlopig verwijderen beschikbaar is voor alle storage-lagen?**  
Ja, de functie voor voorlopig verwijderen is beschikbaar voor alle opslaglagen, met inbegrip van hot, cool en archive. Echter voorlopig verwijderen niet wordt bieden beveiliging voor blobs in de archieflaag overschrijven.

**Kan ik de API-Blob-laag ingesteld naar blobs met voorlopig verwijderde momentopnamen tier gebruiken?**  
Ja. Het voorlopig verwijderde momentopnamen in de oorspronkelijke verblijven, maar de base blob wordt verplaatst naar de nieuwe laag. 

**Premium storage-accounts hebben een limiet van de blob-momentopname van 100 per. Voorlopig verwijderde momentopnamen tellen mee voor deze limiet?**  
Nee, voorlopig verwijderde momentopnamen niet meetellen voor deze limiet.

**Kan ik voorlopig verwijderen voor bestaande storage-accounts inschakelen?**  
Ja, is de functie voor voorlopig verwijderen worden geconfigureerd voor bestaande en nieuwe storage-accounts.

**Als ik een gehele account of de container met voorlopig verwijderen zijn ingeschakeld verwijderen, worden alle bijbehorende blobs worden opgeslagen?**  
Nee, als u een volledige-account of de container verwijdert, alle bijbehorende blobs definitief verwijderd. Als u wilt weten hoe u een storage-account beschermen tegen onopzettelijk verwijderen, Zie het artikel Azure Resource Manager [Resources vergrendelen om onverwachte wijzigingen te voorkomen dat te](../../azure-resource-manager/resource-group-lock-resources.md).

**Kan ik metrische gegevens over capaciteit voor de verwijderde gegevens bekijken?**  
Voorlopig verwijderde gegevens zijn opgenomen als onderdeel van de capaciteit van de totale opslag. Zie voor meer informatie over het bijhouden en de controle van opslagcapaciteit, de [Opslaganalyse](../common/storage-analytics.md) artikel.

**Als ik voorlopig verwijderen uitschakelt, wordt ik nog steeds toegang kunnen krijgen tot voorlopig verwijderde gegevens?**  
Ja, nog steeds mogelijk om te openen en niet-verlopen voorlopig verwijderde gegevens herstellen wanneer voorlopig verwijderen is uitgeschakeld.

**Kan ik lezen en kopiëren van voorlopig verwijderde momentopnamen van mijn blob?**  
Ja, maar u moet eerst verwijderen ongedaan maken in de blob aanroepen.

**Is voorlopig verwijderen beschikbaar voor alle typen blob?**  
Ja, de functie voor voorlopig verwijderen is beschikbaar voor blok-Blobs, toevoeg-Blobs en pagina-Blobs.

**Voorlopig verwijderen voor virtuele-machineschijven beschikbaar is?**  
Voorlopig verwijderen is beschikbaar voor niet-beheerde schijven voor premium en standard. Voorlopig verwijderen kunnen alleen gegevens die zijn verwijderd door te herstellen **Blob verwijderen**, **Put Blob**, **blokkeringslijst plaatsen**, **Put-blokken** en **Blob kopiëren**. Gegevens worden overschreven door een aanroep van **pagina plaatsen** kan niet worden hersteld.

**Moet ik mijn bestaande toepassingen wijzigen om te functie voor voorlopig verwijderen gebruiken?**  
Het is mogelijk om te profiteren van de functie voor voorlopig verwijderen, ongeacht de API-versie die u gebruikt. Echter, als u wilt weergeven en herstellen van voorlopig verwijderde blobs en blob-momentopnamen, moet u versie 2017-07-29 van de [REST-API voor Storage Services](https://docs.microsoft.com/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) of hoger. In het algemeen wordt het altijd beter om de nieuwste versie, ongeacht of u van deze functie gebruikmaakt.

## <a name="next-steps"></a>Volgende stappen
* [.NET-voorbeeldcode](https://github.com/Azure-Samples/storage-dotnet-blob-soft-delete)
* [REST-API voor BLOB-Service](/rest/api/storageservices/blob-service-rest-api)
* [Azure Storage-replicatie](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Maximaal beschikbare toepassingen met RA-GRS ontwerpen](../common/storage-designing-ha-apps-with-ragrs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Wat te doen als een Azure Storage-storing](../common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
