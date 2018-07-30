---
title: SAP HANA-beschikbaarheid binnen één Azure-regio | Microsoft Docs
description: Beschrijft de bewerkingen die SAP HANA op Azure systeemeigen VM's in een Azure-regio.
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
ms.date: 07/27/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4f2defd60ec6b835ec856c9253a92f1d6817e861
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39326000"
---
# <a name="sap-hana-availability-within-one-azure-region"></a>SAP HANA-beschikbaarheid binnen één Azure-regio
Dit artikel beschrijft de verschillende scenario's voor beschikbaarheid binnen één Azure-regio. Azure heeft een groot aantal regio's, verspreid over de hele wereld. Zie voor een lijst van Azure-regio's, [Azure-regio's](https://azure.microsoft.com/regions/). Microsoft biedt voor het implementeren van SAP HANA op virtuele machines binnen een Azure-regio, implementatie van een enkele virtuele machine met een HANA-instantie. Voor grotere beschikbaarheid, kunt u twee VM's met twee HANA-instanties binnen implementeren een [Azure-beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) die gebruikmaakt van HANA-systeemreplicatie voor beschikbaarheid. 

Azure biedt op dit moment [Azure Availability Zones](https://docs.microsoft.com/azure/availability-zones/az-overview). In dit artikel wordt niet voor Beschikbaarheidszones in detail beschreven. Maar dit bevat een algemene discussie over het gebruik van Beschikbaarheidssets ten opzichte van Beschikbaarheidszones.

Azure-regio's waar Beschikbaarheidszones worden aangeboden hebben meerdere datacenters. De datacenters zijn onafhankelijk van elkaar in de levering van voeding, koeling en netwerk. De reden voor het bieden van verschillende zones binnen één Azure-regio is om toepassingen te implementeren in twee of drie Beschikbaarheidszones die worden aangeboden. Problemen met de implementeren in verschillende zones in Power BI en netwerken die betrekking hebben op slechts één Beschikbaarheidszone voor Azure-infrastructuur, de implementatie van uw toepassing binnen een Azure-regio nog steeds functioneel is. Sommige minder capaciteit optreden. Bijvoorbeeld, virtuele machines in één zone verloren zijn gegaan, maar virtuele machines in de andere twee zones is nog steeds actief en werkend. 
 
Een Azure-Beschikbaarheidsset is een logische groeperingsmogelijkheid die ervoor te zorgen helpt dat de VM-resources die u binnen de Beschikbaarheidsset plaatsen van elkaar geïsoleerd fout wanneer ze worden geïmplementeerd in een Azure-datacenter. Azure zorgt ervoor dat de VM's die u in een beschikbaarheidsset plaatst, op meerdere fysieke servers, rekenrekken, opslageenheden en netwerkswitches worden uitgevoerd. In sommige Azure-documentatie, deze configuratie wordt aangeduid als plaatsingen in verschillende [update-en foutdomeinen](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Deze plaatsingen zijn meestal binnen een Azure-datacenter. Ervan uitgaande dat power voedingsbron en problemen beïnvloeding van het datacenter waarin u wilt implementeren, zou de capaciteit in een Azure-regio worden beïnvloed.

De plaatsing van datacenters die staan voor Azure-Beschikbaarheidszones is een compromis tussen het leveren van acceptabele netwerklatentie tussen de services die zijn geïmplementeerd in verschillende zones en een afstand tussen datacenters. Natuurlijke catastrophes in het ideale geval wouldn't invloed op de voeding, netwerk-aanbod en infrastructuur voor alle Beschikbaarheidszones in deze regio. Echter, zoals gigantische natuurlijke catastrophes hebben, Beschikbaarheidszones bieden mogelijk niet altijd de beschikbaarheid die u wilt dat binnen één regio. Denk na over orkaan Maria die het eiland Porto Rico op 20 September 2017 bereikt. De orkaan veroorzaakt in feite een onleesbaar bijna 100 procent op het eiland 90-mile-niveau.

## <a name="single-vm-scenario"></a>Eén VM-scenario

In een scenario voor één VM maakt u een Azure-VM voor de SAP HANA-instantie. U kunt Azure Premium Storage gebruiken voor het hosten van de schijf van het besturingssysteem en alle gegevensschijven van uw. De Azure uptime SLA van 99,9 procent en de SLA's van andere Azure-onderdelen is voldoende voor u om te voldoen aan de beschikbaarheid van uw Sla's voor uw klanten. In dit scenario hebt u niet nodig gebruikmaken van een Azure-Beschikbaarheidsset voor virtuele machines waarop de DBMS-laag wordt uitgevoerd. In dit scenario vertrouwen kunt u op twee verschillende functies:

- Azure virtuele machine automatisch opnieuw opstarten (ook wel Azure service healing genoemd)
- SAP HANA automatisch opnieuw opstarten

Azure virtuele machine automatisch opnieuw opstarten of serviceherstel, is een functie in Azure die geschikt is voor twee niveaus:

- De Azure-serverhost controleert de status van een virtuele machine die wordt gehost op de server-host.
- De Azure-infrastructuurcontroller controleert de status en beschikbaarheid van de server-host.

Een selectievakje health functionaliteit controleert de status van elke virtuele machine die wordt gehost op een Azure-server-host. Als een virtuele machine in een status niet in orde zijn valt, kan een opnieuw opstarten van de virtuele machine worden gestart door de Azure-host-agent waarmee wordt gecontroleerd of de status van de virtuele machine. De infrastructuurcontroller controleert de status van de host door het controleren van veel verschillende parameters die op problemen met de hosthardware wijzen kunnen. Er wordt ook gecontroleerd op de toegankelijkheid van de host via het netwerk. Een indicatie van problemen met de host kan leiden tot de volgende gebeurtenissen:

- Als een slechte status geeft aan de host, wordt opnieuw opstarten van de host en het opnieuw opstarten van de virtuele machines die zijn uitgevoerd op de host geactiveerd.
- Als de host zich niet in orde nadat de computer opnieuw is opgestart, wordt een nieuwe installatie van de virtuele machines die oorspronkelijk op de nu beschadigd knooppunt aan een goed werkende host-server zijn wordt gestart. In dit geval is de oorspronkelijke host gemarkeerd als niet in orde. Optie wordt niet gebruikt voor verdere implementaties totdat deze is uitgeschakeld of vervangen.
- Als de host niet in orde problemen tijdens het opnieuw opstarten heeft, wordt een direct opnieuw opstarten van de virtuele machines op een goed werkende host geactiveerd. 

Met de host en de VM-bewaking verstrekt door Azure worden Azure-VM's die problemen met de host ondervinden automatisch opnieuw opgestart in een gezonde Azure-host. 

>[!IMPORTANT]
>Herstel van Azure-service wordt niet opnieuw opstarten virtuele Linux-machines waar het gastbesturingssysteem te installeren in een paniek kernel-status is. De standaardinstellingen van de meest gebruikte Linux-versies worden niet automatisch opnieuw starten virtuele machines of de server waar de kernel Linux paniek status heeft. De standaard voorziet in plaats daarvan om te voorkomen dat het besturingssysteem kernel paniek status is voor het koppelen van een kernelfoutopsporingsprogramma te analyseren. Azure is dit gedrag door een virtuele machine met het gastbesturingssysteem niet automatisch opnieuw te starten in een dergelijke een staat naleven. Verondersteld wordt dat deze instanties worden zelden gebruikt. U kunt het standaardgedrag om in te schakelen van een herstart van de virtuele machine kan overschrijven. Gedrag inschakelen om de standaardinstelling te wijzigen met de parameter 'kernel.panic' in /etc/sysctl.conf. De tijd die u voor deze parameter instelt is in seconden. Algemene aanbevolen waarden zijn na afloop van 20-30 seconden voordat het opnieuw opstarten via deze parameter wordt geactiveerd. Zie ook <https://gitlab.com/procps-ng/procps/blob/master/sysctl.conf>.

De tweede functie die u op in dit scenario vertrouwt is het feit dat de HANA-service die wordt uitgevoerd in een opnieuw gestart virtuele machine automatisch wordt gestart nadat de virtuele machine opnieuw wordt opgestart. U kunt instellen [HANA-service automatisch opnieuw opstarten](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) via de watchdog-services van de verschillende HANA-services.

U kunt dit scenario voor één VM verbeteren door een koude failoverknooppunt toe te voegen aan de configuratie van een SAP HANA. In de documentatie van SAP HANA, deze instelling wordt aangeroepen [host automatische failover](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Deze configuratie kan zinvol in een on-premises implementatie situatie waarin de serverhardware is beperkt, en u speciaal aan een knooppunt één server als de host automatische failover-knooppunt van een set van productiehosts. Maar in Azure, de onderliggende infrastructuur van Azure waar een goed doel-server voor een geslaagde VM opnieuw op te starten biedt, het geen zin voor het implementeren van SAP HANA-host automatische failover. Vanwege het Azure-serviceherstel, is er geen referentiearchitectuur die voorziet in een stand-by-knooppunt voor HANA-host automatische failover. 

### <a name="special-case-of-sap-hana-scale-out-configurations-in-azure"></a>Speciaal geval van scale-out-configuraties van SAP HANA in Azure
Hoge beschikbaarheid voor scale-out-configuraties van SAP HANA is afhankelijk van service herstel van virtuele Azure-machines en het opnieuw opstarten van de SAP HANA-exemplaar als de virtuele machine actief is en nogmaals uit te voeren. Hoge beschikbaarheid-architectuur op basis van HANA System Replication wilt op een later tijdstip worden geïntroduceerd. 


## <a name="availability-scenarios-for-two-different-vms"></a>Beschikbaarheid van scenario's voor twee verschillende virtuele machines

Als u twee Azure-VM's binnen een Azure-Beschikbaarheidsset, kunt u de actieve tijdsduur tussen deze twee virtuele machines als ze zijn geplaatst in een Azure-Beschikbaarheidsset binnen één Azure-regio kunt verhogen. De basisconfiguratie in Azure zou er als volgt uitzien:

![Diagram van twee VM's met alle lagen](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Ter illustratie van de beschikbaarheid van verschillende scenario's, worden een aantal van de lagen in het diagram weggelaten. Het diagram toont alleen de lagen die virtuele machines, hosts, Beschikbaarheidssets, en Azure-regio's. Azure Virtual Network-exemplaren, resourcegroepen en abonnementen spelen niet een rol in de scenario's in deze sectie beschreven.

### <a name="replicate-backups-to-a-second-virtual-machine"></a>Back-ups repliceren naar een tweede virtuele machine

Een van de meest elementaire instellingen is het gebruik van back-ups. U hebt in het bijzonder transactielogboekback-ups van een virtuele machine naar een andere virtuele machine van Azure verzonden. U kunt het type Azure Storage. Met deze instellingen bent u verantwoordelijk voor het uitvoeren van scripts de kopie van de geplande back-ups die op de eerste virtuele machine met de tweede virtuele machine worden uitgevoerd. Als u gebruiken van de tweede VM-exemplaren wilt, moet u de volledige, incrementele/differentiële en transactielogboekback-ups herstellen tot het punt die u nodig hebt. 

De architectuur lijkt:

![Diagram van twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Deze instelling is niet geschikt voor het bereiken van goede Recovery Point Objective (RPO) en Recovery Time Objective (RTO) tijden. RTO tijden met name zouden Hierdoor kan afnemen vanwege moet de volledige database volledig te herstellen met behulp van de gekopieerde back-ups. Deze instelling is echter handig voor het herstellen van ongewenste gegevens verwijderen van de belangrijkste exemplaren. Met deze instelling op elk gewenst moment kunt u herstellen naar een bepaald punt in tijd, Extraheer de gegevens en importeer de verwijderde gegevens in uw belangrijkste exemplaar. Daarom kan het zinvol een back-up-methode gebruiken in combinatie met andere functionaliteit voor hoge beschikbaarheid. 

Tijdens het back-ups worden gekopieerd, kunt u mogelijk gebruik van een kleinere virtuele machine dan de belangrijkste virtuele machine die op de SAP HANA-instantie wordt uitgevoerd. Houd er rekening mee dat u een kleiner aantal VHD's aan kleinere VM's koppelen kunt. Zie voor meer informatie over de beperkingen van de afzonderlijke VM-typen [grootten voor virtuele Linux-machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

### <a name="sap-hana-system-replication-without-automatic-failover"></a>SAP HANA-systeemreplicatie zonder automatische failover

De scenario's beschreven in deze sectie gebruikt u SAP HANA-systeemreplicatie. Zie voor de SAP-documentatie, [System replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Scenario's zonder automatische failover gelden niet voor configuraties binnen een Azure-regio. Een configuratie zonder automatische failover, verplicht Hoewel een instelling Pacemaker vermijden u om te controleren en failover handmatig. Sinds deze neemt en inspanningen ook worden de meeste klanten vertrouwen op Azure-service in plaats daarvan Retoucheerpenseel. Er zijn enkele randgevallen waar deze configuratie in termen van foutscenario's helpen kan. Of, in sommige gevallen kan een klant kunt om te profiteren van meer efficiëntie.

#### <a name="sap-hana-system-replication-without-auto-failover-and-without-data-preload"></a>SAP HANA-systeemreplicatie zonder automatische failover en vooraf laden van gegevens

In dit scenario kunt u SAP HANA-systeemreplicatie gebruiken om gegevens te verplaatsen in een synchrone manier om een RPO van 0. Aan de andere kant, hebt u een lang genoeg RTO die u niet hoeft failover of gegevens vooraf laden in de cache van HANA-exemplaar. In dit geval is het mogelijk om te realiseren verder economie in uw configuratie door de volgende acties te ondernemen:

- Een ander exemplaar van SAP HANA in de tweede virtuele machine uitgevoerd. De SAP HANA-instantie in de tweede virtuele machine heeft de meeste van het geheugen van de virtuele machine. Als een failover naar de tweede virtuele machine, u het actieve exemplaar van SAP HANA met de gegevens volledig is geladen in de tweede virtuele machine moet, zodat de gerepliceerde gegevens kunnen worden geladen in de cache van de betreffende HANA-instantie in de tweede virtuele machine afgesloten.
- Gebruik een kleinere virtuele machine op de tweede virtuele machine. Als u een failover is uitgevoerd, hebt u een extra stap voordat u de handmatige failover. In deze stap maakt u het formaat van de virtuele machine naar de grootte van de bron-VM. 
 
Het scenario ziet eruit zoals:

![Diagram van twee virtuele machines met storage-replicatie](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Zelfs als u niet vooraf laden van gegevens in de doel-HANA system replication gebruikt, moet u minimaal 64 GB aan geheugen. U moet ook voldoende geheugen naast 64 GB voor de rowstore-gegevens in het geheugen van het doelexemplaar.

#### <a name="sap-hana-system-replication-without-auto-failover-and-with-data-preload"></a>SAP HANA-systeemreplicatie zonder automatische failover en met vooraf laden van gegevens

In dit scenario is de gegevens die worden gerepliceerd naar de HANA-instantie in de tweede virtuele machine vooraf geladen. Dit elimineert de twee voordelen van het niet vooraf laden van gegevens. U kunt een andere SAP HANA-systeem in dit geval niet uitvoeren op de tweede virtuele machine. U niet ook een kleinere virtuele machine gebruiken. Daarom implementeren klanten zelden van dit scenario.

### <a name="sap-hana-system-replication-with-automatic-failover"></a>SAP HANA-systeemreplicatie met automatische failover

In de standaard en meest voorkomende hebben configuratie van de beschikbaarheid binnen één Azure-regio, twee virtuele Azure-machines met Linux SLES een failover-cluster dat is gedefinieerd. Het Linux SLES-cluster is gebaseerd op de [Pacemaker](http://www.linux-ha.org/wiki/Pacemaker) framework, in combinatie met een [stonith instellen](http://linux-ha.org/wiki/STONITH) apparaat. 

De replicatiemodus die wordt gebruikt is gesynchroniseerd vanuit het perspectief van een SAP HANA, en een automatische failover is geconfigureerd. In de tweede virtuele machine fungeert het SAP HANA-exemplaar als een warme stand-by-knooppunt. Een synchrone stream met wijzigingsrecords ontvangt de stand-by-knooppunt van de primaire SAP HANA-instantie. Als transacties worden vastgelegd door de toepassing op het primaire knooppunt HANA, wacht de primaire HANA-knooppunt om te bevestigen van het doorvoeren naar de toepassing tot het secundaire knooppunt van de SAP HANA wordt bevestigd dat de commit-record wordt ontvangen. SAP HANA biedt twee modi voor synchrone replicatie. Voor meer informatie en voor een beschrijving van de verschillen tussen deze twee modi voor synchrone replicatie, Zie het artikel SAP [replicatiemodi voor SAP HANA-systeemreplicatie](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html).

De configuratie van de algehele ziet eruit zoals:

![Diagram van twee virtuele machines met storage-replicatie en failover](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

U kunt ervoor kiezen deze oplossing omdat Hiermee kunt u een een RPO = 0 en een lage RTO. De SAP HANA-clientconnectiviteit zodanig configureren dat de SAP HANA-clients het virtuele IP-adres gebruiken verbinding maken met de configuratie van HANA system replication. Een dergelijke configuratie hoeft u de toepassing opnieuw te configureren als er een failover naar het secundaire knooppunt optreedt. In dit scenario moet de Azure VM-SKU's voor de primaire en secundaire VM's zijn hetzelfde.

## <a name="next-steps"></a>Volgende stappen

Zie voor stapsgewijze instructies over het instellen van deze configuraties in Azure:

- [SAP HANA-systeemreplicatie in Azure VM's instellen](sap-hana-high-availability.md)
- [Hoge beschikbaarheid voor SAP HANA met behulp van systeemreplicatie](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Zie voor meer informatie over de beschikbaarheid van SAP HANA op Azure-regio's:

- [Beschikbaarheid van SAP HANA op Azure-regio 's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

