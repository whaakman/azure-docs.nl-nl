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
ms.openlocfilehash: 0688235d928584df223a3a6a6ca2821282e4cb92
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762681"
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


1. **Is het mogelijk om de installatiekopie van een oudere van SQL Server die wordt niet weergegeven in de Azure-portal implementeren?**

   Ja, met behulp van PowerShell. Zie voor meer informatie over het implementeren van SQL Server-VM's met behulp van PowerShell [over het inrichten van SQL Server-machines met Azure PowerShell](virtual-machines-windows-ps-sql-create.md).

1. **Kan ik een VHD-installatiekopie maken van een SQL Server-VM?**

   Ja, maar er zijn enkele overwegingen. Als u deze VHD naar een nieuwe virtuele machine in Azure implementeert, doet u niet ge de sectie configuratie van SQL Server in de portal. U moet dan de SQL Server-configuratie-opties via PowerShell beheren. Ook wordt u gefactureerd voor het tarief van uw installatiekopie oorspronkelijk is gebaseerd op SQL-VM. Dit geldt zelfs als u SQL Server uit de VHD voordat u implementeert verwijderen. 

1. **Is het mogelijk voor het instellen van de configuraties niet weergegeven in de galerie met virtuele machines (voor een voorbeeld van de Windows 2008 R2 en SQL Server 2012)?**

   Nee. Galerie-installatiekopieën voor virtuele machine die SQL Server op te nemen, moet u een van de opgegeven installatiekopieën via Azure portal of via [PowerShell](virtual-machines-windows-ps-sql-create.md). 


## <a name="creation"></a>Het maken

