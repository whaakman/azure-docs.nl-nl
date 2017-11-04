---
title: Het gebruik van Azure-opslag voor SQL Server-back-up en herstellen | Microsoft Docs
description: Informatie over het back-up van SQL Server naar Azure Storage. Verklaart de voordelen van het back-ups van SQL-databases naar Azure Storage.
services: virtual-machines-windows
documentationcenter: 
author: MikeRayMSFT
manager: jhubbard
tags: azure-service-management
ms.assetid: 0db7667d-ef63-4e2b-bd4d-574802090f8b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mikeray
ms.openlocfilehash: d3df6b25fe524c500cf1a1333ac136e8a29d1484
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Azure Storage gebruiken voor SQL Server-back-up en herstel
## <a name="overview"></a>Overzicht
Beginnen met SQL Server 2012 SP1 CU2, kunt u SQL Server-back-ups schrijven rechtstreeks naar de Azure Blob storage-service. U kunt deze functionaliteit gebruiken tot maken en terugzetten van de Azure Blob-service met een on-premises SQL Server database of SQL Server-database in Azure een virtuele machine. Back-up naar cloud biedt de voordelen van de beschikbaarheid, oneindig geogerepliceerde off-site opslag en eenvoudig van de migratie van gegevens naar en vanuit de cloud. Instructies voor BACKUP of RESTORE kunt u met behulp van Transact-SQL- of SMO uitgeven.

