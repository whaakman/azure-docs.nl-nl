---
title: SAP-werkbelasting planning en implementatie-controlelijst | Microsoft Docs
description: Controlelijst voor het plannen en implementaties van SAP-workloads op Azure
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/07/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 29ec56f3b360a4a19269555c1504ae7ff6446f8e
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54189147"
---
# <a name="sap-workload-on-azure-planning-and-deployment-checklist"></a>SAP-werkbelasting op Azure controlelijst voor planning en implementatie 

Deze controlelijst is ontworpen voor klanten die hun SAP NetWeaver, S/4HANA, en Hybris toepassingen verplaatsen naar Azure Infrastructure as a Service.  Deze controlelijst moet worden gecontroleerd door een klant en/of de SAP-partner tijdens de duur van het project. Het is belangrijk dat veel van de controles worden uitgevoerd aan het begin van het project en in de planningsfase. Wanneer de implementatie is voltooid, kunnen elementaire wijzigingen voor geïmplementeerde Azure-infrastructuur of software-SAP-releases behoorlijk gecompliceerd zijn. Bekijk deze controlelijst bij belangrijke mijlpalen gedurende een project.  Kleine problemen kunnen worden gedetecteerd voordat ze grote problemen worden en voldoende tijd voor het opnieuw samenstellen en testen van de benodigde wijzigingen bestaat. De controlelijst in geen manieren claims moeten worden voltooid. Afhankelijk van uw afzonderlijke situatie, mogelijk zijn er veel meer controles die moeten worden uitgevoerd. 

De controlelijst verzameld bevat geen taken die onafhankelijk van Azure zijn.  Voorbeeld: SAP-toepassing Interfaces veranderen tijdens de overgang naar Azure openbare Cloud of naar een hosting provider.    

Deze controlelijst kan ook worden gebruikt voor geïmplementeerde systemen. Nieuwe functies zoals Write Accelerator, Beschikbaarheidszones en nieuwe VM-typen kunnen is toegevoegd nadat u hebt geïmplementeerd.  Daarom is het nuttig om te controleren van de controlelijst periodiek om te controleren of dat u zich bewust bent van nieuwe functies in de Azure-platform. 

## <a name="project-preparation-and-planning-phase"></a>Project voorbereidings- en planningsfase
In deze fase is een migratie van SAP-workload naar Azure openbare cloud gepland. De minimale set entiteiten en items besproken en lijst, zoals gedefinieerd:

