---
title: Back-up en herstel voor SQL Server in de virtuele machines in Azure | Microsoft Docs
description: Beschrijft de overwegingen voor back-up en herstel voor SQL Server-databases die zijn uitgevoerd op Azure Virtual Machines.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: 4b90d1b9b2ee64722d3c92bcbd8fa205c9b59ebd
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809604"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Back-up en herstel voor SQL Server in Azure Virtual Machines

In dit artikel biedt richtlijnen voor de back-up en herstel beschikbare opties voor SQL Server in Windows Azure virtuele Machines worden uitgevoerd. Azure-opslag onderhoudt drie kopieën van elke virtuele machine van Azure-schijf om bescherming te bieden bescherming tegen gegevensverlies of fysieke gegevensbeschadiging. Dus in tegenstelling tot on-premises kunt hoeft u niet te concentreren op hardwarefouten. U moet echter nog steeds back-SQL Server-databases als bescherming tegen toepassings- of -fouten, zoals onbedoeld gegevens invoegingen of verwijderingen. In dit geval is het belangrijk om te kunnen herstellen naar een bepaald punt in tijd.

Het eerste deel van dit artikel biedt een overzicht van de beschikbare back-up en opties voor terugzetten. Dit wordt gevolgd door de volgende secties meer informatie over elke strategie bevatten.

## <a name="backup-and-restore-options"></a>Opties voor back-up en herstel

De volgende tabel bevat informatie over verschillende opties voor de back-up en herstel voor SQL Server op Azure VM's uitgevoerd:

