---
title: Azure storage gebruiken voor SQL Server back-up en herstellen | Microsoft Docs
description: Leer hoe u back-up van SQL Server naar Azure Storage. Verklaart de voordelen van het back-ups van SQL-databases naar Azure Storage.
services: virtual-machines-windows
documentationcenter: ''
author: MikeRayMSFT
manager: craigg
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: 0fdf768008161fbb72e48dae937a4172222dbb63
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239743"
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Azure Storage gebruiken voor SQL Server-back-up en herstel
## <a name="overview"></a>Overzicht
Beginnen met SQL Server 2012 SP1 CU2, kunt u back-ups van SQL Server schrijven rechtstreeks naar de Azure Blob storage-service. U kunt deze functionaliteit gebruiken tot maken en terugzetten van de Azure Blob-service met een on-premises SQL Server-database of SQL Server-database in een virtuele machine van Azure. Back-up naar de cloud biedt de voordelen van de beschikbaarheid, onbeperkte externe opslagruimte voor geo-replicatie en gemakkelijk van de migratie van gegevens naar en vanuit de cloud. U kunt BACKUP of RESTORE-instructies met behulp van Transact-SQL of SMO uitgeven.

SQL Server 2016 introduceert nieuwe mogelijkheden; u kunt [bestand momentopnameback-up](https://msdn.microsoft.com/library/mt169363.aspx) nagenoeg onmiddellijke back-ups en zeer snelle herstelbewerkingen uitvoeren.

In dit onderwerp wordt uitgelegd waarom u kiezen voor Azure-opslag voor back-ups van SQL en vervolgens wordt de betrokken onderdelen beschreven. U kunt de resources die aan het einde van het artikel gebruiken voor toegang tot scenario's en aanvullende informatie om te beginnen met het gebruik van deze service met de back-ups van uw SQL Server.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Voordelen van het gebruik van de Azure Blob-Service voor back-ups van SQL Server
Er zijn verschillende uitdagingen die u regelmatig tegenkomen bij het back-ups van SQL Server. Deze uitdagingen zijn onder andere opslagbeheer, risico's van de fout bij de opslag, toegang tot externe opslag- en hardware-configuratie. Veel van deze uitdagingen vallen met behulp van de Azure Blob store-service voor SQL Server back-ups. Houd rekening met de volgende voordelen:

* **Gebruiksgemak**: opslaan van uw back-ups in Azure-blobs is een handige, flexibel en eenvoudig toegang krijgen tot externe optie. Het maken van externe opslagruimte voor uw SQL Server back-ups net zo gemakkelijk worden kunnen als het wijzigen van uw bestaande scripts/jobs gebruiken de **back-up naar URL** syntaxis. Externe opslag moet meestal toe vanaf de locatie van de productie-database om te voorkomen dat een enkele ramp die mogelijk van invloed zijn op zowel de externe en productie-database-locaties. Door te [geo-replicatie voor uw Azure blobs](../../../storage/common/storage-redundancy.md), hebt u een extra beschermingslaag in het geval van een ramp die invloed kan zijn op de hele regio.
* **Back-archief**: de Azure Blob Storage-service biedt een betere alternatief voor de vaak gebruikte tapeoptie voor het archiveren van back-ups. Opslag op tape mogelijk fysieke verzending naar een gebouw van een externe en metingen om de media te beveiligen. Uw back-ups opslaan in Azure Blob Storage biedt een momentopname, maximaal beschikbare, en een duurzame archiveren optie.
* **Beheerde hardware**: Er is geen extra overhead van hardwarebeheer met Azure-services. Azure-services beheren van de hardware en geo-replicatie voor redundantie en bescherming tegen hardwarestoringen.
* **Onbeperkte opslag**: door een directe back-ups naar Azure-blobs is ingeschakeld, hebt u toegang tot vrijwel onbeperkte opslag. U kunt ook heeft tot een back-up een virtuele machine van Azure-schijf grenzen op basis van machinegrootte. Er is een limiet aan het aantal schijven die u aan een virtuele machine van Azure voor back-ups koppelen kunt. Deze limiet is 16 schijven voor een extra grote instantie en minder voor kleinere exemplaren.
* **Back-up van beschikbaarheid**: back-ups die zijn opgeslagen in Azure-blobs zijn beschikbaar vanaf elke locatie en op elk gewenst moment en kunnen eenvoudig worden geopend voor herstelbewerkingen naar een on-premises SQL-Server of een andere SQL Server in een Azure-machine, zonder de noodzaak voor database koppelen/loskoppelen of downloaden en de VHD te koppelen.
* **Kosten**: betaal alleen voor de service die wordt gebruikt. Kan worden rendabele als een optie voor externe en back-archief. Zie de [prijscalculator van Azure](https://go.microsoft.com/fwlink/?LinkId=277060 "Prijscalculator"), en de [artikel Azure-prijzen](https://go.microsoft.com/fwlink/?LinkId=277059 "prijzen artikel") voor meer informatie.
* **Storage-momentopnamen**: als de databasebestanden worden opgeslagen in een Azure-blob en u met behulp van SQL Server 2016, kunt u [bestand momentopnameback-up](https://msdn.microsoft.com/library/mt169363.aspx) nagenoeg onmiddellijke back-ups en zeer snelle herstelbewerkingen uitvoeren.

Zie voor meer informatie, [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://go.microsoft.com/fwlink/?LinkId=271617).

De volgende twee secties introduceren de Azure Blob storage-service, met inbegrip van de vereiste SQL Server-onderdelen. Het is belangrijk om te begrijpen van de onderdelen en hun interactie is met back-up en herstellen vanaf de Azure Blob storage-service.

## <a name="azure-blob-storage-service-components"></a>De onderdelen van een Azure Blob Storage-Service
De volgende Azure-onderdelen worden gebruikt wanneer de back-up op de Azure Blob storage-service.

| Onderdeel | Beschrijving |
| --- | --- |
| **Opslagaccount** |Het opslagaccount dat is het beginpunt voor alle storage-services. Voor toegang tot een Azure Blob Storage-service, moet u eerst een Azure Storage-account maken. Zie voor meer informatie over Azure Blob storage-service, [over het gebruik van de Azure Blob Storage-Service](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** |Een container kan is een groepering van een reeks blobs en een onbeperkt aantal Blobs. Voor het schrijven van een SQL-Server back-up naar een Azure Blob-service, u hebt ten minste het root-container gemaakt. |
| **Blob** |Een bestand van willekeurig type en grootte. BLOBs kunnen worden opgevraagd met de volgende URL-indeling: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Zie voor meer informatie over pagina-Blobs, [inzicht in blok- en pagina-Blobs](https://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-onderdelen
De volgende SQL Server-onderdelen worden gebruikt wanneer de back-up op de Azure Blob storage-service.

| Onderdeel | Beschrijving |
| --- | --- |
| **URL** |De URL bevat een URI Uniform Resource Identifier () een unieke back-upbestand. De URL wordt gebruikt voor de locatie en naam van de back-upbestand van de SQL Server. De URL moet verwijzen naar een werkelijke blob, niet alleen een container. Als de blob niet bestaat, wordt dit gemaakt. Als een bestaande blob die is opgegeven, back-up mislukt, tenzij de > WITH FORMAT-optie is opgegeven. Hieronder volgt een voorbeeld van de URL die u in de back-up-opdracht opgeven wilt: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wordt aanbevolen maar niet vereist. |
| **Referentie** |De informatie die is vereist voor het verbinding maken en verifiëren met Azure Blob storage-service wordt opgeslagen als een referentie.  In de volgorde voor SQL Server back-ups schrijven naar een Azure Blob of herstellen uit moet een SQL Server-referentie worden gemaakt. Zie voor meer informatie, [SQL Server-referentie](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Als u ervoor kiest om te kopiëren en een back-upbestand uploaden naar de Azure Blob storage-service, moet u een pagina-blob-type gebruiken als uw opslagoptie als u van plan bent dit bestand te gebruiken voor herstelbewerkingen. HERSTELLEN van een blok-blob-type mislukken met een fout.
> 
> 

## <a name="next-steps"></a>Volgende stappen
1. Maak een Azure-account als u er nog geen hebt. Als u Azure evalueert, kunt u overwegen de [gratis proefversie](https://azure.microsoft.com/free/).
2. Ga via een van de volgende zelfstudies die u u begeleidt bij het maken van een storage-account en het uitvoeren van een herstelpunt.
   
   * **SQL Server 2014**: [zelfstudie: SQL Server 2014 back-up en herstel van Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [zelfstudie: met behulp van de Microsoft Azure Blob storage-service met SQL Server 2016-databases](https://msdn.microsoft.com/library/dn466438.aspx)
3. Bekijk aanvullende documentatie beginnen met [SQL Server-back-up en herstel met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

Als u problemen hebt, raadpleegt u het onderwerp [SQL Server back-up naar URL Best Practices en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx).

Voor andere SQL-Server back-up maken en opties voor terugzetten, Zie [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