1. Op hoog niveau ontwerpdocument: dit document moet bevatten:
    1. De huidige voorraad van SAP-onderdelen en toepassingen en inventarisatie van de doel-toepassingen op Azure
    2. Een architectuur op hoog niveau oplossing
    3. Besluit op Azure-regio's voor de implementatie in. Voor een lijst van Azure-regio's, Controleer de [Azure-regio's](https://azure.microsoft.com/global-infrastructure/regions/). Voor services die beschikbaar zijn in elk van de Azure-regio's, controleert u het artikel [producten beschikbaar per regio](https://azure.microsoft.com/global-infrastructure/services/)
    4. Netwerken verbinding maken vanaf on-premises naar Azure-architectuur. Start om uzelf bekend zijn met de [blauwdruk voor Azure Virtual Datacenter](https://docs.microsoft.com/azure/architecture/vdc/)
    5. Beveiligingsprincipes voor het uitvoeren van hbi-gegevens in Azure. Voor het lezen van materiaal beginnen met [documentatie over Azure-beveiliging](https://docs.microsoft.com/azure/security/)
2.  Technisch ontwerpdocument – bevat:
    1.  Een diagram van de oplossing 
    2.  De grootte van rekenkracht, opslag en netwerkonderdelen in Azure. Voor SAP-grootte van virtuele Azure-machines, moet u de SAP-notitie voor ondersteuning [#1928533](https://launchpad.support.sap.com/#/notes/1928533) 
    3.  Architectuur van zakelijke continuïteit en herstel na noodgevallen
    4.  Gedetailleerde OS, DB, Kernel en SAP ondersteuning voor servicepack-versies. Het is niet een gezien het feit dat een OS-versie die wordt ondersteund door SAP NetWeaver of S/4HANA in Azure VM's wordt ondersteund. Hetzelfde geldt voor DBMS-releases. Het is verplicht dat de volgende bronnen selecteren ophalen voordat u uitlijnen en indien nodig bijwerken SAP-releases, DBMS-versies of OS releases om te worden in een SAP en Azure ondersteund venster. Het is verplicht dat u binnen SAP bent en Azure combinaties van de release ondersteunde voor volledige ondersteuning van SAP en Microsoft. Indien nodig, moet u plannen voor het upgraden van sommige van de softwareonderdelen. Meer informatie over ondersteunde SAP-, OS- en DBMS-software wordt beschreven in deze locaties:
        1.  SAP-notitie voor ondersteuning [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Deze opmerking definieert de minimale OS-versies worden ondersteund in Azure-VM's. Het definieert ook de minimale database-versies die zijn vereist voor de meeste niet-HANA-database. De opmerking geeft ook de SAP-grootte van de verschillende SAP ondersteunde Azure-VM-typen.
        2.  SAP-notitie voor ondersteuning [#2039619](https://launchpad.support.sap.com/#/notes/2039619). De opmerking definieert de ondersteuningsmatrix voor Oracle op Azure. Houd er rekening mee dat Oracle alleen Windows- en Oracle Linux als gastbesturingssysteem in Azure voor de werkbelasting van SAP ondersteunt. Deze ondersteuningsverklaring is van toepassing voor de SAP-toepassingslaag evenals SAP-instanties. Oracle biedt echter geen ondersteuning voor hoge beschikbaarheid voor SAP Central Services. Als gevolg hiervan moet u mogelijk een ander besturingssysteem alleen voor SAP Central Services, die geen verbinding met de Oracle-DBMS maken
        3.  SAP-notitie voor ondersteuning [#2235581](https://launchpad.support.sap.com/#/notes/2235581) ophalen van de ondersteuningsmatrix voor SAP HANA op de verschillende OS-versies
        4.  SAP HANA ondersteunde Azure-VM's en [HANA grote instanties](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) worden vermeld [hier](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)
        5.  [SAP-Product beschikbaarheid Matrix](https://support.sap.com/en/)
        6.  Andere SAP-opmerkingen voor andere specifieke SAP-producten  
    5.  Het wordt aangeraden de strikte 3-Laagse ontwerpen voor SAP-productiesystemen. Combineren ASCS + APP servers op dezelfde virtuele machine wordt niet aanbevolen.  Met behulp van configuraties van clusters met multi-SID voor SAP Central Services wordt ondersteund door Windows als gastbesturingssysteem op Azure. Terwijl de configuraties van SAP Central Services multi-SID-clusters worden niet ondersteund met Linux-besturingssystemen op Azure. Documentatie voor het geval van Windows Gast OS vindt u:
        1.  [SAP ASCS/SCS-exemplaar multi-SID hoge beschikbaarheid met Windows Server Failover Clustering en gedeelde schijf op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-shared-disk)
        2.  [SAP ASCS/SCS-exemplaar multi-SID hoge beschikbaarheid met Windows Server Failover Clustering en bestandsshare op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ascs-ha-multi-sid-wsfc-file-share)
    6.  Hoge beschikbaarheid en disaster recovery-architectuur
        1.  Op basis van de RTO en RPO definiëren wat de hoge beschikbaarheid en disaster recovery-architectuur moet er als volgt uitzien
        2.  Voor hoge beschikbaarheid binnen dezelfde zone controleren wat de gewenste DBMS-systemen te bieden in Azure heeft. De meeste DBMS-systemen bieden synchrone methoden van een synchrone hot stand-by, aanbevolen voor productiesystemen.
        3.  Controleer welke mogelijkheden worden aangeboden door de verschillende DBMS-leveranciers voor herstel na noodgevallen in verschillende Azure-regio's. De meeste van deze ondersteuning voor asynchrone replicatie of back-upfunctie
        4.  Voor de SAP-toepassingslaag definiëren of u zou worden uitgevoerd de regressie van uw bedrijf testsystemen, die in het ideale geval replica's van uw productie-implementaties worden, in dezelfde Azure-regio of in de DR-regio. In het laatste geval is, kunt u dat zakelijke regressie systeem richten als doel voor herstel na Noodgevallen voor uw productie
        5.  Als u geen te niet-productiesystemen in de DR-site besluit, zoekt u in Azure Site Recovery als levensvatbaar methode voor het repliceren van de SAP-toepassingslaag in de Azure-DR-regio. Zie ook [herstel na noodgevallen voor een implementatie met meerdere lagen SAP NetWeaver-app instellen](https://docs.microsoft.com/azure/site-recovery/site-recovery-sap) 
        6.  Als u wilt gebruiken een gecombineerde HA-/ DR configuratie zodat [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview) uzelf bekend zijn met de Azure-regio's waar Beschikbaarheidszones beschikbaar zijn en met beperkingen die kunnen worden geïntroduceerd door toegenomen maken netwerk latentie tussen twee Beschikbaarheidszones  
3.  Klant/Partner moet een inventarisatie van alle SAP-interfaces (SAP als niet-SAP) maken. 
4.  Ontwerp van Foundation Services-ontwerp - dit ontwerp omvat items zoals
    1.  Active Directory en DSN ontwerpen
    2.  Netwerktopologie in Azure en de toewijzing van verschillende SAP-systemen
    3.  [Toegang op basis van rollen](https://docs.microsoft.com/azure/role-based-access-control/overview) structuur voor uw verschillende teams die infrastructuur en SAP-toepassingen in Azure beheren
    3.  Topologie van de resourcegroep 
    4.  [Strategie voor het labelen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#tags-and-billing)
    5.  Naming Convention voor virtuele machines en andere onderdelen van de infrastructuur en/of logische namen
5.  Microsoft Premier-ondersteuningscontract – identificeren MS Technical Account Manager (TAM). Vereisten door SAP Lees voor ondersteuning van SAP-notitie voor ondersteuning [#2015553](https://launchpad.support.sap.com/#/notes/2015553) 
6.  Aantal Azure-abonnementen en quotum voor kernen voor de verschillende abonnementen definiëren. [Voor ondersteuningsverzoeken ter verhoging van de quota's van Azure-abonnementen openen](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) indien nodig 
7.  Gegevensreductie en gegevensmigratie plannen voor SAP-gegevens migreren naar Azure. Voor SAP NetWeaver-systemen heeft SAP richtlijnen over het behouden van het volume van een groot aantal gegevens beperkt. SAP gepubliceerd [deze ingrijpende handleiding](https://help.sap.com/http.svc/rc/2eb2fba8f8b1421c9a37a8d7233da545/7.0/en-US/Data_Management_Guide_Version_70E.PDF) over het gegevensbeheer van in SAP ERP-systemen. Echter bepaalde inhoud is van toepassing op NetWeaver en S/4HANA-systemen in het algemeen.
8.  Definieer en geautomatiseerde implementatie bepalen. Doel van automation achter implementaties van een op Azure is om te implementeren in een deterministische wijze en deterministische resultaten krijgt. Veel klanten Power Shell of op basis van CLI-scripts gebruiken. Maar er zijn verschillende open-source-technologieën die kunnen worden gebruikt voor het implementeren van Azure-infrastructuur voor SAP en zelfs SAP-software te installeren. Voorbeelden vindt u in github:
    1.  [Geautomatiseerde SAP-oplossingen in Azure-Cloud](https://github.com/Azure/sap-hana)
    2.  [SAP HANA-installatie](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)
9.  Definieer een reguliere ontwerp en implementatie revisie uitgebracht tussen u als klant, systeemintegrator, Microsoft en andere betrokken partijen

 
## <a name="pilot-phase-optional"></a>Testfase (optioneel)
 
De test kunt uitvoeren voordat u parallel aan het project plannen en voorbereiden. De fase kan ook worden gebruikt voor het testen van benaderingen en het ontwerp gemaakt in de fase plannen en voorbereiden. De testfase kan worden uitgebreid naar een echte bewijs van concepten. Het verdient aanbeveling om te stellen en een volledige HA-/ DR-oplossing te valideren en beveiligingsontwerp tijdens een proef-implementatie. In sommige gevallen klant kunnen schaalbaarheid tests ook worden uitgevoerd in deze fase. Andere klanten gebruiken de implementatie van SAP-systemen voor sandbox als testfase. Dus we ervan uitgaan dat u hebt vastgesteld dat een systeem dat u migreren naar Azure dat wilt voor het uitvoeren van een leider.

1.  Optimaliseer gegevens worden overgebracht naar Azure. Sterk afhankelijk van de klant cases overboeking via [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) van on-premises is snelste als het Circuit Express voldoende bandbreedte heeft. Met andere klanten plannen via internet om te worden sneller
2.  In het geval van een SAP heterogene platform-migratie, die bestaat uit een exporteren en importeren van de databasegegevens, testen, en optimaliseren van exporteren en importeren van de fasen. Voor grote migraties met betrekking tot SQL Server als de doelplatform, kunnen u aanbevelingen vinden [hier](https://blogs.msdn.microsoft.com/saponsqlserver/2017/05/08/sap-osdb-migration-to-sql-server-faq-v6-2-april-2017/). U kunt de aanpak van migratie Monitor/SWPM uitvoeren als u niet een upgrade van een gecombineerde versie of [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) verwerken wanneer u de migratie bij een upgrade van de release SAP combineren en te voldoen aan bepaalde bron- en DBMS-platform combinaties zoals beschreven, bijvoorbeeld in [Database migratie optie (DMO) van de som 2.0 SP04](https://launchpad.support.sap.com/#/notes/2644872). 
    1.  Exporteren naar bron, exporteren-bestand uploaden naar Azure en importeren van prestaties.  Maximaliseren overlapping tussen importeren en exporteren
    2.  Volume van de database tussen de doel- en bestemmings-platform om aan in de grootte van de infrastructuur te evalueren    
    3.  Valideren en timing optimaliseren 
3.  Technische validatie 
    1.  Soorten VM
        1.  Valideren van de bronnen op de opmerkingen bij de ondersteuning van SAP en SAP HANA-hardware-directory SAP PAM opnieuw om het ervoor te zorgen dat er geen wijzigingen in de ondersteunde VM's voor Azure, OS-versies ondersteund voor deze VM-typen en SAP en DBMS versies ondersteund
        2.  Opnieuw valideren van de grootte van uw toepassing en de infrastructuur die u op Azure implementeert. In het geval van het verplaatsen van bestaande toepassingen, u vaak kan worden afgeleid de benodigde SAP's van de infrastructuur die u gebruikt en de [SAP benchmark webpagina](https://www.sap.com/dmc/exp/2018-benchmark-directory/#/sd) en dit vergelijken met de SAP's getallen die worden vermeld in de SAP-notitie voor ondersteuning [#1928533](https://launchpad.support.sap.com/#/notes/1928533). Houd er ook [in dit artikel](https://blogs.msdn.microsoft.com/saponsqlserver/2018/11/04/saps-ratings-on-azure-vms-where-to-look-and-where-you-can-get-confused/) in gedachten
        3.  Evalueren en te testen van de grootte van uw Azure-VM's met betrekking tot de maximale doorvoer en de netwerkdoorvoer van de verschillende VM-typen die u hebt gekozen in de planningsfase. De gegevens kunnen worden gevonden:
            1.  [Grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). Het is belangrijk rekening houden met de **schijfdoorvoer, met maximale zonder caching** voor schaling
            2.  [Grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) is het belangrijk om te overwegen de **schijfdoorvoer, met maximale zonder caching** voor schaling
    2.  Storage
        1.  Azure Premium Storage gebruiken voor Database-VM 's
        2.  Gebruik [Azure beheerde schijven](https://azure.microsoft.com/services/managed-disks/)
        3.  Gebruik Azure Write Accelerator voor DBMS log schijven met M-serie. Houd rekening met Write accelerator limieten en -gebruik, zoals beschreven in [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)
        4.  Voor de verschillende DBMS-systemen, Controleer de [algemene SAP DBMS-documentatie gerelateerde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general) en de specifieke documentatie DBMS het algemene document verwijst u naar
        5.  Voor SAP HANA, meer details worden gedocumenteerd in [configuraties van SAP HANA-infrastructuur en bewerkingen op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
    3.  Netwerken
        1.  Test en evalueer uw VNet-infrastructuur en de distributie van uw SAP-toepassingen in of in de verschillende virtuele netwerken
            1.  De benadering van de hub en spoke-architectuur voor virtueel netwerk of microsegmentation binnen een enkel Azure-netwerk op basis van evalueren
                1.  Kosten vanwege het uitwisselen van gegevens tussen [Azure VNets gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Controleer voor de kosten van [prijzen voor Virtual Network](https://azure.microsoft.com/pricing/details/virtual-network/)
                2.  Snelle verbinding verbreken van de peering tussen virtuele Azure-netwerken in vergelijking met wijziging NSG aan een subnet binnen een virtueel netwerk voor gevallen waarin werd een veiligheidsrisico inhouden van toepassingen of virtuele machines die worden gehost in een subnet van het virtuele netwerk isoleren
                3.  Centrale logboekregistratie en bewaking van netwerkverkeer tussen on-premises, buitenwereld en het virtuele datacenter dat in Azure worden hebt gemaakt
            2.  Evalueren en te testen gegevenspad tussen SAP-toepassingslaag en SAP DBMS-laag. 
                1.  Alle plaatsing van [Azure Network Virtual Appliances](https://azure.microsoft.com/solutions/network-appliances/) in het communicatiepad tussen de SAP-toepassing en de DBMS-laag van een SAP NetWeaver, Hybris of S/4HANA van SAP-systemen is niet helemaal worden ondersteund
                2.  SAP-toepassingslaag en SAP DBMS in verschillende virtuele netwerken die niet worden gekoppeld plaatsen wordt niet ondersteund
                3.  [Azure ASG en NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview) worden ondersteund voor het definiëren van routes tussen de SAP-toepassingslaag en SAP DBMS-laag
            3.  Zorg ervoor dat [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) is ingeschakeld op de virtuele machines die worden gebruikt op het niveau van de SAP-toepassing en de SAP DBMS-laag. Houd er rekening mee dat verschillende niveaus van het besturingssysteem nodig zijn voor de ondersteuning van versnelde netwerken in Azure:
                1.  Windows Server 2012 R2 of een nieuwere versie
                2.  SUSE Linux 12 SP3 of een nieuwere versie
                3.  RHEL 7.4 of een nieuwere versie
                4.  Oracle Linux 7.5. Met behulp van de kernel RHCKL, moet de versie 3.10.0-862.13.1.el7. Met behulp van de Oracle-UEK is kernel versie 5 vereist
            4.   Testen en evalueren van de netwerklatentie tussen SAP-toepassingslaag VM en de DBMS-VM op basis van SAP-notitie voor ondersteuning [#500235](https://launchpad.support.sap.com/#/notes/500235) en ondersteuning-SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalueren van de resultaten op basis van de richtlijnen voor network-latentie van SAP-notitie voor ondersteuning [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). De netwerklatentie moet binnen het bereik gemiddeld en goed. Uitzonderingen zijn van toepassing op het verkeer tussen VM's en HANA grote instantie eenheden zoals beschreven [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)
            5.   Zorg ervoor dat ILB-implementaties zijn ingesteld voor het gebruik van de Direct Server Return is. Deze instelling wordt Verminder de latentie in gevallen waarin Azure ILB's worden gebruikt voor configuraties met hoge beschikbaarheid in de DBMS-laag
    4.   Hoge beschikbaarheid en disaster recovery-implementaties. 
        1.   Als u het niveau van de SAP-toepassing implementeren zonder het definiëren van een specifieke Azure-Beschikbaarheidszone, zorgt u ervoor dat alle virtuele machines met SAP dialoogvenster instantie of middleware exemplaren van een enkele SAP-systeem worden geïmplementeerd een [Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). 
            1.   Als u geen hoge beschikbaarheid nodig voor de SAP Central Services en de DBMS-systemen hebt, kunnen deze VM's worden geïmplementeerd in dezelfde Beschikbaarheidsset als het niveau van de SAP-toepassing
        2.   Als u de SAP Central Services en de DBMS-laag voor hoge beschikbaarheid met passieve replica's beveiligt, hebt u de twee knooppunten voor SAP Central Services in een afzonderlijke Beschikbaarheidsset en de twee DBMS-knooppunt in een andere Beschikbaarheidsset
        3.   Als u in Azure-Beschikbaarheidszones implementeert kunt u Beschikbaarheidssets kan niet gebruikmaken van. U moet echter om ervoor te zorgen dat u de actieve en passieve knooppunten voor Central Services implementeren in twee verschillende Beschikbaarheidszones, waarin de laagste latentie tussen zones.
            1.   Houd er rekening mee dat u wilt gebruiken [Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) voor het geval van het opstellen van Windows of Pacemaker failover-Clusters voor de laag DBMS-systemen en SAP Central Services in meerdere Beschikbaarheidszones. [Basic Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus) kan niet worden gebruikt voor zonegebonden implementaties 
    5.   Time-outinstellingen
        1.   SAP NetWeaver-ontwikkelaar traceringen van de verschillende SAP-exemplaren controleren en zorg ervoor dat er geen verbinding onderbrekingen tussen de server in de wachtrij plaatsen en de SAP-werkprocessen worden vermeld. Deze verbinding-einden worden vermeden door in te stellen van deze twee registerparameters:
            1.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveTime = 120000 - Zie ook [in dit artikel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957549(v=technet.10))
            2.   HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\KeepAliveInterval = 120000 - Zie ook [in dit artikel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc957548(v=technet.10)) 
        2.   Om te voorkomen dat GUI time-outs tussen 1-premises SAP-GUI-interfaces en lagen van SAP-toepassing geïmplementeerd in Azure geïmplementeerd, moet u controleren of de volgende parameters zijn ingesteld in de default.pfl of het exemplaar van het profiel:
            1.   rdisp/keepalive_timeout = 3600
            2.   rdisp/keepalive = 20
        3.   Als u een Windows-failovercluster-configuratie gebruikt, zorg ervoor dat de tijd om te reageren op niet-reagerend knooppunten correct is ingesteld voor Azure. Het Microsoft-artikel [afstemmen Failover Cluster netwerk drempelwaarden](https://blogs.msdn.microsoft.com/clustering/2012/11/21/tuning-failover-cluster-network-thresholds/) een lijst met parameters en hoe die invloed hebben op failover gevoeligheid. Van de parameters moeten deze twee parameters worden ingesteld door de waarden:
            1.   SameSubNetDelay = 2
            2.   SameSubNetThreshold = 15
4.   Testen van herstelprocedures van de hoge beschikbaarheid en herstel na noodgeval
    1.   Failover-situaties simuleren door het afsluiten van virtuele machines of besturingssystemen in de modus voor paniek plaatsen om te kunnen beoordelen of uw failoverconfiguraties werkt zoals ontworpen. 
    2.   Meet de tijden die nodig is voor het uitvoeren van een failover. Als de tijdstippen te lang duren, kunt u overwegen:
        1.   Gebruiken voor SUSE Linux SBD apparaten in plaats van de Agent beide kanten van Azure om failover te versnellen
        2.   Voor SAP HANA, als het laden van gegevens te lang duurt kunt u overwegen om in te richten meer opslag bandbreedte
    3.   Testen van back-up en restore-sequentie en af te stemmen indien nodig
    4.   Testen in de regio DR-functionaliteit en architectuur
5.  Beveiligingscontrole
    1.  Test de geldigheid van de Azure-rol gebaseerd toegangsbeheer (RBAC)-architectuur die u hebt geïmplementeerd. Doel is om te scheiden en beperken van toegang en machtigingen van verschillende teams. Als u bijvoorbeeld moet SAP Basis teamleden kunnen VM's implementeert en het toewijzen van schijven in Azure storage in een bepaald Azure-netwerk. De Basis van de SAP-team mag echter niet zijn eigen virtuele netwerken maken of wijzigen van de instellingen van bestaande virtuele netwerken. Aan de andere kant moeten leden van het netwerkteam niet mogelijk voor het implementeren van virtuele machines in virtuele netwerken waar de SAP-toepassing en DBMS-VM's worden uitgevoerd. Noch de leden van het netwerkteam moet kunnen kenmerken van virtuele machines wijzigen of zelfs verwijderen VM's of schijven.  
    2.  Controleer of [NSG en ASC](https://docs.microsoft.com/azure/virtual-network/security-overview) regels worden naar behoren werkt en de beveiligde resources afschermen
    3.  Zorg ervoor dat alle resources die moeten worden versleuteld zijn versleuteld. Definieer en processen voor het back-up van certificaten, opslaan, en deze certificaten toegankelijk en herstellen van de versleutelde entiteiten worden uitgevoerd. 
    4.  Gebruik [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq) en/of voor waar mogelijk van een besturingssysteem oogpunt ondersteunt OS-schijven
    5.  Controleer dat niet te veel lagen van versleuteling zijn gebruikt. Het verstandig beperkt om te gebruiken Azure Disk encryption en klikt u vervolgens op top een van de methoden transparante Databaseversleuteling DBMS-systemen
6.  Prestatietesten
    1.  In SAP op basis van SAP-tracering en metingen, vergelijken bovenste 10 online rapporten naar de huidige implementatie, indien van toepassing 
    2.  In SAP op basis van SAP-tracering en metingen, vergelijken top 10 batch-taken naar de huidige implementatie, indien van toepassing 
    3.  In SAP op basis van SAP-tracering en metingen, vergelijken gegevensoverdrachten via interfaces in de SAP-systeem. Richt u op de interfaces waarvan u weet dat de overdracht nu wordt verzonden tussen verschillende locaties, zoals die van on-premises naar Azure 


## <a name="non-production-phase"></a>Niet-productie-fase 
In deze fase veronderstellen we dat na een geslaagde pilot of PoC u begint aan niet-productie SAP-systemen in Azure implementeert. De geleerde lessen en ervaringen buiten de POC-fase van de concepten moeten worden aangepast aan dergelijke implementatie. De criteria en stappen in de PoC pas in dit type ook implementaties. In deze fase u normaal gesproken ontwikkeling systemen implementeert, eenheid test systemen en zakelijke regressie test systemen in Azure. Het verdient aanbeveling dat ten minste één niet-productie-systeem in een SAP-toepassing-regel de configuratie volledig hoge beschikbaarheid, heeft zoals de toekomstige productiesysteem u. Er zijn extra stappen die u moet rekening houden met tijdens deze fase:  

1.  Voordat u doorgaat systemen van de oude platform in Azure resourcegegevens, zoals CPU-gebruik, opslagdoorvoer en IOPS-gegevens verzamelen. Met name van de DBMS-laag eenheden, maar ook van de toepassing layer-eenheden. Ook netwerk- en latentie meten.
2.  Noteer de beschikbaarheid van tijd gebruikspatronen van de systemen. Doel is om te achterhalen of niet-productiesystemen moeten worden 7 x 24 uur per dag beschikbaar is of dat er zijn niet-productiesystemen die kunnen worden afgesloten tijdens bepaalde fasen van een week of maand
3.  Test en bepalen of u wilt maken van eigen installatiekopieën van het besturingssysteem voor uw virtuele machines in Azure of dat u wilt een installatiekopie uit de galerie met installatiekopieën van Azure gebruikt. Als u van een installatiekopie uit de Azure-galerie gebruikmaakt, zorg ervoor dat u de juiste installatiekopie die overeenkomt met het contract voor ondersteuning met de leverancier van uw besturingssysteem uitvoeren. Voor sommige leveranciers OS bieden Azure galerieën aan uw eigen installatiekopieën met licentie. Voor andere installatiekopieën van het besturingssysteem, ondersteuning is inbegrepen in de prijs die door Azure aangeboden. Als u besluit te maken van uw eigen installatiekopieën van het besturingssysteem, vindt u documentatie in deze artikelen:
    1.  U kunt een gegeneraliseerde installatiekopie van een Windows-virtuele machine geïmplementeerd in Azure op basis van bouwen [deze documentatie](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)
    2.  U kunt een gegeneraliseerde installatiekopie van een Linux VM die is geïmplementeerd in Azure op basis van bouwen [deze documentatie](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image)
3.  Zorg ervoor dat u te voldoen aan de vereisten van het ondersteuningsteam die SAP met betrekking tot Microsoft support-overeenkomsten heeft. Informatie kan worden gevonden in de SAP-notitie voor ondersteuning [#2015553](https://launchpad.support.sap.com/#/notes/2015553). Raadpleeg het document voor HANA grote instanties, [Onboarding-vereisten](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-onboarding-requirements)
4.  Zorg ervoor dat de juiste personen krijgen [meldingen gepland onderhoud](https://azure.microsoft.com/blog/a-new-planned-maintenance-experience-for-your-virtual-machines/), dit het geval is, u de downtime en opnieuw opstarten van virtuele machines in de tijd kiezen kunt
5.  Lees gestaag Azure-documentatie van Microsoft presentaties op kanalen, zoals [Channel 9](https://channel9.msdn.com/) voor nieuwe functionaliteit die van toepassing op uw implementaties zijn mogelijk
6.  Selectievakje SAP-opmerkingen met betrekking tot Azure, zoals ondersteuning Opmerking [#1928533](https://launchpad.support.sap.com/#/notes/1928533) voor nieuwe VM-SKU's of nieuwe ondersteunde OS- en DBMS-release. Nieuwe VM-typen op basis van oudere VM typen in prijzen, dus, dat kunt u virtuele machines te implementeren met de beste prijs-prestatieverhouding ratio vergelijken
7.  Valideren van de bronnen op de opmerkingen bij de SAP-ondersteuning, SAP HANA-hardware-directory en SAP PAM opnieuw om ervoor te zorgen dat er zijn geen wijzigingen in de ondersteunde VM's voor Azure, ondersteunde OS-versies op deze virtuele machines en ondersteunde SAP en DBMS releases
8.  Controleer [hier](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) voor nieuwe HANA--SKU's in Azure en vergelijken met de computer die u gepland en uiteindelijk wijzigen gecertificeerde als u de eenheden met betere prijs prestaties hoeveelheid prijzen 
9.  Aanpassen van uw implementatiescripts gebruikmaken van nieuwe VM-typen en opnemen van nieuwe functies van Azure die u wilt gebruiken
10. Na de implementatie van de infrastructuur, testen en evalueren van de netwerklatentie tussen SAP-toepassingslaag VM en de DBMS-VM op basis van SAP-notitie voor ondersteuning [#500235](https://launchpad.support.sap.com/#/notes/500235) en ondersteuning-SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalueren van de resultaten op basis van de richtlijnen voor network-latentie van SAP-notitie voor ondersteuning [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). De netwerklatentie moet binnen het bereik gemiddeld en goed. Uitzonderingen zijn van toepassing op het verkeer tussen VM's en HANA grote instantie eenheden zoals beschreven [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance). Zorg ervoor dat geen van de beperkingen die worden vermeld [overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general#azure-network-considerations) en [configuraties van SAP HANA-infrastructuur en bewerkingen op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations) van toepassing op uw implementatie
11. De controles uitvoeren, zoals vermeld in de fase testen van concepten voordat u de werkbelasting
12. Als de werkbelasting van toepassing is, het Resourcegebruik van deze systemen worden vastgelegd in Azure en vergelijken met de records die u hebt verkregen via uw oude platform. VM-grootte van de toekomstige implementaties aanpassen als u ziet dat er grotere verschillen. Houd er rekening mee dat in het geval van downsizing-, opslag- en netwerkbandbreedten van een virtuele machine wordt ook worden teruggebracht:
    1.  [Grootten voor Windows virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json). 
    2.  [Grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json) 
13. Werken op een kopie van systeemfunctionaliteit en processen. Doel is om gemakkelijk voor u om te kopiëren van een systeem voor de ontwikkeling of een testsysteem, dus dat teams project kunnen behalen nieuwe systemen. Houd rekening met [SAP LaMa](https://wiki.scn.sap.com/wiki/display/ATopics/SAP+Landscape+Management+%28SAP+LaMa%29+at+a+Glance) als een hulpprogramma dat u deze taken.
14. Optimaliseren en specificeren van uw team toegang krijgt tot Azure op basis van rollen, machtigingen en processen om ervoor te zorgen dat u een scheiding van het recht op de een-zijde hebt. Aan de andere kant, die u wilt alle teams ingeschakeld voor het uitvoeren van hun taken in de Azure-infrastructuur hebt.
15. Oefening-, test- en document hoge beschikbaarheid en noodherstel herstelprocedures zodat uw medewerkers die taken uitvoeren. Nadelen te identificeren en nieuwe functionaliteit van Azure die u in uw implementaties integreert aan te passen

 
## <a name="production-preparation-phase"></a>Voorbereidingsfase voor productie 
Dit is de fase waarin u wilt verzamelen van de ervaringen en geleerde lessen van uw niet-productie-implementaties en toepassen in de toekomst productie-implementaties. Verder naar de fasen voordat u moet ook voorbereiden van het werk van de gegevensoverdracht tussen uw huidige locatie van de host en Azure. 

1.  Vereiste SAP versie-upgrades van uw productiesystemen doorlopen voordat u verplaatst naar Azure
2.  Ik ga hiermee akkoord met de eigenaren van bedrijven in de functionele en business-tests die moeten worden uitgevoerd na de migratie van het productiesysteem
    1.  Zorg ervoor dat alle deze tests worden uitgevoerd met de bronsystemen in de huidige locatie van de host. U wilt voorkomen dat de tests voor de eerste keer nadat het systeem wordt verplaatst naar Azure wordt uitgevoerd
2.  Productie-migratieproces testen in Azure. Bouw groepen van productiesystemen die moeten worden op dezelfde host locatie in het geval van niet alle productiesystemen, naar Azure verplaatst in dezelfde periode. Oefening en test de gegevensmigratie. Lijst met algemene methoden, zoals:
    1.  DBMS-methoden, zoals back-up/herstel gebruiken in combinatie met het verzenden van SQL Server AlwaysOn, HANA System Replication of logboek seed-en synchroniseren van de inhoud van de database in Azure
    2.  Gebruik back-up/herstel voor kleinere databases
    3.  Gebruik SAP migratie bewaken in SAP SWPM hulpprogramma geïmplementeerd om uit te voeren heterogene migraties
    4.  Gebruik de [SAP DMO](https://blogs.sap.com/2013/11/29/database-migration-option-dmo-of-sum-introduction/) verwerken als u wilt combineren met de upgrade van een SAP-release. Houd er rekening mee dat niet alle combinaties tussen bron en doel DBMS-systemen worden ondersteund. Meer informatie vindt u in de notities van specifieke SAP-ondersteuning voor de verschillende versies van DMO. Bijvoorbeeld, [Database migratie optie (DMO) van de som 2.0 SP04](https://launchpad.support.sap.com/#/notes/2644872)
    5.  Test of overdracht van gegevens via internet of via ExpressRoute beter in de doorvoer is in het geval u moet back-ups verplaatsen of SAP bestanden exporteren. Houd er rekening mee dat voor het geval van gegevens te verplaatsen via internet, moet u mogelijk enkele van uw NSG/ASG beveiligingsregels die u nodig hebt voor toekomstige productiesystemen wijzigen
3.  Voordat u doorgaat systemen van de oude platform in Azure resourcegegevens, zoals CPU-gebruik, opslagdoorvoer en IOPS-gegevens verzamelen. Met name van de DBMS-laag eenheden, maar ook van de toepassing layer-eenheden. Netwerk- en lat ook meten
4.  ency.
4.  Valideren van de bronnen op de opmerkingen bij de SAP-ondersteuning, SAP HANA-hardware-directory en SAP PAM opnieuw om ervoor te zorgen dat er zijn geen wijzigingen in de ondersteunde VM's voor Azure, ondersteunde OS-versies op deze virtuele machines en ondersteunde SAP en DBMS releases 
4.  Implementatiescripts om de meest recente wijzigingen die u op VM-typen en functionaliteit van Azure besloten aan te passen
5.  Na de implementatie van de infrastructuur en toepassing gaat via een reeks controles in order valideren:
    1.  Het juiste VM-typen is geïmplementeerd met de juiste kenmerken en opslaggrootte
    2.  Controleer of de virtuele machines op de juiste en de gewenste OS releases en patches zijn en uniform zijn
    3.  Controleer of de beveiliging van de virtuele machines zijn zo nodig en uniform
    4.  Controleer of de juiste toepassing releases en patches hebt geïnstalleerd en worden geïmplementeerd
    5.  De virtuele machines hebt geïmplementeerd in Azure-Beschikbaarheidssets zoals gepland
    6.  Azure Premium Storage is gebruikt voor de latentie gevoelige schijven of waar de [VM SLA van 99,9% één](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) is vereist
    7.  Controleer voor de correcte implementatie van Azure Write Accelerator
        1.  Zorg ervoor dat binnen de virtuele machine, opslagruimten of stripe sets hebt u de juiste manier opgebouwd voor schijven die Azure Write Accelerator ondersteuning nodig hebben
    8.  [Azure managed disks](https://azure.microsoft.com/services/managed-disks/) exclusief zijn gebruikt
    9.  Virtuele machines hebt geïmplementeerd in de juiste Beschikbaarheidssets en Beschikbaarheidszones
    10. Zorg ervoor dat [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) is ingeschakeld op de virtuele machines die worden gebruikt op het niveau van de SAP-toepassing en de SAP DBMS-laag
    11. Er is geen plaatsing van [Azure Network Virtual Appliances](https://azure.microsoft.com/solutions/network-appliances/) in het communicatiepad voor tussen de SAP-toepassing en de DBMS-laag van een SAP NetWeaver Hybris of S/4HANA op basis van SAP-systemen
    12. ASG en NSG-regels communicatie toestaan als de gewenste en geplande en communicatie blokkeren indien nodig
    13. Time-outinstellingen zijn zoals eerder beschreven juist ingesteld
    14. Testen en evalueren van de netwerklatentie tussen SAP-toepassingslaag VM en de DBMS-VM op basis van SAP-notitie voor ondersteuning [#500235](https://launchpad.support.sap.com/#/notes/500235) en ondersteuning-SAP-notitie [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). Evalueren van de resultaten op basis van de richtlijnen voor network-latentie van SAP-notitie voor ondersteuning [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E). De netwerklatentie moet binnen het bereik gemiddeld en goed. Uitzonderingen zijn van toepassing op het verkeer tussen VM's en HANA grote instantie eenheden zoals beschreven [hier](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture#networking-architecture-for-hana-large-instance)
    15. Controleer of versleuteling is geïmplementeerd, indien nodig, en met de versleutelingsmethode die nodig zijn
    16. Controleer of de zojuist geïmplementeerde infrastructuur verbinding interfaces en andere toepassingen kunnen maken
6.  Maak een playbook voor reacties op Azure gepland onderhoud. Definieer de volgorde van de systemen en virtuele machines in het geval van gepland onderhoud opnieuw worden opgestart
    

## <a name="go-live-phase"></a>Live fase gaan
Voor de fase Go Live moet u ervoor zorgen dat uw playbooks die u hebt ontwikkeld in eerdere fasen Volg. Voer de stappen uit die u hebt getest en getraind. Ga niet akkoord laatste wijzigingen in configuraties en verwerken. Naast die van toepassing de volgende:

1. Controleren of Azure portal-bewaking en andere controleprogramma's werkt.  Aanbevolen hulpprogramma's zijn Perfmon (Windows) of SAR (Linux): 
    1.  Tellers voor CPU 
        1.  Gemiddelde CPU-tijd: totaal (alle CPU)
        2.  Gemiddelde CPU-tijd: elke afzonderlijke processor (dus 128 processors op virtuele machine m128)
        3.  CPU-tijd kernel: elke afzonderlijke processor
        4.  CPU-tijd gebruiker – elke afzonderlijke processor
    5.  Geheugen 
        1.  Beschikbaar geheugen
        2.  Geheugenpagina in/sec
        3.  Geheugenpagina out/sec
    4.  Schijf 
        1.  Schijf lezen kb/sec – per afzonderlijke schijf 
        2.  Schijf lezen per seconde – per afzonderlijke schijf
        3.  Schijf lezen/ms: per afzonderlijke schijf lezen
        4.  Schijf schrijven kb/sec – per afzonderlijke schijf 
        5.  Schijf schrijven per seconde – per afzonderlijke schijf
        6.  Schijf schrijven ms/lezen – per afzonderlijke schijf
    5.  Netwerk 
        1.  Pakketten per seconde
        2.  Netwerkpakketten out/sec
        3.  Netwerk kb/sec
        4.  Netwerk kb out/sec 
2.  Voer alle validatietests uit die u met de eigenaren van bedrijven overeengekomen na de migratie van de gegevens. Geef alleen toestemming validatie van de resultaten waarin u werkt met resultaten op de oorspronkelijke bronsystemen
3.  Controleer of interfaces functioneren en of andere toepassingen kunnen communiceren met de zojuist geïmplementeerde productiesystemen
4.  Controleer het transport en correctie systeem via SAP transactie stm
5.  Databaseback-ups uitvoeren nadat het systeem wordt vrijgegeven voor productie
6.  Back-ups van virtuele machine voor de SAP-toepassingslaag VM's uitvoeren nadat het systeem wordt vrijgegeven voor productie
7.  Voor SAP-systemen die geen onderdeel van de huidige go live zijn fase, maar communiceren met de SAP-systemen dat u in deze fase go live naar Azure verplaatst, moet u de host naam buffer in SM51 opnieuw instellen. Deze stap wordt verwijderen van oude IP-adressen in de cache die is gekoppeld aan de namen van de exemplaren van een toepassing die u naar Azure verplaatst  


## <a name="post-production"></a>Postproductie
In deze fase draait alles om bewaking, het besturingssysteem en het beheer van het systeem. Vanuit het oogpunt van een SAP, zijn de gebruikelijke taken die u met uw oude hostinglocatie zijn vereist om uit te voeren van toepassing. Er zijn Azure specifieke taken die u wilt doen:

1. Azure-facturen voor hoge Bezig met laden systemen analyseren
2. Optimale prijs-prestatieverhouding-efficiëntie op de virtuele machine kant- en opslag
3. Tijd systemen kunnen worden afgesloten optimaliseren  


## <a name="next-steps"></a>Volgende stappen
Raadpleeg de documentatie:

- [Azure virtuele Machines, planning en implementatie van SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)
- [Azure Virtual Machines-implementatie voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide)
- [Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_general)

