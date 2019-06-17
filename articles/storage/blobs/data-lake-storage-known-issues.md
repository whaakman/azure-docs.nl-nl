---
title: Bekende problemen met Azure Data Lake Storage Gen2 | Microsoft Docs
description: Meer informatie over de beperkingen en bekende problemen met Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: normesta
ms.openlocfilehash: 446b49cbf3fdf3d4cde37b2a7c4ac2d9f0a811b1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67061330"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Bekende problemen met Azure Data Lake Storage Gen2

In dit artikel bevat de functies en hulpprogramma's die nog niet ondersteund of slechts gedeeltelijk ondersteund voor opslagaccounts waarvoor een hiërarchische naamruimte (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>BLOB storage-API 's

BLOB storage-API's zijn uitgeschakeld om te voorkomen dat de functie ervan problemen die kunnen ontstaan omdat Blob Storage-API's zijn nog niet compatibel met Azure Data Lake Gen2 API's.

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Wat te doen met bestaande hulpprogramma's, toepassingen en services

Als een van deze gebruik van Blob-API's, en u wilt gebruiken voor het werken met alle inhoud die u naar uw account uploadt, klikt u vervolgens niet inschakelt een hiërarchische naamruimte op uw Blob storage-account tot de Blob-API's worden gebruikt in combinatie met Azure Data Lake Gen2 API's.

Met behulp van een storage-account zonder een hiërarchische naamruimte betekent dat u geen toegang tot specifieke functies van Data Lake Storage Gen2, zoals map- en systeemtoegangscontrolelijsten vervolgens hebt.

### <a name="what-to-do-with-unmanaged-virtual-machine-vm-disks"></a>Wat te doen met niet-beheerde schijven van de virtuele Machine (VM)

Dit is afhankelijk van het uitgeschakelde Blob Storage-API's, dus als u wilt inschakelen, een hiërarchische naamruimte op een storage-account kunt u overwegen ze worden geplaatst in een storage-account dat beschikt niet over de hiërarchische naamruimte functie is ingeschakeld.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Wat te doen als u Blob-API's gebruikt voor het laden van gegevens voor Blob-API's zijn uitgeschakeld

Als u deze API's gebruikt om gegevens te laden voordat ze zijn uitgeschakeld en u een productie-vereiste hebt voor toegang tot die gegevens, klikt u vervolgens Neem contact op met Microsoft Support met de volgende informatie:

> [!div class="checklist"]
> * Abonnements-ID (de GUID, niet de naam).
> * Namen van Storage-account.
> * Of u actief in productie wordt beïnvloed, en zo ja, voor welke storage-accounts?
> * Zelfs als u niet actief beïnvloed in productie, laat ons weten of moet u deze gegevens moeten worden gekopieerd naar een ander opslagaccount om een bepaalde reden, en indien dit het geval is, waarom?

In de volgende situaties, kunnen we herstellen toegang tot de Blob-API voor een beperkte periode zodat u deze gegevens kunt kopiëren naar een opslagaccount dat u beschikt niet over de hiërarchische naamruimte functie is ingeschakeld.

## <a name="all-other-features-and-tools"></a>Alle andere functies en hulpprogramma 's

De volgende tabel geeft een lijst van alle andere functies en hulpprogramma's die nog niet ondersteund of slechts gedeeltelijk ondersteund voor opslagaccounts waarvoor een hiërarchische naamruimte (Azure Data Lake Storage Gen2).

| Functie / hulpprogramma    | Meer informatie    |
|--------|-----------|
| **API's voor Data Lake Storage Gen2 storage-accounts** | Gedeeltelijk ondersteund <br><br>U kunt Data Lake Storage Gen2 **REST** API's, maar in andere Blob-SDK's, zoals de SDK's voor .NET, Java, Python-API's zijn nog niet beschikbaar.|
| **AzCopy** | Ondersteuning voor specifieke versies <br><br>Alleen de meest recente versie van AzCopy gebruiken ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). Eerdere versies van AzCopy zoals AzCopy v8.1, worden niet ondersteund.|
| **Azure Blob storage Lifecycle management-beleid** | Nog niet ondersteund |
| **Azure Content Delivery Network (CDN)** | Nog niet ondersteund|
| **Azure Event Grid** | Nog niet ondersteund |
| **Azure search** |Nog niet ondersteund|
| **Azure-opslagverkenner** | Ondersteuning voor specifieke versies <br><br>Gebruik alleen versie `1.6.0` of hoger. <br>Versie `1.6.0` is beschikbaar als een [gratis download](https://azure.microsoft.com/features/storage-explorer/).|
| **BLOB-container ACL 's** |Nog niet ondersteund|
| **Blobfuse** |Nog niet ondersteund|
| **Aangepaste domeinen** |Nog niet ondersteund|
| **Diagnostische logboeken** |Nog niet ondersteund|
| **File System Explorer** | Beperkte ondersteuning |
| **Onveranderbare opslag** |Nog niet ondersteund <br><br>Onveranderbare-opslag biedt de mogelijkheid voor het opslaan van gegevens in een [WORM (Write Once, Read Many)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) staat.|
| **Op objectniveau lagen** |Nog niet ondersteund <br><br>Bijvoorbeeld: Premium-, warm, koud en archief lagen.|
| **Ondersteuning voor PowerShell en CLI** | Beperkte functionaliteit <br><br>U kunt een account maken met behulp van Powershell of CLI. U kan geen bewerkingen uitvoeren of instellen van toegangsbeheerlijsten voor bestandssystemen, mappen en bestanden.|
| **Statische websites** |Nog niet ondersteund <br><br>Met name de mogelijkheid om bestanden naar [statische websites](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Toepassingen van derden** | Beperkte ondersteuning <br><br>Toepassingen van derden die REST-API's gebruiken om te werken blijft werken als u deze met Data Lake Storage Gen2 gebruiken. <br>Als u een toepassing die gebruikmaakt van de Blob-API's hebt, wordt die toepassing zeer waarschijnlijk problemen hebben als u deze toepassing met Data Lake Storage Gen2 gebruiken. Zie voor meer informatie, de [Blob storage-API's zijn uitgeschakeld voor Data Lake Storage Gen2 storage-accounts](#blob-apis-disabled) sectie van dit artikel.|
| **Versiebeheer functies** |Nog niet ondersteund <br><br>Dit omvat [momentopnamen](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) en [voorlopig verwijderen](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).|
|