1. **Hoe maak ik een virtuele machine van Azure met SQL Server?**

   De eenvoudigste oplossing is het maken van een virtuele Machine met SQL Server. Zie voor een zelfstudie over het aanmelden voor Azure en het maken van een SQL-VM vanuit de portal, [een SQL Server-machine inrichten in Azure portal](virtual-machines-windows-portal-sql-server-provision.md). Kunt u de installatiekopie van een virtuele machine die gebruikmaakt van betalen-per-seconde SQL Server-licenties, of kunt u een afbeelding waarmee u uw eigen SQL Server-licentie meenemen. U hebt ook de optie van het handmatig installeren van SQL Server op een virtuele machine met ofwel een vrij gelicentieerde versie (ontwikkelaar of Express) of door een on-premises licentie opnieuw te gebruiken. Als u uw eigen licentie meenemen, hebt u [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.

1. **Hoe kan ik mijn on-premises SQL Server-database migreren naar de Cloud?**

   Eerst een virtuele Azure-machine maken met een SQL Server-exemplaar. Vervolgens uw on-premises databases migreren naar dat exemplaar. Zie voor strategieën voor migratie van gegevens, [een SQL Server-database migreren naar SQL Server in een Azure VM](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licentieverlening

1. **Hoe kan ik mijn gelicentieerd exemplaar van SQL Server installeren op een Azure-VM?**

   Er zijn twee manieren om dit te doen. U kunt een van de [installatiekopieën van de VM inrichten die ondersteuning biedt voor licenties](virtual-machines-windows-sql-server-iaas-overview.md#BYOL). Dit staat ook bekend als een BYOL (Bring-Your-Own-License). Een andere optie is om SQL Server-installatiemedia te kopiëren naar een Windows Server-VM, en vervolgens SQL Server te installeren op de VM. Als u SQL Server echter handmatig installeert, is er geen integratie met de portal en wordt de extensie voor de SQL Server IaaS-agent niet ondersteund. Hierdoor werken functies zoals Geautomatiseerd patchen niet in dit scenario. Daarom raden we u aan een van de installatiekopieën uit de BYOL-galerie te gebruiken. Voor het gebruik van BYOL- of uw eigen SQL Server-media op een Azure VM, moet u hebben [License Mobility through Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/). Zie [Pricing guidance for SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md) (Prijsrichtlijnen voor SQL Server Azure VM's) voor meer informatie.


1. **Moet ik betalen om een SQL-server een licentie te verlenen op een Azure-VM, als deze SQL-server alleen wordt gebruikt als stand-by of om een failover uit te voeren?**

   Als u Software Assurance en License Mobility gebruiken zoals wordt beschreven in [virtuele Machine Veelgestelde vragen over licenties](https://azure.microsoft.com/pricing/licensing-faq/), en u niet hoeft te betalen voor een licentie op één SQL Server die als een passieve secundaire replica in de implementatie van een HA deelneemt. In andere gevallen moet u betalen voor deze licentie.

1. **Kan ik een VM wijzigen zodat mijn eigen SQL Server-licentie wordt gebruikt, wanneer de VM is gemaakt vanuit een van de Betalen per gebruik-installatiekopieën uit de galerie?**

   Ja. U kunt eenvoudig verplaatsen tussen de twee licentiemodellen verplaatsen als u oorspronkelijk hebt gestart met de installatiekopie van een galerie met betalen per gebruik. U kunt uw licentie echter niet wijzigen in een Betalen per gebruik-licentie als u oorspronkelijk bent begonnen met een BYOL-installatiekopie. Zie voor meer informatie, [de licentiemodel voor een virtuele machine van SQL Server wijzigen](virtual-machines-windows-sql-ahb.md).

   > [!Note]
   > Deze functie is op dit moment alleen beschikbaar voor klanten van de openbare Cloud.

1. **Kan ik het beste BYOL-installatiekopieën of SQL VM RP gebruiken om een nieuwe SQL VM te maken?**

   BYOL-installatiekopieën (Bring-Your-Own-License) zijn alleen beschikbaar voor EA-klanten. Andere klanten met Software Assurance moeten de SQL-VM-resourceprovider gebruiken om te maken van een SQL-VM met [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/licensing-faq/). 

1. **Treedt er downtime op voor SQL Server tijdens het schakelen tussen licentiemodellen?**

   Nee. [Wijzigen van de licentiemodel](virtual-machines-windows-sql-ahb.md) vereist geen uitvaltijd voor SQL Server als de wijziging wordt onmiddellijk van kracht en niet opnieuw opstarten van de virtuele machine vereist. Echter, voor het registreren van uw SQL Server-VM met de SQL-VM-resourceprovider, de [SQL IaaS-extensie](virtual-machines-windows-sql-server-agent-extension.md) is een vereiste en installeren van de SQL IaaS-extensie de SQL Server-service opnieuw is opgestart. Daarom kunt u de SQL IaaS-extensie, indien nodig, beter installeren tijdens een onderhoudsvenster. 

1. **Kunnen CSP-abonnementen Azure Hybrid Benefit activeren?**

   Ja, Azure Hybrid Benefit is beschikbaar voor CSP-abonnementen. CSP-klanten moeten eerst een betalen per gebruik-installatiekopie, implementeren en vervolgens [wijzigen van de licentiemodel](virtual-machines-windows-sql-ahb.md) voor bring-your-own-license.  

1. **Brengt het registreren van mijn VM bij de nieuwe SQL VM-resourceprovider extra kosten met zich mee?**

   Nee. De SQL VM-resourceprovider zorgt alleen voor extra beheerbaarheid voor SQL Server op een Azure-VM, zonder extra kosten. 

1. **Is de SQL VM-resourceprovider beschikbaar voor alle klanten?**
 
   Ja. Alle klanten kunnen zich registreren bij de nieuwe SQL VM-resourceprovider. Echter alleen klanten met Software Assurance-voordeel kunnen activeren de [Azure Hybrid Benefit (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/) (of byol-model) op een virtuele machine van SQL Server. 

1. **Wat gebeurt er met de _Microsoft.SqlVirtualMachine_ resource als de VM-resource is verplaatst of verwijderd?** 

   Wanneer de Microsoft.Compute/VirtualMachine-resource is verwijderd of verplaatst, en vervolgens de gekoppelde Microsoft.SqlVirtualMachine-resource is op de hoogte gesteld aan de bewerking asynchroon te repliceren.

1. **Wat gebeurt er met de virtuele machine als de _Microsoft.SqlVirtualMachine_ resource wordt verwijderd?**

    De resource Microsoft.Compute/VirtualMachine wordt niet negatief beïnvloed wanneer de Microsoft.SqlVirtualMachine-resource is verwijderd. De licentie wijzigingen wordt echter standaard terug naar de oorspronkelijke bron van installatiekopie. 

1. **Is het mogelijk om zelf geïmplementeerde SQL Server-VM's te registreren bij de SQL VM-resourceprovider?**

    Ja. Als u SQL Server vanuit uw eigen media geïmplementeerd en de SQL IaaS-extensie die kunt u uw SQL Server-machine registreren met de resourceprovider om op te halen van de beheerbaarheidsvoordelen geleverd door de SQL IaaS-extensie geïnstalleerd. U kunt een zelf geïmplementeerde SQL VM echter niet converteren naar Betalen per gebruik.

## <a name="administration"></a>Beheer

1. **Kan ik een tweede exemplaar van een SQL Server op dezelfde VM installeren? Kan ik geïnstalleerde functies van het standaardexemplaar wijzigen?**

   Ja. De SQL Server-installatiemedia bevindt zich in een map op de **C** station. Voer **Setup.exe** vanaf die locatie naar het nieuwe SQL Server-exemplaren toe te voegen of te wijzigen andere geïnstalleerd functies van SQL Server op de machine. Houd er rekening mee dat sommige functies, zoals geautomatiseerde back-ups, automatisch patchen en Azure Key Vault-integratie, alleen op basis van het standaardexemplaar werken of een benoemd exemplaar als waarvan correct is geconfigureerd (Zie vraag 3). 

1. **Kan ik het standaardexemplaar van SQL Server verwijderen?**

   Ja, maar er is een aantal overwegingen. Zoals vermeld in het vorige antwoord, er zijn functies die afhankelijk van zijn de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md).  Als u het standaardexemplaar zonder te verwijderen van de IaaS-extensie ook verwijdert, wordt de extensie blijft het zoeken naar en gebeurtenislogboek kunt genereren. Deze fouten komen uit een van de volgende twee bronnen: **Microsoft SQL Server-Referentiebeheer** en **Microsoft SQL Server IaaS Agent**. Een van de fouten lijkt mogelijk op de volgende:

      Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk.

   Als u besluit het standaardexemplaar verwijderen, verwijdert ook de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) ook.

1. **Kan ik een benoemd exemplaar van SQL Server gebruiken met de extensie IaaS**?
   
   Ja, als het benoemde exemplaar is het enige exemplaar op de SQL Server, en als de oorspronkelijke standaardexemplaar is [correct verwijderd](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md#installation). Als er geen standaardexemplaar is en er meerdere benoemde exemplaren op een enkele virtuele machine voor SQL-Server zijn, mislukt de IaaS-extensie te installeren. 

1. **Kan ik SQL Server volledig verwijderen uit een SQL-VM?**

   Ja, maar blijft u kosten in rekening gebracht voor uw SQL-VM wordt beschreven in [prijsinformatie voor SQL Server Azure VMs](virtual-machines-windows-sql-server-pricing-guidance.md). Als u de SQL Server niet langer nodig hebt, kunt u een nieuwe virtuele machine implementeren en de gegevens en toepassingen migreren naar de nieuwe virtuele machine. Vervolgens kunt u de virtuele SQL Server-machine verwijderen.
   
## <a name="updating-and-patching"></a>Updates en patches

1. **Hoe wijzig ik naar een nieuwe versie/editie van SQL Server in een Azure VM?**

   Klanten met Software Assurance kunnen in-place upgrades van de SQL-Server die wordt uitgevoerd op een virtuele Azure-machine met behulp van de installatiemedia in de Portal voor Volume Licensing. Echter is, er momenteel geen manier om te wijzigen van de editie van een exemplaar van SQL Server. Een nieuwe Azure-machine maken met de gewenste editie van SQL Server en migreer de databases naar de nieuwe server met behulp van standaard [gegevensmigratietechnieken](virtual-machines-windows-migrate-sql.md).

1. **Hoe worden updates en servicepacks toegepast op een SQL Server-VM?**

   virtuele machines geven u controle over de hostmachine, inclusief wanneer en hoe u updates wilt toepassen. Voor het besturingssysteem, kunt u windows-updates handmatig toepassen of u kunt de planningsservice inschakelen [automatisch patchen](virtual-machines-windows-sql-automated-patching.md). Automated Patching installeert alle updates die als belangrijk zijn aangeduid, met inbegrip van SQL Server-updates in deze categorie. Overige optionele updates voor SQL Server moeten handmatig worden geïnstalleerd.

## <a name="general"></a>Algemeen

1. **SQL Server Failover Cluster Instances (FCI) is worden ondersteund op Azure Virtual machines?**

   Ja. U kunt [maken van een Windows-failovercluster op Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) en Storage Spaces Direct (S2D) gebruiken voor de clusteropslag. U kunt ook de clustering of opslag oplossingen van derden gebruiken zoals wordt beschreven in [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Op dit moment de [SQL Server IaaS Agent-extensie](virtual-machines-windows-sql-server-agent-extension.md) wordt niet ondersteund voor FCI van SQL Server op Azure. Het is raadzaam dat u de extensie van virtuele machines die deel uitmaken van de FCI verwijderen. Deze uitbreiding biedt ondersteuning voor functies, zoals geautomatiseerde back-ups en Patching en sommige portal functies voor SQL. Deze functies werken niet voor SQL-VM's nadat de agent is verwijderd.

1. **Wat is het verschil tussen de SQL-VM's en de service SQL Database?**

   Conceptueel gezien die SQL Server op een virtuele machine van Azure niet die verschilt van het SQL Server wordt uitgevoerd in een extern datacenter. Daarentegen [SQL-Database](../../../sql-database/sql-database-technical-overview.md) database-as-a-service biedt. Met SQL Database hebt u geen toegang tot de machines die uw databases hosten. Zie voor een volledige vergelijking [een SQL Server-cloudoptie kiezen: Azure SQL (PaaS) Database of SQL Server op Azure VM's (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Hoe installeer ik hulpprogramma's voor SQL-gegevens op mijn Azure-VM?**

    Download en installeer de SQL-gegevens-hulpprogramma's van [Microsoft SQL Server Data Tools - Business Intelligence voor Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

1. **Gedistribueerde transacties worden bij MSDTC ondersteund op SQL Server-VM's?**
   
    Ja. Lokale DTC wordt ondersteund voor SQL Server 2016 SP2 en hoger. Toepassingen moeten echter worden getest bij het gebruik van AlwaysOn availability groups als transacties die onderweg zijn tijdens een failover mislukken en moet opnieuw worden uitgevoerd. Geclusterde DTC is beschikbaar vanaf Windows Server 2019. 

## <a name="resources"></a>Resources

**Virtuele Windows-machines**:

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
