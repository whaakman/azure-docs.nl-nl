---
title: Back-up en herstel voor SQL Server | Microsoft Docs
description: Beschrijft de overwegingen voor back-up en herstel voor SQL Server-databases die zijn uitgevoerd op Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/15/2016
ms.author: mikeray
ms.openlocfilehash: 65557938673c5442758396a47873be1016e0f71b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Back-up en herstel voor SQL Server in Azure Virtual Machines
## <a name="overview"></a>Overzicht
Azure-opslag onderhoudt 3 kopieën van elke virtuele machine van Azure-schijf om bescherming te bieden bescherming tegen gegevensverlies of fysieke gegevensbeschadiging. Dus in tegenstelling tot on-premises kunt hoeft u niet te hoeven maken over deze. Echter, u moet nog steeds back-up van uw SQL Server-databases als bescherming tegen toepassings- of -fouten (bijvoorbeeld verkeerde gegevens invoegen of verwijderen van een tabel) en kunnen worden hersteld naar een punt in tijd.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

U kunt voor SQL Server wordt uitgevoerd in Azure Virtual machines, met systeemeigen back-up en herstellen met behulp van gekoppelde schijven voor de bestemming van back-upbestanden technieken. Er is echter een limiet aan het aantal schijven die u aan een virtuele machine van Azure koppelen kunt, op basis van de [grootte van de virtuele machine](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Er is ook de overhead van Schijfbeheer in overweging moet nemen.

Beginnend met SQL Server 2014, kunt u back-up en herstellen naar Microsoft Azure Blob-opslag. SQL Server 2016 biedt ook verbeteringen voor deze optie. Bovendien voor databasebestanden is opgeslagen in Microsoft Azure Blob storage, biedt SQL Server 2016 een optie voor bijna onmiddellijk uitgevoerde back-ups en snelle herstelacties met Azure momentopnamen. In dit artikel biedt een overzicht van deze opties en extra informatie kunt vinden op [SQL Server-back-up en herstel met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx).

> [!NOTE]
> Zie voor een beschrijving van de opties voor back-ups van zeer grote databases [NTFS-status en SQL Server-Database back-up strategieën voor Azure Virtual Machines](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).
> 
> 

De volgende secties bevatten informatie die specifiek zijn voor de verschillende versies van SQL Server wordt ondersteund in Azure een virtuele machine.

## <a name="sql-server-virtual-machines"></a>Virtuele Machines van SQL Server
Als uw exemplaar van SQL Server wordt uitgevoerd op een virtuele Machine van Azure, uw databasebestanden al zich bevinden op gegevensschijven met in Azure. Deze schijven bevinden zich in Azure Blob-opslag. Dus duren de redenen waarom een back-ups van uw database en de methoden u wijziging enigszins. Overweeg het volgende. 

* U hoeft niet meer back-ups als u wilt bieden bescherming tegen hardware-of omdat Microsoft Azure deze beveiliging als onderdeel van de Microsoft Azure-service biedt.
* U moet nog steeds back-ups als bescherming tegen gebruikersfouten of voor het archiveren of wettelijke redenen administratieve doeleinden wilt bieden.
* U kunt het back-upbestand opslaan rechtstreeks in Azure. Zie de volgende secties die richtlijnen te voor de verschillende versies van SQL Server bieden voor meer informatie.

## <a name="sql-server-2016"></a>SQL Server 2016
Biedt ondersteuning voor Microsoft SQL Server 2016 [back-up en herstel met Azure blobs](https://msdn.microsoft.com/library/jj919148.aspx) onderdelen gevonden in SQL Server 2014. Maar bevat ook de volgende verbeteringen:

| Verbetering van de 2016 | Details |
| --- | --- |
| **Striping** |Wanneer een back-up naar Microsoft Azure blob storage, SQL Server 2016 biedt ondersteuning voor back-ups op meerdere blobs zodat back-ups van grote databases maximaal 12,8 TB. |
| **Back-up van de momentopname** |Door het gebruik van Azure momentopnamen biedt back-up van SQL Server-bestand momentopname bijna onmiddellijk uitgevoerde back-ups en snelle herstelbewerkingen voor databasebestanden opgeslagen met behulp van de Azure Blob storage-service. Deze mogelijkheid kunt u uw back-up vereenvoudigen en beleid herstellen. Back-up van de momentopname van de bestanden ondersteunt ook punt in tijd terugzetten. Zie voor meer informatie [Momentopnameback-ups voor databasebestanden in Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Back-upplanning beheerd** |SQL Server Managed Backup naar Azure ondersteunt nu het aangepaste schema's. Zie voor meer informatie [SQL Server Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

Zie voor een zelfstudie over de mogelijkheden van SQL Server 2016 bij gebruik van Azure Blob-opslag, [zelfstudie: met behulp van de Microsoft Azure Blob storage-service met SQL Server 2016 databases](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014
SQL Server 2014, bevat de volgende verbeteringen:

1. **Back-up en herstel op Azure**:
   
   * *SQL Server back-up naar URL* heeft nu ondersteuning in SQL Server Management Studio. De optie voor het back-up naar Azure is nu beschikbaar wanneer u Backup of Restore taak of de wizard Onderhoudsplan SQL Server Management Studio. Zie voor meer informatie [SQL Server back-up naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
   * *SQL Server Managed Backup naar Azure* zijn nieuwe functionaliteit waarmee geautomatiseerde back-beheer. Dit is vooral handig voor het automatiseren van back-beheer voor exemplaren van SQL Server 2014 uitgevoerd op een Azure-Machine. Zie voor meer informatie [SQL Server Managed Backup naar Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
   * *Automatische back-up* biedt extra automation automatisch inschakelen van *SQL Server Managed Backup naar Azure* op alle bestaande en nieuwe databases voor een virtuele SQL Server-machine in Azure. Zie voor meer informatie [Automatische back-up voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
   * Zie voor een overzicht van de opties voor SQL Server 2014 back-up naar Azure [SQL Server-back-up en herstel met Microsoft Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
2. **Versleuteling**: SQL Server 2014 ondersteunt de versleuteling van gegevens bij het maken van een back-up. Ondersteunt verschillende algoritmen en het gebruik osf een certificaat of de asymmetrische sleutel. Zie voor meer informatie [back-Upversleuteling](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012
Zie voor gedetailleerde informatie over SQL Server-back-up en herstel in SQL Server 2012 [back-up en herstellen van SQL Server-Databases (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

U start in SQL Server 2012 SP1 cumulatieve Update 2, kunt u reservekopie en herstellen van de Azure Blob Storage-service. Deze uitbreiding kan worden gebruikt voor back-up van SQL Server-databases op een SQL-Server uitgevoerd op een virtuele Machine van Azure of een lokaal exemplaar. Zie voor meer informatie [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

De voordelen van het gebruik van de Azure Blob storage-service onder andere de mogelijkheid voor het overslaan van de schijf 16-limiet voor het gekoppelde schijven eenvoudig beheer, de directe beschikbaarheid van de back-upbestand naar een ander exemplaar van SQL Server-exemplaar is uitgevoerd op een virtuele machine van Azure of een lokaal exemplaar voor de migratie of noodherstel. Zie voor een volledige lijst van de voordelen van een Azure blob storage-service voor back-ups van SQL Server, de *voordelen* in sectie [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Zie voor aanbevolen werkwijzen besproken en informatie over probleemoplossing [back-up en herstellen van Best Practices (Azure Blob Storage-Service)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008
Zie voor SQL Server-back-up en herstel in SQL Server 2008 R2 [Backing up and Restoring Databases in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Zie voor SQL Server-back-up en herstel in SQL Server 2008 [Backing up and Restoring Databases in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Volgende stappen
Als u van plan bent uw implementatie van SQL Server in een virtuele machine in Azure, kunt u inrichting richtlijnen vinden in de volgende zelfstudie: [inrichten van een virtuele Machine van SQL Server op Azure met Azure Resource Manager](virtual-machines-windows-portal-sql-server-provision.md).

Hoewel back-up en herstel kunnen worden gebruikt om uw gegevens te migreren, zijn er mogelijk eenvoudiger migratiepaden van gegevens met SQL Server op een virtuele machine in Azure. Zie voor een volledige beschrijving van de opties en aanbevelingen [een Database migreren naar SQL Server op een virtuele machine van Azure](virtual-machines-windows-migrate-sql.md).

Bekijk de andere [bronnen voor het uitvoeren van SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

