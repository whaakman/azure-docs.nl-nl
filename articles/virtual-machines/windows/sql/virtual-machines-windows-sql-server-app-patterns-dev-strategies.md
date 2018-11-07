---
title: SQL Server-Toepassingspatronen op VM's | Microsoft Docs
description: In dit artikel bevat informatie over toepassingspatronen voor SQL Server op Azure Virtual machines. Het biedt oplossingsarchitecten en ontwikkelaars een basis voor goede architectuur en ontwerp.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: craigg
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: 9ccf7d85b52759a3f2cd64facc6ee8d7255f0fac
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51253173"
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Toepassingspatronen en ontwikkelingsstrategieën voor SQL Server in Azure Virtual Machines
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Overzicht:
Bepalen welke toepassingspatroon of patronen te gebruiken voor uw toepassingen op basis van SQL-Server in Azure-omgeving is een belangrijke beslissing nemen en vereist een goed begrip van hoe SQL Server en elk onderdeel van de infrastructuur van Azure samenwerken. Met SQL Server in Azure Infrastructure Services, kunt u eenvoudig migreren, onderhouden en controleren van uw bestaande SQL Server-toepassingen ingebouwd in Windows Server voor virtuele machines in Azure.

Het doel van dit artikel is voor oplossingsarchitecten en ontwikkelaars een basis voor goede architectuur en ontwerp, dat ze volgen kunnen bij het migreren van bestaande toepassingen naar Azure, evenals het ontwikkelen van nieuwe toepassingen in Azure.

Voor elk toepassingspatroon vindt u een on-premises-scenario, de respectieve ingeschakeld voor de cloud-oplossing en de gerelateerde technische aanbevelingen beschreven. Bovendien wordt het artikel beschreven Azure-specifieke ontwikkelingsstrategieën zodat u uw toepassingen goed kunt ontwerpen. Het verdient vanwege de vele mogelijke toepassings-patronen, architecten en ontwikkelaars de meest geschikte patroon voor hun toepassingen en gebruikers moeten kiezen.

**Medewerkers aan technische:** Luis Carlos Vargas haring, Madhan Arumugam Ramakrishnan

**Technische controleurs:** Corey Sanders, Drew McDaniel, Narayan Annamalai, nir die aan Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Inleiding
U kunt veel soorten toepassingen met n-aantal lagen ontwikkelen door te scheiden van de onderdelen van de andere toepassingslagen op verschillende computers ook als afzonderlijke onderdelen in. Bijvoorbeeld, kunt u de clienttoepassing plaatsen en bedrijfsregels onderdelen op één computer, front-endweblaag en laag voor gegevenstoegang in een andere computer en een back-end-database-laag in een andere computer. Dit soort structureren helpt u bij elke laag van elkaar isoleren. Als u waar gegevens vandaan komen wijzigen, moet u niet wijzigen van de client of web-toepassing, maar alleen de laag voor gegevenstoegang.

Een typische *n-aantal lagen* toepassing bevat de presentatielaag, de bedrijfslaag en de gegevenslaag:

| Laag | Beschrijving |
| --- | --- |
| **Presentatie** |De *presentatielaag* (weblaag, front-end-laag) is de laag waarop gebruikers werken interactief met een toepassing. |
| **Business** |De *bedrijfslaag* (middelste laag) is de laag die de presentatie-laag en de gegevenslaag gebruiken om te communiceren met elkaar en bevat de kernfunctionaliteit van het systeem. |
| **Gegevens** |De *gegevenslaag* is in feite de server waarin de gegevens van een toepassing (bijvoorbeeld een server met SQL Server). |

