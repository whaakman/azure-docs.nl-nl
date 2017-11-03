---
title: Azure PowerShell-Script steekproef - berekenen blob-container grootte | Microsoft Docs
description: De grootte van een container in Azure Blob-opslag berekenen door de grootte van elk van de blobs totaal te berekenen.
services: storage
documentationcenter: na
author: fhryo-msft
manager: cbrooks
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: sample
ms.date: 10/23/2017
ms.author: fryu
ms.openlocfilehash: 46f3eac0129da41062caba2da090f9e532505d67
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="calculate-the-size-of-a-blob-storage-container"></a>De grootte van een Blob storage-container berekenen

Dit script wordt de grootte van een container in Azure Blob storage berekend door het totaal te berekenen van de grootte van de blobs in de container.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="understand-the-size-of-blob-storage-container"></a>De grootte van Blob storage-container begrijpen

Totale grootte van Blob storage-container bevat de grootte van de container zelf en de grootte van alle blobs in de container.

Hieronder wordt beschreven hoe de opslagcapaciteit voor Blob-Containers en Blobs wordt berekend. In de onderstaande Len(X): het aantal tekens in de tekenreeks.

### <a name="blob-containers"></a>BLOB-Containers

Hieronder ziet u hoe u schat de hoeveelheid opslagruimte is verbruikt per blob-container:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Hier volgt de uitsplitsing:
* 48 bytes overhead voor elke container omvat het laatst is gewijzigd, machtigingen, instellingen voor openbare en sommige metagegevens van het systeem.
* De containernaam wordt opgeslagen als Unicode-zodat het aantal tekens en vermenigvuldigen met 2.
* Voor elke blobcontainermetagegevens opgeslagen, bewaren wij de lengte van de naam (opgeslagen als ASCII), plus de lengte van de tekenreekswaarde.
* De 512 bytes per ondertekend identificatie bevat ondertekende id-naam, de starttijd, verlooptijd en machtigingen.

### <a name="blobs"></a>Blobs

Hieronder ziet u hoe u schat de hoeveelheid opslagruimte per blob verbruikt:

* Blok-Blob (base blob of momentopname)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
SizeInBytes(data in unique committed data blocks stored) +
SizeInBytes(data in uncommitted data blocks)
`

* Pagina-Blob (base blob of momentopname)

`
124 bytes + Len(BlobName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
number of nonconsecutive page ranges with data * 12 bytes +
SizeInBytes(data in unique pages stored)
`

Hier volgt de uitsplitsing:

* 124 bytes overhead voor blob, waaronder het laatst is gewijzigd, grootte, Cache-Control, Content-Type, Content-Language Content-Encoding, Content-MD5, machtigingen, momentopname informatie, Lease en sommige metagegevens van het systeem.
* De blob-naam wordt opgeslagen als Unicode zodat het aantal tekens en veelvoud van 2.
* Klik voor elke metagegevens opgeslagen, de lengte van de naam (opgeslagen als ASCII), plus de lengte van de tekenreekswaarde.
* Klik voor blok-Blobs
    * 8 bytes voor de lijst met geblokkeerde websites
    * Aantal blokken keer de grootte van het blok-ID in bytes
    * Plus de grootte van de gegevens in alle van de toegewezen en niet-doorgevoerde blokkeert. Opmerking: wanneer momentopnamen worden gebruikt, de grootte van deze alleen bevat de unieke gegevens voor deze blob base of een momentopname. Als de niet-doorgevoerde blokken niet worden gebruikt na een week, garbage collector zijn verzameld worden en klik vervolgens op dat moment ze niet meer meetelt voor facturering daarna.
* Klik voor pagina-Blobs
    * Aantal niet-aaneengesloten paginabereiken met gegevens keren 12 bytes. Dit is het aantal unieke paginabereiken u ziet bij het aanroepen van de API GetPageRanges.
    * Plus de grootte van de gegevens in bytes van alle van de pagina's. Opmerking wanneer momentopnamen worden gebruikt, de grootte van deze omvat alleen unieke pagina's voor de base blob of momentopname blob wordt geteld.

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het Azure Storage facturering [Understanding Windows Azure-opslag facturering](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Voorbeelden van extra opslagruimte PowerShell-script kunnen worden gevonden in [voorbeelden van PowerShell voor Azure Storage](../blobs/storage-samples-blobs-powershell.md).
