---
title: SQL Server op Azure Virtual Machines Veelgestelde vragen over Windows | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure Virtual machines.
services: virtual-machines-windows
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/04/2017
ms.author: v-shysun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d692d806bab072fc2c4b734677223bad4142561
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-windows-azure-virtual-machines"></a>Veelgestelde vragen over SQL Server op Windows Azure Virtual Machines

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

In dit onderwerp vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren [SQL Server op Windows Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> In dit onderwerp legt de nadruk op specifieke met SQL Server op VM's van Windows. Als u SQL Server op een virtuele Linux-machines uitvoert, raadpleegt u de [Linux Veelgestelde vragen over](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. **Hoe maak ik een virtuele machine van Azure met SQL Server**

   De gemakkelijke oplossing is het maken van een virtuele Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en een SQL-VM maken vanuit de portal, [een SQL Server-machine inrichten in de Azure Portal](virtual-machines-windows-portal-sql-server-provision.md). U kunt de installatiekopie van een virtuele machine die gebruikmaakt van betalen per minuut SQL Server-licentieverlening selecteren of kunt u een installatiekopie die u kunt u uw eigen SQL Server-licentie. U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrijelijk gelicentieerde versie (Developer of Express) of door een lokale licentie opnieuw te gebruiken. Als u uw eigen licentie zetten, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Wat is het verschil tussen de SQL-VM's en de SQL-Database-service?**

   Conceptueel gezien die SQL Server op Azure een virtuele machine niet die verschilt van het SQL Server wordt uitgevoerd in een externe datacenter. Daarentegen [SQL-Database](../../../sql-database/sql-database-technical-overview.md) database-as-a-service biedt. Met SQL-Database, u geen toegang tot de machines die hosten van uw databases. Zie voor een volledige vergelijking [een cloud SQL Server-optie kiezen: Azure SQL (PaaS) Database of SQL Server op Azure Virtual machines (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hoe kan ik mijn lokale SQL Server-database migreren naar de Cloud?**

   Eerst een virtuele machine van Azure maken met een SQL Server-exemplaar. Vervolgens migreert u uw lokale databases naar dit exemplaar. Zie voor strategieën voor migratie van gegevens, [een SQL Server-database migreren naar SQL Server in een Azure VM](virtual-machines-windows-migrate-sql.md).

1. **Kan ik een tweede exemplaar van SQL Server installeren op dezelfde virtuele machine Kan ik geïnstalleerde functies van het standaardexemplaar wijzigen?**

   Ja. De SQL Server-installatiemedia bevindt zich in een map op de **C** station. Voer **Setup.exe** van die locatie nieuwe exemplaren van SQL Server toevoegen of wijzigen andere geïnstalleerd functies van SQL Server op de machine. Houd er rekening mee dat sommige functies, zoals automatische back-up automatisch patchen en integratie van Azure Sleutelkluis, alleen op basis van het standaardexemplaar werken.

1. **Kan ik het standaardexemplaar van SQL Server verwijderen?**

   Ja. Maar er zijn enkele overwegingen. Zoals vermeld in het vorige antwoord, functies die afhankelijk zijn van de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md) werkt alleen voor het standaardexemplaar. Als u het standaardexemplaar verwijdert, wordt de extensie blijft zoekt u deze en kan er fouten optreden gebeurtenislogboek. Deze fouten zijn van de volgende twee bronnen: **Microsoft SQL Server-Referentiebeheer** en **Microsoft SQL Server IaaS Agent**. Een van de fouten is mogelijk de volgende strekking:

      Er is een netwerkgerelateerde of exemplaarspecifieke fout opgetreden tijdens het maken van een verbinding met SQL Server. De server is niet gevonden of is niet toegankelijk.

   Als u verwijderen van het standaardexemplaar wilt, ook verwijdert de [uitbreiding voor SQL Server IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md) ook.
   
   >[!NOTE]
   >Een SQL Server Azure virtuele machine wordt in rekening gebracht zoals beschreven in [richtlijnen voor Azure VM's van SQL Server-prijzen](virtual-machines-windows-sql-server-pricing-guidance.md). Als u SQL Server verwijdert, blijven de kosten. Als u SQL Server niet meer nodig hebt, kunt u een nieuwe virtuele machine implementeren en migreren van gegevens en toepassingen naar de nieuwe virtuele machine. Vervolgens kunt u de virtuele machine van SQL Server verwijderen.

1. **Hoe voer ik een upgrade naar een nieuwe versie /-editie van SQL Server in een Azure VM?**

   Er is momenteel geen in-place upgrade voor SQL Server wordt uitgevoerd in een Azure VM. Een nieuwe virtuele machine van Azure maken met de gewenste SQL Server-versie /-editie, en vervolgens uw databases te migreren naar de nieuwe server met behulp van standaard [gegevens migratie technieken](virtual-machines-windows-migrate-sql.md).

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure VM?**

   Er zijn twee manieren om dit te doen. U kunt inrichten een van de [installatiekopieën van virtuele machines die ondersteuning biedt voor licenties](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Een andere optie is de SQL Server-installatiemedia kopiëren naar een Windows Server-VM en installeer vervolgens SQL Server op de virtuele machine. Voor licentieverlening redenen, moet er [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Kan ik een virtuele machine voor het gebruik van mijn eigen SQL Server-licentie als deze is gemaakt van een van de installatiekopieën betalen naar gebruik galerie wijzigen?**

   Nee. U kunt niet overschakelen van het betalen per minuut licentieverlening voor het gebruik van uw eigen licentie. Maak een nieuwe Azure virtuele machine met een van de [BYOL installatiekopieën](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), en vervolgens uw databases te migreren naar de nieuwe server met behulp van standaard [gegevens migratie technieken](virtual-machines-windows-migrate-sql.md).

1. **Worden Failover Cluster exemplaren (FCI) voor SQL Server op Azure Virtual machines ondersteund?**

   Ja. U kunt [maken van een Failover-Cluster van Windows op Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) en opslagruimten Direct (S2D) gebruiken voor de clusteropslag. U kunt ook de clustering of opslag oplossingen van derden gebruiken zoals beschreven in [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

1. **Heb ik betalen naar SQL Server op een Azure VM-licentie als deze alleen wordt gebruikt voor stand-by/failover?**

   Als uw Software Assurance en License Mobility zoals beschreven in [virtuele Machine Veelgestelde vragen over licenties](http://azure.microsoft.com/pricing/licensing-faq/) en er geen licentie voor een SQL Server die als een passieve secundaire replica in de implementatie van een HA deelneemt betalen. Anders moet u een licentie voor het betalen.

1. **Hoe worden updates en servicepacks toegepast op een virtuele machine van SQL Server?**

   Virtuele machines kunt u bepalen de hostmachine, inclusief wanneer en hoe u updates wilt toepassen. Voor het besturingssysteem kunt u windows-updates handmatig toepassen, of u kunt een Scheduler-service aangeroepen inschakelen [automatisch patchen](virtual-machines-windows-sql-automated-patching.md). Automated Patching installeert alle updates die als belangrijk zijn aangeduid, met inbegrip van SQL Server-updates in deze categorie. Overige optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

1. **Is het mogelijk voor het instellen van configuraties niet weergegeven in de galerie met virtuele machine (voor + SQL Server 2012 bijvoorbeeld Windows 2008 R2)?**

   Nee. Voor het galerij-installatiekopieën voor virtuele machine met SQL Server, moet u een van de opgegeven installatiekopieën.

1. **Hoe installeer hulpprogramma's voor SQL-gegevens op mijn Azure VM?**

    Download en installeer de SQL-gegevens hulpprogramma's van [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resources

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Inrichten van een SQL Server Windows VM](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Database migreren naar SQL Server op een virtuele machine in Azure](virtual-machines-windows-migrate-sql.md)
* [Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassing patronen en ontwikkelingsstrategieën voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server virtuele Linux-machine inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [SQL Server op Linux-documentatie](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
