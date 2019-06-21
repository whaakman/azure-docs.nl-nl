---
title: Blok-blobs Store op apparaten - Azure IoT Edge | Microsoft Docs
description: Informatie over functies voor cloudlagen en time-to-live, ondersteunde blob-opslagbewerkingen zien en verbinding maken met uw blob storage-account.
author: arduppal
manager: mchad
ms.author: arduppal
ms.reviewer: arduppal
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f70ca550f1688551abb94bb30ba4f76eb3c36404
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303969"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>Gegevens aan de rand met Azure Blob Storage Store op IoT Edge (preview)

Azure Blob-opslag op IoT Edge biedt een [blok-blob](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) opslagoplossing aan de rand. Een blob storage-module op uw IoT Edge-apparaat zich gedraagt als een Azure blok-blob-service, maar de blok-blobs worden lokaal opgeslagen op uw IoT Edge-apparaat. U kunt toegang tot uw blobs met behulp van de methodes SDK van Azure storage of blob API-aanroepen die u al hebt gebruikt om te blokkeren.

Deze module wordt geleverd met **deviceToCloudUpload** en **deviceAutoDelete** functies.
> [!NOTE]
> Azure Blob-opslag op IoT Edge is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bekijk de video voor een korte inleiding in
> [!VIDEO https://www.youtube.com/embed/QhCYCvu3tiM]

**deviceToCloudUpload** is een configureerbare functionaliteit, zodat u kunt het automatisch uploaden van de gegevens van uw lokale blob-opslag naar Azure met onregelmatige connectiviteit-ondersteuning voor internet. Hiermee kunt u:

- Schakel aan/uit de functie deviceToCloudUpload.
- De volgorde waarin de gegevens worden gekopieerd naar Azure, zoals NewestFirst of OldestFirst kiezen.
- Geef de Azure Storage-account die u wilt dat uw gegevens die zijn geüpload.
- Geef de containers die u wilt uploaden naar Azure. Deze module kunt u zowel bron- en container-namen opgeven.
- Kies de mogelijkheid om te verwijderen van de blobs onmiddellijk nadat de uploaden naar opslag in de cloud is voltooid
- Blob-upload volledig (met behulp van `Put Blob` bewerking) en niveau uploaden blokkeren (met behulp van `Put Block` en `Put Block List` operations).

In deze module wordt niveau uploaden blokkeren, als uw blob uit blokken bestaat. Hier volgen enkele algemene scenario's:

- Uw toepassing blokken van een eerder geüploade blob updates, deze module alleen de bijgewerkte blokken en niet de hele blob uploadt.
- De module is blob uploaden en verbinding met internet wordt opgeslagen en wanneer de connectiviteit is weer opnieuw dat heeft alleen de resterende blokken en niet de hele blob uploadt.

Als het beëindigen van een onverwachte processen (zoals stroomstoring) tijdens het uploaden van een blob gebeurt, wordt alle blokken die vervallen voor het uploaden zijn geüpload, wanneer de module weer online komt.

**deviceAutoDelete** is een configureerbare functionaliteit wanneer de module wordt automatisch verwijderd uw blobs uit de lokale opslag wanneer de opgegeven duur (gemeten in minuten) is verlopen. Hiermee kunt u:

- Schakel aan/uit de functie deviceAutoDelete.
- Geef de tijd in minuten (deleteAfterMinutes) op waarna die de blobs worden automatisch verwijderd.
- Kies de mogelijkheid om de blob te bewaren terwijl deze wordt geüpload als deleteAfterMinutes-outwaarde is verlopen.

Scenario's waarbij gegevens zoals video's, afbeeldingen, financiële gegevens, ziekenhuis gegevens of gegevens die moeten worden bewaard lokaal, later die kan worden lokaal verwerkt of verzonden naar de cloud zijn goede voorbeelden het gebruik van deze module.

In dit artikel worden de concepten met betrekking tot Azure Blob-opslag op IoT Edge-container die wordt uitgevoerd van een blob-service op uw IoT Edge-apparaat.

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

- U kunt uw ontwikkelcomputer of een virtuele machine als een IoT Edge-apparaat gebruiken met de volgende stappen in de Quick Start voor [Linux](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

- De Azure Blob-opslag op IoT Edge-module biedt ondersteuning voor de volgende configuraties:

  | Besturingssysteem | Architectuur |
  | ---------------- | ----- | ----- |
  | Ubuntu Server 16.04 | AMD64 |
  | Ubuntu-Server 18.04 | AMD64 |
  | Windows 10 IoT Enterprise | AMD64 |
  | Windows Server 2019 | AMD64 |
  | Raspbian stretch | ARM32 |

Cloudresources:

Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.

## <a name="devicetocloudupload-and-deviceautodelete-properties"></a>Eigenschappen deviceToCloudUpload en deviceAutoDelete

Gewenste eigenschappen gebruiken om in te stellen deviceToCloudUploadProperties en deviceAutoDeleteProperties. Ze kunnen worden ingesteld tijdens de implementatie of later worden gewijzigd door het bewerken van de moduledubbel zonder de noodzaak om opnieuw te implementeren. We raden het "Moduledubbel' controleren voor `reported configuration` en `configurationValidation` om ervoor te zorgen waarden correct zijn doorgegeven.

### <a name="devicetoclouduploadproperties"></a>deviceToCloudUploadProperties

De naam van deze instelling is `deviceToCloudUploadProperties`

| Veld | Mogelijke waarden | Uitleg |
| ----- | ----- | ---- |
| uploadOn | true, false | Standaard is ingesteld op `false`, als u wilt inschakelen op instellen op `true`|
| uploadOrder | NewestFirst, OldestFirst | Hiermee kunt u de volgorde waarin de gegevens worden gekopieerd naar Azure kiezen. Standaard is ingesteld op `OldestFirst`. De order wordt bepaald door de tijd voor laatst gewijzigd van Blob |
| cloudStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` een verbindingsreeks waarmee u om op te geven van de Azure Storage-account die u wilt dat uw gegevens geüpload. Geef `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Toevoegen van de juiste EndpointSuffix van Azure waar gegevens worden geüpload, dit varieert voor Global Azure, Azure Government en Microsoft Azure Stack. |
| storageContainersForUpload | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Hiermee kunt u opgeven van de containernamen van de die u wilt uploaden naar Azure. Deze module kunt u zowel bron- en container-namen opgeven. Als u de naam van de container niet opgeeft, wordt deze automatisch de containernaam van de als toegewezen `<IoTHubName>-<IotEdgeDeviceID>-<ModuleName>-<SourceContainerName>`. U kunt maken van sjabloon van tekenreeksen voor de naam van de doel-container, bekijk de kolom mogelijke waarden. <br>* %h -> naam van de IoT-Hub (3-50 tekens). <br>* %d -> IoT Edge-apparaat-ID (1 tot 129 tekens). <br>* %m -> modulenaam (1 tot 64 tekens). <br>* %c -> naam van de Broncontainer (3 tot 63 tekens). <br><br>Maximale grootte van de containernaam van de is 63 tekens, bij het automatisch toewijzen van de naam van de container als de grootte van de container is groter dan 63 tekens lang, die deze wordt trim elke sectie (IoTHubName, IotEdgeDeviceID, modulenaam, SourceContainerName) tot en met 15 tekens. |
| deleteAfterUpload | true, false | Standaard is ingesteld op `false`. Als deze is ingesteld op `true`, er worden gegevens automatisch verwijderd wanneer het uploaden naar opslag in de cloud is voltooid |

### <a name="deviceautodeleteproperties"></a>deviceAutoDeleteProperties

De naam van deze instelling is `deviceAutoDeleteProperties`

| Veld | Mogelijke waarden | Uitleg |
| ----- | ----- | ---- |
| deleteOn | true, false | Standaard is ingesteld op `false`, als u wilt inschakelen op instellen op `true`|
| deleteAfterMinutes | `<minutes>` | Geef de tijd in minuten. De module verwijderen automatisch uw blobs uit de lokale opslag wanneer deze waarde is verlopen |
| retainWhileUploading | true, false | Standaard is ingesteld op `true`, en deze wordt de blob behouden terwijl deze wordt uploaden naar opslag in de cloud als deleteAfterMinutes is verlopen. U kunt dit instellen op `false` en de gegevens worden verwijderd zodra deleteAfterMinutes verloopt. Opmerking: Voor deze eigenschap werkt uploadOn moet worden ingesteld op true|

## <a name="configure-log-files"></a>Logboekbestanden configureren

Zie voor informatie over het configureren van logboekbestanden voor de module deze [aanbevolen procedures voor productie](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Verbinding maken met uw blob storage-module

U kunt de accountnaam en accountsleutel dat u hebt geconfigureerd voor de module voor toegang tot de blob-opslag op uw IoT Edge-apparaat gebruiken.

Geef uw IoT Edge-apparaat als de blobeindpunt voor alle opslag aanvragen die u uitvoert. U kunt [maken van een verbindingsreeks voor een expliciete opslageindpunt](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) met behulp van de gegevens van de IoT Edge-apparaat en de accountnaam op die u hebt geconfigureerd.

- Voor modules die zijn geïmplementeerd op hetzelfde apparaat als waarop de Azure Blob-opslag op IoT Edge-module wordt uitgevoerd, het eindpunt van blob is: `http://<module name>:11002/<account name>`.
- Voor externe modules of toepassingen die op een ander apparaat dan waar de Azure Blob-opslag op IoT Edge-module is uitgevoerd, wordt uw netwerkconfiguratie, al naar gelang worden uitgevoerd, zodat het gegevensverkeer van uw externe module of de toepassing het apparaat kunt bereiken de blob-eindpunt is met de Azure Blob-opslag op IoT Edge-module, een van:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Storage-Quick Start-voorbeelden

De documentatie van Azure Blob Storage bevat voorbeeldcode Quick Start in verschillende talen. U kunt deze voorbeelden als u wilt testen van Azure Blob-opslag op IoT Edge door het veranderen van de blob-eindpunt verbinding maken met uw lokale blob storage-module kunt uitvoeren.

De volgende Quick Start-voorbeelden talen die ook worden ondersteund door de IoT Edge, zodat u ze als IoT Edge-modules samen met de blob storage-module implementeren kunt gebruiken:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Verbinding maken met uw lokale opslag met Azure Storage Explorer

U kunt [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) verbinding maken met uw lokale storage-account.

1. Download en installeer Azure Storage Explorer

1. Verbinding maken met Azure Storage met behulp van een verbindingsreeks

1. Verbindingsreeks opgeven: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Doorloop de stappen om verbinding te maken.

1. Container in uw lokale storage-account maken

1. Beginnen met uploaden van bestanden als blok-blobs.
   > [!NOTE]
   > Deze module biedt geen ondersteuning voor pagina-blobs.

1. U kunt verbinding maken met Azure storage-accounts waarbij u de gegevens wilt uploaden. Het biedt een weergave voor uw account met lokale opslag en de Azure storage-account

## <a name="supported-storage-operations"></a>Ondersteunde opslagbewerkingen

BLOB storage-modules op IoT Edge gebruikt u de dezelfde Azure Storage-SDK's en consistent zijn met de versie van 17-04-2017 van de API van Azure Storage voor blok-blob-eindpunten. Latere releases zijn afhankelijk van de behoeften van klanten.

Omdat niet alle Azure Blob Storage-bewerkingen worden ondersteund door Azure Blob-opslag op IoT Edge, vindt deze sectie u de status van elk.

### <a name="account"></a>Account

Ondersteund:

- Containers weergeven

Niet-ondersteund:

- Ophalen en instellen van blob service-eigenschappen
- Voorbereidende blob-aanvraag
- Blob-servicestatistieken ophalen
- Ophalen van accountgegevens

### <a name="containers"></a>Containers

Ondersteund:

- Maken en verwijderen van container
- Containereigenschappen en metagegevens ophalen
- Blobs vermelden
- Ophalen en instellen van de container ACL
- Metagegevens van de container set

Niet-ondersteund:

- Lease-container

### <a name="blobs"></a>Blobs

Ondersteund:

- Put, ophalen en verwijderen van blob
- Ophalen en instellen van blob-eigenschappen
- Ophalen en instellen van blob-metagegevens

Niet-ondersteund:

- Lease-blob
- Momentopname maken van blob
- Kopieer en kopiëren van de blob afbreken
- Verwijdering van blob ongedaan maken
- Bloblaag instellen

### <a name="block-blobs"></a>Blok-blobs

Ondersteund:

- Blok plaatsen
- En lijst met geblokkeerde websites

Niet-ondersteund:

- Blokkeren van URL plaatsen

## <a name="release-notes"></a>Releaseopmerkingen

Hier volgen de [release-opmerkingen in docker hub](https://hub.docker.com/_/microsoft-azure-blob-storage) voor deze module

## <a name="feedback"></a>Feedback

Uw feedback is belangrijk voor ons om deze module en de bijbehorende functies handig en eenvoudig te gebruiken. Deel uw feedback en laat het ons weten hoe we kunnen verbeteren.

U kunt ons op bereiken absiotfeedback@microsoft.com

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure Blob-opslag implementeren op IoT Edge](how-to-deploy-blob.md)