| Een strategie voor | SQL-versies | Beschrijving |
|---|---|---|---|
| [Automatische back-up](#automated) | 2014<br/> 2016<br/> 2017 | Automatische back-up kunt u regelmatig back-ups voor alle databases op een virtuele machine van SQL Server. Back-ups worden opgeslagen in Azure storage tot maximaal 30 dagen. Beginnend met SQL Server 2016, biedt automatische back-up v2 aanvullende opties zoals handmatig te plannen en de frequentie van volledige en logboekback-ups configureren. |
| [Azure Backup voor virtuele SQL-machines](#azbackup) | 2012<br/> 2014<br/> 2016<br/> 2017 | Azure Backup biedt een back-upfunctionaliteit voor Enterprise-klasse voor SQL Server dat wordt uitgevoerd in Azure Virtual machines. Met deze service kunt u back-ups voor meerdere servers en duizenden databases centraal beheren. Databases kunnen worden hersteld naar een bepaald punt in tijd in de portal. Biedt een aanpasbare bewaarbeleid die back-ups voor jaar kunt onderhouden. Deze functie is momenteel in de openbare preview. |
| [Handmatige back-up](#manual) | Alle | Afhankelijk van uw versie van SQL Server zijn er verschillende manieren handmatig back-up en herstellen van SQL Server wordt uitgevoerd op een Azure VM. In dit scenario bent u verantwoordelijk voor hoe de databases zijn back-up en de locatie voor de opslag en beheer van deze back-ups. |

De volgende secties wordt elke optie in meer detail beschreven. Het laatste gedeelte van dit artikel bevat een samenvatting in de vorm van een Functiematrix.

## <a id="autoamted"></a> Automatische back-up

Automatische back-up biedt een automatische back-service voor SQL Server Standard en Enterprise-edities uitgevoerd in een Windows Azure VM. Deze service wordt geboden door de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md), dat automatisch is geïnstalleerd op de installatiekopieën van de SQL Server Windows virtuele machines in de Azure portal.

Alle databases worden back-up naar Azure storage-account die u configureert. Back-ups kunnen worden versleuteld en maximaal 30 dagen bewaard.

SQL Server 2016 en hogere VM's bieden meer aanpassingsopties met v2 voor automatische back-up. Deze verbeteringen zijn onder andere:

- Back-ups van de database
- Handmatige back-upschema en tijdvenster
- Volledige en log-bestand back-upfrequentie

Als u een database herstellen, moet u de vereiste back-bestanden niet vinden in het opslagaccount en een herstel uitvoeren op uw SQL-VM met SQL Server Management Studio (SSMS) of de Transact-SQL-opdrachten.

Zie een van de volgende artikelen voor meer informatie over het configureren van automatische back-up voor virtuele SQL-machines:

- **SQL Server 2016/2017**: [automatische back-v2 voor virtuele Machines in Azure ](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [automatische back-up voor virtuele Machines van SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)

## <a id="azbackup"></a> Azure Backup voor virtuele SQL-machines (openbare Preview)

[Azure Backup](/azure/backup/) biedt u een back-upfunctionaliteit voor Enterprise-klasse voor SQL Server wordt uitgevoerd in Azure VM's. Alle back-ups worden opgeslagen en beheerd in een Recovery Services-kluis. Er zijn verschillende voordelen, met deze oplossing, met name voor ondernemingen biedt:

- **Back-up nul infrastructuur**: U hoeft niet te beheren van back-upservers of opslaglocaties.
- **Schaal**: veel SQL-VM's en duizenden databases beveiligen.
- **Betalen naar gebruik**: deze mogelijkheid is een afzonderlijke service van Azure Backup maar net als bij alle Azure-services, betaalt u alleen voor wat u gebruikt.
- **Centraal beheer en controle**: centraal beheren van al uw back-ups, met inbegrip van andere werkbelastingen die Azure Backup, vanaf één dashboard in Azure ondersteunt.
- **Beleid gebaseerde back-up en retentie**: standaard back-upbeleid voor regelmatige back-ups maken. Tot stand brengen bewaarbeleid voor het onderhouden van back-ups voor jaar.
- **Ondersteuning voor SQL Always On**: detecteren en de configuratie van een SQL Server Always On beveiligen en voldoen aan de back-beschikbaarheidsgroep back-up voorkeur.
- **15 minuten Recovery Point Objective (RPO)**: configureren SQL-transactielogboeken tot elke 15 minuten.
- **Punt in tijd terugzetten**: de portal gebruiken om databases te herstellen naar een bepaald punt in tijd zonder handmatig herstellen in meerdere volledig, differentiële, en logboekback-ups.
- **E-mailwaarschuwingen voor mislukte geconsolideerd**: configureren geconsolideerd e-mailmeldingen op fouten.
- **Toegangsbeheer op basis van rollen**: bepalen wie kunt beheren van back-up en herstelbewerkingen via de portal.

Bekijk de volgende video voor een snel overzicht van hoe het werkt samen met een demo's:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2dNbw]

Deze oplossing Azure Backup voor SQL-VM's is momenteel in de openbare preview. Zie voor meer informatie [maakt u een Back-up van SQL Server-database naar Azure](../../../backup/backup-azure-sql-database.md).

## <a id="manual"></a> Handmatige back-up

Als u wilt handmatig beheren van back-up en herstelbewerkingen op uw virtuele SQL-machines, zijn er verschillende opties, afhankelijk van de versie van SQL Server die u gebruikt. Zie een van de volgende artikelen op basis van uw versie van SQL Server voor een overzicht van back-up en herstel:

- [Back-up en herstel voor SQL Server 2016 of hoger](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Back-up en herstel voor SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms187048%28v=sql.120%29.aspx)
- [Back-up en herstel voor SQL Server 2012](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [Back-up en herstel voor SQL Server SQL Server 2008 R2](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [Back-up en herstel voor SQL Server 2008](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

De volgende secties beschrijven verschillende handmatige back-up en herstel van de opties in meer detail.

### <a name="backup-to-attached-disks"></a>Back-up naar de gekoppelde schijven

U kunt voor SQL Server wordt uitgevoerd in Azure Virtual machines, met systeemeigen back-up en herstellen met behulp van gekoppelde schijven op de virtuele machine voor het doel van de back-upbestanden technieken. Er is echter een limiet aan het aantal schijven die u aan een virtuele machine van Azure koppelen kunt, op basis van de [grootte van de virtuele machine](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Er is ook de overhead van Schijfbeheer in overweging moet nemen.

Zie voor een voorbeeld van het handmatig maken van een volledige databaseback-up met behulp van SQL Server Management Studio (SSMS) of de Transact-SQL, [een volledige Database back-up maken](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server).

### <a name="backup-to-url"></a>Back-up naar URL

Beginnend met SQL Server 2012 SP1 CU2, kunt u back-up en herstellen rechtstreeks naar Microsoft Azure Blob-opslag is ook bekend als back-up naar URL. SQL Server 2016 geïntroduceerd ook de volgende verbeteringen voor deze functie:

| Verbetering van de 2016 | Details |
| --- | --- |
| **Striping** |Wanneer een back-up naar Microsoft Azure blob storage, SQL Server 2016 biedt ondersteuning voor back-ups op meerdere blobs zodat back-ups van grote databases maximaal 12,8 TB. |
| **Back-up van de momentopname** |Door het gebruik van Azure momentopnamen biedt back-up van SQL Server-bestand momentopname bijna onmiddellijk uitgevoerde back-ups en snelle herstelbewerkingen voor databasebestanden opgeslagen met behulp van de Azure Blob storage-service. Deze mogelijkheid kunt u uw back-up vereenvoudigen en beleid herstellen. Back-up van de momentopname van de bestanden ondersteunt ook punt in tijd terugzetten. Zie voor meer informatie [Momentopnameback-ups voor databasebestanden in Azure](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Zie een van de volgende artikelen op basis van uw versie van SQL Server voor meer informatie:

- **SQL Server 2016/2017**: [SQL Server back-up naar URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [SQL Server 2014 back-up naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [SQL Server 2012 back-up naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Beheerde back-up

Beginnend met SQL Server 2014, automatiseert Managed Backup het maken van back-ups naar Azure-opslag. Achter de schermen worden maakt beheerd back-up gebruik van de back-up naar URL-functie die wordt beschreven in de vorige sectie van dit artikel. Beheerde back-up is ook de onderliggende functie die de SQL Server VM automatische back-up-service ondersteunt.

U begint in SQL Server 2016, is beheerde back-up aanvullende opties voor het plannen van systeemdatabase back-up en volledige en back-upfrequentie logboek.

Zie een van de volgende artikelen op basis van uw versie van SQL Server voor meer informatie:

- [Beheerde back-up naar Microsoft Azure voor SQL Server 2016 of hoger](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [Beheerde back-up naar Microsoft Azure voor SQL Server 2014](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Besluit matrix

De volgende tabel geeft een overzicht van de mogelijkheden van elke optie back-up en herstel voor SQL Server virtuele machines in Azure.

|| **Automatische back-up** | **Azure Backup voor SQL** | **Handmatige back-up** |
|---|---|---|---|
| Vereist extra Azure-service |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-upbeleid in Azure-portal configureren | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| In Azure-portal-databases herstellen |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Meerdere servers in één dashboard beheren |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Terugzetten naar eerder tijdstip | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| 15 minuten beoogd herstelpunt (RPO) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Korte termijn back-up bewaarbeleid (dagen) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Op lange termijn back-up bewaarbeleid (maanden, jaar) |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Ingebouwde ondersteuning voor SQL Server Always On |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-up naar Azure Storage-accounts | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(automatisch) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(automatisch) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(de klant beheerd) |
| Beheer van opslag- en back-up-bestanden | | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |  |
| Back-up naar de gekoppelde schijven op de virtuele machine |   |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Centrale aanpasbare back-rapporten |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Geconsolideerde e-mailwaarschuwingen voor fouten |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Bewaking op basis van OMS aanpassen |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Monitor voor back-uptaken met SSMS of Transact-SQL-scripts | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Herstellen van databases met SSMS of Transact-SQL-scripts | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Volgende stappen

Als u van plan bent uw implementatie van SQL Server in een virtuele machine in Azure, kunt u inrichting richtlijnen vinden in de volgende handleiding: [inrichten van een virtuele machine van Windows SQL Server in de Azure portal](virtual-machines-windows-portal-sql-server-provision.md).

Hoewel back-up en herstel kunnen worden gebruikt om uw gegevens te migreren, zijn er mogelijk eenvoudiger migratiepaden van gegevens met SQL Server op een virtuele machine in Azure. Zie voor een volledige beschrijving van de opties en aanbevelingen [een Database migreren naar SQL Server op een virtuele machine van Azure](virtual-machines-windows-migrate-sql.md).