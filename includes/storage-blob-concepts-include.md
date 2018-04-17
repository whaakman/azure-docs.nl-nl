---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 203f5a766c4c8a8f1e577f6be1e18d0f9ac95403
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
Azure Blob storage is een oplossing voor de opslag van Microsoft object voor de cloud. BLOB storage is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens.

BLOB-opslag is ideaal voor:

* Het leveren van afbeeldingen of documenten rechtstreeks naar een browser.
* Het opslaan van bestanden voor gedistribueerde toegang.
* Streaming van video en audio.
* Schrijven naar de logboekbestanden.
* Opslag van gegevens voor back-up en herstel, herstel na noodgevallen en archivering.
* Opslaan van gegevens voor analyse door een on-premises of Azure gehoste service.

Objecten in Blob storage kunnen worden geopend vanuit overal ter wereld via HTTP of HTTPS. Gebruikers of clienttoepassingen toegang tot blobs via URL's, de [REST-API van Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage), of een Azure Storage-clientbibliotheek. De clientbibliotheken storage beschikbaar zijn voor meerdere talen, waaronder [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/en/latest/index.html), [PHP](http://azure.github.io/azure-storage-php/), en [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Concepten van Blob service

BLOB storage gebruik van drie bronnen: uw storage-account, de containers in het account en de blobs in een container. Het volgende diagram toont de relatie tussen deze resources.

![Diagram van de opslagarchitectuur Blob (object)](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Opslagaccount

Alle toegang tot gegevensobjecten in Azure Storage gebeurt via een opslagaccount. Zie voor meer informatie [over Azure storage-accounts](../articles/storage/common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="container"></a>Container

Een container ordent een reeks blobs, net als bij een map in een bestandssysteem. Alle blobs zich binnen een container bevinden. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs opslaan. De naam van een container mag alleen kleine letters bevatten.

### <a name="blob"></a>Blob
 
Azure Storage biedt drie typen blobs: blok-blobs, toevoeg-blobs, en [pagina-blobs](../articles/storage/blobs/storage-blob-pageblob-overview.md) (gebruikt voor de VHD-bestanden).

* Blok-blobs opslaan tekst en binaire gegevens, tot ongeveer 4,7 TB. Blok-blobs bestaan uit blokken met gegevens die afzonderlijk kunnen worden beheerd.
* Toevoeg-BLOB's bestaan uit blokken, zoals blok-blobs, maar zijn geoptimaliseerd voor toevoegbewerkingen. Toevoeg-blobs zijn ideaal voor scenario's zoals het registreren van gegevens van virtuele machines.
* Pagina-blobs store willekeurige toegang bestanden tot maximaal 8 TB in grootte. Pagina-blobs opslaan van de VHD-bestanden die back-virtuele machines.

Alle blobs zich binnen een container bevinden. Een container is vergelijkbaar met een map in een bestandssysteem. U kunt meer blobs in de virtuele mappen indelen en ze doorlopen als een bestandssysteem. 

Voor zeer grote gegevenssets waarbij netwerkbeperkingen het downloaden of uploaden van gegevens van of naar Blob Storage via de kabel onrealistisch maken, kunt u een set harde schijven opsturen naar Microsoft om gegevens rechtstreeks in het datacenter te importeren of exporteren. Zie voor meer informatie [gebruik van de Microsoft Azure Import/Export-Service gegevens overdragen naar Blob Storage](../articles/storage/common/storage-import-export-service.md).
  
Zie [Containers, blobs en metagegevens een naam geven en hiernaar verwijderen](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) voor meer informatie over de naamgeving van containers en blobs.
