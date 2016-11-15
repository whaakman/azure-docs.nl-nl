---
title: Overzicht van SQL Server op virtuele machines in Azure | Microsoft Docs
description: "Meer informatie over het uitvoeren van volledige SQL Server-edities op virtuele machines in Azure. U vindt hier rechtstreekse koppelingen naar alle installatiekopieën voor virtuele machines met SQL Server en gerelateerde inhoud."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fcc2883ae52bb3d4be098ce741bbf5f5047777d2


---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Overzicht van SQL Server op virtuele machines in Azure
In dit onderwerp wordt beschreven hoe u SQL Server kunt uitvoeren op virtuele machines (VM's) in Azure. Ook vindt u hier [koppelingen naar portalinstallatiekopieën](#option-1-create-a-sql-vm-with-per-minute-licensing) en een overzicht van [algemene taken](#manage-your-sql-vm).

> [!NOTE]
> Zie [Een virtuele machine met SQL Server inrichten in Azure Portal](virtual-machines-windows-portal-sql-server-provision.md) als u al bekend bent met SQL Server en alleen wilt zien hoe u een virtuele machine met SQL Server implementeert.
> 
> 

## <a name="overview"></a>Overzicht
Als u een databasebeheerder of een ontwikkelaar bent, kunt u met VM's in Azure de on-premises SQL Server-werkbelastingen en toepassingen naar de cloud verplaatsen. De volgende video geeft een technisch overzicht van virtuele machines met SQL Server in Azure.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

In de video komen de volgende onderwerpen ter sprake:

| Time | Onderwerp |
| --- | --- |
| 00:21 |Wat zijn VM's in Azure? |
| 01:45 |Beveiliging |
| 02:50 |Connectiviteit |
| 03:30 |Betrouwbaarheid en prestaties van de opslag |
| 05:20 |Formaten van virtuele machines |
| 05:54 |Hoge beschikbaarheid en SLA |
| 07:30 |Configuratieondersteuning |
| 08:00 |Bewaking |
| 08:32 |Demo: een virtuele machine met SQL Server 2016 maken |

> [!NOTE]
> In de video gaat het over SQL Server 2016, maar er zijn in Azure VM-installatiekopieën beschikbaar voor vele versies van SQL Server, zoals 2008, 2012, 2014 en 2016. 
> 
> 

## <a name="scenarios"></a>Scenario's
U kunt er om vele redenen voor kiezen om uw gegevens in Azure te hosten. Als uw toepassing wordt verplaatst naar Azure, kunt u de prestaties verbeteren door ook de gegevens te verplaatsen. Maar er zijn nog andere voordelen. U hebt automatisch toegang tot meerdere datacenters voor een wereldwijde aanwezigheid en herstel na een noodgeval. De gegevens zijn ook zeer goed beveiligd en duurzaam.

SQL Server dat wordt uitgevoerd op virtuele machines in Azure is één mogelijkheid om relationele gegevens in Azure op te slaan. Het is een goede keuze voor verschillende scenario's. U wilt bijvoorbeeld de virtuele machine in Azure zo identiek mogelijk aan een on-premises computer met SQL Server configureren. Of u wilt misschien aanvullende toepassingen en services uitvoeren op dezelfde databaseserver. Er zijn twee belangrijke informatiebronnen die u helpen nog meer scenario's en overwegingen te bedenken:

* In [SQL Server op virtuele machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) vindt u een overzicht van de beste scenario's voor het gebruik van SQL Server op virtuele machines in Azure. 
* Zie [Een SQL Server-optie voor de cloud kiezen: Azure SQL (PaaS) Database of SQL Server op virtuele machines in Azure (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) voor een gedetailleerde vergelijking tussen SQL Database en SQL Server dat wordt uitgevoerd op een virtuele machine.

## <a name="create-a-new-sql-vm"></a>Een nieuwe SQL-VM maken
De volgende gedeelten bevatten directe koppelingen naar Azure Portal voor de galerie met installatiekopieën van virtuele SQL Server-machines. Afhankelijk van de installatiekopie die u selecteert, kunt u de SQL Server-licentiekosten betalen per minuut of uw eigen licentie gebruiken (Bring Your Own Licence, BYOL).

Stapsgewijze richtlijnen voor dit proces vindt u in de zelfstudie [Een virtuele SQL Server-machine inrichten in Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). Bekijk ook de [Aanbevolen procedures voor de beste prestaties voor virtuele SQL Server-machines](virtual-machines-windows-sql-performance.md), waarin wordt uitgelegd hoe u de juiste machinegrootte selecteert, evenals andere functies die tijdens het inrichten beschikbaar zijn.

## <a name="option-1-create-a-sql-vm-with-perminute-licensing"></a>Optie 1: een SQL-VM maken met licentiekosten per minuut
De volgende tabel bevat een matrix met beschikbare SQL Server-installatiekopieën in de galerie met virtuele machines. Klik op een koppeling om te beginnen met het maken van een nieuwe virtuele SQL-machine met de versie, de editie en het besturingssysteem die u hebt opgegeven.

| Version | Besturingssysteem | Editie |
| --- | --- | --- |
| **SQL 2016** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2) |
| **SQL 2014 SP1** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2) |
| **SQL 2014** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2) |
| **SQL 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |
| **SQL 2012 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2) |
| **SQL 2012 SP2** |Windows Server 2012 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012) |
| **SQL 2008 R2 SP3** |Windows Server 2008 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2) |
| **SQL 2008 R2 SP3** |Windows Server 2012 |[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012) |

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Optie 2: een SQL-VM maken met een bestaande licentie
U kunt ook uw eigen licentie gebruiken (Bring Your Own Licence, BYOL). In dit scenario betaalt u alleen voor de virtuele machine, zonder eventuele extra kosten voor SQL Server-licentieverlening. Als u uw eigen licentie wilt gebruiken, raadpleegt u onderstaande matrix met SQL Server-versies, SQL Server-edities en besturingssystemen. In de portal worden de namen van deze installatiekopieën voorafgegaan door **{BYOL}**.