Toepassingslagen beschrijven de logische groeperingen van de functies en onderdelen in een toepassing. terwijl lagen de fysieke distributie van de functies en onderdelen op afzonderlijke fysieke servers, computers, netwerken of externe locaties beschrijven. De lagen van een toepassing kunnen zich bevinden op dezelfde fysieke computer (dezelfde laag) of kunnen worden verdeeld over de afzonderlijke computers (n-aantal lagen) en de onderdelen in elke laag communiceren met componenten in andere lagen via goed gedefinieerde interfaces. U kunt zien van de termijn laag als die verwijzen naar distributiepunten fysieke patronen zoals twee lagen, drie lagen en n-aantal lagen. Een **tier-2-toepassingspatroon** bevat twee toepassingslagen: application server en database-server. De rechtstreekse communicatie plaatsvindt tussen de toepassingsserver en de database-server. De toepassingsserver bevat zowel de web-laag en de bedrijfslaag onderdelen. In **3-laagse toepassingspatroon**, er zijn drie toepassingslagen: web server, application server, waarin de zakelijke logicalaag en/of business-laag voor gegevenstoegang, en de database-server. De communicatie tussen de webserver en de database-server wordt uitgevoerd op de toepassingsserver. Zie voor gedetailleerde informatie over toepassingslagen en lagen [Microsoft Application Architecture Guide](https://msdn.microsoft.com/library/ff650706.aspx).

Voordat u begint met het lezen van dit artikel, hebt u kennis over de basisconcepten van SQL Server en Azure. Zie voor meer informatie, [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) en [Azure.com](https://azure.microsoft.com/).

Dit artikel beschrijft de verschillende patronen die geschikt is voor uw eenvoudige toepassingen, evenals de complexe zakelijke toepassingen kunnen worden. Voordat u met gedetailleerde informatie over elk patroon, wordt aangeraden dat u met de beschikbare services voor gegevensopslag in Azure, zoals raken moet [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md), en [SQL Server in een Azure-Machine](virtual-machines-windows-sql-server-iaas-overview.md). Als u het beste ontwerpbeslissingen te nemen voor uw toepassingen, te begrijpen wanneer u de storage-service van welke gegevens duidelijk.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>SQL Server in een Azure-Machine, kiezen als:
* U moet een besturingselement in het SQL Server en Windows. Dit kan bijvoorbeeld, dus ook de SQL Server-versie, speciale hotfixes, prestatieconfiguratie, enzovoort.
* U moet een volledige compatibiliteit met SQL Server on-premises en wilt overstappen van bestaande toepassingen naar Azure als-is.
* U wilt gebruikmaken van de mogelijkheden van de Azure-omgeving, maar Azure SQL Database biedt geen ondersteuning voor alle functies die uw toepassing vereist. Dit omvat bijvoorbeeld de volgende gebieden:
  
  * **Grootte van de database**: op het moment dat dit artikel is bijgewerkt, SQL Database biedt ondersteuning voor een database van maximaal 1 TB aan gegevens. Als uw toepassing meer dan 1 TB aan gegevens vereist en u niet wilt dat aangepaste sharding-oplossingen wilt implementeren, wordt het aanbevolen dat u SQL Server in een Azure Virtual machines gebruiken. Voor de meest recente informatie, Zie [schalen van Azure SQL-Database](https://msdn.microsoft.com/library/azure/dn495641.aspx), [DTU-Based aanschaffen Model](../../../sql-database/sql-database-service-tiers-dtu.md), en [op vCore gebaseerde Purchasing Model](../../../sql-database/sql-database-service-tiers-vcore.md)(preview).
  * **Naleving van HIPAA**: klanten in de gezondheidszorg en Independent Software Vendors (ISV's) kunt [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) in plaats van [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md) omdat SQL Server in een virtuele Machine van Azure wordt door de HIPAA Business Associate Agreement (BAA) beschreven. Zie voor meer informatie over naleving [Microsoft Azure Trust Center: naleving](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Functies op exemplaarniveau**: op dit moment biedt geen ondersteuning voor SQL Database-functies die niet in de database woont (zoals gekoppelde Servers Agent taken, FileStream, Service Broker, enz.). Zie voor meer informatie, [Azure SQL Database-richtlijnen en beperkingen](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>(eenvoudig) tier-1: één virtuele machine
In dit toepassingspatroon implementeert u uw SQL Server-toepassing en de database in een zelfstandige virtuele machine in Azure. Dezelfde virtuele machine bevat uw client-/ webtoepassing, business-onderdelen, laag voor gegevenstoegang en de database-server. De presentatie, het bedrijf en de code voor gegevenstoegang logisch zijn gescheiden, maar zich fysiek in een machine met één server. De meeste klanten beginnen met dit toepassingspatroon en vervolgens, ze uitschalen door meer webrollen of virtuele machines toe te voegen aan het systeem.

Dit patroon is handig wanneer:

* U wilt uitvoeren van een eenvoudige migratie naar Azure-platform om te beoordelen of het platform van uw toepassing vereisten of niet beantwoordt.
* U wilt behouden van alle toepassingslagen die wordt gehost in dezelfde virtuele machine in de dezelfde Azure-Datacenter Verminder de latentie tussen lagen.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.

Het volgende diagram ziet u een eenvoudige on-premises scenario en hoe u de cloudoplossing ingeschakeld in een enkele virtuele machine in Azure kunt implementeren.

![patroon voor tier-1-toepassingen](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Implementatie van de business-laag (bedrijfslogica en data access-onderdelen) op dezelfde fysieke laag als die de presentatielaag kan de prestaties van toepassingen, maximaliseren, tenzij u moet een afzonderlijke laag vanwege problemen voor schaalbaarheid en beveiliging.

Omdat dit een zeer algemene patroon bij, mogelijk nuttig voor u het volgende artikel voor migratie voor het verplaatsen van uw gegevens naar uw SQL Server-VM: [een Database migreren naar SQL Server op een Azure-VM](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-laagse (eenvoudig): meerdere virtuele machines
In dit toepassingspatroon implementeert u een 3-laagse toepassing in Azure door elke toepassingslaag in een andere virtuele machine. Dit biedt een flexibele omgeving voor een eenvoudig scenario's voor scale-up en scale-out. Wanneer een virtuele machine uw client-/ webtoepassing bevat, een als host fungeert voor de bedrijfsonderdelen van uw en andere die als host fungeert voor de database-server.

Dit patroon is handig wanneer:

* Wilt u een migratie van complexe databasetoepassingen naar Azure Virtual Machines uit te voeren.
* Wilt u andere toepassingslagen worden gehost in verschillende regio's. Bijvoorbeeld, u mogelijk hebt gedeeld databases die zijn geïmplementeerd in meerdere regio's voor rapportagedoeleinden gebruikt.
* U wilt verplaatsen van zakelijke toepassingen vanuit de on-premises gevirtualiseerde platformen naar Azure Virtual Machines. Zie voor gedetailleerde informatie over het zakelijke toepassingen, [wat is er een bedrijfstoepassing](https://msdn.microsoft.com/library/aa267045.aspx).
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.

Het volgende diagram ziet u hoe u een eenvoudige 3-laagse toepassing kunt plaatsen in Azure door elke toepassingslaag in een andere virtuele machine.

![3-laagse toepassingspatroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

In dit toepassingspatroon, moet u er slechts één virtuele machine (VM) is in elke laag. Als u meerdere virtuele machines in Azure hebt, raden wij u stelt een virtueel netwerk. [Azure-netwerk](../../../virtual-network/virtual-networks-overview.md) maakt u een grens van een vertrouwde beveiliging en hiermee ook kunnen VM's onderling communiceren via privé IP-adres. Bovendien altijd Zorg ervoor dat alle verbindingen via Internet alleen gaat u naar de presentatielaag. Wanneer dit patroon te volgen, beheren van de regels voor netwerkbeveiligingsgroepen voor het beheren van toegang. Zie voor meer informatie, [toestaan van externe toegang tot uw virtuele machine via de Azure-portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

In het diagram kan internetprotocollen TCP, UDP, HTTP of HTTPS zijn.

> [!NOTE]
> Instellen van een virtueel netwerk in Azure is gratis. Echter, in rekening gebracht voor de VPN-gateway die is verbonden met on-premises. Deze kosten zijn gebaseerd op de hoeveelheid tijd die de verbinding ingericht en beschikbaar is.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>tier-2 en 3-laagse met presentatie-laag scale-out
In dit toepassingspatroon implementeert u database tier-2 of 3-laagse toepassing naar Azure Virtual Machines door elke toepassingslaag in een andere virtuele machine. Bovendien opschalen kunt u de presentatielaag vanwege de hogere volume van binnenkomende aanvragen van clients.

Dit patroon is handig wanneer:

* U wilt verplaatsen van zakelijke toepassingen vanuit de on-premises gevirtualiseerde platformen naar Azure Virtual Machines.
* Wilt u de schaal vergroten de presentatielaag vanwege de hogere volume van binnenkomende aanvragen van clients.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.
* Wilt u eigenaar bent van een infrastructuur-omgeving die op aanvraag omhoog of omlaag kunt schalen.

Het volgende diagram ziet u hoe u de toepassingslagen kunt plaatsen op meerdere virtuele machines in Azure door het uitbreiden van de presentatielaag vanwege de hogere volume van binnenkomende aanvragen van clients. Zoals te zien is in het diagram, wordt Azure Load Balancer is verantwoordelijk voor het distribueren van verkeer over meerdere virtuele machines en ook bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de webservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag.

![Patroon voor toepassingen - presentatie-laag scale-out](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Aanbevolen procedures voor tier 2, 3-laagse of n-tier-patronen die u meerdere virtuele machines in één laag
Het wordt aanbevolen dat u de virtuele machines die deel uitmaken van dezelfde laag in dezelfde cloudservice en in dezelfde de beschikbaarheid plaatst instellen. Bijvoorbeeld, plaatst u een reeks webservers in **CloudService1** en **AvailabilitySet1** en een set met database-servers in **CloudService2** en  **AvailabilitySet2**. Een beschikbaarheidsset in Azure kunt u de knooppunten voor hoge beschikbaarheid plaats in afzonderlijke foutdomeinen en upgradedomeinen.

Als u wilt gebruikmaken van meerdere VM-exemplaren van een laag, moet u Azure Load Balancer configureren tussen de toepassingslagen. Voor het configureren van Load Balancer in elke laag, maakt u een eindpunt met load balancing afzonderlijk op elke laag van virtuele machines. Voor een bepaalde laag, moet u eerst VM's maken in dezelfde cloudservice. Dit zorgt ervoor dat ze hetzelfde openbare virtuele IP-adres hebben. Maak vervolgens een eindpunt op een van de virtuele machines op die laag. Wijs vervolgens hetzelfde eindpunt toe aan de andere virtuele machines op dat niveau voor de taakverdeling. Door het maken van een load balancer-set, moet u verkeer verdelen over meerdere virtuele machines en kunt ook de Load Balancer om te bepalen welk knooppunt verbinding maken wanneer een back-end-VM-knooppunt is mislukt. Bijvoorbeeld, met meerdere exemplaren van de webservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag.

Als een best practice, zorg er altijd dat alle verbindingen via internet eerst gaat u naar de presentatielaag. De presentatielaag toegang heeft tot de bedrijfslaag en vervolgens de bedrijfslaag toegang heeft tot de gegevenslaag. Zie voor meer informatie over het toestaan van toegang tot de presentatielaag [toestaan van externe toegang tot uw virtuele machine via de Azure-portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Houd er rekening mee dat de Load Balancer in Azure vergelijkbaar aan load balancers in een on-premises-omgeving werkt. Zie voor meer informatie, [taakverdeling voor Azure-infrastructuurservices](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Bovendien is het raadzaam dat u een particuliere netwerk voor uw virtuele machines instellen met behulp van Azure Virtual Network. Hierdoor kunnen ze onderling communiceren via privé IP-adres. Zie voor meer informatie, [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>tier-2 en 3-laagse met business laag scale-out
In dit toepassingspatroon implementeert u database tier-2 of 3-laagse toepassing naar Azure Virtual Machines door elke toepassingslaag in een andere virtuele machine. Bovendien, als u wilt distribueren van de server-onderdelen van de toepassing naar meerdere virtuele machines vanwege de complexiteit van uw toepassing.

Dit patroon is handig wanneer:

* U wilt verplaatsen van zakelijke toepassingen vanuit de on-premises gevirtualiseerde platformen naar Azure Virtual Machines.
* U wilt distribueren van de server-onderdelen van de toepassing naar meerdere virtuele machines vanwege de complexiteit van uw toepassing.
* U wilt verplaatsen van zakelijke logica zware on-premises LOB (line-of-business) toepassingen naar Azure Virtual Machines. LOB-toepassingen zijn een set van kritieke toepassingen die cruciaal zijn voor het uitvoeren van een onderneming, zoals boekhouding, human resources (uur), salarisadministratie, toeleveringsketenbeheer, en resourceplanning toepassingen.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.
* Wilt u eigenaar bent van een infrastructuur-omgeving die op aanvraag omhoog of omlaag kunt schalen.

Het volgende diagram ziet u een on-premises scenario en de oplossing in de cloud is ingeschakeld. In dit scenario kunt u de toepassingslagen in meerdere virtuele machines in Azure door het uitbreiden van de bedrijfslaag, met de zakelijke logicalaag en data access-componenten plaatsen. Zoals te zien is in het diagram, wordt Azure Load Balancer is verantwoordelijk voor het distribueren van verkeer over meerdere virtuele machines en ook bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de bedrijfslaag. Zie voor meer informatie, [aanbevolen procedures voor tier 2, 3-laagse of n-tier application-patronen die meerdere virtuele machines in één laag hebt](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Patroon voor toepassingen met business laag scale-out](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>tier-2 en 3-laagse met presentatie en business-lagen scale-out en HADR
In dit toepassingspatroon implementeert u database tier-2 of 3-laagse toepassing naar Azure Virtual Machines door het distribueren van de presentatielaag (webserver) en de business laag (toepassingsserver)-onderdelen op meerdere virtuele machines. U kunt bovendien oplossingen voor hoge beschikbaarheid en noodherstel herstel implementeren voor uw databases in virtuele machines van Azure.

Dit patroon is handig wanneer:

* U wilt verplaatsen van zakelijke toepassingen van gevirtualiseerde platformen on-premises naar Azure door het implementeren van hoge beschikbaarheid van SQL Server- en herstelfunctionaliteit bij noodgevallen.
* Wilt u de schaal vergroten de presentatielaag en de bedrijfslaag vanwege de hogere volume van binnenkomende aanvragen van clients en de complexiteit van uw toepassing.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.
* Wilt u eigenaar bent van een infrastructuur-omgeving die op aanvraag omhoog of omlaag kunt schalen.

Het volgende diagram ziet u een on-premises scenario en de oplossing in de cloud is ingeschakeld. In dit scenario opschalen kunt u de presentatielaag en de zakelijke tier-onderdelen op meerdere virtuele machines in Azure. U kunt bovendien hoge beschikbaarheid en noodherstel recovery (HADR) technieken voor het SQL Server-databases implementeren in Azure.

Meerdere exemplaren van een toepassing die wordt uitgevoerd in verschillende zorg virtuele machines ervoor dat u aanvragen voor taakverdeling over ze zijn. Wanneer u meerdere virtuele machines hebt, moet u om ervoor te zorgen dat alle virtuele machines toegankelijk is en worden uitgevoerd op één punt in tijd zijn. Als u taakverdeling kunt configureren, wordt Azure Load Balancer houdt de status van virtuele machines en correct zorgt ervoor dat binnenkomende oproepen naar de goed werkende VM-knooppunten. Zie voor meer informatie over het instellen van de taakverdeling van de virtuele machines [taakverdeling voor Azure-infrastructuurservices](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Met meerdere exemplaren van de web- en toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatie- en bedrijfslagen.

![Scale-out en hoge beschikbaarheid](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Aanbevolen procedures voor het vereisen van SQL HADR toepassingspatronen
Bij het instellen van hoge beschikbaarheid van SQL Server en oplossingen voor herstel na noodgevallen in Azure Virtual Machines, instellen van een virtueel netwerk voor uw virtuele machines met [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) is verplicht.  Virtuele machines binnen een Virtueelnetwerk hebben een stabiele privé IP-adres, zelfs na een service-uitvaltijd, dus u kunt voorkomen dat de updatetijd die nodig is voor DNS-naamomzetting. Bovendien wordt het virtuele netwerk kunt u uw on-premises netwerk uitbreiden naar Azure en maakt u een grens van een vertrouwde beveiliging. Bijvoorbeeld, als uw toepassing heeft bedrijfsdomein beperkingen (zoals Windows-verificatie, Active Directory), instellen van [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) nodig is.

De meeste klanten die in Azure worden uitgevoerd van productiecode, ervoor zorgen dat de primaire en secundaire replica's in Azure.

Zie voor uitgebreide informatie en zelfstudies voor hoge beschikbaarheid en technieken voor herstel na noodgevallen, [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>tier-2 en 3-laagse met behulp van Azure-VM's en Cloud Services
In dit toepassingspatroon implementeert u tier-2 of 3-laagse toepassing naar Azure met zowel [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md) (web- en werkrollen rollen - Platform as a Service (PaaS)) en [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) () Infrastructure as a Service (IaaS)). Met behulp van [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor de presentatie-laag/business laag en de SQL Server in [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de gegevens laag is het nuttig zijn voor de meeste toepassingen uitvoeren op Azure. De reden is dat met een compute-instantie die wordt uitgevoerd op Cloud Services biedt een eenvoudig beheer, implementatie, bewaking en scale-out.

Met Cloud Services, Azure houdt de infrastructuur voor u, voert routineonderhoud, patches van de besturingssystemen en probeert te herstellen van fouten voor service- en hardware. Wanneer uw toepassing moet de scale-out, is automatische en handmatige scale-out-opties zijn beschikbaar voor uw cloud service-project door vergroten of verkleinen van het aantal instanties of virtuele machines die worden gebruikt door uw toepassing. Bovendien kunt u on-premises Visual Studio om uw toepassing in een cloud service-project in Azure te implementeren.

Kortom, als u niet wilt dat eigenaar van uitgebreide beheertaken voor de presentatie/business laag en uw toepassing niet vereisen geen complexe configuratie van software of het besturingssysteem is geïnstalleerd, gebruikt u Azure Cloud Services. Als Azure SQL Database biedt geen ondersteuning voor alle functies die u zoekt, SQL Server gebruiken in een Azure Virtual Machines voor de gegevenslaag. Een toepassing uitvoert in Azure Cloud Services en opslaan van gegevens in Azure Virtual Machines worden gecombineerd voor de voordelen van beide services. Zie de sectie in dit onderwerp voor een gedetailleerde vergelijking op [ontwikkelingsstrategieën in Azure vergelijken](#comparing-web-development-strategies-in-azure).

In dit toepassingspatroon omvat de presentatielaag een Webrol die een Cloud Services-onderdeel wordt uitgevoerd in de uitvoeringsomgeving van Azure en deze is aangepast voor web application programming ondersteund door IIS en ASP.NET. De bedrijfs- of back-end-laag omvat een werkrol, die een Cloud Services-onderdeel wordt uitgevoerd in de uitvoeringsomgeving van Azure en het is handig voor het ontwikkelen van gegeneraliseerde en achtergrondverwerking kan uitvoeren voor een Webrol. De databaselaag bevindt zich in een SQL-Server-machine in Azure. De communicatie tussen de presentatielaag en de databaselaag gebeurt rechtstreeks of via de bedrijfslaag-onderdelen voor worker-rol.

Dit patroon is handig wanneer:

* U wilt verplaatsen van zakelijke toepassingen van gevirtualiseerde platformen on-premises naar Azure door het implementeren van hoge beschikbaarheid van SQL Server- en herstelfunctionaliteit bij noodgevallen.
* Wilt u eigenaar bent van een infrastructuur-omgeving die op aanvraag omhoog of omlaag kunt schalen.
* Azure SQL Database biedt geen ondersteuning voor alle functies die uw toepassing of de database nodig heeft.
* U wilt stress testen voor de verschillende werkbelastingniveaus uitvoeren, maar op hetzelfde moment u wilt niet en veel fysieke machines voortdurend onderhouden.

Het volgende diagram ziet u een on-premises scenario en de oplossing in de cloud is ingeschakeld. In dit scenario plaatst u de presentatielaag in webrollen, de bedrijfslaag in werkrollen, maar de gegevenslaag op virtuele machines in Azure. Meerdere exemplaren van de presentatielaag uitgevoerd in verschillende webrollen zorgt ervoor dat voor het laden van aanvragen verdelen over deze in. Wanneer u Azure Cloud Services met Azure Virtual Machines combineert, wordt aangeraden dat u instelt [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) ook. Met [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), u kunt hebben stabiel en permanente privé IP-adressen binnen dezelfde cloudservice in de cloud. Wanneer u een virtueel netwerk voor uw virtuele machines definieert en services in de cloud, kun ze onderling communiceren via privé IP-adres. Bovendien hebben virtuele machines en Azure web-/ werkrollen in dezelfde [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) lage latentie en beter beveiligde connectiviteit. Zie voor meer informatie, [wat is een cloudservice](../../../cloud-services/cloud-services-choose-me.md).

In het diagram kunt zien, verdeelt het verkeer over meerdere virtuele machines en bepaalt ook welke webserver of toepassingsserver verbinding maken met Azure Load Balancer. Met meerdere exemplaren van de web- en toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag en de bedrijfslaag. Zie voor meer informatie, [Best practices voor toepassingen waarvoor SQL HADR patronen](#best-practices-for-application-patterns-requiring-sql-hadr).

![Toepassingspatronen met Cloud Services](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Een andere benadering voor het implementeren van dit patroon is het gebruik van een geconsolideerde Webrol die bevat zowel de presentatielaag en de business laag onderdelen zoals wordt weergegeven in het volgende diagram. Dit patroon is handig voor toepassingen waarvoor stateful ontwerp. Omdat Azure stateless compute-knooppunten op web-en werkrollen biedt, wordt afgeraden om een logica voor het opslaan van de sessiestatus met behulp van een van de volgende technologieën te implementeren: [Azure Caching](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) of [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).

![Toepassingspatronen met Cloud Services](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Patroon met virtuele machines in Azure, Azure SQL Database en Azure App Service (Web-Apps)
Het voornaamste doel van dit patroon is leert u hoe u Azure-infrastructuur als een service (IaaS)-onderdelen met Azure-platform-as-a-service-onderdelen (PaaS) in uw oplossing te combineren. Dit patroon is gericht op Azure SQL Database voor relationele gegevensopslag. Deze omvatten geen SQL Server in een Azure-machine, die deel uitmaakt van de Azure-infrastructuur als een serviceaanbieding.

In dit toepassingspatroon implementeert u een databasetoepassing in Azure door de presentatie- en bedrijfslagen plaatsen in dezelfde virtuele machine en de toegang tot een database in Azure SQL Database (SQL Database)-servers. U kunt de presentatielaag implementeren met behulp van de traditionele manier websites op basis van een IIS-oplossingen. Of u kunt een gecombineerde presentatie en de bedrijfslaag implementeren met behulp van [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Dit patroon is handig wanneer:

* U al een bestaande SQL Database-server geconfigureerd in Azure hebt en u wilt snel testen van uw toepassing.
* U wilt testen van de mogelijkheden van Azure-omgeving.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* Van uw zakelijke logica en gegevens die de onderdelen van de toegang kunnen worden ingesloten in een webtoepassing.

Het volgende diagram ziet u een on-premises scenario en de oplossing in de cloud is ingeschakeld. In dit scenario, moet u de toepassingslagen in een enkele virtuele machine in Azure en toegang tot gegevens in Azure SQL Database plaatsen.

![Patroon voor gemengde-toepassingen](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Als u ervoor kiest een gecombineerde web- en toepassingslaag implementeert met behulp van Azure Web Apps, raden wij u aan de middelste laag of toepassing laag te houden als dynamic link libraries (DLL's) in de context van een web-App.

Bovendien, bekijk de aanbevelingen die in de [vergelijking van de web-ontwikkelingsstrategieën in Azure](#comparing-web-development-strategies-in-azure) sectie aan het einde van dit artikel voor meer informatie over het programmeren van technieken.

## <a name="n-tier-hybrid-application-pattern"></a>Patroon voor een N-tier hybride toepassingen
In het patroon voor een n-tier hybride toepassingen implementeert u uw toepassing in meerdere lagen die zijn gedistribueerd tussen on-premises en Azure. Daarom maakt u een flexibele en herbruikbare hybride systeem, die u kunt wijzigen of een specifieke laag toevoegen zonder te wijzigen van de andere lagen. Om uit te breiden van uw bedrijfsnetwerk naar de cloud, gebruikt u [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) service.

Dit patroon van de toepassing hybride is handig wanneer:

* U wilt maken van toepassingen die deels in de cloud worden uitgevoerd en deels on-premises.
* U wilt migreren van sommige of alle elementen van een bestaande on-premises toepassing naar de cloud.
* Wilt u zakelijke toepassingen vanuit de on-premises gevirtualiseerde platformen verplaatsen naar Azure.
* Wilt u eigenaar bent van een infrastructuur-omgeving die op aanvraag omhoog of omlaag kunt schalen.
* Wilt u snel inrichten van ontwikkeling en testomgevingen gedurende korte perioden.
* Wilt u een voordelige manier naar de back-ups maken voor database-bedrijfstoepassingen.

Het volgende diagram ziet u een patroon voor een n-tier hybride toepassingen die zich over on-premises en Azure uitstrekt. Zoals u in het diagram, on-premises infrastructuur omvat [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) domeincontroller ter ondersteuning van verificatie en autorisatie. Houd er rekening mee dat het diagram ziet u een scenario, waarin bepaalde onderdelen van de gegevenslaag bevinden zich in een on-premises datacenter dat sommige onderdelen van de gegevenslaag live in Azure. Afhankelijk van de behoeften van uw toepassing, kunt u verschillende andere hybride scenario's implementeren. U kunt bijvoorbeeld de presentatielaag en de bedrijfslaag bewaren in een on-premises-omgeving, maar de gegevenslaag in Azure.

![Patroon toepassing met meerdere lagen](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure, kunt u Active Directory gebruiken als een zelfstandige clouddirectory voor uw organisatie, of u kunt ook integreren met bestaande on-premises Active Directory met [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Zoals te zien is in het diagram, de onderdelen van de laag bedrijven toegang hebben tot meerdere gegevensbronnen, zoals op [SQL Server in Azure](virtual-machines-windows-sql-server-iaas-overview.md) via een persoonlijke interne IP-adres, on-premises SQL Server via [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), of [SQL-Database](../../../sql-database/sql-database-technical-overview.md) met behulp van de .NET Framework data provider-technologieën. In dit diagram is Azure SQL Database een optionele gegevens storage-service.

U kunt de volgende werkstroom in de opgegeven volgorde implementeren in meerdere lagen hybride toepassingspatroon:

1. Bepalen welke toepassingen van de enterprise-database die worden teruggezet moeten naar de cloud met behulp van de [Microsoft Assessment and Planning (MAP) Toolkit](https://microsoft.com/map). De MAP Toolkit verzamelt inventaris-en prestatiegegevens van computers die u voor virtualisatie overweegt en aanbevelingen biedt over capaciteit en de evaluatie van planning.
2. Plan de resources en de configuratie is vereist in het Azure-platform, zoals storage-accounts en virtuele machines.
3. Instellen van de netwerkverbinding tussen het bedrijfsnetwerk on-premises en [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Als u de verbinding tussen het bedrijfsnetwerk on-premises en een virtuele machine in Azure instelt, moet u een van de volgende twee methoden gebruiken:
   
   1. Verbinding maken tussen on-premises en Azure via openbare eindpunten op een virtuele machine in Azure. Deze methode biedt een eenvoudig in te stellen en kunt u het gebruik van SQL Server-verificatie in uw virtuele machine. Bovendien instellen van uw regels voor netwerkbeveiligingsgroepen voor het beheren van openbare verkeer naar de virtuele machine. Zie voor meer informatie, [toestaan van externe toegang tot uw virtuele machine via de Azure-portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Een verbinding tussen on-premises en Azure via Azure virtueel particulier netwerk (VPN)-tunnel tot stand brengen. Deze methode kunt u beleid voor domeinen met een virtuele machine in Azure uitbreiden. Bovendien kunt u firewallregels instellen en gebruiken van Windows-verificatie in uw virtuele machine. Azure ondersteunt momenteel beveiligde site-naar-site-VPN- en punt-naar-site VPN-verbindingen:
      
      * Met beveiligde site-naar-site-verbinding, kunt u geen netwerkverbinding tussen uw on-premises netwerk en het virtuele netwerk in Azure. Het verdient aanbeveling om verbinding te maken van uw on-premises data center-omgeving naar Azure.
      * Met beveiligde punt-naar-site-verbinding, kunt u verbinding met het netwerk tot stand brengen tussen uw virtuele netwerk in Azure en uw afzonderlijke computers die overal worden uitgevoerd. Het verdient aanbeveling voornamelijk voor ontwikkelings- en testdoeleinden.
      
      Zie voor meer informatie over hoe u verbinding maken met SQL Server in Azure [verbinding maken met een SQL Server-VM op Azure](virtual-machines-windows-sql-connect.md).
4. Instellen van geplande taken en waarschuwingen die back-up van on-premises gegevens in een virtuele machine-schijf in Azure. Zie voor meer informatie, [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx) en [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).
5. Afhankelijk van de behoeften van uw toepassing, kunt u een van de volgende drie gangbare scenario's implementeren:
   
   1. U kunt uw webserver, een toepassingsserver en een hoofdlettergevoelig gegevens in een database-server in Azure houden terwijl u de gevoelige gegevens on-premises houden.
   2. U kunt uw webserver en toepassingsserver on-premises terwijl de database-server in een virtuele machine in Azure.
   3. Kunt u uw database-server, de webserver en on-premises toepassingsserver dat u de databasereplica op virtuele machines in Azure. Deze instelling kan de webservers van on-premises of reporting toepassingen voor toegang tot de database-replica's in Azure. Daarom kunt u de werkbelasting in een on-premises database lagere bereiken. Het is raadzaam dat u dit scenario voor zware implementeert workloads en ontwikkeling doeleinden worden gelezen. Zie voor informatie over het maken van database-replica's in Azure AlwaysOn-beschikbaarheidsgroepen op [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Vergelijking van de web-ontwikkelingsstrategieën in Azure
Als u wilt implementeren en een meerlagige toepassing op basis van SQL Server in Azure implementeert, kunt u een van de volgende twee methoden programmeren:

* Instellen van een traditionele webserver (IIS - Internet Information Services) in Azure en access-databases in SQL Server in Azure Virtual Machines.
* Implementeren en een cloudservice implementeren in Azure. Controleer vervolgens of toegang hebt tot deze cloudservice in SQL Server-databases in Azure Virtual machines. Een service in de cloud kan meerdere web- en werkrollen functies bevatten.

De volgende tabel bevat een vergelijking van de traditionele manier websites-ontwikkeling met Azure Cloud Services en Azure Web Apps met betrekking tot SQL Server in Azure Virtual Machines. De tabel bevat Azure Web Apps is het mogelijk SQL Server gebruiken in Azure VM als een gegevensbron voor Azure Web Apps via de openbare virtuele IP-adres of de DNS-naam.

|  | Traditionele webontwikkeling in Azure Virtual Machines | Cloudservices in Azure | Met Azure-Web-Apps voor webhosting |
| --- | --- | --- | --- |
| **Toepassingsmigratie van on-premises** |Bestaande toepassingen, zoals-is. |Toepassingen moeten web-en werkrollen. |Bestaande toepassingen, zoals-maar geschikt is voor onafhankelijke webtoepassingen en webservices waarvoor snelle schaalbaarheid. |
| **De ontwikkeling en implementatie** |Visual Studio, WebMatrix, Visual Web Developer, WebDeploy, FTP, TFS, IIS-beheer, PowerShell. |Visual Studio, TFS, met Azure SDK PowerShell. Elke cloudservice heeft twee omgevingen waarop u de service-pakket en de configuratie kunt implementeren: fasering en productie. U kunt een cloudservice implementeren naar de staging-omgeving te testen voordat u het niveau naar productie verhogen. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, Mercurial, TFS, Web implementeert, PowerShell. |
| **Beheer en instellen** |U bent verantwoordelijk voor beheertaken op de toepassing, gegevens, firewall-regels, virtueel netwerk en besturingssysteem. |U bent verantwoordelijk voor beheertaken op de toepassing, gegevens, firewallregels en virtueel netwerk. |U bent verantwoordelijk voor beheertaken op de toepassing en alleen gegevens. |
| **Hoge beschikbaarheid en herstel na noodgevallen (HADR)** |Het is raadzaam dat u virtuele machines in dezelfde beschikbaarheidsset en in dezelfde cloudservice plaatsen. Bijhouden van uw virtuele machines in dezelfde kan beschikbaarheidsset Azure plaats van de knooppunten voor hoge beschikbaarheid in afzonderlijke foutdomeinen en upgradedomeinen. Op dezelfde manier te houden van uw virtuele machines in dezelfde cloudservice kunt taakverdeling en virtuele machines rechtstreeks met elkaar kunnen communiceren via het lokale netwerk binnen een Azure-Datacenter.<br/><br/>U bent verantwoordelijk voor het implementeren van een hoge beschikbaarheid en een oplossing voor noodherstel voor SQL Server in Azure Virtual Machines om te voorkomen uitvaltijd. Zie voor ondersteunde HADR technologieën [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>U bent verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Azure kan uw virtuele machines verplaatsen als de hostcomputer in het datacenter is mislukt vanwege problemen met de hardware. Bovendien kan er geplande uitvaltijd van uw virtuele machine wanneer de hostmachine is bijgewerkt in verband met beveiligings- of software updates. Daarom raden wij aan dat u ten minste twee virtuele machines in elke toepassingslaag om te controleren of de continue beschikbaarheid onderhouden. Azure biedt geen SLA voor een enkele virtuele machine. |De fouten die voortvloeien uit de onderliggende hardware of besturingssysteem software wordt beheerd door Azure. Het is raadzaam dat u meerdere exemplaren van een web- of werkrol om te controleren of de hoge beschikbaarheid van uw toepassing implementeren. Zie voor meer informatie, [Cloudservices, virtuele Machines en Virtual Network Service Level Agreement](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).<br/><br/>U bent verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Voor databases die in een SQL Server-database in een Azure-VM zich bevindt, bent u verantwoordelijk voor het implementeren van een hoge beschikbaarheid en noodherstel hersteloplossing om te voorkomen uitvaltijd. Zie voor ondersteunde HDAR technologieën, hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines.<br/><br/>**SQL Server-Database Mirroring**: gebruik met Azure Cloud Services (web/worker-rollen). SQL Server-VM's en een cloudserviceproject kunnen zich in hetzelfde Azure-netwerk. Als SQL Server-VM zich niet in hetzelfde Virtueelnetwerk, moet u een SQL Server-Alias voor het routeren van communicatie met het exemplaar van SQL Server maken. Bovendien hebben de aliasnaam moet overeenkomen met de naam van de SQL Server. |Hoge beschikbaarheid is overgenomen van de Azure-werkrollen, Azure-blobopslag en Azure SQL Database. Azure Storage heeft bijvoorbeeld drie replica's van alle blob-, tabel- en wachtrijgegevens. Op elk willekeurig moment drie replica's van gegevens die worden uitgevoerd voor Azure SQL Database worden opgeslagen: één primaire replica en twee secundaire replica's. Zie voor meer informatie, [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) en [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).<br/><br/>Wanneer u SQL Server in Azure VM als een gegevensbron voor Azure Web Apps, houd er rekening mee dat Azure Web Apps biedt geen ondersteuning voor Azure Virtual Network. Met andere woorden, moeten alle verbindingen vanuit Azure Web Apps met SQL Server-VM's in Azure gaan via openbare eindpunten van virtuele machines. Dit kan enkele beperkingen voor hoge beschikbaarheid en herstel na noodgevallen veroorzaken. De clienttoepassing in Azure Web Apps verbinding maken met SQL Server-VM met het spiegelen van de database zou bijvoorbeeld niet worden geen verbinding maken met de nieuwe primaire server, zoals Databasespiegeling is vereist dat u Azure Virtual Network tussen SQL Server-host virtuele machines in Azure instellen. Daarom kan het gebruik **SQL Server-Database Mirroring** met Azure Web Apps wordt momenteel niet ondersteund.<br/><br/>**SQL Server AlwaysOn Availability Groups**: U kunt AlwaysOn Availability Groups instellen bij het gebruik van Azure Web Apps met SQL Server-VM's in Azure. Maar moet u AlwaysOn beschikbaarheidsgroep-Listener voor het routeren van de communicatie met de primaire replica via openbare taakverdeling eindpunten configureren. |
| **cross-premises connectiviteit** |Azure Virtual Network kunt u verbinding maken met on-premises. |Azure Virtual Network kunt u verbinding maken met on-premises. |Azure-netwerk wordt ondersteund. Zie voor meer informatie, [Web Apps Virtual Network-integratie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Schaalbaarheid** |Omhoog schalen is beschikbaar in de grootten van virtuele machines verhogen of meer schijven toe te voegen. Zie voor meer informatie over de grootten van virtuele machines, [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Voor Database-Server**: Scale-out is beschikbaar via de partitionering technieken database en SQL Server AlwaysOn Availability groups.<br/><br/>Voor werkbelastingen voor zware lezen, kunt u [AlwaysOn Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) op meerdere secundaire knooppunten, evenals SQL Server-replicatie.<br/><br/>Voor workloads met hoge, kunt u horizontale partitionering gegevens over meerdere fysieke servers voor toepassing scale-out implementeren.<br/><br/>Bovendien kunt u een scale-out implementeren met behulp van [SQL Server met gegevensafhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx). Met Data afhankelijke routering (DDR), moet u voor het implementeren van het mechanisme voor partitionering in de clienttoepassing, meestal in de zakelijke tier-laag voor het routeren van aanvragen voor de database met meerdere knooppunten van de SQL Server. De bedrijfslaag bevat toewijzingen aan hoe de gegevens zijn gepartitioneerd en welk knooppunt de gegevens bevat.<br/><br/>U kunt toepassingen schalen die virtuele machines worden uitgevoerd. Zie voor meer informatie, [over het schalen van een toepassing](../../../cloud-services/cloud-services-how-to-scale-portal.md).<br/><br/>**Belangrijke opmerking**: de **voor automatisch schalen** functie in Azure kunt u automatisch vergroten of verkleinen van de virtuele Machines die worden gebruikt door uw toepassing. Deze functie zorgt ervoor dat de eindgebruikerservaring wordt niet negatief beïnvloed tijdens perioden met en VM's worden afgesloten wanneer de vraag klein is. Het verdient aanbeveling Stel de optie voor automatisch schalen niet voor je cloudservice als het SQL Server-VM's bevat. De reden is dat de functie voor automatisch schalen Azure om te schakelen op een virtuele machine kunt wanneer het CPU-gebruik in die virtuele machine is hoger dan enkele drempelwaarde en een virtuele machine uitschakelen wanneer het CPU-gebruik lager is dan het gaat. De functie voor automatisch schalen is handig voor staatloze toepassingen, zoals webservers, waarbij elke virtuele machine de werkbelasting zonder alle verwijzingen naar een eerdere status kunt beheren. De functie voor automatisch schalen is echter niet handig voor stateful toepassingen, zoals SQL Server, waarbij slechts één instantie kan schrijven naar de database. |Omhoog schalen is beschikbaar via meerdere web- en werkrollen functies. Zie voor meer informatie over de grootten van virtuele machines voor webrollen en werkrollen, [groottes voor Cloud Services configureren](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Bij het gebruik van **Cloudservices**, kunt u meerdere functies om te distribueren verwerking en ook bereiken flexibele schaling van uw toepassing. Elke service in de cloud bevat een of meer webrollen en/of werkrollen, elk met een eigen taaktoepassings- en configuratie. U kunt omhoog een service in de cloud door het aantal rolexemplaren (virtuele machines) geïmplementeerd voor een rol en omlaag schalen een service in de cloud door te verminderen van het aantal rolinstanties. Zie voor gedetailleerde informatie [Azure gebruiksmodellen](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Scale-out is beschikbaar via ondersteuning voor ingebouwde Azure hoge beschikbaarheid via [Cloudservices, virtuele Machines en Virtual Network Service Level Agreement](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) en Load Balancer.<br/><br/>Voor een toepassing met meerdere lagen, wordt u aangeraden dat u web/worker rollen toepassing verbindt met de databaseserver-VM's via Azure Virtual Network. Bovendien biedt Azure load balancer voor virtuele machines in dezelfde cloudservice gebruikersaanvragen spreiden over deze. Virtuele machines verbonden op deze manier kunnen rechtstreeks met elkaar communiceren via het lokale netwerk binnen een Azure-Datacenter.<br/><br/>U kunt instellen **voor automatisch schalen** op de Azure-portal, evenals de tijden van de planning. Zie voor meer informatie, [configureren van de functie voor automatisch schalen voor een Cloudservice in de portal](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Omhoog en omlaag schalen**: U kunt vergroten/verkleinen de grootte van de instantie (VM) die is gereserveerd voor uw website.<br/><br/>Scale-out: U kunt meer gereserveerde instanties (VM's) toevoegen voor uw website.<br/><br/>U kunt instellen **voor automatisch schalen** op de portal, evenals de tijden van de planning. Zie voor meer informatie, [hoe u Apps schalen](../../../app-service/web-sites-scale.md). |

Zie voor meer informatie over het kiezen tussen deze programming methoden [Azure Web Apps, Cloud Services en virtuele machines: wanneer gebruikt u welk](../../../app-service/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het uitvoeren van SQL Server in Azure Virtual machines [SQL Server op Azure Virtual Machines Overview](virtual-machines-windows-sql-server-iaas-overview.md).

