---
title: Back-up en herstel voor SQL Server in virtuele machines in Azure | Microsoft Docs
description: Beschrijft de overwegingen voor back-up en herstel voor SQL Server-databases op Azure Virtual Machines worden uitgevoerd.
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
ms.openlocfilehash: 47d06ce5a03fe51ba345f60e2feff958ed4a890b
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56871159"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Back-up en herstel voor SQL Server in Azure Virtual Machines

In dit artikel bevat richtlijnen voor de back-up en herstel beschikbare opties voor SQL Server die wordt uitgevoerd in een Windows-machine in Azure. Azure-opslag onderhoudt drie kopieën van elke Azure-VM-schijf naar de bescherming tegen gegevensverlies of beschadiging van gegevens van fysieke garanderen. In tegenstelling tot on-premises hoeft u dus te richten op problemen met de hardware. U moet echter nog steeds back-uw SQL Server-databases om te beveiligen tegen toepassings- of fouten, zoals onbedoeld gegevens invoegingen of verwijderingen. In dit geval is het belangrijk om te kunnen herstellen naar een bepaald punt in tijd.

Het eerste deel van dit artikel biedt een overzicht van de beschikbare back-up en opties voor het herstellen. Dit wordt gevolgd door de volgende secties meer informatie over elke strategie bevatten.

## <a name="backup-and-restore-options"></a>Opties voor back-up en herstel

De volgende tabel bevat informatie over verschillende opties voor back-up en herstel voor SQL Server die wordt uitgevoerd op virtuele Azure-machines:

