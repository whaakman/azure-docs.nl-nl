---
title: SQL Server op Windows-Machines in de veelgestelde vragen over Azure | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure Virtual machines.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 906cec35c30c277ac1e8bc641d12fe37c29413dd
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427175"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Veelgestelde vragen over SQL Server wordt uitgevoerd op Windows-machines in Azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over het uitvoeren [SQL Server on Windows Virtual Machines in Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> In dit artikel is gericht op problemen die specifiek zijn voor SQL Server op Windows-VM's. Als u SQL Server op virtuele Linux-machines uitvoert, ziet de [Linux Veelgestelde vragen over](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Installatiekopieën

1. **Welke galerie met installatiekopieën van SQL Server virtuele machines zijn beschikbaar?**

   Azure heeft installatiekopieën van virtuele machines voor alle ondersteunde primaire versies van SQL Server op alle edities voor zowel Windows als Linux. Zie voor meer informatie, de volledige lijst van [Windows VM-installatiekopieën](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) en [Linux VM-installatiekopieën](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Zijn bestaande galerie met installatiekopieën van de SQL Server-machines bijgewerkt?**

   Elke twee maanden, SQL Server-installatiekopieën in de galerie met virtuele machines worden bijgewerkt met de meest recente Windows en Linux-updates. Dit omvat alle updates die zijn gemarkeerd als urgent in Windows Update, met inbegrip van belangrijke updates voor SQL Server-beveiliging en servicepacks voor Windows-installatiekopieën. Dit omvat de meest recente updates voor Linux-installatiekopieën. Cumulatieve updates voor SQL Server worden verwerkt voor Linux en Windows. Voor Linux, zijn de cumulatieve updates voor SQL Server ook opgenomen in het vernieuwen. Maar Windows VM's worden op dit moment niet bijgewerkt met cumulatieve updates voor SQL Server of Windows Server.

1. **Kunnen SQL Server-installatiekopieën voor virtuele machines verwijderd uit de galerie?**

   Ja. Azure heeft slechts één afbeelding per primaire versie en editie. Bijvoorbeeld, wanneer een nieuw servicepack van SQL Server wordt uitgebracht, voegt Azure een nieuwe installatiekopie naar de galerie voor die servicepack. De SQL Server-installatiekopie voor de voorafgaande servicepack wordt onmiddellijk verwijderd uit de Azure-portal. Het is echter nog steeds beschikbaar voor het inrichten van PowerShell voor de komende drie maanden. De vorige installatiekopie met service pack is niet meer beschikbaar na drie maanden. Dit verwijderingsbeleid is ook van toepassing als een SQL Server-versie niet ondersteund wordt wanneer het einde van de levenscyclus is bereikt.

1. **Kan ik een VHD-installatiekopie maken van een SQL Server-VM?**

   Ja, maar er zijn enkele overwegingen. Als u deze VHD naar een nieuwe virtuele machine in Azure implementeert, doet u niet ge de sectie configuratie van SQL Server in de portal. U moet dan de SQL Server-configuratie-opties via PowerShell beheren. Ook wordt u gefactureerd voor het tarief van uw installatiekopie oorspronkelijk is gebaseerd op SQL-VM. Dit geldt zelfs als u SQL Server uit de VHD voordat u implementeert verwijderen. 

1. **Is het mogelijk voor het instellen van de configuraties niet weergegeven in de galerie met virtuele machines (voor een voorbeeld van de Windows 2008 R2 en SQL Server 2012)?**

   Nee. Galerie-installatiekopieën voor virtuele machine die SQL Server op te nemen, moet u een van de opgegeven installatiekopieën selecteren.

## <a name="creation"></a>Het maken

1. **Hoe maak ik een virtuele machine van Azure met SQL Server?**

   De eenvoudigste oplossing is het maken van een virtuele Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en het maken van een SQL-VM vanuit de portal, [een SQL Server-machine inrichten in Azure portal](virtual-machines-windows-portal-sql-server-provision.md). Kunt u de installatiekopie van een virtuele machine die gebruikmaakt van betalen-per-seconde SQL Server-licenties, of kunt u een afbeelding waarmee u uw eigen SQL Server-licentie meenemen. U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrij gelicentieerde versie (ontwikkelaar of Express) of door een on-premises licentie opnieuw te gebruiken. Als u uw eigen licentie meenemen, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Hoe kan ik mijn on-premises SQL Server-database migreren naar de Cloud?**

   Eerst een virtuele Azure-machine maken met een SQL Server-exemplaar. Vervolgens uw on-premises databases migreren naar dat exemplaar. Zie voor strategieën voor migratie van gegevens, [een SQL Server-database migreren naar SQL Server in een Azure VM](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure VM?**

   Er zijn twee manieren om dit te doen. U kunt inrichten van een van de [installatiekopieën voor virtuele machines die ondersteuning biedt voor licenties](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), die ook wel bekend als bring-your-own-license (BYOL). Een andere optie is het installatiemedium van SQL Server naar een Windows Server VM kopiëren en vervolgens de installatie van SQL Server op de virtuele machine. Echter, als u SQL Server handmatig installeren, er geen integratie met de portal is en de SQL Server IaaS Agent-extensie wordt niet ondersteund, dus functies zoals automatische back-up en Automated patching uit handen werkt niet in dit scenario. Om deze reden wordt aangeraden een van de galerie met BYOL-installatiekopieën te gebruiken. Voor het gebruik van BYOL- of uw eigen SQL Server-media op een Azure VM, moet u hebben [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Kan ik een virtuele machine voor het gebruik van mijn eigen SQL Server-licentie, als deze is gemaakt op basis van een van de galerie met betalen per gebruik-installatiekopieën wijzigen?**

   Nee. U kunt niet overstappen van het betalen-per-seconde voor het gebruik van uw eigen licentie-licentieverlening. Maak een nieuwe Azure-machine met behulp van een van de [BYOL-installatiekopieën](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), en vervolgens migreren van uw databases naar de nieuwe server met behulp van standaard [gegevensmigratietechnieken](virtual-machines-windows-migrate-sql.md).

1. **Heb ik betalen voor de licentie voor SQL Server op een Azure-VM als deze alleen wordt gebruikt voor de stand-by/failover?**

   Als u Software Assurance en License Mobility gebruiken zoals wordt beschreven in [virtuele Machine Veelgestelde vragen over licenties](http://azure.microsoft.com/pricing/licensing-faq/) en u niet hoeft te betalen voor een licentie op één SQL Server die als een passieve secundaire replica in de implementatie van een HA deelneemt. Anders moet u deze licentie te betalen.


## <a name="administration"></a>Beheer

1. **Kan ik een tweede exemplaar van SQL Server installeren op dezelfde virtuele machine? Kan ik de geïnstalleerde functies van het standaardexemplaar wijzigen?**

   Ja. De SQL Server-installatiemedia bevindt zich in een map op de **C** station. Voer **Setup.exe** vanaf die locatie naar het nieuwe SQL Server-exemplaren toe te voegen of te wijzigen andere geïnstalleerd functies van SQL Server op de machine. Houd er rekening mee dat sommige functies, zoals geautomatiseerde back-ups, automatisch patchen en Azure Key Vault-integratie, alleen moet worden uitgevoerd op basis van het standaardexemplaar plaatst.

1. **Kan ik het standaardexemplaar van SQL Server verwijderen?**

   Ja, maar er zijn enkele overwegingen. Zoals vermeld in het vorige antwoord, functies die afhankelijk zijn van de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) werken alleen op het standaardexemplaar plaatst. Als u het standaardexemplaar verwijdert, wordt de extensie blijft het zoeken naar en gebeurtenislogboek kunt genereren. Deze fouten zijn uit de volgende twee bronnen: **Microsoft SQL Server-Referentiebeheer** en **Microsoft SQL Server IaaS Agent**. Het is mogelijk dat een van de fouten die vergelijkbaar is met het volgende:

      Er is een netwerk- of instantiefout-fout opgetreden tijdens het maken van een verbinding met SQL Server. De server is niet gevonden of is niet toegankelijk.

   Als u besluit het standaardexemplaar verwijderen, verwijdert ook de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) ook.

1. **Kan ik SQL Server geheel verwijderen uit een SQL-VM?**

   Ja, maar blijft u kosten in rekening gebracht voor uw SQL-VM wordt beschreven in [prijsinformatie voor SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md). Als u SQL Server niet meer nodig hebt, kunt u een nieuwe virtuele machine implementeren en migreren van de gegevens en toepassingen naar de nieuwe virtuele machine. Vervolgens kunt u de SQL Server-machine.
   
## <a name="updating-and-patching"></a>Updates en patches

1. **Hoe voer ik een upgrade naar een nieuwe versie/editie van SQL Server in een Azure VM?**

   Er is momenteel geen in-place upgrade voor SQL Server in een Azure-VM wordt uitgevoerd. Een nieuwe Azure-machine maken met de gewenste versie/editie van SQL Server en migreer de databases naar de nieuwe server met behulp van standaard [gegevensmigratietechnieken](virtual-machines-windows-migrate-sql.md).

1. **Hoe worden updates en servicepacks toegepast op een SQL Server-VM?**

   Virtuele machines geven u controle over de hostmachine, inclusief wanneer en hoe u updates wilt toepassen. Voor het besturingssysteem, kunt u windows-updates handmatig toepassen of u kunt de planningsservice inschakelen [automatisch patchen](virtual-machines-windows-sql-automated-patching.md). Automated Patching installeert alle updates die als belangrijk zijn aangeduid, met inbegrip van SQL Server-updates in deze categorie. Overige optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

## <a name="general"></a>Algemeen

1. **SQL Server Failover Cluster Instances (FCI) is worden ondersteund op Azure Virtual machines?**

   Ja. U kunt [maken van een Windows-failovercluster op Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) en Storage Spaces Direct (S2D) gebruiken voor de clusteropslag. U kunt ook de clustering of opslag oplossingen van derden gebruiken zoals wordt beschreven in [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Op dit moment de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) wordt niet ondersteund voor FCI van SQL Server op Azure. Het is raadzaam dat u de extensie van virtuele machines die deel uitmaken van de FCI verwijderen. Deze uitbreiding biedt ondersteuning voor functies, zoals geautomatiseerde back-ups en Patching en sommige portal functies voor SQL. Deze functies werken niet voor SQL-VM's nadat de agent is verwijderd.

1. **Wat is het verschil tussen de SQL-VM's en de service SQL Database?**

   Conceptueel gezien die SQL Server op een virtuele machine van Azure niet die verschilt van het SQL Server wordt uitgevoerd in een extern datacenter. Daarentegen [SQL-Database](../../../sql-database/sql-database-technical-overview.md) database-as-a-service biedt. Met SQL Database hebt u geen toegang tot de machines die uw databases hosten. Zie voor een volledige vergelijking [kiest u een SQL Server-cloudoptie: Azure SQL (PaaS) Database of SQL Server op Azure Virtual machines (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hoe installeer ik hulpprogramma's voor SQL-gegevens op mijn Azure-VM?**

    Download en installeer de SQL-gegevens-hulpprogramma's van [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resources

**Windows-VM's**:

* [Overzicht van SQL Server op een Windows VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Een SQL Server Windows VM inrichten](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Database migreren naar SQL Server op een Azure-VM](virtual-machines-windows-migrate-sql.md)
* [Hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassingspatronen en ontwikkelingsstrategieën voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een virtuele Linux-machine](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server Linux-VM inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen over (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
