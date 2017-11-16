---
title: SQL Server-toepassing patronen op virtuele machines | Microsoft Docs
description: In dit artikel bevat informatie over application patronen voor SQL Server op Azure Virtual machines. Biedt oplossingsarchitecten en ontwikkelaars een basis voor goede toepassingsarchitectuur en het ontwerp.
services: virtual-machines-windows
documentationcenter: na
author: ninarn
manager: jhubbard
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 41863c8d-f3a3-4584-ad86-b95094365e05
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: ninarn
ms.openlocfilehash: ed2ff59ad33408bef70f332f8a93eb8679bf328f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Toepassingspatronen en ontwikkelingsstrategieën voor SQL Server in Azure Virtual Machines
[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="summary"></a>Overzicht:
Bepalen welke toepassing patroon of patronen te gebruiken voor uw toepassingen op basis van SQL-Server in Azure-omgeving is een belangrijk ontwerpbeslissing en vereist een goed begrip van hoe SQL Server en elk onderdeel van de infrastructuur van Azure samenwerken. Met SQL Server in Azure Infrastructure Services, kunt u gemakkelijk migreren, onderhouden en controleren van uw bestaande SQL Server-toepassingen gebouwd-on Windows Server naar virtuele machines in Azure.

Het doel van dit artikel is om oplossingsarchitecten en ontwikkelaars een basis voor goede toepassingsarchitectuur en het ontwerp, dat ze na kunnen bij het migreren van bestaande toepassingen zowel Azure als ontwikkelen van nieuwe toepassingen in Azure.

Voor elk patroon toepassing vindt u een lokale scenario, de respectievelijke cloud-oplossing en de verwante technische aanbevelingen. Het artikel behandelt bovendien strategieën voor het ontwikkelen van Azure-specifieke, zodat u uw toepassingen goed kunt ontwerpen. Vanwege de vele mogelijke toepassing patronen, het raadzaam dat architecten en ontwikkelaars de meest geschikte patroon voor hun toepassingen en gebruikers moeten kiezen.

**Medewerkers aan technische:** Luis Jeroen Vargas haring, Madhan Arumugam Ramakrishnan

**Technische controleurs:** Corey schuurmachines, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani, Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Inleiding
Door de onderdelen van de lagen van de verschillende groepen van toepassingen op verschillende computers ook als afzonderlijke onderdelen in elkaar te scheiden, kunt u verschillende soorten n-aantal lagen toepassingen ontwikkelen. Bijvoorbeeld, kunt u de clienttoepassing plaatsen en bedrijfsregels onderdelen op één computer, front-endweblaag en laag voor gegevenstoegang in een andere computer en een laag van de back-end-database in een andere computer. Dit soort structureren helpt elke laag van elkaar isoleren. Als u waar gegevens vandaan wijzigt, hoeft u niet te wijzigen van de client of web-toepassing, maar alleen de laag voor gegevenstoegang.

Een typische *n-aantal lagen* toepassing beschikt over de presentatielaag, de bedrijfslaag en de gegevenslaag:

| Laag | Beschrijving |
| --- | --- |
| **Presentatie** |De *presentatielaag* (weblaag, front-laag) is de laag waarin gebruikers met een toepassing werken. |
| **Bedrijven** |De *bedrijfslaag* (middelste laag) is de laag die laag van de presentatie en de gegevenslaag gebruiken om te communiceren met elkaar en bevat de kernfunctionaliteit van het systeem. |
| **Gegevens** |De *gegevenslaag* is in feite de server waarop een toepassing gegevens opgeslagen (bijvoorbeeld een server met SQL Server). |

Toepassingslagen beschrijven de logische groeperingen van de functies en onderdelen in een toepassing. terwijl lagen de fysieke distributie van de functies en onderdelen op afzonderlijke fysieke servers, computers, netwerken of externe locaties beschrijft. De lagen van een toepassing kunnen zich bevinden op dezelfde fysieke computer (dezelfde laag) of kunnen worden verdeeld over de afzonderlijke computers (n-laag) en de onderdelen in elke laag communiceren met andere lagen via goed gedefinieerde interfaces-onderdelen. U kunt zien van de termijn laag als verwijzingen naar fysieke distributiepatronen, zoals twee lagen en drie lagen n-aantal lagen. Een **2-tier application-patroon** bevat twee toepassingslagen: application server en database-server. Er gebeurt rechtstreekse communicatie tussen de toepassingsserver en de databaseserver. De toepassingsserver bevat zowel-weblaag en bedrijfslaag onderdelen. In **3-laagse toepassing patroon**, er zijn drie toepassingslagen: webserver, toepassingsserver, die de zakelijke logicalaag en/of business-laag voor gegevenstoegang bevat, en de databaseserver. De communicatie tussen de webserver en de database-server wordt via de toepassingsserver uitgevoerd. Zie voor gedetailleerde informatie over toepassingslagen en lagen [Microsoft toepassing Architecture Guide](https://msdn.microsoft.com/library/ff650706.aspx).

Voordat u dit artikel leest, moet u informatie gegeven over de basisconcepten van SQL Server en Azure hebben. Zie voor informatie [SQL Server Books Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) en [Azure.com](https://azure.microsoft.com/).

In dit artikel beschrijft enkele toepassing patronen die geschikt is voor uw eenvoudige toepassingen, evenals de zeer complexe bedrijfstoepassingen kunnen zijn. Voordat u met gedetailleerde informatie over elk patroon, het is raadzaam dat u moet vertrouwd raken met de beschikbare gegevens storage-services in Azure, zoals [Azure Storage](../../../storage/common/storage-introduction.md), [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md), en [SQL Server in een virtuele Machine van Azure](virtual-machines-windows-sql-server-iaas-overview.md). Als u wilt de beste ontwerpbeslissingen voor uw toepassingen, begrijpen wanneer duidelijk welke gegevens storage-service gebruiken.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>SQL Server in Azure een virtuele Machine, kiezen als:
* U moet een besturingselement in SQL Server en Windows. Dit kan bijvoorbeeld de versie van SQL Server, speciale hotfixes, prestaties, configuratie, enzovoort bevatten.
* U moet een volledige compatibiliteit met SQL Server on-premises en wilt overstappen van bestaande toepassingen naar Azure-is.
* U wilt gebruikmaken van de mogelijkheden van de Azure-omgeving, maar Azure SQL Database biedt geen ondersteuning voor alle functies die vereist zijn voor uw toepassing. Dit omvat de volgende gebieden:
  
  * **Databasegrootte**: op het moment dat dit artikel is bijgewerkt, SQL Database biedt ondersteuning voor een database van maximaal 1 TB aan gegevens. Als uw toepassing meer dan 1 TB aan gegevens vereist en u niet wilt om aangepaste sharding-oplossingen te implementeren, het raadzaam dat u SQL Server in een virtuele Machine van Azure. Zie voor de meest recente informatie [schalen uit Azure SQL-Databases](https://msdn.microsoft.com/library/azure/dn495641.aspx) en [Azure SQL Database servicecategorieën en prestatieniveaus](../../../sql-database/sql-database-service-tiers.md).
  * **HIPAA naleving**: gezondheidszorg klanten en onafhankelijke softwareleveranciers (ISV's) kunt [SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) in plaats van [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md) omdat SQL Server in een virtuele Machine van Azure is gedekt door HIPAA Business koppelen overeenkomst (BAA). Zie voor informatie over naleving, [Microsoft Azure Trust Center: naleving](https://azure.microsoft.com/support/trust-center/compliance/).
  * **Instantieniveau functies**: op dit moment functies die buiten de database live biedt geen ondersteuning voor SQL-Database (zoals gekoppelde Servers Agent taken, FileStream, Service Broker, enz.). Zie voor meer informatie [Azure SQL Database richtlijnen en beperkingen](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1-laag (eenvoudig): één virtuele machine
In dit patroon toepassing implementeert u uw SQL Server-toepassing en de database in een zelfstandige virtuele machine in Azure. Dezelfde virtuele machine bevat uw client-/ webtoepassing, bedrijfsonderdelen gegevenstoegangslaag en de databaseserver. De presentatie, zakelijke en gegevens toegangscode logisch van elkaar worden gescheiden, maar zich fysiek bevinden in een machine met één server. De meeste klanten beginnen met het patroon van deze toepassing en vervolgens zij uitbreiden door meer webrollen of virtuele machines toe te voegen aan het systeem.

Dit patroon van toepassing is handig wanneer:

* U wilt uitvoeren van een eenvoudige migratie naar Azure-platform om te evalueren of het platform van uw toepassing vereisten of niet beantwoordt.
* U wilt behouden van alle toepassingslagen die worden gehost in dezelfde virtuele machine in de dezelfde Azure-datacenter te verminderen de latentie tussen lagen.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.

Het volgende diagram ziet u een eenvoudige on-premises scenario en hoe u de cloudoplossing ingeschakeld in een enkele virtuele machine in Azure kunt implementeren.

![1-tier application-patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

De business-laag (bedrijfslogica en data access-onderdelen) implementeert op dezelfde fysieke laag als de laag voor presentatie kunt toepassingsprestaties, maximaliseren, tenzij u moet een afzonderlijke laag vanwege problemen schaalbaarheid of beveiliging.

Aangezien dit een zeer gangbaar patroon beginnen, handig kunnen het volgende artikel op de migratie om uw gegevens te verplaatsen naar uw SQL Server-VM: [een Database migreren naar SQL Server op een virtuele machine van Azure](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-laagse (eenvoudig): meerdere virtuele machines
In dit patroon toepassing implementeert u een 3-laagse toepassing in Azure door het plaatsen van elke toepassingslaag in een andere virtuele machine. Dit biedt een flexibele omgeving voor een eenvoudig scenario's voor scale-up en scale-out. Wanneer een virtuele machine uw client-/ webtoepassing bevat, andere die als host fungeert voor de bedrijfsonderdelen van uw en andere die als host fungeert voor de database-server.

Dit patroon van toepassing is handig wanneer:

* U wilt uitvoeren van een migratie van complexe databasetoepassingen op Azure Virtual Machines.
* Wilt u lagen van de verschillende groepen van toepassingen worden gehost in verschillende regio's. Bijvoorbeeld, kunt u gedeelde databases die worden geïmplementeerd in meerdere regio's voor rapportagedoeleinden.
* U wilt verplaatsen bedrijfstoepassingen van on-premises gevirtualiseerde platforms voor Azure Virtual Machines. Zie voor een gedetailleerde bespreking van de zakelijke toepassingen, [wat is er een zakelijke toepassing](https://msdn.microsoft.com/library/aa267045.aspx).
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.

Het volgende diagram laat zien hoe u een eenvoudige 3-laagse toepassing in Azure kunt plaatsen door het plaatsen van elke toepassingslaag in een andere virtuele machine.

![3-laagse toepassing patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

In dit patroon toepassing is er slechts één virtuele machine (VM) in elke laag. Als u meerdere virtuele machines in Azure hebt, raden wij aan dat u een virtueel netwerk instellen. [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) maakt een beveiligingsgrens vertrouwde en kunt u ook virtuele machines onderling communiceren via het privé IP-adres. Bovendien altijd Zorg ervoor dat alle internetverbindingen alleen gaat u naar de presentatielaag. Wanneer dit patroon van een toepassing te volgen, beheren de netwerkbeveiligingsgroepen om de toegang beperken. Zie voor meer informatie [toestaan van externe toegang tot uw virtuele machine met de Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

In het diagram zijn Internet-protocollen TCP, UDP, HTTP of HTTPS.

> [!NOTE]
> Instellen van een virtueel netwerk in Azure is gratis. Echter, u in rekening worden gebracht voor de VPN-gateway die is verbonden met on-premises. Deze kosten is gebaseerd op de hoeveelheid tijd die de verbinding ingericht en beschikbaar is.
> 
> 

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>laag 2 en 3-laagse met presentatie laag scale-out
In dit patroon toepassing kunt u database laag 2 of 3-laagse toepassing op Azure Virtual Machines implementeren door het plaatsen van elke toepassingslaag in een andere virtuele machine. Bovendien uitschalen u de presentatielaag vanwege toegenomen volume van binnenkomende aanvragen van clients.

Dit patroon van toepassing is handig wanneer:

* U wilt verplaatsen bedrijfstoepassingen van on-premises gevirtualiseerde platforms voor Azure Virtual Machines.
* Wilt u de presentatielaag vanwege toegenomen volume van binnenkomende aanvragen van clients worden uitgebreid.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.
* Wilt u de eigenaar van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden geschaald.

Het volgende diagram laat zien hoe u de toepassingslagen in meerdere virtuele machines in Azure kunt plaatsen door het uitbreiden van de presentatielaag vanwege toegenomen volume van binnenkomende aanvragen van clients. Zoals in het diagram kunt zien, is Azure Load Balancer verantwoordelijk voor het distribueren van verkeer over meerdere virtuele machines en ook bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de webservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag.

![Patroon toepassing - presentatie laag scale-out](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Aanbevolen procedures voor laag 2, 3-laagse of n-aantal lagen patronen die meerdere virtuele machines hebben met één laag
Het raadzaam dat u de virtuele machines die deel uitmaken van hetzelfde niveau in dezelfde cloudservice en in dezelfde de beschikbaarheid van de plaatsen ingesteld. Bijvoorbeeld, plaatst u een reeks webservers in **CloudService1** en **AvailabilitySet1** en een set van databaseservers in **CloudService2** en **AvailabilitySet2**. Beschikbaarheidsset in Azure kunt u de knooppunten voor hoge beschikbaarheid in afzonderlijke foutdomeinen plaatsen en upgradedomeinen.

Als u wilt gebruikmaken van meerdere VM-exemplaren van een laag, moet u Azure Load Balancer configureren tussen de toepassingslagen. Voor het configureren van Load Balancer in elke laag, een eindpunt met gelijke taakverdeling op virtuele machines voor elke laag afzonderlijk maken. Voor een specifieke laag, moet u eerst virtuele machines maken in dezelfde cloudservice. Dit zorgt ervoor dat ze hetzelfde openbare virtuele IP-adres hebben. Maak vervolgens een eindpunt op een van de virtuele machines op die laag. Hetzelfde eindpunt vervolgens toewijzen aan de andere virtuele machines op die laag voor taakverdeling. Door het maken van een set met gelijke taakverdeling verkeer verdelen over meerdere virtuele machines en ook toestaan dat de Load Balancer om te bepalen welk knooppunt om te verbinden wanneer een back-end-VM-knooppunt is mislukt. Bijvoorbeeld, met meerdere exemplaren van de webservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag.

Als een best practice altijd Zorg dat alle internetverbindingen eerst gaat u naar de presentatielaag. De laag voor presentatie toegang heeft tot de bedrijfslaag en vervolgens de bedrijfslaag toegang heeft tot de gegevenslaag. Zie voor meer informatie over het toestaan van toegang tot de laag voor presentatie [toestaan van externe toegang tot uw virtuele machine met de Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Houd er rekening mee dat de Load Balancer in Azure vergelijkbaar werkt aan load balancers in een on-premises omgeving. Zie voor meer informatie [taakverdeling voor Azure-infrastructuurservices](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Bovendien is het raadzaam dat u een particulier netwerk instellen voor uw virtuele machines met behulp van Azure Virtual Network. Hierdoor kunnen ze communiceren onderling via het privé IP-adres. Zie voor meer informatie [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>laag 2 en 3-laagse met business laag scale-out
In dit patroon toepassing kunt u database laag 2 of 3-laagse toepassing op Azure Virtual Machines implementeren door het plaatsen van elke toepassingslaag in een andere virtuele machine. Bovendien, als u wilt distribueren van de server-onderdelen van de toepassing met meerdere virtuele machines vanwege de complexiteit van uw toepassing.

Dit patroon van toepassing is handig wanneer:

* U wilt verplaatsen bedrijfstoepassingen van on-premises gevirtualiseerde platforms voor Azure Virtual Machines.
* Wilt u de server-onderdelen van de toepassing met meerdere virtuele machines vanwege de complexiteit van uw toepassing te distribueren.
* U wilt verplaatsen zakelijke logica zware on-premises LOB (line-of-business) toepassingen voor Azure Virtual Machines. LOB-toepassingen bestaan uit een set van essentiële toepassingen die essentieel zijn voor het uitvoeren van een bedrijf, zoals accounting, human resources (uur), salaris, geef keten management en resourceplanning toepassingen zijn.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.
* Wilt u de eigenaar van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden geschaald.

Het volgende diagram laat zien een lokale scenario en de cloudoplossing ingeschakeld. In dit scenario kunt u de toepassingslagen in meerdere virtuele machines in Azure plaatsen door het uitbreiden van de bedrijfslaag met de zakelijke logicalaag en data access-componenten. Zoals in het diagram kunt zien, is Azure Load Balancer verantwoordelijk voor het distribueren van verkeer over meerdere virtuele machines en ook bepalen welke webserver verbinding maken met. Met meerdere exemplaren van de toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de bedrijfslaag. Zie voor meer informatie [aanbevolen procedures voor laag 2, 3-laagse of n-tier application-patronen die meerdere virtuele machines hebben met één laag](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Patroon toepassing met business laag scale-out](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>laag 2 en 3-laagse met presentatie en zakelijke lagen scale-out en HADR
In dit patroon toepassing kunt u database laag 2 of 3-laagse toepassing op Azure Virtual Machines implementeren door het distribueren van de presentatielaag (webserver) en de business-laag (toepassingsserver)-onderdelen met meerdere virtuele machines. U kunt bovendien oplossingen voor hoge beschikbaarheid en noodherstel herstel implementeren voor uw databases in virtuele machines in Azure.

Dit patroon van toepassing is handig wanneer:

* U wilt verplaatsen bedrijfstoepassingen gevirtualiseerde platforms on-premises naar Azure door het implementeren van hoge beschikbaarheid van SQL Server- en herstelfuncties na noodgevallen.
* Wilt u de presentatielaag en de bedrijfslaag als gevolg van hogere volume van binnenkomende aanvragen van clients en de complexiteit van uw toepassing worden uitgebreid.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.
* Wilt u de eigenaar van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden geschaald.

Het volgende diagram laat zien een lokale scenario en de cloudoplossing ingeschakeld. In dit scenario dat u uitschalen de presentatielaag en de onderdelen van de laag bedrijven in meerdere virtuele machines in Azure. U kunt bovendien hoge beschikbaarheid en disaster recovery (HADR) technieken voor SQL Server-databases implementeren in Azure.

Virtuele machines Zorg met meerdere exemplaren van een toepassing in verschillende ervoor dat u aanvragen voor taakverdeling over ze. Wanneer u meerdere virtuele machines hebt, moet u ervoor zorgen dat alle virtuele machines toegankelijk en worden uitgevoerd op een punt in tijd zijn. Als u taakverdeling configureert, wordt Azure Load Balancer houdt de status van virtuele machines en binnenkomende oproepen aan de orde werkende VM knooppunten correct wordt verwezen. Zie voor informatie over het instellen van de taakverdeling van de virtuele machines, [taakverdeling voor Azure-infrastructuurservices](../tutorial-load-balancer.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Met meerdere exemplaren van web- en toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatie en lagen.

![Scale-out en hoge beschikbaarheid](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Aanbevolen procedures voor de toepassing patronen SQL HADR vereisen
Bij het instellen van hoge beschikbaarheid van SQL Server en oplossingen voor herstel na noodgevallen in Azure Virtual Machines, instellen van een virtueel netwerk voor uw virtuele machines met [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) is verplicht.  Virtuele machines binnen een virtueel netwerk heeft een stabiele privé IP-adres ook na de uitvaltijd van een service, dus u kunt voorkomen dat de updatetijd die nodig is voor DNS-naamomzetting. Bovendien wordt het virtuele netwerk kunt u uw on-premises netwerk uitbreiden naar Azure en maakt een beveiligingsgrens vertrouwd. Bijvoorbeeld, als uw toepassing heeft bedrijfsdomein beperkingen (zoals Windows-verificatie, Active Directory), instellen van [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) nodig is.

De meeste klanten productiecode op Azure uitvoert, worden behouden van primaire en secundaire replica's in Azure.

Zie voor uitgebreide informatie en zelfstudies voor hoge beschikbaarheid en disaster recovery technieken [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>laag 2 en 3-laagse met behulp van Azure VM's en Cloudservices
In dit patroon toepassing u laag 2 of 3-laagse toepassing in Azure implementeren met behulp van beide [Azure Cloud Services](../../../cloud-services/cloud-services-choose-me.md#tellmecs) (web- en werkrollen rollen - Platform als een Service (PaaS)) en [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (infrastructuur als een Service (IaaS)). Met behulp van [Azure Cloud Services](https://azure.microsoft.com/documentation/services/cloud-services/) voor de laag/business presentatielaag en de SQL-Server in [Azure Virtual Machines](../overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) voor de gegevens laag is nuttig voor de meeste toepassingen die worden uitgevoerd op Azure. De reden is dat met een compute-exemplaar met Cloud-Services biedt een eenvoudig beheer, implementatie, bewaking en scale-out.

Met Cloud Services, Azure houdt de infrastructuur voor u, voert routineonderhoud, patches van de besturingssystemen en probeert te herstellen van service en de hardware fouten. Als uw toepassing moet de scale-out, is automatische en handmatige scale-out-opties zijn beschikbaar voor uw cloudserviceproject door te verhogen of verlagen van het aantal exemplaren of virtuele machines die worden gebruikt door uw toepassing. U kunt bovendien lokale Visual Studio gebruiken om uw toepassing in een cloud service-project in Azure te implementeren.

Kortom, u niet wilt dat uitgebreide eigenaar administratieve taken voor de presentatie/bedrijven servicetier als uw toepassing niet complexe configuratie van de software of het besturingssysteem vereist, gebruikt u Azure Cloud Services. Als Azure SQL Database biedt geen ondersteuning voor alle functies die u zoekt, SQL Server gebruiken in een virtuele Machine van Azure voor de gegevenslaag. Een toepassing uitvoeren op Azure Cloud Services en opslaan van gegevens in Azure Virtual Machines combineert de voordelen van beide services. Zie de sectie in dit onderwerp voor een gedetailleerde vergelijking op [vergelijken ontwikkelingsstrategieën in Azure](#comparing-web-development-strategies-in-azure).

In dit patroon toepassing bevat de presentatielaag een Webrol, waardoor een Cloud Services-onderdeel wordt uitgevoerd in de van de Azure-uitvoeringsomgeving en deze kan worden aangepast voor het programmeren van web application ondersteund door IIS en ASP.NET. De laag bedrijfs- of back-end omvat een werkrol die een Cloud Services-onderdeel wordt uitgevoerd in de van de Azure-uitvoeringsomgeving en het is nuttig voor algemene ontwikkeling en achtergrondverwerking voor een Webrol kan uitvoeren. De databaselaag bevindt zich in een virtuele machine van SQL Server in Azure. De communicatie tussen de presentatielaag en de databaselaag gebeurt rechtstreeks of via de bedrijfslaag – onderdelen voor worker-rol.

Dit patroon van toepassing is handig wanneer:

* U wilt verplaatsen bedrijfstoepassingen gevirtualiseerde platforms on-premises naar Azure door het implementeren van hoge beschikbaarheid van SQL Server- en herstelfuncties na noodgevallen.
* Wilt u de eigenaar van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden geschaald.
* Azure SQL Database biedt geen ondersteuning voor alle functies die uw toepassing of de database moet.
* U wilt stress testen voor verschillende werkbelastingniveaus uitvoeren, maar tegelijkertijd u niet wilt beheer en onderhoud veel fysieke machines voortdurend.

Het volgende diagram laat zien een lokale scenario en de cloudoplossing ingeschakeld. In dit scenario moet u de presentatielaag in de bedrijfslaag in werkrollen, maar de gegevenslaag op virtuele machines in Azure webrollen plaatsen. Meerdere exemplaren van de presentatielaag uitgevoerd in verschillende webrollen zorgt u ervoor om te laden van aanvragen verdelen over ertussen. Wanneer u Azure Cloud Services met Azure Virtual Machines combineert, raden we aan dat u instelt [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) ook. Met [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), u kunt hebben stabiele en permanente privé IP-adressen in dezelfde cloud service in de cloud. Nadat u een virtueel netwerk voor uw virtuele machines definiëren en cloud services, kunnen ze starten onderling communiceren via het privé IP-adres. Bovendien hebben virtuele machines en Azure web/worker rollen in hetzelfde [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) lage latentie en veiliger connectiviteit. Zie voor meer informatie [wat is een cloudservice](../../../cloud-services/cloud-services-choose-me.md).

In het diagram kunt zien, wordt verkeer over meerdere virtuele machines en bepaalt ook welke webserver of toepassingsserver verbinding maken met Azure Load Balancer. Met meerdere exemplaren van de web- en toepassingsservers achter een load balancer zorgt ervoor dat de hoge beschikbaarheid van de presentatielaag en de bedrijfslaag. Zie voor meer informatie [Best practices voor toepassing vereisen SQL HADR patronen](#best-practices-for-application-patterns-requiring-sql-hadr).

![Toepassing van patronen met Cloudservices](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Een andere manier voor het implementeren van dit patroon van toepassing is het gebruik van een geconsolideerde Webrol die bevat presentatielaag en business laag onderdelen, zoals wordt weergegeven in het volgende diagram. Dit patroon van toepassing is nuttig voor toepassingen waarvoor stateful ontwerp. Omdat Azure staatloze rekenknooppunten voor web-en werkrollen bevat, het is raadzaam dat u een logica voor het opslaan van de sessiestatus met behulp van een van de volgende technologieën implementeren: [Azure Caching](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure Table Storage](../../../cosmos-db/table-storage-how-to-use-dotnet.md) of [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).

![Toepassing van patronen met Cloudservices](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Patroon met virtuele machines in Azure, Azure SQL Database en -Azure App Service (Web-Apps)
Het voornaamste doel van dit patroon van toepassing is op hoe u Azure-infrastructuur als een service (IaaS)-onderdelen met Azure-platform as a service onderdelen (PaaS) in uw oplossing combineert. Dit patroon is gericht op Azure SQL Database voor relationele gegevensopslag. Dit omvat geen SQL Server in Azure een virtuele machine, die deel uitmaakt van de Azure-infrastructuur als een serviceaanbieding.

In dit patroon toepassing kunt u de databasetoepassing van een in Azure implementeert met de presentatie en lagen plaatsen in dezelfde virtuele machine en een database in Azure SQL Database (SQL Database)-servers te openen. U kunt de presentatielaag implementeren met behulp van de traditionele IIS-webservices-oplossingen. Of u kunt een samengevoegde presentatie en de bedrijfslaag implementeren met behulp van [Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/).

Dit patroon van toepassing is handig wanneer:

* U hebt al een bestaande SQL-Database-server geconfigureerd in Azure en u wilt snel testen van uw toepassing.
* U wilt testen van de mogelijkheden van Azure-omgeving.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* Van uw zakelijke logica en gegevens die de onderdelen van de toegang kunnen worden ingesloten in een webtoepassing.

Het volgende diagram laat zien een lokale scenario en de cloudoplossing ingeschakeld. In dit scenario moet u de toepassingslagen in een enkele virtuele machine in Azure en toegang tot gegevens in Azure SQL Database plaatsen.

![Patroon van gemengde toepassing](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Als u een gecombineerde web en de toepassingslaag implementeert wilt met behulp van Azure Web Apps, raden wij aan dat u de categorie middelste laag of een toepassing als dynamic link libraries (DLL's) in de context van een webtoepassing houden.

Bovendien Bekijk de aanbevelingen gegeven in de [strategieën voor web-ontwikkeling in Azure vergelijken](#comparing-web-development-strategies-in-azure) sectie aan het einde van dit artikel voor meer informatie over het programmeren van technieken.

## <a name="n-tier-hybrid-application-pattern"></a>N-aantal lagen hybride toepassing patroon
In hybride n-tier application patroon, moet u uw toepassing met meerdere lagen die zijn gedistribueerd tussen on-premises en Azure implementeren. Daarom kunt u een systeem flexibele en herbruikbare hybride maken die u kunt wijzigen of bepaalde categorie toevoegen zonder te wijzigen van de andere lagen. Als u wilt uitbreiden van uw bedrijfsnetwerk naar de cloud, gebruikt u [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md) service.

Dit patroon van de toepassing hybride is handig wanneer:

* U wilt ontwikkelen van toepassingen die deels in de cloud worden uitgevoerd en deels on-premises.
* U wilt migreren van sommige of alle elementen van een bestaande on-premises toepassing naar de cloud.
* Wilt u bedrijfstoepassingen verplaatsen van on-premises gevirtualiseerde platforms voor naar Azure.
* Wilt u de eigenaar van een infrastructuur-omgeving die omhoog en omlaag op aanvraag kan worden geschaald.
* Wilt u snel inrichten ontwikkeling en testen omgevingen gedurende korte perioden.
* Wilt u een voordelige manier te laten worden back-ups voor bedrijfstoepassingen in de database.

Het volgende diagram ziet u een patroon met lagen hybride toepassing die zich over on-premises en Azure. Zoals u in het diagram, lokale infrastructuur omvat [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) domeincontroller ondersteuning bieden voor verificatie en autorisatie. Houd er rekening mee dat het diagram een scenario waarin bepaalde onderdelen van de gegevenslaag bevinden zich in een on-premises Datacenter toont dat sommige onderdelen van de gegevenslaag live in Azure. Afhankelijk van de behoeften van uw toepassing, kunt u verschillende andere hybride scenario's implementeren. U kunt bijvoorbeeld de presentatielaag en de bedrijfslaag bewaren in een on-premises-omgeving, maar de gegevenslaag in Azure.

![N-tier application-patroon](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

In Azure, kunt u Active Directory gebruiken als een zelfstandige cloud-map voor uw organisatie, of u kunt ook integreren met bestaande op de lokale Active Directory met [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Zoals u in het diagram, de laag bedrijfsonderdelen kunnen openen met meerdere gegevensbronnen, zoals [SQL-Server in Azure](virtual-machines-windows-sql-server-iaas-overview.md) via een particulier interne IP-adres, on-premises SQL Server via [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md), of naar [SQL-Database](../../../sql-database/sql-database-technical-overview.md) met behulp van de .NET Framework data provider-technologieën. In dit diagram is Azure SQL Database een optionele gegevens storage-service.

U kunt de volgende werkstroom in de opgegeven volgorde implementeren in n-aantal lagen hybride toepassing patroon:

1. Identificeren van bedrijfstoepassingen op database die worden teruggezet moet naar de cloud met behulp van de [Microsoft Assessment and Planning (MAP) Toolkit](http://microsoft.com/map). De MAP Toolkit verzamelt inventaris-en prestatiegegevens van computers die u voor virtualisatie overweegt en aanbevelingen biedt over capaciteit en evaluatie planning.
2. Plan de resources en de configuratie is vereist in de Azure-platform, zoals virtuele machines en opslagaccounts.
3. Instellen van de netwerkverbinding tussen het bedrijfsnetwerk on-premises en [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Als u de verbinding tussen het bedrijfsnetwerk on-premises en een virtuele machine in Azure instelt, moet u een van de volgende twee methoden gebruiken:
   
   1. Verbinding maken tussen on-premises en Azure via openbare eindpunten op een virtuele machine in Azure. Deze methode biedt een eenvoudige installatie en kunt u SQL Server-verificatie gebruiken in de virtuele machine. Bovendien instellen van uw netwerkbeveiligingsgroepen openbaar verkeer naar de virtuele machine te beheren. Zie voor meer informatie [toestaan van externe toegang tot uw virtuele machine met de Azure portal](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
   2. Verbinding maken tussen on-premises en Azure via Azure virtueel particulier netwerk (VPN)-tunnel. Deze methode kunt u domeinbeleid aan een virtuele machine in Azure kunt uitbreiden. U kunt bovendien firewallregels instellen en gebruiken van Windows-verificatie in de virtuele machine. Op dit moment ondersteunt Azure beveiligde site-naar-site VPN- en punt-naar-site VPN-verbindingen:
      
      * Met beveiligde site-naar-site-verbinding, kunt u geen netwerkverbinding tussen uw on-premises netwerk en het virtuele netwerk in Azure. Het wordt aanbevolen voor het verbinden van uw on-premises data center-omgeving naar Azure.
      * U kunt de netwerkverbinding tussen het virtuele netwerk in Azure en de afzonderlijke computers waarop het overal maken met beveiligde punt-naar-site-verbinding. Dit is vooral aanbevolen voor ontwikkelings- en testdoeleinden.
      
      Zie voor informatie over verbinding maken met SQL Server in Azure, [verbinden met een SQL Server-VM op Azure](virtual-machines-windows-sql-connect.md).
4. Geplande taken en waarschuwingen die back-up on-premises gegevens op een schijf van de virtuele machine in Azure instellen. Zie voor meer informatie [SQL Server-back-up en herstel met Azure Blob Storage-Service](https://msdn.microsoft.com/library/jj919148.aspx) en [back-up en herstel voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).
5. Afhankelijk van de behoeften van uw toepassing, kunt u een van de volgende drie gangbare scenario's implementeren:
   
   1. U kunt uw webserver, toepassingsserver en hoofdlettergevoelig gegevens in een databaseserver in Azure houden terwijl u de gevoelige gegevens on-premises behouden.
   2. U kunt uw webserver houden en toepassingsserver on-premises terwijl de database-server in een virtuele machine in Azure.
   3. U kunt houden van uw database-server, webserver, en toepassingsserver on-premises terwijl u de databasereplica op virtuele machines in Azure bewaren. Deze instelling kan de lokale webservers of reporting toepassingen toegang tot de database-replica's in Azure. U kunt daarom bereiken om te verlagen van de werkbelasting in een on-premises database. Het is raadzaam dat u dit scenario voor zware implementeren gelezen werkbelastingen en ontwikkeling doeleinden. Zie voor informatie over het maken van databasereplica's in Azure AlwaysOn Availability Groups op [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="comparing-web-development-strategies-in-azure"></a>Vergelijking van strategieën voor web-ontwikkeling in Azure
Als u wilt implementeren en een meerlaagse op basis van SQL Server-toepassing in Azure implementeert, kunt u een van de volgende twee programming methoden:

* Een traditionele webserver (IIS - Internet Information Services) instellen in Azure en access-databases in SQL Server in Azure Virtual Machines.
* Implementeren en implementeren van een cloudservice in Azure. Controleer vervolgens of deze cloudservice toegankelijk databases in SQL Server op Azure Virtual machines. Een cloudservice kan meerdere web- en werkrollen functies bevatten.

De volgende tabel bevat een vergelijking van traditionele websites ontwikkelen met Azure Cloud Services en Azure-Web-Apps met betrekking tot de SQL Server in Azure Virtual Machines. De tabel bevat Azure Web Apps is het mogelijk SQL Server gebruiken in Azure VM als gegevensbron voor Azure-Web-Apps via de openbare virtuele IP-adres of de DNS-naam.

|  | Traditionele webontwikkeling in Azure Virtual Machines | Cloudservices in Azure | Webhosting met Azure-Web-Apps |
| --- | --- | --- | --- |
| **Toepassingsmigratie van on-premises** |Bestaande toepassingen als-is. |Toepassingen moeten web-en werkrollen. |Bestaande toepassingen, zoals-maar geschikt is voor ingesloten webtoepassingen en webservices waarvoor snelle schaalbaarheid. |
| **Ontwikkeling en implementatie** |Visual Studio, WebMatrix, Visual Web Developer, Web Deploy, FTP, TFS, IIS-beheer, PowerShell. |Visual Studio, Azure SDK, TFS, PowerShell. Elke cloudservice heeft twee omgevingen waarop u uw servicepakket en -configuratie kunt implementeren: fasering en productie. U kunt een cloudservice kunt implementeren op de testomgeving te testen voordat u het verhogen naar productie. |Visual Studio, WebMatrix, Visual Web Developer, FTP, GIT, BitBucket, CodePlex, DropBox, GitHub, volgt, TFS, Web implementeert, PowerShell. |
| **Beheer en Setup** |U bent zelf verantwoordelijk voor beheertaken op de toepassing, gegevens, firewall-regels, virtueel netwerk en besturingssysteem. |U bent zelf verantwoordelijk voor beheertaken op de toepassing, gegevens, firewall-regels en virtueel netwerk. |U bent zelf verantwoordelijk voor beheertaken over de toepassing en alleen gegevens. |
| **Hoge beschikbaarheid en herstel na noodgevallen (HADR)** |Het is raadzaam dat u virtuele machines in dezelfde beschikbaarheidsset en in dezelfde cloudservice plaatsen. Behouden van uw virtuele machines in dezelfde kan beschikbaarheidsset Azure plaats van de knooppunten voor hoge beschikbaarheid in domeinen met afzonderlijke fouten en upgradedomeinen. Op dezelfde manier, waarbij u uw virtuele machines in dezelfde cloudservice Hiermee taakverdeling en virtuele machines met elkaar kunnen communiceren via het lokale netwerk binnen een Azure-Datacenter.<br/><br/>U bent zelf verantwoordelijk voor het implementeren van een hoge beschikbaarheid en noodherstel voor SQL Server in Azure Virtual Machines om te voorkomen dat de uitvaltijd. Zie voor ondersteunde HADR technologieën, [hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>U bent zelf verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Azure kan uw virtuele machines kunt verplaatsen als de hostcomputer in het datacenter is mislukt vanwege hardwareproblemen met de. Bovendien kan er geplande uitvaltijd van uw VM als de hostcomputer wordt bijgewerkt voor beveiliging of software updates. Daarom is het raadzaam dat u ten minste twee virtuele machines in elke toepassingslaag om te controleren of de continue beschikbaarheid behouden blijft. Azure biedt geen SLA voor één virtuele machine. Zie voor meer informatie [technische richtlijnen voor Azure tolerantie](../../../resiliency/resiliency-technical-guidance.md). |De storingen ten gevolge van de onderliggende hardware of besturingssysteem software wordt beheerd door Azure. Het is raadzaam dat u meerdere exemplaren van een web- of worker-rol om te controleren of de hoge beschikbaarheid van uw toepassing implementeren. Zie voor informatie [Cloudservices, virtuele Machines en virtuele netwerk Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](../../../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>U bent zelf verantwoordelijk voor het back-ups van uw eigen gegevens en toepassingen.<br/><br/>Voor de databases die zich in een SQL Server-database in een virtuele machine in Azure, bent u verantwoordelijk voor het implementeren van een hoge beschikbaarheid en noodherstel hersteloplossing om te voorkomen dat de uitvaltijd. Zie voor ondersteunde HDAR technologieën, hoge beschikbaarheid en herstel na noodgevallen voor SQL Server in Azure Virtual Machines.<br/><br/>**SQL Server Database Mirroring**: gebruik met Azure Cloud Services (web/werkrollen). SQL Server-VM's en een cloudserviceproject kunnen zich in hetzelfde virtuele netwerk Azure. Als SQL Server VM zich niet in hetzelfde virtuele netwerk, moet u een SQL Server-Alias voor het routeren van communicatie met het exemplaar van SQL Server maken. Bovendien hebben de aliasnaam moet overeenkomen met de naam van de SQL Server. |Hoge beschikbaarheid is overgenomen van de Azure-werkrollen, Azure blob-opslag en Azure SQL Database. Azure Storage heeft bijvoorbeeld drie replica's van alle blob, table en queue-gegevens. Op elk gewenst moment houdt Azure SQL Database drie replica's van gegevens die worden uitgevoerd: één primaire replica en twee secundaire replica's. Zie voor meer informatie [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) en [Azure SQL Database](../../../sql-database/sql-database-technical-overview.md).<br/><br/>Wanneer u SQL Server in Azure VM als gegevensbron voor Azure Web Apps, houd er rekening mee Azure Web Apps biedt geen ondersteuning voor Azure Virtual Network. Met andere woorden, moeten alle verbindingen vanuit Azure Web Apps met SQL Server-VM's in Azure gaan via openbare eindpunten van virtuele machines. Dit kan enkele beperkingen voor hoge beschikbaarheid en herstel na noodgevallen veroorzaken. Bijvoorbeeld, zou de clienttoepassing op Azure Web Apps die verbinding maken met SQL Server-VM met databasespiegeling niet kunnen verbinding maken met de nieuwe primaire server als databasespiegeling, instellen van Azure Virtual Network tussen SQL Server-host virtuele machines in Azure vereist. Daarom kan het gebruik **SQL Server Database Mirroring** met Azure-Web-Apps wordt momenteel niet ondersteund.<br/><br/>**SQL Server AlwaysOn-beschikbaarheidsgroepen**: U kunt AlwaysOn Availability Groups instellen bij het gebruik van Azure Web Apps met SQL Server-VM's in Azure. Maar moet u AlwaysOn beschikbaarheidsgroep-Listener voor het routeren van de communicatie met de primaire replica via openbare eindpunten voor netwerktaakverdeling configureren. |
| **cross-premises connectiviteit** |Verbinding maken met on-premises kunt u Azure Virtual Network. |Verbinding maken met on-premises kunt u Azure Virtual Network. |Virtuele Azure-netwerk wordt ondersteund. Zie voor meer informatie [Web Apps virtuele netwerkintegratie](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/). |
| **Schaalbaarheid** |Scale-up is beschikbaar door het verhogen van de grootte van virtuele machines of meer schijven toe te voegen. Zie voor meer informatie over de grootte van virtuele machines, [grootten van virtuele machines voor Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).<br/><br/>**Voor de databaseserver**: Scale-out is beschikbaar via de partitionering technieken database en SQL Server AlwaysOn-beschikbaarheidsgroepen.<br/><br/>Voor werkbelastingen met zware gelezen, kunt u [AlwaysOn Availability Groups](https://msdn.microsoft.com/library/hh510230.aspx) op meerdere secundaire knooppunten, evenals een SQL Server-replicatie.<br/><br/>Voor werkbelastingen zware schrijven, kunt u horizontale partitionering gegevens over meerdere fysieke servers om de toepassing scale-out implementeren.<br/><br/>Bovendien kunt u een scale-out implementeren met behulp van [SQL-Server met gegevensafhankelijke routering](https://technet.microsoft.com/library/cc966448.aspx). Met gegevens afhankelijke routering (DDR), moet u het mechanisme voor partitionering implementeren in de clienttoepassing, doorgaans in de laag business-laag voor het routeren van de database-aanvragen met meerdere knooppunten van de SQL Server. De bedrijfslaag bevat toewijzingen aan hoe de gegevens is gepartitioneerd en welk knooppunt de gegevens bevat.<br/><br/>Toepassingen die virtuele machines worden uitgevoerd, kunnen worden geschaald. Zie voor meer informatie [schalen van een toepassing](../../../cloud-services/cloud-services-how-to-scale-portal.md).<br/><br/>**Belangrijke opmerking**: de **automatisch schalen** functie in Azure kunt u automatisch vergroten of verkleinen van de virtuele Machines die worden gebruikt door uw toepassing. Deze functie zorgt ervoor dat de eindgebruikerservaring wordt niet negatief beïnvloed tijdens de piekuren en virtuele machines worden afgesloten wanneer de vraag laag is. Het verdient aanbeveling Stel de optie voor automatisch schalen die niet voor uw cloudservice als het SQL Server-VM's bevat. De reden is dat de functie voor automatisch schalen Azure een virtuele machine inschakelen kunt wanneer het CPU-gebruik in die virtuele machine is hoger dan enkele drempelwaarde en een virtuele machine uitschakelen wanneer het CPU-gebruik lager is dan het gaat. De functie voor automatisch schalen is nuttig voor de staatloze toepassingen, zoals webservers, waar elke virtuele machine de werkbelasting zonder alle verwijzingen naar een eerdere status kunt beheren. De functie voor automatisch schalen is echter niet handig voor stateful toepassingen, zoals SQL Server, waarbij slechts één exemplaar schrijven naar de database kunt. |Scale-up is beschikbaar via meerdere web- en werkrollen functies. Zie voor meer informatie over de grootte van virtuele machines voor webrollen en werkrollen [grootten configureren voor Cloud Services](../../../cloud-services/cloud-services-sizes-specs.md).<br/><br/>Wanneer u **Cloudservices**, kunt u meerdere functies om te distribueren verwerking en ook bereiken flexibel schalen van uw toepassing definiëren. Elke service in de cloud bevat een of meer webrollen en/of werkrollen, elk met een eigen toepassingsbestanden en de configuratie. Als u scale-up een cloudservice doordat het aantal rolexemplaren (virtuele machines) geïmplementeerd voor een rol en omlaag schalen een cloudservice door te verlagen van het aantal rolinstanties. Zie voor gedetailleerde informatie [Azure uitvoering modellen](../../../cloud-services/cloud-services-choose-me.md).<br/><br/>Scale-out is beschikbaar via de ingebouwde Azure hoge beschikbaarheid ondersteuning via [Cloudservices, virtuele Machines en virtuele netwerk Service Level Agreement](http://www.microsoft.com/download/details.aspx?id=38427) en Load Balancer.<br/><br/>Voor een toepassing met meerdere lagen, wordt u aangeraden dat u verbinding web/worker rollen toepassing maakt naar de database server VM's via Azure Virtual Network. Azure biedt bovendien voor de load balancer voor virtuele machines in dezelfde cloudservice gebruikersaanvragen verspreid over ze. Virtuele machines verbonden op deze manier kunnen rechtstreeks met elkaar communiceren via het lokale netwerk binnen een Azure-Datacenter.<br/><br/>U kunt instellen **automatisch schalen** op de Azure-portal, evenals de tijden planning. Zie voor meer informatie [automatisch schalen voor een Cloudservice in de portal configureren](../../../cloud-services/cloud-services-how-to-scale-portal.md). |**Omhoog en omlaag geschaald**: U kunt vergroten/verkleinen de grootte van het exemplaar (VM) die is gereserveerd voor uw website.<br/><br/>Scale-out: U kunt meer gereserveerde exemplaren (VM's) toevoegen voor uw website.<br/><br/>U kunt instellen **automatisch schalen** op de portal, evenals de tijden planning. Zie voor meer informatie [How to Scale Web Apps](../../../app-service/web-sites-scale.md). |

Zie voor meer informatie over het kiezen tussen deze programming methoden [Azure Web Apps, Cloud Services en virtuele machines: gebruik hiervan](../../../app-service/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het uitvoeren van SQL Server op Azure Virtual machines [SQL Server op Azure Virtual Machines Overview](virtual-machines-windows-sql-server-iaas-overview.md).

