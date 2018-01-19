---
title: Werken met momentopnamen van de share (preview) | Microsoft Docs
description: Een momentopname van de share is een alleen-lezen-versie van een Azure-bestanden-share die wordt uitgevoerd op een punt in tijd, als een manier om back-up van de share.
services: storage
documentationcenter: .net
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: renash
ms.openlocfilehash: c4a5f7d28601867c383b8b348568e4bb580a81eb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-share-snapshots-preview"></a>Werken met momentopnamen van de share (preview)
Een momentopname van een share (preview) is een alleen-lezen-versie van een Azure-bestanden-share die wordt uitgevoerd op een punt in tijd. Nadat een momentopname van een share is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. Een momentopname van een share biedt een manier om back-up van de share zoals deze wordt weergegeven op een moment. 

In dit artikel leert u hoe te maken, beheren en delen momentopnamen te verwijderen. Zie voor meer informatie de [overzicht van de momentopname delen](storage-snapshots-files.md) of de [snapshot-Veelgestelde vragen over](storage-files-faq.md).

## <a name="create-a-share-snapshot"></a>De momentopname van een bestandsshare maken

U kunt een momentopname van een share maken met behulp van de Azure-portal, PowerShell, Azure CLI, de REST-API of een Azure-opslag-SDK. De volgende secties wordt beschreven hoe een momentopname van een share maken met behulp van de portal, CLI en PowerShell. 

U kunt een momentopname van het delen van een bestandsshare nemen terwijl deze gebruikt wordt. Share momentopnamen vastleggen echter alleen de gegevens die al is geschreven naar een bestandsshare op het moment dat de opdracht share momentopname is uitgegeven. Dit mogelijk alle gegevens die is opgeslagen in het cachegeheugen door toepassingen of het besturingssysteem uitsluiten.

### <a name="create-a-share-snapshot-by-using-the-portal"></a>Maken van een momentopname van een bestandsshare met behulp van de portal  
Ga voor het maken van een momentopname van de share point-in-time, om het bestand te delen in de portal en selecteer **momentopname maken van een**.

>   ![Menu momentopname in de portal](./media/storage-snapshots-create/portal-create-snapshot.png)


### <a name="create-a-share-snapshot-by-using-azure-cli-20"></a>Maken van een momentopname van een bestandsshare met behulp van Azure CLI 2.0
U kunt een momentopname van een share maken met behulp van de `az storage share snapshot` opdracht:

```azurecli-interactive
az storage share snapshot -n <share name>
```

Voorbeelduitvoer:
```json
{
  "metadata": {},
  "name": "<share name>",
  "properties": {
    "etag": "\"0x8D50B7F9A8D7F30\"",
    "lastModified": "2017-10-04T23:28:22+00:00",
    "quota": null
  },
  "snapshot": "2017-10-04T23:28:35.0000000Z"
}
```

### <a name="create-a-share-snapshot-by-using-powershell"></a>Maken van een momentopname van een bestandsshare met behulp van PowerShell
U kunt een momentopname van een share maken met behulp van de `$share.Snapshot()` opdracht:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