| Strategie | SQL-versies | Description |
|---|---|---|---|
| [Automatische back-up](#automated) | 2014<br/> 2016<br/> 2017 | Automatische back-up kunt u plannen van regelmatige back-ups voor alle databases op een virtuele machine van SQL Server. Back-ups worden opgeslagen in Azure storage voor 30 dagen. U begint met SQL Server 2016, biedt automatische back-up v2 extra opties zoals het configureren van handmatig te plannen en de frequentie van volledige en logboekback-ups. |
| [Azure Backup voor SQL-VM 's](#azbackup) | 2012<br/> 2014<br/> 2016<br/> 2017 | Azure Backup biedt een back-upfunctionaliteit voor Enterprise-klasse voor SQL Server die wordt uitgevoerd in virtuele Azure-machines. Met deze service kunt u back-ups voor meerdere servers en schaling van duizenden databases centraal beheren. Databases kunnen worden hersteld naar een bepaald punt in tijd in de portal. Het biedt een aanpasbare bewaarbeleid dat back-ups voor jaar kunt onderhouden. Deze functie is momenteel in openbare preview. |
| [Handmatige back-up](#manual) | Alle | Afhankelijk van uw versie van SQL Server zijn er verschillende manieren handmatig back-up en herstellen van SQL Server op een Azure-VM wordt uitgevoerd. In dit scenario bent u verantwoordelijk voor hoe uw databases back-ups en de locatie voor de opslag en beheer van deze back-ups. |

De volgende secties wordt elke optie in meer detail beschreven. De laatste sectie van dit artikel bevat een samenvatting in de vorm van een Functiematrix.

## <a id="automated"></a> Automatische back-up

Automatische back-up biedt een automatische back-upservice voor SQL Server Standard en Enterprise-edities die worden uitgevoerd in een Windows-VM in Azure. Deze service wordt geleverd door de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md), die automatisch is geïnstalleerd op SQL Server Windows-installatiekopieën voor virtuele machines in Azure portal.

Alle databases worden back-ups op een Azure storage-account die u configureert. Back-ups kunnen worden versleuteld en maximaal 30 dagen bewaard.

SQL Server 2016 en hoger VM's bieden meer opties voor het aanpassen met geautomatiseerde back-ups v2. Deze verbeteringen zijn onder andere:

- Back-ups van de database
- Handmatige back-upschema en tijdvenster
- Volledige en logboek-back-upfrequentie

Als u een database herstellen, moet u de vereiste bestanden voor back-up niet vinden in het opslagaccount en een herstel uitvoeren op de SQL-VM met behulp van SQL Server Management Studio (SSMS) of de Transact-SQL-opdrachten.

Zie een van de volgende artikelen voor meer informatie over het configureren van automatische back-up voor SQL-VM's:

- **SQL Server 2016/2017**: [Automatische back-up v2 voor virtuele Machines van Azure](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [Automatische back-up voor virtuele Machines van SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)

## <a id="azbackup"></a> Azure Backup voor SQL-VM's (Preview-versie)

[Azure Backup](/azure/backup/) biedt u een back-upfunctionaliteit voor Enterprise-klasse voor SQL Server wordt uitgevoerd in Azure VM's. Alle back-ups worden opgeslagen en beheerd in een Recovery Services-kluis. Er zijn diverse voordelen die deze oplossing, met name voor ondernemingen biedt:

- **Back-up van nul infrastructuur**: U hoeft niet te beheren van back-upservers of opslaglocaties.
- **Schaal**: Veel SQL-VM's en schaling van duizenden databases beveiligen.
- **Pay-As-You-Go**: Deze mogelijkheid is een afzonderlijke service die is geleverd door Azure Backup, maar als met alle Azure-services, betaalt u alleen voor wat u gebruikt.
- **Centraal beheer en controle**: Centraal beheer al uw back-ups, met inbegrip van andere werkbelastingen die Azure Backup, vanaf één dashboard in Azure ondersteunt.
- **Beleid voor back-up en retentie gestuurde**: Standaard back-upbeleid voor regelmatige back-ups maken. Tot stand brengen voor het bewaren van voor het onderhouden van back-ups voor jaar.
- **Ondersteuning voor SQL altijd op**: Detecteren en beschermen van de configuratie van een SQL Server Always On en voldoen aan de back-back-upvoorkeur voor beschikbaarheidsgroep.
- **15 minuten Recovery Point Objective (RPO)**: Configureer SQL transactielogboekback-ups maximaal elke 15 minuten.
- **Punt in tijd herstel**: De portal gebruiken om databases te herstellen naar een bepaald punt in tijd zonder handmatig herstellen meerdere volledige, differentiële en logboekback-ups.
- **E-mailwaarschuwingen voor fouten geconsolideerd**: Geconsolideerde e-mailmeldingen fouten configureren.
- **Op rollen gebaseerd toegangsbeheer**: Bepalen wie kan beheren van back-up en herstelbewerkingen via de portal.

Bekijk de volgende video voor een snel overzicht van hoe het werkt samen met een demo:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2dNbw]

Deze oplossing Azure back-up voor SQL-VM's is momenteel in openbare preview. Zie voor meer informatie, [maakt u een Back-up van SQL Server-database naar Azure](../../../backup/backup-azure-sql-database.md).

## <a id="manual"></a> Handmatige back-up

Als u wilt handmatig beheren van back-up en herstellen van bewerkingen op uw SQL-VM's, zijn er verschillende opties, afhankelijk van de versie van SQL Server die u gebruikt. Zie voor een overzicht van back-up en herstel, een van de volgende artikelen op basis van uw versie van SQL Server:

- [Back-up en herstel voor SQL Server 2016 en hoger](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Back-up en herstel voor SQL Server 2014](https://msdn.microsoft.com/library/ms187048%28v=sql.120%29.aspx)
- [Back-up en herstel voor SQL Server 2012](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [Back-up en herstel voor SQL Server SQL Server 2008 R2](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [Back-up en herstel voor SQL Server 2008](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

In de volgende secties beschrijven enkele handmatige back-up en herstellen van de opties in meer detail.

### <a name="backup-to-attached-disks"></a>Back-up naar de gekoppelde schijven

U kunt voor SQL Server die wordt uitgevoerd in virtuele Azure-machines, met systeemeigen back-up en herstellen met behulp van gekoppelde schijven op de virtuele machine voor de bestemming van de back-upbestanden technieken. Er is echter een limiet aan het aantal schijven die u aan een Azure-machine koppelen kunt, op basis van de [grootte van de virtuele machine](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Er is ook de overhead van het Schijfbeheer om te overwegen.

Zie voor een voorbeeld van hoe u handmatig maakt een volledige databaseback-up met behulp van SQL Server Management Studio (SSMS) of de Transact-SQL, [een volledige Database back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server).

### <a name="backup-to-url"></a>Back-up naar URL

Beginnen met SQL Server 2012 SP1 CU2, kunt u back-up en herstellen rechtstreeks naar Microsoft Azure Blob-opslag, die ook bekend als back-up naar URL. SQL Server 2016 wordt tevens de volgende verbeteringen voor deze functie geïntroduceerd:

| Uitbreiding van 2016 | Details |
| --- | --- |
| **Striping** |Wanneer de back-up naar Microsoft Azure blob storage, SQL Server 2016 biedt ondersteuning voor back-ups op meerdere blobs om in te schakelen back-ups van grote databases, tot een maximum van 12,8 TB. |
| **Momentopnameback-up** |Door het gebruik van Azure-momentopnamen biedt SQL Server-bestand Momentopnameback-up nagenoeg onmiddellijke back-ups en snelle herstelbewerkingen voor databasebestanden die zijn opgeslagen met behulp van de Azure Blob storage-service. Deze manier kunt u het vereenvoudigen van de back-up en herstellen van beleid. Bestand-momentopnameback-up ondersteunt ook punt in tijd herstel. Zie voor meer informatie, [Momentopnameback-ups van databasebestanden in Azure](https://docs.microsoft.com/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Zie voor meer informatie, een van de volgende artikelen op basis van uw versie van SQL Server:

- **SQL Server 2016/2017**: [SQL Server back-up naar URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [SQL Server 2014 back-up naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [SQL Server 2012 back-up naar URL](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Beheerde back-up

Beginnen met SQL Server 2014, automatiseert Managed Backup het maken van back-ups naar Azure storage. Achter de schermen maakt Managed Backup gebruik van de back-up naar URL-functie die wordt beschreven in de vorige sectie van dit artikel. Beheerde back-up is ook de onderliggende functie die ondersteuning biedt voor de SQL Server VM automatische back-up-service.

Beheerde back-up hebt vanaf SQL Server 2016, u extra opties voor het plannen, systeemdatabase back-up en volledige en back-upfrequentie log.

Zie een van de volgende artikelen op basis van uw versie van SQL Server voor meer informatie:

- [Beheerde back-up naar Microsoft Azure voor SQL Server 2016 en hoger](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [Managed Backup in Microsoft Azure voor SQL Server 2014](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Besluit matrix

De volgende tabel geeft een overzicht van de mogelijkheden van elke optie back-up en herstel voor SQL Server-machines in Azure.

|| **Automatische back-up** | **Azure Backup voor SQL** | **Handmatige back-up** |
|---|---|---|---|
| Vereist extra Azure-service |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-upbeleid configureren in Azure portal | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Herstellen van databases in Azure portal |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Meerdere servers in één dashboard beheren |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Terugzetten naar eerder tijdstip | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| 15 minuten Recovery Point Objective (RPO) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Op korte termijn back-upbewaarbeleid (dagen) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-beleid met een langetermijnbewaarperiode (maanden, jaar) |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Ingebouwde ondersteuning voor SQL Server Always On |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-up naar Azure-Opslagaccount (s) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(automatisch) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(automatisch) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png)(beheerd door de klant) |
| Beheer van opslag en back-up-bestanden | | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |  |
| Back-up naar de gekoppelde schijven op de virtuele machine |   |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Centrale aanpasbare Backup-rapporten |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Geconsolideerde e-mailwaarschuwingen voor fouten |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Aanpassen van controles op basis van Log Analytics |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Back-uptaken controleren met behulp van SSMS of Transact-SQL-scripts | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Terugzetten met behulp van SSMS of Transact-SQL-scripts | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Ja](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Volgende stappen

Als u van plan bent uw implementatie van SQL Server in een Azure-VM, vindt u inrichting richtlijnen in de volgende handleiding: [Hoe u een Windows SQL Server-machine inrichten in Azure portal](virtual-machines-windows-portal-sql-server-provision.md).

Hoewel back-up en herstel kunnen worden gebruikt om uw gegevens te migreren, zijn er mogelijk eenvoudiger migratiepaden van gegevens naar SQL Server op een Azure-VM. Zie voor een volledige beschrijving van de opties voor de migratie en aanbevelingen, [een Database migreren naar SQL Server op een Azure-VM](virtual-machines-windows-migrate-sql.md).
