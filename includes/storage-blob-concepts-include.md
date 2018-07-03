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
ms.openlocfilehash: 63593ff5f02f5e37fc25c988c4cef071a03a00b4
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37066030"
---
Azure Blob Storage is Microsoft's oplossing voor opslag van objecten in de cloud. Blob Storage is geoptimaliseerd voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens.

Blob-opslag is ideaal voor:

* Het rechtstreeks aan een browser leveren van afbeeldingen of documenten.
* De opslag van bestanden voor gedistribueerde toegang.
* Streaming van video en audio.
* Schrijven naar logboekbestanden.
* De opslag van gegevens voor back-up en herstel, herstel na noodgevallen en archivering.
* De opslag van gegevens voor analyse door een on-premises of in Azure gehoste service.

Objecten in Blob-opslag zijn overal ter wereld toegankelijk via HTTP of HTTPS. Gebruikers of clienttoepassingen hebben toegang tot blobs via URL's, de [REST-API van Azure Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) of een Azure Storage-clientbibliotheek. Deze clientbibliotheken zijn beschikbaar voor meerdere talen, waaronder [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](http://azure.github.io/azure-storage-node), [Python](https://docs.microsoft.com/python/azure/), [PHP](http://azure.github.io/azure-storage-php/) en [Ruby](http://azure.github.io/azure-storage-ruby).

## <a name="blob-service-concepts"></a>Concepten van Blob service

Bij Blob-opslag worden drie resources beschikbaar gemaakt: uw opslagaccount, de containers in het account en de blobs in een container. Het volgende diagram geeft de relatie tussen deze resources weer.

![Diagram van de Blob-(object)opslagarchitectuur](./media/storage-blob-concepts-include/blob1.png)

### <a name="storage-account"></a>Opslagaccount

Alle gegevensobjecten in Azure Storage zijn toegankelijk via een opslagaccount. Zie [Over Azure-opslagaccounts](../articles/storage/common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie.

### <a name="container"></a>Container

Een container kan een of meer blobs bevatten, net zoals een map een of meer bestanden kan bevatten in een bestandssysteem. Alle blobs bevinden zich binnen een container. Een opslagaccount kan een onbeperkt aantal containers bevatten en een container kan een onbeperkt aantal blobs bevatten. De naam van een container mag alleen kleine letters bevatten.

### <a name="blob"></a>Blob
 
Er zijn drie typen blobs in Azure Storage: blok-blobs, toevoeg-blobs en [pagina-blobs](../articles/storage/blobs/storage-blob-pageblob-overview.md) (gebruikt voor VHD-schijven).

* Blok-blobs worden gebruikt voor het opslaan van tekst- en binaire gegevens, tot ongeveer 4,7 TB. Blok-blobs bestaan uit blokken met gegevens die afzonderlijk kunnen worden beheerd.
* Toevoeg-blobs bestaan uit blokken zoals blok-blobs, maar zijn geoptimaliseerd voor toevoegbewerkingen. Toevoeg-blobs zijn ideaal voor scenario's zoals het registreren van logboekgegevens van virtuele machines.
* Pagina-blobs worden gebruikt voor het opslaan van bestanden voor willekeurige toegang tot maximaal 8 TB in grootte. Pagina-blobs worden gebruikt voor het opslaan van de VHD-bestanden die VM's ondersteunen.

Alle blobs bevinden zich binnen een container. Een container is vergelijkbaar met een map in een bestandssysteem. U kunt meer blobs in virtuele mappen indelen en deze doorlopen zoals in een bestandssysteem. 

Voor zeer grote gegevenssets waarbij netwerkbeperkingen het downloaden of uploaden van gegevens van of naar Blob Storage via de kabel onrealistisch maken, kunt u een set harde schijven opsturen naar Microsoft om gegevens rechtstreeks in het datacenter te importeren of exporteren. Zie [De Microsoft Azure Import/Export-service gebruiken om gegevens over te brengen naar Blob Storage](../articles/storage/common/storage-import-export-service.md) voor meer informatie.
  
Zie [Containers, blobs en metagegevens een naam geven en hiernaar verwijderen](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) voor meer informatie over de naamgeving van containers en blobs.