```

## <a name="perform-common-share-snapshot-operations"></a>Algemene bestandsshare momentopname bewerkingen uitvoeren

U kunt opsommen en de share momentopnamen die zijn gekoppeld aan de bestandsshare met behulp van de **vorige versies** tabblad in Windows, via REST, de clientbibliotheek, PowerShell en de portal. Nadat de bestandsshare is gekoppeld, kunt u alle vorige versies van het bestand weergeven met behulp van de **vorige versies** tabblad in Windows. 

De volgende secties wordt beschreven hoe u de Azure-portal, Windows en Azure CLI 2.0 weergeven door te bladeren en terugzetten van momentopnamen van de share.

### <a name="share-snapshot-operations-in-the-portal"></a>Delen van de momentopname in de portal

U kunt zoeken op alle momentopnamen van de share voor een bestand delen in de portal en blader naar een momentopname van een bestandsshare om de inhoud ervan weer te geven.

#### <a name="view-a-share-snapshot"></a>Een momentopname van een share bekijken
Op de bestandsshare onder **momentopname**, selecteer **momentopnamen weergeven**.

![De opdracht 'Momentopnamen bekijken' in de portal](./media/storage-snapshots-list-browse/snapshot-view-portal.png)

#### <a name="list-and-browse-to-share-snapshot-content"></a>Lijst en blader naar de inhoud van de momentopname delen
De lijst weergeven met momentopnamen van de share en blader vervolgens naar de inhoud van een momentopname rechtstreeks door de gewenste tijdstempel selecteren.

![Geselecteerde momentopname in de lijst met tijdstempels](./media/storage-snapshots-list-browse/snapshot-browsefiles-portal.png)

U kunt ook selecteren de **Connect** bekijken zodat u een momentopname van knop op uw lijst met de `net use` opdracht en het pad naar een momentopname van een bepaalde bestandsshare. U kunt vervolgens rechtstreeks bladeren naar het die momentopname.


![Deelvenster verbinding met de opdracht vak](./media/storage-snapshots-list-browse/snapshot-connect-portal.png)

#### <a name="download-or-restore-from-a-share-snapshot"></a>Downloaden of herstel van een momentopname van een share
Vanuit de portal downloaden of een bestand terugzetten vanuit een momentopname met de **downloaden** of **herstellen** knop.

![Download- en Restore knoppen](./media/storage-snapshots-list-browse/snapshot-download-restore-portal.png)

### <a name="share-snapshot-operations-in-windows"></a>Delen van de momentopname in Windows
Als u momentopnamen van de share van de bestandsshare al hebt gemaakt, bekijkt u eerdere versies van een share, map of een bepaald bestand van de gekoppelde bestandsshare in Windows. Als u bijvoorbeeld is hier hoe u kunt de functie vorige versies weergeven en herstellen van een eerdere versie van een map in Windows.

> [!Note]  
> U kunt dezelfde bewerkingen op het niveau van de share- en bestandsniveau uitvoeren. Alleen de versie met wijzigingen voor dit bestand of map wordt weergegeven in de lijst. Als een bepaalde map of bestand niet is gewijzigd tussen twee share momentopnamen, wordt in de lijst voor eerdere versie van share-niveau, maar niet in de vorige Versielijst van de map of bestand van de momentopname van de share weergegeven.

#### <a name="mount-a-file-share"></a>Een bestandsshare koppelen
Eerst de bestandsshare koppelen met behulp van de `net use` opdracht.

#### <a name="open-a-mounted-share-in-file-explorer"></a>Een gekoppelde share in Windows Verkenner openen
Ga naar de Windows Verkenner en de gekoppelde share vinden.

![Gekoppelde share in Windows Verkenner](./media/storage-snapshots-list-browse/snapshot-windows-mount.png)

#### <a name="list-previous-versions"></a>Lijst met eerdere versies
Blader naar het item of het bovenliggende item dat moet worden hersteld. Dubbelklik om te gaan naar de gewenste map. Met de rechtermuisknop en selecteer **eigenschappen** in het menu.

![Met de rechtermuisknop op het menu voor een geselecteerde map](./media/storage-snapshots-list-browse/snapshot-windows-previous-versions.png)

Selecteer **vorige versies** voor een overzicht van momentopnamen van de share voor deze map. De lijst kan worden geladen, afhankelijk van de netwerksnelheid en het aantal momentopnamen van de share in de map een paar seconden duren.

![Tabblad Vorige versies](./media/storage-snapshots-list-browse/snapshot-windows-list.png)

U kunt selecteren **openen** openen van een momentopname van een bepaald. 

![Momentopname geopend](./media/storage-snapshots-list-browse/snapshot-browse-windows.png)

#### <a name="restore-from-a-previous-version"></a>Herstellen van een eerdere versie
Selecteer **herstellen** om de inhoud van de gehele map recursief op de aanmaaktijd van de share momentopname kopiëren naar de oorspronkelijke locatie.
 ![Knop in waarschuwingsbericht herstellen](./media/storage-snapshots-list-browse/snapshot-windows-restore.png)

### <a name="share-snapshot-operations-in-azure-cli-20"></a>Delen van de momentopname in de Azure CLI 2.0
U kunt Azure CLI 2.0 bewerkingen uit te voeren zoals aanbieding share momentopnamen Bladeren als momentopname inhoud wilt delen, herstellen of downloaden van bestanden van momentopnamen van de share of het verwijderen van bestandsshare momentopnamen.

#### <a name="list-share-snapshots"></a>Lijst share momentopnamen

U kunt momentopnamen van een bepaalde share weergeven met behulp van [ `az storage share list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list) met `--include-snapshots`:

