---
title: SAP HANA-beschikbaarheid binnen één Azure-regio | Microsoft Docs
description: Beschrijft de SAP HANA-bewerkingen op de systeemeigen virtuele machines in Azure in een Azure-regio.
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
ms.date: 03/05/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 47dba73a6c22d11953485a69435000d3d2fe6f55
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>SAP HANA-beschikbaarheid binnen één Azure-regio
In dit artikel beschrijft de verschillende scenario's voor beschikbaarheid binnen één Azure-regio. Azure heeft veel regio's, verdeeld over de hele wereld. Zie voor een overzicht van Azure-regio's [Azure-regio's](https://azure.microsoft.com/regions/). Microsoft biedt voor het implementeren van SAP HANA op virtuele machines binnen een Azure-regio, implementatie van een enkele virtuele machine met een exemplaar HANA. Voor grotere beschikbaarheid, kunt u twee virtuele machines met twee HANA-exemplaren binnen implementeren een [Azure beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) die gebruikmaakt van HANA system replicatie voor beschikbaarheid. 

Azure biedt momenteel, is een openbare preview van [Azure beschikbaarheid zones (preview)](https://docs.microsoft.com/azure/availability-zones/az-overview). In dit artikel beschreven niet beschikbaarheid zones in detail. Maar we een algemene discussie over het gebruik van beschikbaarheidssets versus beschikbaarheid zones bevatten.

Wat is het verschil tussen een beschikbaarheidsset en een zone beschikbaarheid in Azure? Azure-regio's waar beschikbaarheid zones worden aangeboden hebben meerdere datacenters. De datacentra zijn onafhankelijk van elkaar in de levering van stroombron, koeling en het netwerk. De reden voor het aanbieden van verschillende zones binnen één Azure-regio is zodat u toepassingen kunt implementeren tussen twee of drie beschikbaarheid zones die worden aangeboden. Ervan uitgaande dat de bron- of problemen power de infrastructuur van de zone slechts één beschikbaarheid beïnvloedt, is de implementatie van uw toepassing binnen een Azure-regio nog steeds volledig functioneel als u gebruikmaakt van beschikbaarheid zones. Sommige minder capaciteit optreden. Bijvoorbeeld, zou virtuele machines in een zone zijn mogelijk verloren gegaan, maar virtuele machines in de andere twee zones nog steeds actief. 
 
Een Azure beschikbaarheidsset is een logische groepering-functie die ervoor zorgen dat de VM-resources die u in de beschikbaarheidsset plaatst van elkaar geïsoleerd fout zijn wanneer ze zijn geïmplementeerd in een Azure-datacenter. Azure zorgt ervoor dat de virtuele machines die u binnen een beschikbaarheid plaatsen over meerdere fysieke servers, compute rekken eenheden voor opslag en netwerkswitches uitvoeren ingesteld. In sommige Azure-documentatie, deze configuratie wordt aangeduid als plaatsingen in andere [update en fouttolerantie domeinen](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Deze plaatsingen zijn meestal binnen een Azure-datacenter. Ervan uitgaande dat de bron- en problemen power invloed kan zijn op het datacenter die u implementeert, zou de capaciteit in een Azure-regio worden beïnvloed.

De plaatsing van datacenters waarbij Azure beschikbaarheid zones is een compromis tussen de netwerklatentie tussen geïmplementeerd in verschillende zones die aanvaardbaar is voor de meeste toepassingen en een specifieke afstand tussen datacenters van services te leveren. Natuurlijke catastrophes in het ideale geval wouldn't invloed op de voeding, netwerk-levering en infrastructuur voor alle beschikbaarheid zones in deze regio. Echter, zoals gigantische natuurlijke catastrophes hebben, beschikbaarheid zones altijd bieden geen beschikbaarheid van de gewenste binnen één regio. Denk na over orkaan Maria die eiland van Portorico op 20 September 2017 bereikt. De orkaan veroorzaakt in feite een onleesbaar bijna 100 procent op het eiland 90-mijl-niveau.

## <a name="single-vm-scenario"></a>Eén VM-scenario

In een enkel VM-scenario maakt u een Azure-VM voor het SAP HANA-exemplaar. U kunt Azure Premium-opslag gebruiken als host voor de besturingssysteemschijf en alle gegevensschijven van uw. De Azure bedrijfstijd SLA van 99,9 procent en de serviceovereenkomsten van andere Azure-onderdelen is voldoende om te voldoen aan uw beschikbaarheids-Sla's voor uw klanten. In dit scenario hebt u niet nodig voor het benutten van Azure beschikbaarheidsset voor de DBMS-laag worden uitgevoerd. In dit scenario zijn u afhankelijk van twee verschillende functies:

- Azure virtuele machine automatisch opnieuw opstarten (ook wel Azure service herstel)
- SAP HANA automatisch opnieuw opstarten

Azure virtuele machine automatisch opnieuw starten of service herstel, wordt een functionaliteit in Azure die op twee niveaus werkt:

- De Azure-serverhost, controleert de status van een virtuele machine die wordt gehost op de server-host.
- De controller voor de Azure bewaakt de status en beschikbaarheid van de server-host.

Een selectievakje health functionaliteit controleert de status van elke virtuele machine die wordt gehost op een server op Azure-host. Als een virtuele machine in een nonhealthy status valt, kan een opnieuw opstarten van de virtuele machine worden geïnitieerd door de Azure-host-agent die de status van de virtuele machine controleert. De controller controleert de status van de host door het controleren van veel verschillende parameters die kunnen wijzen op problemen met de host. Ook wordt gecontroleerd op de toegankelijkheid van de host via het netwerk. Een indicatie van problemen met de host kan leiden tot de volgende gebeurtenissen:

- Als de host geeft een ongeldige status heeft, opnieuw opstarten van de host en het opnieuw opstarten van de virtuele machines die zijn uitgevoerd op de host.
- Als de host niet in orde na het opnieuw opstarten, opnieuw opstarten van de host en het opnieuw opstarten van de virtuele machines die oorspronkelijk als host op de host op een host in orde is fungeerden. In dit geval is de host gemarkeerd als niet in orde. Optie wordt niet gebruikt voor verdere implementaties totdat dit is uitgeschakeld of vervangen.
- Als de beschadigde host problemen tijdens het opnieuw opstarten wordt een onmiddellijke herstart van de virtuele machines op een host in orde heeft. 

Met de host en de VM bewaking door Azure worden Azure VM's die hostproblemen ondervinden automatisch opnieuw gestart op een gezonde Azure-host. 

Het tweede onderdeel waarvoor u in dit scenario is het feit dat de HANA-service die wordt uitgevoerd in een VM opnieuw gestart automatisch wordt gestart nadat de virtuele machine opnieuw is opgestart. U kunt instellen [HANA service automatisch opnieuw opstarten](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) via de services watchdog van de verschillende HANA-services.

U kunt dit scenario met één VM door een koude failoverknooppunt toe te voegen aan een SAP HANA-configuratie verbeteren. In de SAP HANA-documentatie voor deze installatie wordt aangeroepen [automatische failover host](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Deze configuratie kan het zinvol in een lokale implementatie situatie waarbij de serverhardware is beperkt en toe te wijzen aan een knooppunt met één server als host voor automatische failover-knooppunt voor een set van productiehosts. Maar in Azure, waarbij de onderliggende infrastructuur van Azure een goed doelserver voor een geslaagde VM opnieuw starten biedt, niet dat u voor het implementeren van SAP HANA-host automatisch een failover. Als gevolg hiervan hebben we geen referentiearchitectuur die voorziet in een stand-by-knooppunt voor HANA host automatisch een failover. Dit geldt ook voor SAP HANA scale-out configuraties.

## <a name="availability-scenarios-for-two-different-vms"></a>Beschikbaarheid scenario's voor twee verschillende virtuele machines

Als u twee Azure VM's binnen een Azure beschikbaarheidsset gebruikt, kunt u de actieve tijdsduur tussen deze twee virtuele machines verhogen als ze in Azure beschikbaarheidsset binnen een Azure-regio zijn geplaatst. De basisinstallatie in Azure eruit als volgt:

![Diagram van twee virtuele machines met alle lagen](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Ter illustratie van de beschikbaarheid van andere scenario's, is enkele van de lagen in het diagram worden weggelaten. Het diagram toont alleen lagen die virtuele machines, hosts, beschikbaarheidssets en Azure-regio's. Azure Virtual Network-instanties, resourcegroepen en abonnementen spelen niet een rol in de scenario's in deze sectie beschreven.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>Back-ups worden gerepliceerd naar een tweede virtuele machine

Een van de meest rudimentaire instellingen is het gebruik van back-ups. In het bijzonder, moet u wellicht transactielogboekback-ups van één virtuele machine naar een andere virtuele machine van Azure verzonden. U kunt het opslagtype van Azure. In deze installatie bent u verantwoordelijk voor het exemplaar van de geplande back-ups die worden uitgevoerd op de eerste virtuele machine naar de tweede virtuele machine uitvoeren van scripts. Als u gebruiken van de tweede VM-instanties wilt, moet u de volledige, incrementele/differentiële en transactielogboekback-ups herstellen naar het punt die u nodig hebt. 

De architectuur ziet er als volgt:

![Diagram van twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Deze instelling is niet goed geschikt voor het bereiken van goede Recovery Point Objective (RPO) en herstel tijd Objective (RTO) tijden. RTO keren zouden met name vanwege moet de volledige database volledig te herstellen met behulp van de gekopieerde back-ups afnemen. Deze instelling is echter handig voor het herstellen van ongewenste gegevens verwijderen op de belangrijkste exemplaren. Met deze instelling op elk gewenst moment kunt u herstellen naar een bepaald punt in tijd, uitpakken van de gegevens en importeer de verwijderde gegevens in uw belangrijkste exemplaar. Daarom kan het zinvol een back-up om methode te gebruiken in combinatie met andere functionaliteit van hoge beschikbaarheid. 

Terwijl de back-ups worden gekopieerd, kunt u mogelijk een kleinere VM dan de belangrijkste VM waarop het SAP HANA-exemplaar wordt gebruikt. Houd er rekening mee dat u een kleiner aantal VHD's aan een kleinere VM's koppelen kunt. Zie voor meer informatie over de beperkingen van afzonderlijke VM typen [grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

### <a name="sap-hana-system-replication-without-automatic-failover"></a>Replicatie voor SAP HANA-systeem zonder automatische failover

De scenario's in deze sectie beschreven SAP HANA system replicatie gebruiken. Zie voor documentatie over SAP [System replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Twee virtuele machines in Azure één Azure-regio hebben verschillende configuraties, dus er bepaalde verschillen in RTO zijn. Scenario's zonder automatische failover mogelijk in het algemeen niet van toepassing op virtuele machines in een Azure-regio. Dit is omdat voor de meeste fouten in de Azure-infrastructuur, het herstel van de Azure-service opnieuw is opgestart de primaire virtuele machine op een andere host. Er zijn enkele rand gevallen waar deze configuratie in termen van scenario's is mislukt helpen kan. Of, in sommige gevallen kan een klant mee meer efficiëntie.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>SAP HANA system replicatie zonder automatische failover en zonder vooraf laden van gegevens

In dit scenario kunt u replicatie voor SAP HANA-systeem gebruiken om gegevens te verplaatsen op een synchrone manier voor het bereiken van een RPO van 0. Anderzijds, hebt u een lang genoeg RTO die u niet hoeft failover of gegevens vooraf laden in de cache HANA-exemplaar. In dit geval is het mogelijk om verdere economie in uw configuratie door de volgende acties uitvoeren:

- Voer een ander SAP HANA-exemplaar in de tweede VM. Het SAP HANA-exemplaar in de tweede VM neemt de meeste van het geheugen van de virtuele machine. Meestal is dit als een failover naar de tweede virtuele machine wordt uitgevoerd. U kunt de tweede VM afsluiten zodat de gerepliceerde gegevens in de cache van het exemplaar van HANA in de tweede VM kan worden geladen.
- Gebruik een kleinere VM op de tweede VM. Als een failover optreedt, hebt u een aanvullende stap voordat u de handmatige failover. In deze stap maakt u de grootte van de virtuele machine naar de grootte van de bron-VM. Het scenario ziet er als volgt:

![Diagram van twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Zelfs als u niet vooraf laden van gegevens in de doel-HANA system replicatie gebruikt, moet u minimaal 64 GB aan geheugen. U moet ook voldoende geheugen naast 64 GB de rowstore om gegevens te behouden in het geheugen van de doelinstantie.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>SAP HANA system replicatie zonder automatische failover en met vooraf laden van gegevens

In dit scenario is de gegevens die zijn gerepliceerd naar de HANA-exemplaar in de tweede VM vooraf geladen. Hierdoor worden de twee voordelen van het niet vooraf laden van gegevens. U kunt een ander SAP HANA-systeem in dit geval niet uitvoeren op de tweede VM. U een kleinere VM ook niet gebruiken. Daarom implementeren klanten zelden voor dit scenario.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>Replicatie voor SAP HANA-systeem met automatische failover

In de standaard en meest voorkomende hebben configuratie van de beschikbaarheid binnen één Azure-regio, twee virtuele Azure-machines met Linux SLES een failover-cluster dat is gedefinieerd. Het Linux SLES-cluster is gebaseerd op de [pacemaker heeft](http://www.linux-ha.org/wiki/Pacemaker) framework in combinatie met een [STONITH](http://linux-ha.org/wiki/STONITH) apparaat. 

Vanuit het perspectief van een SAP HANA wordt de replicatiemodus die wordt gebruikt is gesynchroniseerd en een automatische failover is geconfigureerd. In de tweede VM fungeert het exemplaar SAP HANA als een hot stand-by-knooppunt. De stand-by-knooppunt ontvangt een synchrone stroom van wijzigingsrecords van het primaire SAP HANA-exemplaar. Als er transacties worden vastgelegd door de toepassing op het primaire knooppunt HANA, wacht de primaire HANA knooppunt om te bevestigen dat het doorvoeren naar de toepassing totdat het secundaire knooppunt voor SAP HANA bevestigt dat de commit-record wordt ontvangen. SAP HANA biedt twee modi voor synchrone replicatie. Zie het SAP-artikel voor meer informatie en een beschrijving van de verschillen tussen deze twee modi voor synchrone replicatie [replicatie modi voor SAP HANA system replicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

De configuratie van de algehele ziet er als volgt:

![Diagram van twee virtuele machines met storage-replicatie en failover](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

U kunt deze oplossing omdat Hiermee kunt u een RPO bereiken = 0 en een zeer lage RTO. De SAP HANA-clientconnectiviteit zodanig configureren dat de SAP HANA-clients het virtuele IP-adres gebruiken verbinding maken met de replicatie van HANA systeemconfiguratie. Hierdoor is nodig voor de toepassing als er een failover naar het secundaire knooppunt optreedt. In dit scenario wordt moet de Azure VM-SKU's voor de primaire en secundaire virtuele machines hetzelfde zijn.

## <a name="next-steps"></a>Volgende stappen

Zie voor stapsgewijze instructies over het instellen van deze configuraties in Azure:

- [De replicatie voor SAP HANA-systeem in Azure VM's instellen](sap-hana-high-availability.md)
- [Hoge beschikbaarheid voor SAP HANA met behulp van replicatie van systeem](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Zie voor meer informatie over de beschikbaarheid van de SAP HANA volledige Azure-regio's:

- [Beschikbaarheid van het SAP HANA volledige Azure-regio 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

