---
title: Het maken en gebruiken van resourcebestanden - Azure Batch | Microsoft Docs
description: Informatie over het maken van Azure Batch-resource-bestanden van verschillende invoerbronnen.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 03/14/2019
ms.author: lahugh
ms.openlocfilehash: f6693de861658c7174614e80906ee83633fcb313
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58501089"
---
# <a name="creating-and-using-resource-files"></a>Het maken en gebruiken van bestanden

Een Azure Batch-taak moet vaak een vorm van gegevens te verwerken. Bronbestanden zijn de middelen om deze gegevens naar de Batch virtuele machine (VM) via een taak. Alle soorten taken resource ondersteuningsbestanden: taken, taken, jobvoorbereidingstaken, jobvrijgevingstaken, enzovoort worden gestart. In dit artikel bevat informatie over een aantal algemene methoden voor het maken van bestanden en plaats deze op een virtuele machine.  

Bronbestanden zijn een mechanisme voor het plaatsen van gegevens naar een virtuele machine in Batch, maar het type gegevens en hoe deze wordt gebruikt is flexibel. Er zijn echter enkele algemene scenario's:

1. Algemene bestanden op elke virtuele machine met behulp van resourcebestanden op een begintaak inrichten
1. Invoergegevens moeten worden verwerkt door taken inrichten

Algemene bestanden mogelijk bijvoorbeeld bestanden op een begintaak gebruikt voor het installeren van toepassingen die de taken worden uitgevoerd. Invoergegevens kan worden onbewerkte afbeelding of video gegevens of gegevens moeten worden verwerkt door de Batch.

## <a name="types-of-resource-files"></a>Typen bestanden

Er zijn een aantal verschillende opties beschikbaar voor het genereren van bestanden. Het proces voor het maken van bestanden is afhankelijk van waar de oorspronkelijke gegevens worden opgeslagen.

Opties voor het maken van een bronbestand:

- [De URL van de opslag-container](#storage-container-url): Hiermee genereert u een bronbestand van een storage-container in Azure
- [Naam van de opslagcontainer](#storage-container-name): Genereert een bronbestand van de naam van een container in Azure storage-account gekoppeld aan Batch
- [Webeindpunt](#web-endpoint): Hiermee genereert u een bronbestand van elke geldige HTTP-URL

### <a name="storage-container-url"></a>De URL van de opslag-container

Met behulp van een storage-container-URL, betekent dat u toegang hebt tot bestanden in een storage-container in Azure. Met de juiste machtigingen

In deze C# bijvoorbeeld de bestanden al zijn geüpload naar een Azure storage-container als blob-opslag. Voor toegang tot de gegevens die nodig zijn om een resourcebestand te maken, moeten we eerst toegang krijgen tot de storage-container.

Een shared access signature (SAS) URI met de juiste machtigingen voor toegang tot de storage-container maken. Stel de verlooptijd en machtigingen voor de SAS. In dit geval is niet begintijd opgegeven, zodat de SAS geldig onmiddellijk wordt en verloopt van twee uur nadat deze gegenereerd.

```csharp
SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
{
    SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
    Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List
};
```

> [!NOTE]
> Voor toegang tot de container, moet u hebben beide `Read` en `List` machtigingen, terwijl met toegang tot blob, u alleen hoeft `Read` machtiging.

Zodra de machtigingen zijn geconfigureerd, maak het SAS-token en de SAS-URL voor toegang tot de storage-container. Met behulp van de geformatteerde SAS-URL voor de opslagcontainer genereren van een resourcebestand met [ `FromStorageContainerUrl` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.resourcefile.fromstoragecontainerurl?view=azure-dotnet).

```csharp
CloudBlobContainer container = blobClient.GetContainerReference(containerName);

string sasToken = container.GetSharedAccessSignature(sasConstraints);
string containerSasUrl = String.Format("{0}{1}", container.Uri, sasToken);

ResourceFile inputFile = ResourceFile.FromStorageContainerUrl(containerSasUrl);
```

Een alternatief voor het genereren van een SAS-URL is het inschakelen van anonieme, openbare leestoegang tot een container en de blobs in Azure Blob-opslag. Op deze manier kunt u alleen-lezen toegang tot deze resources verlenen zonder het delen van uw accountsleutel en zonder een SAS. Openbare leestoegang wordt doorgaans gebruikt voor scenario's waar u bepaalde blobs moet altijd beschikbaar voor anonieme toegang voor lezen. Als dit scenario past bij uw oplossing, raadpleegt u de [anonieme toegang tot blobs](../storage/blobs/storage-manage-access-to-resources.md) artikel voor meer informatie over het beheren van toegang tot uw blob-gegevens.

### <a name="storage-container-name"></a>Naam van de opslagcontainer

In plaats van te configureren en het maken van een SAS-URL, kunt u de naam van uw Azure storage-container gebruiken voor toegang tot uw blob-gegevens. De opslagcontainer moet in de Azure storage-account dat gekoppeld aan uw Batch-account, bekend als het account autostorage gebruikt. De containernaam van de opslag van een account autostorage, kunt u als volgt te omzeilen, configureren en het maken van een SAS-URL voor toegang tot een opslagcontainer.

In dit voorbeeld veronderstellen we dat de gegevens moet worden gebruikt voor het maken van resource al in een Azure Storage-account dat is gekoppeld aan uw Batch-account is. Als u geen account autostorage hebt, raadpleegt u de stappen in [maken van een Batch-account](/create-a-batch-account.md) voor meer informatie over het maken en een account koppelen.

Met behulp van een gekoppelde storage-account, moet u niet maken en configureren van een SAS-URL naar een opslagcontainer. Geef in plaats daarvan de naam van de storage-container in uw gekoppelde storage-account.

```csharp
ResourceFile inputFile = ResourceFile.FromAutoStorageContainer(containerName);
```

### <a name="web-endpoint"></a>Webeindpunt

Gegevens die niet is geüpload naar Azure Storage kan nog steeds worden gebruikt om resourcebestanden te maken. U kunt elke geldige HTTP-URL die de invoergegevens bevat. De URL is dat de Batch-API en vervolgens de gegevens worden gebruikt om een resourcebestand te maken.

In de volgende C# bijvoorbeeld: de ingevoerde gegevens op fictieve GitHub-eindpunt wordt gehost. De API worden opgehaald van het bestand van het webeindpunt geldig en genereert u een resourcebestand om te worden verbruikt door de taak. Er zijn geen referenties zijn nodig voor dit scenario.

```csharp
ResourceFile inputFile = ResourceFile.FromUrl("https://github.com/foo/file.txt", filePath);
```

## <a name="tips-and-suggestions"></a>Tips en suggesties

Elke Azure Batch-taak maakt gebruik van bestanden anders, daarom Batch heeft de opties die beschikbaar zijn voor het beheren van bestanden op taken. De volgende scenario's zijn niet bedoeld om te worden uitgebreid, maar in plaats daarvan betrekking hebben op een paar veelvoorkomende situaties en aanbevelingen.

### <a name="many-resource-files"></a>Veel bestanden

Uw Batch-taak bevat mogelijk meerdere taken dat alle bestanden die dezelfde, algemene gebruiken. Als algemene taakbestanden worden gedeeld door veel taken, met behulp van een toepassingspakket voor de bestanden in plaats van bestanden mogelijk een betere optie. Toepassingspakketten bieden optimalisatie voor downloadsnelheid. Ook gegevens in de als toepassingspakketten in de cache tussen taken, zodat als de taakbestanden niet vaak wijzigt, toepassingspakketten mogelijk geschikt voor uw oplossing. Met toepassingspakketten kunt wilt u niet handmatig beheren van meerdere bestanden of genereren van SAS-URL's voor toegang tot de bestanden in Azure Storage. Batch is bijzonder op de achtergrond met Azure Storage voor het opslaan en implementeren van toepassingspakketten rekenknooppunten.

Als elke taak veel bestanden die uniek is voor die taak heeft, worden bestanden voornamelijk waarschijnlijk de beste optie. Taken die gebruikmaken van unieke bestanden vaak moeten worden bijgewerkt of vervangen, dit is geen net zo gemakkelijk te maken met de inhoud van de toepassing-pakketten. Bronbestanden voorzien in aanvullende flexibiliteit voor het bijwerken, toevoegen of bewerken van afzonderlijke bestanden.

### <a name="number-of-resource-files-per-task"></a>Aantal bestanden per taak

Als er enkele honderden bronbestanden voor een taak wordt opgegeven zijn, kan Batch de taak weigeren voor deze te groot. Het is raadzaam klein te houden de taken door het minimaliseren van het aantal bestanden op de taak zelf.

Als er is geen manier om te beperken het aantal bestanden van de taak moeten, kunt u de taak optimaliseren door een enkele resource-bestand die verwijst naar een opslagcontainer van bronbestanden te maken. U doet dit door uw bestanden in een Azure Storage-container geplaatst en de verschillende modi 'Container' van de resourcebestanden gebruiken. De blob-voorvoegsel-opties gebruiken om op te geven van verzamelingen van bestanden worden gedownload voor uw taken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [toepassingspakketten](batch-application-packages.md) als alternatief voor bestanden.
- Zie voor meer informatie over het gebruik van containers voor bronbestanden [containerwerkbelastingen](batch-docker-container-workloads.md).
- Zie voor meer informatie over het verzamelen en de uitvoergegevens opslaat van uw taken, [opdracht- en taakuitvoer behouden](batch-task-output.md).
- Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.