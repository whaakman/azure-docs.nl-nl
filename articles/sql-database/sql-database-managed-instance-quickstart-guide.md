---
title: Quickstart - Azure SQL Database Managed Instance | Microsoft Docs
description: Leer hoe u snel aan de slag kunt gaan met Azure SQL Database - Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 77deed43c106a451d3de768989233c749e1280e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468169"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Aan de slag met Azure SQL Database - Managed Instance

[Azure SQL Database Managed Instance](sql-database-managed-instance-index.yml) is een volledig beheerde PaaS-versie van SQL Server die wordt gehost in Azure-cloud en geplaatst in uw eigen VNet met het particuliere IP-adres. In dit gedeelte leert u hoe u snel beheerde exemplaren kunt configureren en maken en uw databases kunt migreren.

## <a name="quickstart-overview"></a>Overzicht van quickstarts

Dit gedeelte bevat een overzicht van de beschikbare artikelen waarmee u snel aan de slag kunt met beheerde exemplaren. De eenvoudigste manier om uw eerste beheerde exemplaar te maken is [de Azure-portal](sql-database-managed-instance-get-started.md) te gebruiken, waar u de noodzakelijke parameters (gebruikersnaam/wachtwoord, aantal kernen, maximale opslag) kunt configureren, en automatisch een Azure-netwerkomgeving kunt maken zonder de netwerkdetails en infrastructuurvereisten te kennen. Zorg ervoor dat u een [abonnementstype](sql-database-managed-instance-resource-limits.md#supported-subscription-types) hebt waarmee het exemplaar mag worden gemaakt.

Als u een eigen netwerk hebt dat u wilt gebruiken of als u het netwerk wilt aanpassen, bekijk dan hoe u de [ netwerkomgeving configureert](#configure-network-environment) voor beheerde exemplaren.

Wanneer u uw beheerde exemplaar maakt, moet u op een van de volgende manieren verbinding maken met het exemplaar:

* Maak een [virtuele Azure-machine](sql-database-managed-instance-configure-vm.md) met geïnstalleerde SQL Server Management Studio en andere apps die kunnen worden gebruikt voor toegang tot uw beheerde exemplaar in een subnet binnen hetzelfde VNet waarin uw beheerde exemplaar is geplaatst. Virtuele machine mag zich niet in hetzelfde subnet als uw beheerde exemplaren bevinden.
* Stel een [punt-naar-site-verbinding](sql-database-managed-instance-configure-p2s.md) op uw computer in waarmee u uw computer kunt aansluiten op het VNet waar het beheerde exemplaar is geplaatst en waarmee u het beheerde exemplaar kunt gebruiken zoals elke andere SQL Server in uw netwerk.

Als alternatief kunt u een ExpressRoute- of site-naar-site-verbinding van uw lokale netwerk gebruiken. Deze methoden worden echter helaas niet besproken in deze quickstarts.

Wanneer u een beheerd exemplaar maakt en toegang configureert, kunt u beginnen met het migreren van uw databases die zich op de on-premises SQL Server of Azure-VM's bevinden. Houd er rekening mee dat de migratie mislukt als de brondatabase die u wilt migreren niet-ondersteunde functies bevat. Om fouten te voorkomen en de compatibiliteit te controleren, kunt u [Data Migration Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) installeren. DMA analyseert uw databases op SQL Server en vindt eventuele problemen die de migratie naar het beheerde exemplaar zouden kunnen blokkeren, zoals de aanwezigheid van FileStream-bestanden of meerdere logboekbestanden. Als u deze problemen oplost, zijn uw databases klaar om naar het beheerde exemplaar te gaan. [Database Experimentation Assistant](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) is een ander handig hulpmiddel dat uw werkbelasting op SQL Server kan vastleggen en deze opnieuw kan afspelen op het beheerde exemplaar, zodat u kunt bepalen of zich prestatieproblemen gaan voordoen als u migreert naar het beheerde exemplaar.

Zodra u zeker weet dat u uw database naar het beheerde exemplaar kunt migreren, kunt u de functionaliteit [Systeemeigen herstel](sql-database-managed-instance-get-started-restore.md) gebruiken, waarmee u met een Transact-SQL-opdracht een back-up van uw database maakt, deze naar een Azure-blogopslag uploadt en de database met een Transact-SQL-opdracht in de blobopslag herstelt.

Met deze quickstarts kunt u snel databases configureren, maken en op uw beheerde exemplaren zetten. In sommige scenario's moet u de implementatie van het beheerde exemplaar en de vereiste netwerkomgeving aanpassen of automatiseren. Deze scenario's worden hieronder beschreven.

## <a name="customizing-network-environment"></a>De netwerkomgeving aanpassen

Hoewel het VNet/subnet automatisch kan worden geconfigureerd met [de Azure-portal](sql-database-managed-instance-get-started.md) wanneer het exemplaar wordt gemaakt, kan het een goed idee zijn om het te maken voordat u begint de beheerde exemplaren te maken, zodat u de parameters van het VNet en subnet kunt configureren. De eenvoudigste manier om de netwerkomgeving te maken en configureren is om de sjabloon voor de [implementatie van Azure Resource](sql-database-managed-instance-create-vnet-subnet.md) te gebruiken. Deze sjabloon maakt en configureert het netwerk en subnet waar het exemplaar wordt geplaatst. U hoeft slechts de implementatieknop voor Azure Resource Manager in te drukken en het formulier met parameters in te vullen. Als alternatief kunt u [PowerShell-script](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) gebruiken om het maken van het netwerk te automatiseren.

Als u al een VNet en subnet hebt waar u uw beheerde exemplaar wilt implementeren, dan moet u er zeker van zijn dat uw VNet en subnet aan de [netwerkvereisten](sql-database-managed-instance-connectivity-architecture.md#network-requirements) voldoen. Gebruik dit [PowerShell-script om te controleren of uw subnet correct is geconfigureerd](sql-database-managed-instance-configure-vnet-subnet.md). Dit script is niet slechts verantwoordelijk voor het valideren van uw netwerk en het rapporteren van problemen. Het laat u ook weten wat er moet worden gewijzigd en stelt voor deze noodzakelijke wijzigingen aan uw VNet/subnet uit te voeren. Voer dit script uit als u uw VNet/subnet niet handmatig wilt configureren en na aanzienlijke herconfiguraties van uw netwerkinfrastructuur. Wilt u uw eigen netwerk maken en configureren? Lees dan [de documentatie over beheerde exemplaren](sql-database-managed-instance-connectivity-architecture.md) en [deze gids](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-managed-instance"></a>Maken van beheerde exemplaren automatiseren

 Als u de netwerkomgeving niet hebt gemaakt zoals beschreven in de vorige stap, dan kan de Azure-portal dit voor u doen. Het enige nadeel hiervan is dat de omgeving wordt geconfigureerd met enkele standaardparameters die u later niet meer kunt wijzigen. Als alternatief kunt u [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), [PowerShell met een sjabloon voor beheerde exemplaren](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md) of [Azure CLI](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/) gebruiken.

## <a name="migrating-to-managed-instance-with-minimal-downtime"></a>Naar beheerd exemplaar migreren met minimale downtime

Met de artikelen in deze quickstarts kunt u snel beheerde exemplaren instellen en uw databases verplaatsen. Met het systeemeigen herstel moet u echter wachten tot de databases zijn hersteld, wat leidt tot een korte downtime van uw toepassing, met name als de database groter is. Als u uw productiedatabase verplaatst, hebt u waarschijnlijk een betere migratiemethode nodig met minimale downtime. [Data Migration Service](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) is een migratieservice die uw database met minimale downtime kan migreren door de wijzigingen die u in de brondatabase hebt aangebracht stapsgewijs te pushen naar een database die u aan het herstellen bent naar het beheerde exemplaar. Op deze manier kunt u snel uw toepassing van de bron- naar de doeldatabase overzetten met minimale downtime.

## <a name="next-steps"></a>Volgende stappen

* Vind [hier een lijst met ondersteunde functies in het beheerde exemplaar](sql-database-features.md) en [hier details en bekende problemen](sql-database-managed-instance-transact-sql-information.md). 
* Meer informatie over de [technische kenmerken van het beheerde exemplaar](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
* Geavanceerdere zelfstudies vindt u in [het instructiegedeelte](sql-database-howto-managed-instance.md). 
