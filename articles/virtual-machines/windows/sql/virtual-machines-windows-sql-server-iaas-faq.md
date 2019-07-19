---
title: Veelgestelde vragen over Windows Virtual Machines in Azure SQL Server | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het uitvoeren van SQL Server op Azure-Vm's.
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
ms.author: mathoma
ms.openlocfilehash: 7f6ec1ee65727fb8c3c7d98f696c288e95ec880a
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876198"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-virtual-machines-in-azure"></a>Veelgestelde vragen over SQL Server die worden uitgevoerd op virtuele Windows-machines in azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

In dit artikel vindt u antwoorden op enkele van de meest voorkomende vragen over het uitvoeren van [SQL Server op Windows virtual machines in azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Dit artikel is gericht op problemen die specifiek zijn voor SQL Server op Windows-Vm's. Als u SQL Server on Linux Vm's uitvoert, raadpleegt u de [Veelgestelde vragen over Linux](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a>Installatie kopieën

1. **Wat SQL Server afbeeldingen van de galerie met virtuele machines zijn beschikbaar?** 

   Azure beheert installatie kopieën van virtuele machines voor alle ondersteunde grote releases van SQL Server op alle edities van Windows en Linux. Zie de volledige lijst met [Windows VM-installatie](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) kopieën en [Linux VM-installatie kopieën](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create)voor meer informatie.

1. **Zijn bestaande SQL Server afbeeldingen van de galerie met virtuele machines bijgewerkt?**

   SQL Server installatie kopieën in de galerie met virtuele machines worden elke twee maanden bijgewerkt met de nieuwste updates voor Windows en Linux. Voor Windows-installatie kopieën omvat dit alle updates die zijn gemarkeerd als belang rijk in Windows Update, met inbegrip van belang rijke SQL Server beveiligings updates en-service packs. Voor Linux-installatie kopieën bevat dit de meest recente systeem updates. SQL Server cumulatieve updates worden anders afgehandeld voor Linux en Windows. Voor Linux worden SQL Server cumulatieve updates ook opgenomen in het vernieuwen. Op dit moment worden Windows-Vm's niet bijgewerkt met SQL Server of cumulatieve updates voor Windows Server.

1. **Kunnen SQL Server installatie kopieën van virtuele machines worden verwijderd uit de galerie?**

   Ja. In azure wordt slechts één installatie kopie per primaire versie en editie bewaard. Wanneer bijvoorbeeld een nieuw SQL Server Service Pack wordt uitgebracht, voegt Azure een nieuwe installatie kopie toe aan de galerie voor dat Service Pack. De SQL Server-installatie kopie voor het vorige Service Pack wordt direct verwijderd uit de Azure Portal. Het is echter nog steeds beschikbaar voor het inrichten van Power shell voor de volgende drie maanden. Na drie maanden is de vorige installatie kopie van het Service Pack niet meer beschikbaar. Dit verwijderings beleid is ook van toepassing als een SQL Server-versie niet wordt ondersteund wanneer het einde van de levens cyclus wordt bereikt.


1. **Is het mogelijk om een oudere installatie kopie te implementeren van SQL Server die niet zichtbaar is in de Azure Portal?**

   Ja, met behulp van Power shell. Zie [SQL Server virtuele machines inrichten met Azure PowerShell](virtual-machines-windows-ps-sql-create.md)voor meer informatie over het implementeren van SQL Server-vm's met behulp van Power shell.

1. **Kan ik een VHD-installatie kopie maken van een SQL Server VM?**

   Ja, maar er zijn enkele overwegingen. Als u deze VHD implementeert op een nieuwe virtuele machine in azure, wordt de configuratie sectie SQL Server niet weer geven in de portal. Vervolgens moet u de SQL Server configuratie opties beheren via Power shell. Ook worden er kosten in rekening gebracht voor het tarief van de virtuele SQL-machine waarop uw installatie kopie oorspronkelijk is gebaseerd. Dit geldt ook als u SQL Server van de VHD verwijdert voordat u implementeert. 

1. **Is het mogelijk configuraties in te stellen die niet worden weer gegeven in de galerie met virtuele machines (bijvoorbeeld Windows 2008 R2 + SQL Server 2012)?**

   Nee. Voor installatie kopieën in de galerie met virtuele machines die SQL Server bevatten, moet u een van de installatie kopieën selecteren via de Azure Portal of via [Power shell](virtual-machines-windows-ps-sql-create.md). 


## <a name="creation"></a>Zelf

1. **Hoe kan ik een virtuele machine van Azure met SQL Server maken?**

   De eenvoudigste oplossing is het maken van een virtuele machine die SQL Server bevat. Zie [een SQL Server virtuele machine inrichten in de Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)voor een zelf studie over het aanmelden voor Azure en het maken van een SQL-VM vanuit de portal. U kunt een installatie kopie van een virtuele machine selecteren die gebruikmaakt van betalen per seconde SQL Server-licentie verlening, of u kunt een installatie kopie gebruiken waarmee u uw eigen SQL Server-licentie meebrengt. U hebt ook de mogelijkheid om SQL Server hand matig te installeren op een virtuele machine met een gratis licentie (ontwikkelaar of Express) of door een on-premises licentie opnieuw te gebruiken. Als u uw eigen licentie meebrengt, moet u beschikken over [License Mobility via Software Assurance op Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Hoe kan ik mijn on-premises SQL Server Data Base naar de Cloud migreren?**

   Maak eerst een virtuele Azure-machine met een SQL Server-exemplaar. Migreer vervolgens uw on-premises data bases naar dat exemplaar. Zie [een SQL Server-Data Base migreren naar SQL Server in een Azure-VM](virtual-machines-windows-migrate-sql.md)voor strategieën voor gegevens migratie.

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure-VM?**

   Er zijn twee manieren om dit te doen. U kunt een van de [installatiekopieën van de VM inrichten die ondersteuning biedt voor licenties](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Dit staat ook bekend als een BYOL (Bring-Your-Own-License). Een andere optie is om SQL Server-installatiemedia te kopiëren naar een Windows Server-VM, en vervolgens SQL Server te installeren op de VM. Als u SQL Server echter handmatig installeert, is er geen integratie met de portal en wordt de extensie voor de SQL Server IaaS-agent niet ondersteund. Hierdoor werken functies zoals Geautomatiseerd patchen niet in dit scenario. Daarom raden we u aan een van de installatiekopieën uit de BYOL-galerie te gebruiken. Als u BYOL of uw eigen SQL Server-media op een virtuele machine van Azure wilt gebruiken, moet u [License Mobility via Software Assurance hebben op Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Moet ik betalen om een SQL-server een licentie te verlenen op een Azure-VM, als deze SQL-server alleen wordt gebruikt als stand-by of om een failover uit te voeren?**

   Als u Software Assurance hebt en License Mobility gebruikt zoals beschreven in de [Veelgestelde vragen over de licentie verlening van de virtuele machine](https://azure.microsoft.com/pricing/licensing-faq/), hoeft u niet te betalen voor een licentie die SQL server deelneemt aan een passieve secundaire replica in een ha-implementatie. In andere gevallen moet u betalen voor deze licentie.

1. **Kan ik een VM wijzigen zodat mijn eigen SQL Server-licentie wordt gebruikt, wanneer de VM is gemaakt vanuit een van de Betalen per gebruik-installatiekopieën uit de galerie?**

   Ja. U kunt eenvoudig tussen de twee licentie modellen verplaatsen, als u oorspronkelijk hebt begonnen met een galerij afbeelding met betalen per gebruik. U kunt uw licentie echter niet wijzigen in een Betalen per gebruik-licentie als u oorspronkelijk bent begonnen met een BYOL-installatiekopie. Zie [How to Change the License model for a SQL Server VM](virtual-machines-windows-sql-ahb.md)(Engelstalig) voor meer informatie.

   > [!Note]
   > Deze faciliteit is momenteel alleen beschikbaar voor klanten met een open bare Cloud.

1. **Kan ik het beste BYOL-installatiekopieën of SQL VM RP gebruiken om een nieuwe SQL VM te maken?**

   BYOL-installatiekopieën (Bring-Your-Own-License) zijn alleen beschikbaar voor EA-klanten. Andere klanten met Software Assurance moeten de resource provider van de SQL-VM gebruiken om een SQL-VM te maken met [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/licensing-faq/). 

1. **Treedt er downtime op voor SQL Server tijdens het schakelen tussen licentiemodellen?**

   Nee. [Het wijzigen van het licentie model](virtual-machines-windows-sql-ahb.md) vereist geen uitval tijd voor SQL Server, omdat de wijziging onmiddellijk van kracht is en het opnieuw opstarten van de virtuele machine niet vereist is. Als u uw SQL Server-VM echter wilt registreren bij de resource provider van de SQL-VM, is de [SQL IaaS-uitbrei ding](virtual-machines-windows-sql-server-agent-extension.md) een vereiste en installeert u de SQL IaaS-extensie, wordt de SQL Server-service opnieuw gestart. Daarom kunt u de SQL IaaS-extensie, indien nodig, beter installeren tijdens een onderhoudsvenster. 

1. **Kunnen CSP-abonnementen de Azure Hybrid Benefit activeren?**

   Ja, de Azure Hybrid Benefit is beschikbaar voor CSP-abonnementen. CSP-klanten moeten eerst een betalen per gebruik-installatie kopie implementeren en vervolgens [het licentie model wijzigen](virtual-machines-windows-sql-ahb.md) in uw eigen licentie.  

1. **Brengt het registreren van mijn VM bij de nieuwe SQL VM-resourceprovider extra kosten met zich mee?**

   Nee. De SQL VM-resourceprovider zorgt alleen voor extra beheerbaarheid voor SQL Server op een Azure-VM, zonder extra kosten. 

1. **Is de SQL VM-resourceprovider beschikbaar voor alle klanten?**
 
   Ja. Alle klanten kunnen zich registreren bij de nieuwe SQL VM-resourceprovider. Alleen klanten met het voor deel van Software Assurance kunnen de [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) (of BYOL) ook activeren op een SQL Server virtuele machine. 

1. **Wat gebeurt er met de _micro soft. SqlVirtualMachine_ -resource als de VM-resource wordt verplaatst of verwijderd?** 

   Wanneer de micro soft. Compute/VirtualMachine-resource wordt verwijderd of verplaatst, wordt de bijbehorende micro soft. SqlVirtualMachine-resource op de hoogte gesteld om de bewerking asynchroon te repliceren.

1. **Wat gebeurt er met de virtuele machine als de _micro soft. SqlVirtualMachine_ -resource wordt verwijderd?**

    De resource micro soft. Compute/VirtualMachine wordt niet beïnvloed wanneer de micro soft. SqlVirtualMachine-resource wordt verwijderd. De licentie wijzigingen worden echter standaard teruggezet naar de oorspronkelijke installatie kopie bron. 

1. **Is het mogelijk om zelf geïmplementeerde SQL Server-VM's te registreren bij de SQL VM-resourceprovider?**

    Ja. Als u SQL Server vanaf uw eigen media hebt geïmplementeerd en de SQL IaaS-extensie hebt geïnstalleerd, kunt u uw SQL Server VM registreren bij de resource provider om de beheer baarheid van de SQL IaaS-uitbrei ding te verkrijgen. U kunt een zelf geïmplementeerde SQL-VM echter niet converteren naar Betalen per gebruik.

## <a name="administration"></a>Beheer

1. **Kan ik een tweede exemplaar van een SQL Server op dezelfde VM installeren? Kan ik geïnstalleerde functies van het standaardexemplaar wijzigen?**

   Ja. De SQL Server-installatie media bevindt zich in een map op station **C** . Voer **Setup. exe** vanaf die locatie uit om nieuwe exemplaren van SQL Server toe te voegen of om andere geïnstalleerde functies van SQL Server op de computer te wijzigen. Houd er rekening mee dat sommige functies, zoals geautomatiseerde back-ups, automatische patching en Azure Key Vault integratie, alleen kunnen worden uitgevoerd op het standaard exemplaar of een benoemd exemplaar dat correct is geconfigureerd (zie vraag 3). 

1. **Kan ik het standaardexemplaar van SQL Server verwijderen?**

   Ja, maar er is een aantal overwegingen. Zoals vermeld in het vorige antwoord zijn er functies die afhankelijk zijn van de [SQL Server IaaS agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).  Als u het standaard exemplaar verwijdert zonder de IaaS-extensie te verwijderen, blijft de uitbrei ding ernaar zoeken en worden fouten in het gebeurtenis logboek gegenereerd. Deze fouten komen uit een van de volgende twee bronnen: **Microsoft SQL Server referentie beheer** en **Microsoft SQL Server IaaS-agent**. Een van de fouten lijkt mogelijk op de volgende:

      Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk.

   Als u besluit om het standaard exemplaar te verwijderen, moet u ook de uitbrei ding voor de [SQL Server IaaS-agent](virtual-machines-windows-sql-server-agent-extension.md) verwijderen.

1. **Kan ik een benoemd exemplaar van SQL Server met de extensie IaaS gebruiken**?
   
   Ja, als het benoemde exemplaar het enige exemplaar op het SQL Server is, en als het oorspronkelijke standaard exemplaar [correct is verwijderd](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md#installation). Als er geen standaard exemplaar is en er meerdere benoemde exemplaren op één SQL Server virtuele machine zijn, kan de IaaS-extensie niet worden geïnstalleerd. 

1. **Kan ik SQL Server volledig verwijderen uit een SQL-VM?**

   Ja, maar er worden nog steeds kosten in rekening gebracht voor uw SQL-VM, zoals beschreven in [prijs informatie voor SQL Server Azure-vm's](virtual-machines-windows-sql-server-pricing-guidance.md). Als u de SQL Server niet langer nodig hebt, kunt u een nieuwe virtuele machine implementeren en de gegevens en toepassingen migreren naar de nieuwe virtuele machine. Vervolgens kunt u de virtuele SQL Server-machine verwijderen.
   
## <a name="updating-and-patching"></a>Updates en patches

1. **Hoe kan ik overschakelen naar een andere versie of editie van de SQL Server in een Azure VM?**

   Klanten kunnen hun versie/editie van SQL Server wijzigen door gebruik te maken van de installatie media die de gewenste versie of editie van SQL Server bevatten. Zodra de versie is gewijzigd, gebruikt u de Azure Portal om de eigenschap van de editie van de virtuele machine te wijzigen zodat deze nauw keurig overeenkomt met de facturering voor de virtuele machine. Zie [Change Edition of a SQL Server VM](virtual-machines-windows-sql-change-edition.md)(Engelstalig) voor meer informatie. 


1. **Hoe worden updates en service packs toegepast op een SQL Server virtuele machine?**

   virtuele machines geven u controle over de hostmachine, inclusief wanneer en hoe u updates wilt toepassen. Voor het besturings systeem kunt u hand matig Windows-updates Toep assen of kunt u een plannings service met de naam [geautomatiseerde patching](virtual-machines-windows-sql-automated-patching.md)inschakelen. Automated Patching installeert alle updates die als belangrijk zijn aangeduid, met inbegrip van SQL Server-updates in deze categorie. Overige optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

## <a name="general"></a>Algemeen

1. **Worden SQL Server-FCI (failover cluster instances) ondersteund op virtuele machines van Azure?**

   Ja. U kunt [een Windows-failovercluster maken op Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) en OPSLAGRUIMTEN direct (S2D) gebruiken voor de cluster opslag. U kunt ook clustering-of opslag oplossingen van derden gebruiken, zoals beschreven in [hoge Beschik baarheid en herstel na nood gevallen voor SQL Server in Azure virtual machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Op dit moment wordt de [SQL Server IaaS agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) niet ondersteund voor SQL Server FCI op Azure. We raden u aan de uitbrei ding te verwijderen van Vm's die deel uitmaken van de FCI. Deze uitbrei ding ondersteunt functies, zoals automatische back-ups en patches en bepaalde portal functies voor SQL. Deze functies werken niet voor SQL-Vm's nadat de agent is verwijderd.

1. **Wat is het verschil tussen de virtuele machines van SQL en de SQL Database Service?**

   In het algemeen is het uitvoeren van SQL Server op een virtuele machine van Azure niet hetzelfde als het uitvoeren van SQL Server in een extern Data Center. [SQL database](../../../sql-database/sql-database-technical-overview.md) biedt daarentegen data base-as-a-service. Met SQL Database hebt u geen toegang tot de computers die als host fungeren voor uw data bases. Zie [een Cloud SQL Server kiezen voor een volledige vergelijking: Azure SQL-data base (PaaS) of SQL Server op Azure Vm's (](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)IaaS).

1. **Hoe kan ik SQL data-hulpprogram ma's installeren op mijn Azure VM?**

    Down load en installeer de hulpprogram ma's voor SQL-gegevens van [Microsoft SQL Server Data tools-Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313).

1. **Worden gedistribueerde trans acties met MSDTC ondersteund op SQL Server Vm's?**
   
    Ja. Lokale DTC wordt ondersteund voor SQL Server 2016 SP2 en hoger. Toepassingen moeten echter worden getest wanneer er gebruik wordt gemaakt van AlwaysOn-beschikbaarheids groepen, omdat trans acties in de vlucht tijdens een failover mislukken en opnieuw moeten worden geprobeerd. Geclusterde DTC is beschikbaar vanaf Windows Server 2019. 

## <a name="resources"></a>Resources

**Virtuele Windows-machines**:

* [Overzicht van SQL Server op een Windows-VM](virtual-machines-windows-sql-server-iaas-overview.md).
* [Een SQL Server Windows-VM inrichten](virtual-machines-windows-portal-sql-server-provision.md)
* [Een Data Base migreren naar SQL Server op een virtuele Azure-machine](virtual-machines-windows-migrate-sql.md)
* [Hoge Beschik baarheid en herstel na nood gevallen voor SQL Server in azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](virtual-machines-windows-sql-performance.md)
* [Toepassings patronen en ontwikkelings strategieën voor SQL Server in azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Virtuele Linux-machines**:

* [Overzicht van SQL Server op een Linux-VM](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Een SQL Server Linux-VM inrichten](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Veelgestelde vragen (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentatie voor SQL Server op Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