| Version | Besturingssysteem | Editie |
| --- | --- | --- |
| **SQL Server 2016** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2) |
| **SQL Server 2014 SP1** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

> [!IMPORTANT]
> Voor het gebruik van installatiekopieën voor VM’s op basis van BYOL moet u beschikken over een Enterprise-overeenkomst met [licentiemobiliteit via Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Ook hebt u een geldige licentie nodig voor de SQL Server-versie/-editie die u wilt gebruiken. U moet [de benodigde BYOL-informatie aan Microsoft verstrekken](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) binnen **10** dagen na inrichting van uw virtuele machine.
> 
> 

## <a name="manage-your-sql-vm"></a>Uw virtuele SQL-machine beheren
Na het inrichten van uw virtuele SQL Server-machine zijn er verschillende optionele beheertaken die u kunt uitvoeren. Met betrekking tot bepaalde aspecten kunt u SQL Server op exact dezelfde manier configureren en beheren als u met een on-premises SQL Server-exemplaar zou doen. Andere taken zijn echter specifiek voor Azure. In de volgende secties worden enkele van deze taken uitgelicht. Ook vindt u er koppelingen naar meer informatie.

### <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine
Een van de meest elementaire beheerfasen is het verbinding maken met uw virtuele machine met SQL Server via hulpprogramma's als SQL Server Management Studio (SSMS). Zie [Connect to a SQL Server Virtual Machine on Azure](virtual-machines-windows-sql-connect.md) (Verbinding maken met een virtuele machine met SQL Server in Azure) voor instructies voor het verbinding maken met uw nieuwe virtuele machine met SQL Server.

### <a name="migrate-your-data"></a>Uw gegevens migreren
Als u een bestaande database hebt, doet u er verstandig aan deze te verplaatsen naar uw nieuw ingerichte virtuele SQL-machine. Zie [Een database migreren naar SQL Server op een virtuele machine in Azure](virtual-machines-windows-migrate-sql.md) voor een lijst met migratieopties en begeleiding bij de migratie.

### <a name="configure-high-availability"></a>Hoge beschikbaarheid configureren
Als u hoge beschikbaarheid nodig hebt, overweeg dan SQL Server-beschikbaarheidsgroepen te configureren. Hierbij combineert u meerdere virtuele machines in Azure tot een virtueel netwerk. In Azure Portal vindt u een sjabloon voor het instellen van deze configuratie. Zie [Een AlwaysOn-beschikbaarheidsgroep configureren op virtuele machines in Azure Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) voor meer informatie. Als u de beschikbaarheidsgroep en de bijbehorende listener handmatig wilt configureren, raadpleegt u [AlwaysOn-beschikbaarheidsgroepen configureren op virtuele machines in Azure](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Raadpleeg voor andere overwegingen met betrekking tot hoge beschikbaarheid [Hoge beschikbaarheid en herstel na een noodgeval voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Een back-up maken van uw gegevens
Azure VM's kunnen gebruikmaken van [geautomatiseerde back-ups](virtual-machines-windows-sql-automated-backup.md), waarbij er regelmatig back-ups in Blob Storage worden gemaakt van uw database. U kunt deze techniek ook handmatig gebruiken. Zie [Azure Storage gebruiken voor back-up en herstel van SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md) voor meer informatie. Zie [Back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md) voor een overzicht van alle opties voor back-up en herstel.

### <a name="automate-updates"></a>Updates automatiseren
Azure VM's kunnen [geautomatiseerde patching](virtual-machines-windows-sql-automated-patching.md) gebruiken om een onderhoudsvenster te plannen voor het automatisch installeren van belangrijke Windows- en SQL Server-updates.

### <a name="customer-experience-improvement-program-ceip"></a>Programma voor kwaliteitsverbetering (CEIP)
Het programma voor kwaliteitsverbetering (CEIP) is standaard ingeschakeld. Met dit programma worden regelmatig rapporten naar Microsoft verzonden zodat SQL Server kan worden verbeterd. Er hoeft voor het programma voor kwaliteitsverbetering geen beheertaak worden uitgevoerd, tenzij u het na het inrichten wilt uitschakelen. U kunt het programma voor kwaliteitsverbetering aanpassen of uitschakelen door via Extern bureaublad verbinding te maken met de virtuele machine. Voer vervolgens het hulpprogramma **SQL Server-fouten- en gebruiksrapportage** uit. Volg de instructies voor het uitschakelen van rapportage. 

Zie de sectie over het programma voor kwaliteitsverbetering in het onderwerp [Licentievoorwaarden accepteren](https://msdn.microsoft.com/library/ms143343.aspx) voor meer informatie. 

## <a name="next-steps"></a>Volgende stappen
[Bekijk het leertraject](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) voor virtuele SQL Server-machines in Azure.

Nog vragen? Bekijk eerst de [Veelgestelde vragen over SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md). Maar u kunt uw vragen of opmerkingen ook onder aan elk onderwerp over virtuele SQL-machines toevoegen om te communiceren met Microsoft en de community.




<!--HONumber=Nov16_HO2-->


