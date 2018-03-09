---
title: "SAP HANA beschikbaarheid binnen één Azure-regio | Microsoft Docs"
description: Bewerkingen van SAP HANA op virtuele machines in Azure systeemeigen
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/5/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28fe9bc7c8cb1d59df404b7dd7429def54648a18
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>SAP HANA beschikbaarheid binnen één Azure-regio
In deze sectie vindt u verschillende scenario's waarin de scenario's voor beschikbaarheid binnen één Azure-regio. Azure heeft veel regio's die worden verdeeld over de hele wereld. De lijst met Azure-regio's, raadpleegt u de [Azure-gebieden](https://azure.microsoft.com/regions/) artikel. U implementeert SAP HANA op virtuele machines binnen een Azure-regio, biedt Microsoft de implementatie van een enkele virtuele machine met een exemplaar HANA. Of u kunt twee virtuele machines met twee HANA-exemplaren binnen implementeren voor grotere beschikbaarheid een [Azure Beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) die gebruikmaken van HANA System Replication voor beschikbaarheid doeleinden. Azure heeft een openbare preview van [Azure beschikbaarheid Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). Deze Zones beschikbaarheid gaat niet nog in detail worden besproken. Met uitzondering van enkele algemene ideeën over het gebruik van Beschikbaarheidssets versus beschikbaarheid Zones.

Wat is het verschil tussen Beschikbaarheidssets van Azure en beschikbaarheid Zones? De regio's hebt voor Azure regio's waar beschikbaarheid Zones gaat worden aangeboden, meerdere datacenters die onafhankelijk bij levering van stroombron, koeling en het netwerk. Reden voor het aanbieden van verschillende zones binnen één Azure-regio is zodat u kunt toepassingen implementeren op twee of drie beschikbaarheid Zones die worden aangeboden. Ervan uitgaande dat problemen in stroomvoorziening en/of netwerk alleen één beschikbaarheid Zone-infrastructuur beïnvloedt, is de implementatie van uw toepassing binnen een Azure-regio nog steeds volledig functioneel. Uiteindelijk verloren met sommige minder capaciteit sinds een aantal virtuele machines in een zone zijn gegaan. Maar virtuele machines in de andere twee zones nog steeds actief zijn. 
 
Terwijl een Azure-Beschikbaarheidsset is een logische groepering-functie die u in Azure gebruiken kunt om ervoor te zorgen dat de VM-resources die u in plaats zijn geïsoleerd van elkaar wanneer ze zijn geïmplementeerd in een Azure-datacenter is mislukt. Azure zorgt ervoor dat de VM's die u in een beschikbaarheidsset plaatst, op meerdere fysieke servers, rekenrekken, opslageenheden en netwerkswitches worden uitgevoerd. Of als in een aantal andere Azure-documentatie, wordt dit aangeduid als plaatsingen in andere [Update en domeinen met fouten](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Deze plaatsingen zijn meestal binnen een Azure-datacenter. Ervan uitgaande dat problemen in stroomvoorziening en/of netwerk invloed kan zijn op het datacenter dat u zijn geïmplementeerd, zou de capaciteit in een Azure-regio worden beïnvloed.

De plaatsing van datacenters waarbij Azure beschikbaarheid Zones is een compromis tussen het leveren van netwerklatentie tussen services die zijn geïmplementeerd in verschillende zones die geschikt voor de meeste toepassingen en een bepaalde afstand tussen de datacentra zijn. Zodat natuurlijke catastrophes in het ideale geval zouden geen invloed op de kracht en levering van netwerk en infrastructuur voor alle beschikbaarheid Zones in deze regio. Echter, als gigantische natuurlijke catastrophes hebt u geleerd, beschikbaarheid Zones altijd mogelijk niet voor de beschikbaarheid binnen één regio desgewenst. Denk na over orkaan Maria die eiland van Portorico op 20/08/2017 bereikt en er in feite veroorzaakt nabij 100% zwart-out op het 90 mijl wide eiland.   
  


## <a name="single-vm-scenario"></a>Enkel VM-scenario
In dit scenario hebt u een Azure-virtuele machine voor het SAP HANA-exemplaar gemaakt. U hebt Azure Premium-opslag gebruikt voor het hosten van de besturingssysteemschijf en alle gegevensschijven. De beschikbaarheid SLA van 99,9% van Azure en de serviceovereenkomsten van andere Azure-onderdelen is voldoende om te voldoen aan uw beschikbaarheids-Sla's naar uw klanten. In dit scenario kunt u gebruikmaken van een Azure-Beschikbaarheidsset voor virtuele machines die worden uitgevoerd van de laag DBMS is niet nodig hebt. In dit scenario zijn u afhankelijk van twee verschillende functies:

- Azure virtuele machine automatisch opnieuw starten (ook waarnaar wordt verwezen als Azure-Service Healing)
- SAP HANA Auto-Restart

Azure virtuele machine automatisch opnieuw opstarten of 'service healing' is een functionaliteit in Azure die op twee niveaus werkt:

- Azure-serverhost de status van een virtuele machine die wordt gehost op de server controleren
- Azure Controller voor de bewaking van de status en beschikbaarheid van de server-host

Voor elke VM die wordt gehost op een Azure-serverhost, wordt de status van de gehoste VM('s) worden bewaakt door een selectievakje health functionaliteit. Als VMs niet in orde is valt, kan opnieuw opstarten van de virtuele machine worden geïnitieerd door de Azure-host-agent die de status van de virtuele machine controleert. De Azure-Infrastructuurcontroller is de status van de host controleren door het controleren van veel verschillende parameters die wijzen op problemen met de host, maar ook controleert op de toegankelijkheid van de host via het netwerk. Een indicatie van problemen met de host kan leiden tot bewerkingen zoals:

- Opnieuw opstarten van de host en opnieuw starten van de virtuele machines die werden uitgevoerd op de host als een slechte status geeft aan de host
- Opnieuw opstarten van de host en opnieuw opstarten van de VM('s) die oorspronkelijk als op de host op een host in orde host fungeerden als de host niet in orde na het opstarten is. In dit geval de host is gaat worden gemarkeerd als niet in orde en zijn niet gebruikt voor verdere implementaties totdat gewist of vervangen.
- Onmiddellijk opnieuw opstarten van de virtuele machines op een host in orde in gevallen waarin de beschadigde host problemen in het proces opnieuw opstarten heeft. 

Met de host en de VM bewaking door Azure Azure virtuele machines die te op de hostproblemen maken worden automatisch opnieuw opgestart in een gezonde Azure-host 

Het tweede onderdeel dat u in een dergelijk scenario afhankelijk van zijn is het feit dat uw HANA-service die wordt uitgevoerd in een opnieuw gestart VM automatisch wordt gestart na het opstarten van de virtuele machine. De [HANA Service automatisch opnieuw opstarten](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) kan worden geconfigureerd via de services watchdog van de verschillende HANA-services.

Dit scenario met één VM kan krijgen verbeterd door het toevoegen van een failoverknooppunt koude naar een SAP HANA-configuratie. Of als deze wordt genoemd in de documentatie voor SAP HANA als [Host een automatisch Failover](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Deze configuratie kan zinvol zijn voor on-premises implementatie situaties waarin de serverhardware is beperkt en u een knooppunt met één server als Host een automatisch Failover knooppunt voor een set productiehosts reserveren. Echter situaties zoals Azure waarbij de onderliggende infrastructuur van Azure een goed doel-server voor een geslaagde opnieuw starten van een virtuele machine biedt het SAP HANA Host automatisch een Failover-scenario biedt niet verstandig om te implementeren. 

Als gevolg hiervan hebben we geen referentiearchitectuur die voorziet in een stand-by-knooppunt voor HANA Host automatisch een Failover. Dit geldt ook voor SAP HANA scale-out configuraties.


## <a name="availability-scenarios-involving-two-different-vms"></a>Beschikbaarheid scenario's met betrekking tot twee verschillende virtuele machines
Met behulp van twee Azure VM's in Beschikbaarheidssets van Azure kunt u de omhoog-tijd tussen deze twee virtuele machines verhogen als de virtuele machines in een Azure-Beschikbaarheidsset binnen een Azure-regio worden geplaatst. De basisinstallatie in Azure eruit als de afbeeldingen die hier worden weergegeven: ![twee virtuele machines met alle lagen](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Ter verduidelijking van de beschikbaarheid van andere scenario's zijn slechts enkele van de bovenstaande lagen knippen en de afbeeldingen die zijn beperkt tot de lagen van virtuele machines, hosts, Beschikbaarheidssets en Azure-regio's. Azure VNets, resourcegroepen en abonnementen niet afgespeeld een rol voor de scenario's beschreven.

### <a name="replicating-backups-to-second-virtual-machine"></a>Back-ups worden gerepliceerd naar de tweede virtuele machine
Een van de meest rudimentaire instellingen is om back-ups, met name transactielogboekback-ups van een virtuele machine wordt verzonden naar een andere Azure virtuele machine. U hebt de keuze van elk type Azure Storage. U zou zijn verantwoordelijk voor het uitvoeren van scripts de kopie van de geplande back-ups die worden uitgevoerd op de eerste virtuele machine naar de tweede virtuele machine. In geval van een met behulp van de exemplaren van de tweede VM vereisen, moet u de volledige, incrementele/differentiële en transactielogboekback-ups naar de gewenste herstellen. De architectuur eruit zou: ![twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Deze instelling is niet te geschikt is voor het bereiken van goede RPO en RTO tijden. Met name RTO keren zouden afnemen als gevolg van de behoeften van een volledig herstel van de volledige database met de gekopieerde back-ups. Deze instelling is echter bruikbare van ongewenste gegevens verwijderen op de belangrijkste exemplaren wilt herstellen. met een dergelijke installatie zich u op elk gewenst moment herstellen naar een bepaald punt in tijd, de gegevens ophalen en importeren van de verwijderde gegevens in uw belangrijkste exemplaar. Daarom kan het zinvol dergelijke back-up om methode te gebruiken in combinatie met andere functionaliteit van hoge beschikbaarheid. Tijdens de periode wanneer alleen de back-ups worden gekopieerd, krijgt u mogelijk samen met een kleinere VM dan de belangrijkste SAP HANA-exemplaren in wordt uitgevoerd. Maar houd er rekening mee dat kleiner VMs onderlimiet van VHD's die kunnen worden bijgevoegd. Controleer [grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) voor limieten van afzonderlijke VM-typen.

### <a name="using-sap-hana-system-replication-without-automatic-failover"></a>Met SAP HANA System Replication zonder automatische failover
Voor de volgende scenario's gebruikt u replicatie voor SAP HANA-systeem. SAP uitgegeven documentatie vindt u beginnen met het artikel [System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Tussen twee virtuele machines in Azure één Azure-regio zijn er twee verschillende configuraties waarvoor een paar verschillen in de beoogde hersteltijd. In het algemeen zijn de scenario's met automatische failover niet hoeft mogelijk geen te relevant voor scenario's binnen een Azure-regio. Reden hiervoor is dat in de meeste situaties fout in de Azure-infrastructuur, het herstel van de Azure-Service gaat opnieuw opstarten van de primaire virtuele machine op een andere host. Er zijn alleen rand gevallen bekend waarbij een dergelijke configuratie in termen van scenario's fouten helpen kan. Of sommige gevallen die u als klant wilt realiseren, met name om de efficiëntie.

#### <a name="using-hana-system-replication-without-auto-failover-and-without-data-pre-load"></a>Met behulp van HANA System replicatie zonder automatische failover en zonder vooraf laden van gegevens 
Dit is een scenario waarbij u SAP HANA System Replication omwille van de verplaatsen van gegevens in een synchrone manier om een herstel Herstelpuntdoel (RPO) 0. Op de andere kant, hebt u een lang genoeg herstel tijd Objective (RTO), dus die u niet hoeft failover of vooraf laden van de gegevens in de cache HANA-exemplaar. In dat geval hebt u de mogelijkheden om verdere economie in uw configuratie door:

- U kunt een ander exemplaar van de SAP HANA uitvoeren in de tweede VM die de meeste van het geheugen van de virtuele machine duurt. Deze configuratie is meestal een exemplaar dat in het geval van een failover naar de tweede virtuele machine kan worden afgesloten. Ja, dat de gerepliceerde gegevens in de cache van het exemplaar van HANA in de tweede VM kan worden geladen.
- U kunt een kleinere VM als de tweede VM. In geval van een failover hebt u een stap voordat u de handmatige failover waarbij u zou de grootte van de virtuele machine naar de grootte van de bron-VM. Het scenario ziet eruit als:

![Twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Zelfs zonder vooraf laden van gegevens in de doel-HANA System Replication, moet u minimaal 64 GB geheugen en voorbij dat voldoende geheugen de rowstore om gegevens te behouden in het geheugen van de doelinstantie.

#### <a name="using-hana-system-replication-without-auto-failover-and-with-data-pre-load"></a>Met behulp van HANA System replicatie zonder automatische failover en met vooraf laden van gegevens
Het scenario geïntroduceerd voordat het verschil is dat de gegevens die worden gerepliceerd naar de HANA-exemplaar in de tweede VM vooraf is geladen. Hoeft de twee voordelen die u kunt hebben met het scenario niet vooraf laden van gegevens. U kunt een ander SAP HANA-systeem in dit geval niet uitvoeren op de tweede VM. Ook kunt u een kleinere VM. Dit is daarom een scenario nauwelijks geïmplementeerd met klanten


### <a name="using-sap-hana-system-replication-with-automatic-failover"></a>Met behulp van SAP HANA System replicatie met automatische failover

De beschikbaarheidsconfiguratie standaard binnen één Azure-regio, de meeste klanten met SAP HANA implementeert, is een configuratie waarbij de twee Azure virtuele machines met Linux SLES hebben een failover-cluster dat is gedefinieerd. Het cluster Linux SLES is gebaseerd op de [pacemaker heeft](http://www.linux-ha.org/wiki/Pacemaker) framework in combinatie met een [STONITH](http://linux-ha.org/wiki/STONITH) apparaat. Vanuit het perspectief van een SAP HANA wordt de replicatiemodus gebruikt, is gesynchroniseerd en een automatische failover is geconfigureerd. In de tweede VM fungeert het exemplaar SAP HANA als een hot standby-knooppunt, wat een synchrone stroom van wijzigingsrecords van het primaire SAP HANA-exemplaar ontvangt. Als transacties ophalen door de toepassing op het primaire knooppunt HANA vastgelegd, wacht de primaire HANA knooppunt om te bevestigen dat het doorvoeren naar de toepassing totdat het secundaire knooppunt voor SAP HANA bevestigd ontvangst van de commit-record. Modi voor SAP HANA twee verschillende synchrone replicatie. Lees het artikel voor meer informatie en verschillen op deze twee modi voor synchrone replicatie [replicatie modi voor SAP HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html)

De configuratie van de algehele ziet eruit als

![Twee virtuele machines met storage-replicatie en failover](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

Deze oplossing wordt gekozen omdat Hiermee kunt u een RPO bereiken = 0 en een zeer lage RTO tijden. Configureer de SAP HANA-clientconnectiviteit op een manier die de SAP HANA-clients het virtuele IP-adres gebruiken verbinding maken met de replicatieconfiguratie HANA System. Hierdoor is de toepassing bij een failover naar het secundaire knooppunt opnieuw moet. In deze oplossing moet de Azure VM-SKU's voor de primaire of secundaire site hetzelfde zijn.  


## <a name="next-steps"></a>Volgende stappen
Als u stapsgewijze instructies voor het instellen van een dergelijke configuratie in Azure nodig hebt, leest u de artikelen:

- [SAP HANA System replicatie in virtuele machines in Azure instellen](sap-hana-high-availability.md)
- [Uw SAP op Azure – deel 4: hoge beschikbaarheid voor SAP HANA met behulp van replicatie van systeem](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Als u meer informatie over de beschikbaarheid van SAP HANA over Azure-regio's moet, lezen:

- [Beschikbaarheid van de SAP HANA volledige Azure-regio 's](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