```azurecli-interactive 
az storage share list --include-snapshots
```

De opdracht geeft u een lijst van momentopnamen van de share, samen met de bijbehorende eigenschappen. De volgende uitvoer is een voorbeeld:

```json
[
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:44:13.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": "2017-10-04T19:45:18.0000000Z"
  },
  {
    "metadata": null,
    "name": "sharesnapshotdefs",
    "properties": {
      "etag": "\"0x8D50B5F4005C975\"",
      "lastModified": "2017-10-04T19:36:46+00:00",
      "quota": 5120
    },
    "snapshot": null
  }
]
```

#### <a name="browse-to-a-share-snapshot"></a>Blader naar een momentopname van een share
U kunt bladeren naar een momentopname van een bepaalde bestandsshare en de inhoud ervan weergeven met behulp van [ `az storage file list` ](/cli/azure/storage/share?view=azure-cli-latest#az_storage_share_list). Geef de sharenaam van de en de tijdstempel die u bladeren, wilt zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive 
az storage file list --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z' -otable
```

In de uitvoer ziet u dat de inhoud van de momentopname van de share identiek aan de inhoud is van de share op het punt in tijd share momentopname is gemaakt:

```
Name            Content Length    Type    Last Modified
--------------  ----------------  ------  ---------------
HelloWorldDir/                    dir
IMG_0966.JPG    533568            file
IMG_1105.JPG    717711            file
IMG_1341.JPG    608459            file
IMG_1405.JPG    652156            file
IMG_1611.JPG    442671            file
IMG_1634.JPG    1495999           file
IMG_1635.JPG    974058            file

```
#### <a name="restore-from-a-share-snapshot"></a>Herstellen vanuit een momentopname van de share

U kunt een bestand herstellen door te kopiëren of downloaden via de momentopname van de share. Gebruik de `az storage file download` opdracht, zoals wordt weergegeven in het volgende voorbeeld:

```azurecli-interactive 
az storage file download --path IMG_0966.JPG --share-name sharesnapshotdefs --snapshot '2017-10-04T19:45:18.0000000Z'
```

In de uitvoer ziet u dat de inhoud van het gedownloade bestand en de bijbehorende eigenschappen identiek aan de inhoud zijn en eigenschappen op het punt in tijd die delen van momentopname is gemaakt:

```json
{
  "content": null,
  "metadata": {},
  "name": "IMG_0966.JPG",
  "properties": {
    "contentLength": 533568,
    "contentRange": "bytes 0-533567/533568",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentType": "application/octet-stream"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D50B5F49F7ACDF\"",
    "lastModified": "2017-10-04T19:37:03+00:00",
    "serverEncrypted": true
  }
}
```

<<<<<<< HEAD
### <a name="file-share-snapshot-operations-in-azure-powershell"></a>Bewerkingen op momentopname van de bestandsshare in Azure PowerShell
U kunt Azure Powershell gebruiken om uit te voeren dezelfde bewerkingen zoals aanbieding share momentopnamen, bladeren door de inhoud van de momentopname delen, herstellen of share momentopnamen downloaden van bestanden vanuit een momentopname van de share.

#### <a name="list-share-snapshots"></a>Lijst share momentopnamen

Je kunt aanbieden share momentopnamen van het gebruik van een bepaalde bestandsshare`Get-AzureStorageShare`

```powershell
Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
```

#### <a name="browse-share-snapshots"></a>Momentopnamen van de share bladeren
U kunt ook bekijken in een bepaalde bestandsshare om weer te geven van de inhoud met behulp van een momentopname `Get-AzureStorageFile` met de waarde van `-Share` die verwijst naar de specifieke momentopname

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
Get-AzureStorageFile -Share $snapshot
```

#### <a name="restore-from-share-snapshots"></a>Terugzetten van momentopnamen van de share

U kunt een bestand herstellen door te kopiëren of downloaden van een bestand van de share die momentopname over met `Get-AzureStorageFileContent` opdracht

```powershell
$download='C:\Temp\Download'
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $download
```

```powershell
$snapshot = Get-AzureStorageShare -Name "ContosoShare06" -SnapshotTime "6/16/2017 9:48:41 AM +00:00"
$directory = Get-AzureStorageFile -ShareName "ContosoShare06" -Path "ContosoWorkingFolder" | Get-AzureStorageFile
Get-AzureStorageFileContent -Share $snapshot -Path $file -Destination $directory
```


## <a name="delete-azure-files-share-snapshot"></a>Azure Files share momentopname verwijderen
=======
## <a name="delete-a-share-snapshot"></a>Een momentopname delen verwijderen
>>>>>>> 6a1833e10031fbf1ab204bb1f30cb54cf5fbcada

U kunt momentopnamen van de share verwijderen met behulp van de Azure-portal, PowerShell, CLI, de REST-API of een opslag-SDK. De volgende secties wordt beschreven hoe share momentopnamen verwijderen met behulp van de Azure-portal, CLI en PowerShell.

U kunt bladeren om te delen momentopnamen en verschillen tussen de twee momentopnamen met elk hulpprogramma vergelijking weergeven. Vervolgens kunt u bepalen welke share momentopname die u wilt verwijderen. 

U kunt een share met een share niet verwijderen momentopname. Als u de share te verwijderen, moet u eerst alle momentopnamen van de share te verwijderen.

### <a name="delete-a-share-snapshot-by-using-the-portal"></a>Verwijderen van een momentopname van een share met behulp van de portal  
In de portal kunt u gaat u naar de blade van het bestand delen en gebruiken de **verwijderen** knop een of meer share momentopnamen wilt verwijderen.

>   ![Knop verwijderen](./media/storage-snapshots-delete/portal-snapshots-delete.png)


### <a name="delete-a-share-snapshot-by-using-azure-cli-20"></a>Verwijderen van een momentopname van een bestandsshare met behulp van Azure CLI 2.0
U kunt een momentopname van een share verwijderen met behulp van de `[az storage share delete]` opdracht. Gebruik de tijdstempel van de share-momentopname voor de `--snapshot '2017-10-04T23:28:35.0000000Z' ` parameter in het volgende voorbeeld:

```azurecli-interactive
az storage share delete -n <share name> --snapshot '2017-10-04T23:28:35.0000000Z' 
```

Voorbeelduitvoer:
```json
{
  "deleted": true
}
```

### <a name="delete-a-share-snapshot-by-using-powershell"></a>Verwijderen van een momentopname van een bestandsshare met behulp van PowerShell
U kunt een momentopname van een share verwijderen met behulp van de `Remove-AzureStorageShare -Share` opdracht:

```powershell
$connectionstring="DefaultEndpointsProtocol=http;FileEndpoint=http:<Storage Account Name>.file.core.windows.net /;AccountName=:<Storage Account Name>;AccountKey=:<Storage Account Key>"
$sharename=":<file share name>"

$ctx = New-AzureStorageContext -ConnectionString $connectionstring

##Create snapshot
$share=Get-AzureStorageShare -Context $ctx -Name <file share name>
$share.Properties.LastModified
$share.IsSnapshot
$snapshot=$share.Snapshot()

##Delete snapshot
Remove-AzureStorageShare -Share $snapshot

```

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van de momentopname](storage-snapshots-files.md)
* [Veelgestelde vragen over momentopname](storage-files-faq.md)