SQL Server 2016 introduceert nieuwe mogelijkheden; u kunt [back-up van de momentopname van de bestanden](http://msdn.microsoft.com/library/mt169363.aspx) bijna onmiddellijk uitgevoerde back-ups en zeer snelle herstelbewerkingen uit te voeren.

Dit onderwerp wordt uitgelegd waarom u kunt kiezen om Azure-opslag voor SQL-back-ups en vervolgens beschrijving van de onderdelen die zijn betrokken. U kunt de resources die worden geleverd aan het einde van het artikel gebruiken voor toegang tot scenario's en aanvullende informatie om te beginnen met uw SQL Server back-ups met behulp van deze service.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Voordelen van het gebruik van de Azure Blob-Service voor back-ups van SQL Server
Er zijn enkele uitdagingen die u geconfronteerd back-ups van SQL Server. Deze uitdagingen zijn onder andere de kans op fouten in de opslag, toegang tot externe opslag en hardwareconfiguratie opslagbeheer. Veel van deze uitdagingen worden aangepakt met behulp van de store-service van Azure Blob voor back-ups van SQL Server. Houd rekening met de volgende voordelen:

* **Gebruiksgemak**: opslaan van uw back-ups in Azure BLOB's is een handige, flexibele en eenvoudige toegang tot externe optie. Externe opslag maken voor uw SQL Server back-ups kunnen net zo eenvoudig als het wijzigen van uw bestaande scripts/taken gebruiken de **back-up naar URL** syntaxis. Externe opslag moet doorgaans ver genoeg van de locatie voor de productie van een database om te voorkomen dat een enkele noodgeval die voor zowel de externe en productie database-locaties gevolgen mogelijk. Door te kiezen [geo-replicatie uw Azure blobs](../../../storage/common/storage-redundancy.md), hebt u een extra beschermingslaag er in een noodsituatie die invloed kan zijn op de hele regio.
* **Back-archief**: de Azure Blob Storage-service biedt een betere alternatief voor de optie vaak gebruikte tape te archiveren van back-ups. Tapeopslag mogelijk fysieke verzending naar een off-site faciliteit en metingen zijn om de media te beveiligen. Uw back-ups opslaan in Azure Blob Storage biedt een momentopname, maximaal beschikbaar, en een duurzame optie archiveren.
* **Hardware beheerd**: Er is geen extra overhead van hardwarebeheer met Azure-services. Azure-services voor het beheren van de hardware en het bieden van geo-replicatie voor redundantie en bescherming tegen hardwarefouten.
* **Onbeperkte opslag**: doordat een directe back-up naar Azure blobs die u hebt toegang tot vrijwel onbeperkte opslag. U kunt ook heeft back-ups maken van een virtuele machine van Azure-schijf grenzen op basis van de grootte van de machine. Er is een limiet aan het aantal schijven die u aan een virtuele machine van Azure voor back-ups koppelen kunt. Deze limiet is 16 schijven voor een extra groot exemplaar en minder voor kleinere exemplaren.
* **Back-up van beschikbaarheid**: back-ups opgeslagen in Azure blobs beschikbaar zijn vanaf elke locatie en op elk gewenst moment en gemakkelijk toegankelijk zijn voor terugzetbewerkingen voor een lokale SQL Server of een andere SQL-Server in een virtuele Machine van Azure, zonder dat wordt uitgevoerd database koppelen/loskoppelen of downloaden en de VHD koppelen.
* **Kosten**: betaalt alleen voor de service die wordt gebruikt. Kan worden rendabele als een optie voor de externe en back-archief. Zie de [Azure prijscategorie Rekenmachine](http://go.microsoft.com/fwlink/?LinkId=277060 "Prijscalculator"), en de [prijzen van Azure artikel](http://go.microsoft.com/fwlink/?LinkId=277059 "prijzen artikel") voor meer informatie.
* **Opslag-momentopnamen**: als de databasebestanden worden opgeslagen in een Azure-blob en u SQL Server 2016 gebruikt, kunt u [back-up van de momentopname van de bestanden](http://msdn.microsoft.com/library/mt169363.aspx) bijna onmiddellijk uitgevoerde back-ups en zeer snelle herstelbewerkingen uit te voeren.

Zie voor meer informatie [SQL Server-back-up en herstel met Azure Blob Storage-Service](http://go.microsoft.com/fwlink/?LinkId=271617).

De volgende twee secties vindt de Azure Blob storage-service, inclusief de vereiste SQL Server-onderdelen. Het is belangrijk om te begrijpen van de onderdelen en hun interactie met back-up en herstel van de Azure Blob storage-service.

## <a name="azure-blob-storage-service-components"></a>Azure Blob Storage-serviceonderdelen
De volgende Azure onderdelen worden gebruikt wanneer een back-up naar de Azure Blob storage-service.

| Onderdeel | Beschrijving |
| --- | --- |
| **Storage-Account** |Het opslagaccount is het startpunt voor alle storage-services. Als u een Azure Blob Storage-service, moet u eerst een Azure Storage-account maken. Zie voor meer informatie over Azure Blob storage-service, [het gebruik van de Azure Blob Storage-Service](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Container** |Een container is een groepering van een reeks blobs en kan een onbeperkt aantal Blobs bevatten. Voor het schrijven van een SQL Server back-up naar een Azure Blob-service, u hebt ten minste het root-container gemaakt. |
| **BLOB** |Een bestand van willekeurig type en de grootte. BLOBs kunnen worden opgevraagd met de volgende URL-indeling: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Zie voor meer informatie over de pagina-Blobs [Understanding blok en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>SQL Server-onderdelen
De volgende SQL Server-onderdelen worden gebruikt wanneer een back-up naar de Azure Blob storage-service.

| Onderdeel | Beschrijving |
| --- | --- |
| **URL** |De URL bevat een URI Uniform Resource Identifier () om een unieke back-upbestand. De URL wordt gebruikt voor de locatie en naam van de back-upbestand van de SQL Server. De URL moet verwijzen naar een werkelijke blob, niet alleen een container. Als de blob niet bestaat, wordt deze gemaakt. Als een bestaande blob zijn die is opgegeven, back-up mislukt, tenzij de > met indelingsoptie is opgegeven. Hieronder volgt een voorbeeld van de URL die u in de opdracht BACKUP opgeven wilt: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS wordt aanbevolen, maar niet vereist. |
| **Referentie** |De informatie die nodig is voor het verbinding maken en verifiëren met Azure Blob storage-service wordt opgeslagen als een referentie.  SQL Server back-ups schrijven naar een Azure-Blob of herstellen uit worden een SQL Server-referentie gemaakt. Zie voor meer informatie [referenties van het SQL Server](https://msdn.microsoft.com/library/ms189522.aspx). |

> [!NOTE]
> Als u ervoor kiest om te kopiëren en een back-bestand uploaden naar de Azure Blob storage-service, moet u een pagina-blob-type gebruiken als uw opslagoptie als u van plan bent dit bestand te gebruiken voor herstelbewerkingen. HERSTEL van een blok-blob-type mislukt met een fout opgetreden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
1. Een Azure-account maken als u er nog geen hebt. Als u Azure evalueert, overweeg dan de [gratis proefversie](https://azure.microsoft.com/free/).
2. Ga vervolgens via een van de volgende zelfstudies die u helpt bij het maken van een opslagaccount en een herstelbewerking uitvoert.
   
   * **SQL Server 2014**: [zelfstudie: SQL Server 2014 back-up en herstel van Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
   * **SQL Server 2016**: [zelfstudie: met behulp van de Microsoft Azure Blob storage-service met SQL Server 2016-databases](https://msdn.microsoft.com/library/dn466438.aspx)
3. Bekijk de aanvullende documentatie beginnen met [SQL Server-back-up en herstel met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

Als u problemen hebt, raadpleegt u het onderwerp [SQL Server back-up naar URL aanbevolen procedures en probleemoplossing](https://msdn.microsoft.com/library/jj919149.aspx).

Voor andere SQL-Server back-up en opties voor terugzetten, Zie [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

