---
title: Azure PowerShell-script steekproef - berekenen van de totale facturering grootte van een blob-container | Microsoft Docs
description: De totale grootte van een container in Azure Blob-opslag voor factureringsdoeleinden berekenen.
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
ms.date: 11/07/2017
ms.author: fryu
ms.openlocfilehash: c37b416578a76e9b12e29d68e413d851796ccc6f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="calculate-the-total-billing-size-of-a-blob-container"></a>Berekenen van de totale facturering grootte van een blob-container

Dit script wordt de grootte van een container in Azure Blob storage omwille van de schatting van facturering kosten berekend. Het script het totaal van de grootte van de blobs in de container.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

> [!NOTE]
> Deze PowerShell-script wordt de grootte van een container voor factureringsdoeleinden berekend. Als u de grootte van de container voor andere doeleinden berekent, Zie [berekenen van de totale grootte van een Blob storage-container](../scripts/storage-blobs-container-calculate-size-powershell.md) voor een eenvoudigere script dat een schatting geeft.

## <a name="determine-the-size-of-the-blob-container"></a>De grootte van de blob-container bepalen

De totale grootte van de blob-container bevat de grootte van de container zelf en de grootte van alle blobs in de container.

De volgende secties wordt beschreven hoe de opslagcapaciteit voor blob-containers en blobs wordt berekend. In de volgende sectie: Len(X) het aantal tekens in de tekenreeks.

### <a name="blob-containers"></a>BLOB-containers

De volgende berekening wordt beschreven hoe schatting maken van de hoeveelheid opslagruimte die per blob-container wordt gebruikt:

`
48 bytes + Len(ContainerName) * 2 bytes +
For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
For-Each Signed Identifier[512 bytes]
`

Hieronder vindt u de verdeling:
* 48 bytes overhead voor elke container omvat het laatst is gewijzigd, machtigingen, instellingen voor openbare en sommige metagegevens van het systeem.

* De containernaam is opgeslagen als Unicode, dus het aantal tekens en vermenigvuldigen met twee.

* Voor elk blok van metagegevens van de blob-container die opgeslagen, zo bewaren wij de lengte van de naam (ASCII), plus de lengte van de tekenreekswaarde.

* De 512 bytes per ondertekend identificatie bevat ondertekende id-naam, begintijd, verlooptijd en machtigingen.

### <a name="blobs"></a>Blobs

De volgende berekeningen laten zien hoe de hoeveelheid opslagruimte is verbruikt per blob schatten.

* Blok-blob (base blob of momentopname):

   `
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   8 bytes + number of committed and uncommitted blocks * Block ID Size in bytes +
   SizeInBytes(data in unique committed data blocks stored) +
   SizeInBytes(data in uncommitted data blocks)
   `

* Pagina-blob (base blob of momentopname):

   `
   124 bytes + Len(BlobName) * 2 bytes +
   For-Each Metadata[3 bytes + Len(MetadataName) + Len(Value)] +
   number of nonconsecutive page ranges with data * 12 bytes +
   SizeInBytes(data in unique pages stored)
   `

Hieronder vindt u de verdeling:

* 124 bytes overhead voor blob, waaronder:
    - Laatst gewijzigd
    - Grootte
    - Cache-Control
    - Inhoudstype
    - Inhoud taal
    - Codering van inhoud
    - Content-MD5
    - Machtigingen
    - Informatie over
    - Lease
    - Sommige metagegevens van het systeem

* De blob-naam wordt opgeslagen als Unicode, dus het aantal tekens en vermenigvuldigen met twee.

* Voor elk blok van metagegevens die zijn opgeslagen, voegt u de lengte van de naam (opgeslagen als ASCII), plus de lengte van de tekenreekswaarde.

* Voor de blok-blobs:
    * 8 bytes voor de lijst met geblokkeerde websites.
    * Aantal blokken keer de grootte van het blok-ID in bytes.
    * De grootte van de gegevens in alle van de toegewezen en niet-doorgevoerde blokkeert. 
    
    >[!NOTE]
    >Wanneer momentopnamen worden gebruikt, bevat deze grootte alleen de unieke gegevens voor deze blob base of een momentopname. Als de niet-doorgevoerde blokken niet worden gebruikt na een week, zijn deze garbage collector zijn verzameld. Daarna meetellen niet ze voor facturering.

* Pagina-blobs:
    * Het aantal niet-aaneengesloten paginabereiken met gegevens keren 12 bytes. Dit is het aantal unieke paginabereiken u ziet bij het aanroepen van de **GetPageRanges** API.

    * De grootte van de gegevens in bytes van alle pagina's opgeslagen. 
    
    >[!NOTE]
    >Wanneer momentopnamen worden gebruikt, bevat deze grootte alleen de unieke's voor de base blob of de momentopname blob die wordt geteld.

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/storage/calculate-container-size/calculate-container-size-ex.ps1 "Calculate container size")]

## <a name="next-steps"></a>Volgende stappen

- Zie [berekenen van de totale grootte van een Blob storage-container](../scripts/storage-blobs-container-calculate-size-powershell.md) voor een eenvoudige script waarmee een schatting van de grootte van de container.

- Zie voor meer informatie over Azure Storage facturering [Understanding Windows Azure-opslag facturering](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/07/08/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity/).

- Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.4.1).

- U vindt aanvullende voorbeelden van opslag PowerShell script in [voorbeelden van PowerShell voor Azure Storage](../blobs/storage-samples-blobs-powershell.md).
