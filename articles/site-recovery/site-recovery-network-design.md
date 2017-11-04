---
title: Ontwerp van uw netwerkinfrastructuur voor herstel na noodgevallen met Azure Site Recovery | Microsoft Docs
description: Dit artikel worden de overwegingen bij het ontwerpen van netwerk voor Azure Site Recovery
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: pratshar
ms.openlocfilehash: 5b6fb7bac852b29663866e99758241bd5a7ab59e
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Ontwerpen van uw netwerk voor herstel na noodgevallen

Aandachtspunten voor netwerken van dit artikel wordt beschreven wanneer u [Azure Site Recovery](site-recovery-overview.md) voor disaster recovery on-premises naar Azure of naar een secundaire on-premises site. Dit artikel gaat over het definiëren van IP-adresbereiken en subnetten na een failover naar een secundaire locatie.

## <a name="overview"></a>Overzicht

Site Recovery is een Microsoft Azure-service die de beveiliging en herstel van gevirtualiseerde toepassingen omwille van herstel na noodgevallen (BCDR) voor continuïteit business ingedeeld.

In een wereld van 24/7 connectiviteit is het belangrijk om te blijven van uw infrastructuur van bedrijven en toepassingen van en worden uitgevoerd. Het doel van de BCDR is herstellen niet functionerende onderdelen, zodat uw organisatie kunt normale bewerkingen snel hervatten. Ontwikkelen disaster recovery strategieën om te gaan met onwaarschijnlijk gebeurtenissen is erg lastig. Dit komt door de intrinsieke moeite van het voorspellen van de toekomst, met name voor onwaarschijnlijke gebeurtenissen. En vanwege de hoge kosten van het onderhouden van de passende maatregelen ter bescherming tegen groot catastrophes.

Essentieel voor het plannen van BCDR, een herstel tijd Objective (RTO) en herstel Herstelpuntdoel (RPO) moeten worden gedefinieerd in uw BCDR-plan. Wanneer een noodgeval een datacenter biedt, kunt u snel (laag RTO) online brengen van de gerepliceerde virtuele machines zich in de secundaire Datacenter of de Microsoft Azure met minimaal gegevensverlies (lage RPO).

Failover wordt mogelijk gemaakt door Site Recovery, die in eerste instantie kopieert aangewezen virtuele machines van het primaire Datacenter naar het secundaire Datacenter of naar Azure (afhankelijk van het scenario) en de replica's worden vervolgens regelmatig vernieuwd. Bij het plannen van de infrastructuur moet netwerkontwerp worden beschouwd als mogelijke knelpunt kan worden voorkomen dat u van vergadering bedrijf RTO en RPO doelstellingen.  

Wanneer beheerders van plan bent voor het implementeren van een noodherstel, is een van de belangrijkste vragen in een hoe de virtuele machine zou bereikbaar zijn nadat de failover is voltooid. Site Recovery kan de beheerder om te kiezen van het netwerk waarmee een virtuele machine zou worden verbonden na failover. Als de primaire site Azure is of een lokale site die wordt beheerd door een VMM-server is, wordt klikt u vervolgens dit bereikt door middel van netwerktoewijzing. Meer informatie over [netwerktoewijzing in Azure naar Azure DR](site-recovery-network-mapping-azure-to-azure.md) en [netwerktoewijzing uit VMM](site-recovery-network-mapping.md)


Tijdens het ontwerpen van het netwerk voor de site recovery heeft de beheerder twee opties:

* Gebruik een ander IP-adresbereik voor het netwerk op de site recovery. In dit scenario, krijgt de virtuele machine na een failover een nieuw IP-adres en de beheerder moet een DNS-update uitvoeren. 
* Gebruik hetzelfde IP-adresbereik voor het netwerk op de site recovery. Beheerders liever in bepaalde scenario's voor het bewaren van de IP-adressen die ze op de primaire site ook na de failover hebben. Een beheerder moet bijwerken van de routes om aan te geven van de nieuwe locatie van de IP-adressen in een normale scenario. Maar in het scenario waarbij een gespreide subnet tussen de primaire en de herstellocatie is geïmplementeerd, wordt het behouden van de IP-adressen voor de virtuele machines een aantrekkelijke optie. Uitrekken van een subnet van een on-premises netwerk met een Azure-netwerk of tussen twee netwerken in Azure is niet mogelijk.  

Wanneer beheerders van plan bent voor het implementeren van een noodherstel, is een van de belangrijke vragen hun er rekening mee hoe de toepassingen kunnen worden bereikt, worden nadat de failover is voltooid. Moderne toepassingen zijn bijna altijd afhankelijk van de toegang tot op zekere hoogte dus fysiek worden verplaatst dat een service van de ene site naar een andere netwerken moeilijk vertegenwoordigt. Er zijn twee manieren die dit probleem wordt behandeld in oplossingen voor herstel na noodgevallen. De eerste benadering bestaat vaste IP-adressen behouden. Ondanks de services verplaatsen en de hosting-servers in verschillende fysieke locaties, nemen toepassingen de IP-adresconfiguratie ze naar de nieuwe locatie. De tweede oplossing omvat het wijzigen van het IP-adres volledig tijdens de overgang naar de herstelde site. Elke methode biedt verschillende implementatie afwijkingen die hieronder worden samengevat.

Tijdens het ontwerpen van het netwerk voor de site recovery heeft de beheerder twee opties:

## <a name="option-1-retain-ip-addresses"></a>Optie 1: IP-adressen behouden
Disaster recovery proces vanuit het oogpunt van, met vaste IP-adressen lijkt te zijn van de eenvoudigste methode om te implementeren, maar heeft een aantal mogelijke uitdagingen, waardoor dit in de praktijk het minst populaire benadering. Azure Site Recovery biedt de mogelijkheid voor het bewaren van de IP-adressen in alle scenario's. Voordat een wil IP behouden, moet de juiste manier worden besteed aan de beperkingen op de mogelijkheden voor failover legt. Kijken we de factoren die u helpen kunnen bij het maken van een beslissing voor het bewaren van IP-adressen, of niet. Dit kan op twee manieren worden gerealiseerd, met behulp van een gespreide subnet of u een volledige subnet failover uitvoert.

### <a name="stretched-subnet"></a>Gespreide subnet
Hier wordt het subnet beschikbaar gelijktijdig in zowel primaire als de DR locaties gemaakt. Eenvoudige voorwaarden betekent dit dat u kunt een server en de configuratie van de IP-(laag 3) verplaatsen naar de tweede site en het netwerk stuurt het verkeer naar de nieuwe locatie automatisch. Dit is heel eenvoudig om te gaan met vanuit het perspectief van een server, maar heeft een aantal uitdagingen:

* Vanuit het oogpunt Layer 2 (data link-laag) van netwerkapparatuur die een gespreide VLAN kunt beheren is vereist, maar dit minder, van een probleem als het is nu algemeen beschikbaar is geworden. Het tweede en moeilijker probleem is dat door de onderdelen van het VLAN het potentiële foutdomein wordt uitgebreid naar beide sites, wordt in wezen een potentieel risico. Dit is waarschijnlijk niet voorkomt, is het een broadcast-storm gestart, maar kan niet worden geïsoleerd opgetreden. We hebben gemengde mening over dit laatste probleem gevonden en hebben zichtbaar zijn veel geslaagde implementaties evenals 'we won't deze technologie hier implementeren'.
* Gespreide subnet is niet mogelijk als u Microsoft Azure als de DR-site.

### <a name="subnet-failover"></a>Subnet failover
Het is mogelijk voor het implementeren van failover subnet om te profiteren van de voordelen van de gespreide subnet oplossing die hierboven worden beschreven, zonder het uitrekken van het subnet op meerdere sites. Hier een bepaald subnet moeten aanwezig zijn op de Site 1 of 2, maar niet op beide sites tegelijkertijd. Om toegang te houden van de IP-adresruimte in het geval van een failover, is het mogelijk via een programma voor de infrastructuur van de router voor het verplaatsen van de subnetten van de ene site naar een andere rangschikken. In een failover-scenario die de subnetten zou worden verplaatst met de bijbehorende beveiligde virtuele machines. Het grootste nadeel voor deze benadering is bij een storing die u moet het hele subnet verplaatsen. Dit wordt mogelijk OK, maar dit kan invloed hebben op de granulatie van de failover.

We onderzoeken hoe een fictieve onderneming met de naam Contoso, kan de virtuele machines repliceren naar een herstellocatie tijdens mislukken gedurende het gehele subnet. Laten we eerst kijken hoe Contoso kan voor het beheren van hun subnetten tijdens het repliceren van virtuele machines tussen twee on-premises locaties, en vervolgens gaan we hoe subnet failover werkt wanneer [Azure wordt gebruikt als het herstel na noodgevallen site](#failover-to-azure).

#### <a name="fail-over-from-on-premises-to-azure"></a>Failover van on-premises naar Azure 
Azure Site Recovery kunt Azure moet worden gebruikt als een site van het herstel na noodgevallen voor uw virtuele machines.  

We onderzoeken een scenario waarbij een fictief bedrijf met de naam Woodgrove Bank on-premises infrastructuur die als host fungeert voor hun line-of-business-toepassingen en ze hun mobiele toepassingen in Azure worden gehost. Connectiviteit tussen de Woodgrove Bank virtuele machines in Azure en on-premises servers wordt verstrekt door een site-naar-site (S2S) virtuele particuliere netwerk (VPN) of ExpressRoute. Site-naar-site-VPN kunt van Woodgrove Bank virtueel netwerk in Azure worden gezien als een uitbreiding van de Woodgrove Bank on-premises netwerk. Deze communicatie wordt ingeschakeld door de site-naar-site VPN tussen Woodgrove Bank rand en virtuele Azure-netwerk. Woodgrove wil nu Site Recovery gebruiken om de werkbelastingen met primaire Azure-regio naar een andere Azure-regio worden gerepliceerd. Deze optie voldoet aan de behoeften van Woodgrove, dat wil een voordelige DR-optie en kan voor het opslaan van gegevens in openbare cloudomgevingen. Woodgrove heeft om te gaan met toepassingen en configuratie, die afhankelijk van vastgelegde IP-adressen zijn, en dus een vereiste voor het bewaren van IP-adressen voor hun toepassingen na worden niet via een andere regio in Azure.

Woodgrove heeft besloten IP-adressen toewijzen van IP-adresbereik (172.16.1.0/24, 172.16.2.0/24) tot de bronnen in Azure wordt uitgevoerd.

Voor de Woodgrove kunnen virtuele machines repliceren naar Azure terwijl de IP-adressen behouden, moet een Azure-netwerk worden gemaakt. Een uitbreiding van de on-premises netwerk moet zijn zodat applications failover uitvoeren van de lokale site naar Azure naadloos. Azure kunt u site-naar-site, evenals een punt-naar-site VPN-verbinding toevoegen aan de virtuele netwerken in Azure gemaakt. Wanneer u uw site-naar-site-verbinding instelt, wordt Azure-netwerk kunt u voor het routeren van verkeer naar de on-premises locatie (Azure-aanroepen het lokale netwerk) alleen als het IP-adresbereik anders dan het lokale IP-adresbereik, is omdat Azure biedt geen ondersteuning voor uitrekken subnetten.  Dit betekent dat als er een subnet 192.168.1.0/24 lokale, u kunt geen een lokaal netwerk 192.168.1.0/24 toevoegen in het Azure-netwerk. Dit is normaal omdat Azure niet weet dat er geen actieve VM's in het subnet zijn en het subnet alleen ter informatie DR wordt gemaakt. Om te kunnen routeren correct netwerkverkeer buiten een Azure-netwerk die niet in strijd zijn de subnetten in het netwerk en het LAN-netwerk.

![Vóór de Subnet-Failover](./media/site-recovery-network-design/network-design7.png)

Vóór de failover

Om te voldoen aan de bedrijfsvereisten van hun Woodgrove, moeten we de volgende werkstromen implementeren:

* Een extra netwerk maakt, roepen laat het ons herstel netwerk, waarbij de failover virtuele machines moet worden gemaakt.
* Het hetzelfde IP-adres dat de virtuele machine een on-premises om ervoor te zorgen dat het IP-adres voor een virtuele machine na een failover wordt bewaard, gaat u naar het tabblad configureren onder VM-eigenschappen opgeven en klik op opslaan. Wanneer de failover van de virtuele machine wordt uitgevoerd, wordt het opgegeven IP-adres in Azure Site Recovery toewijzen aan de virtuele machine.

![Eigenschappen van het netwerk](./media/site-recovery-network-design/network-design8.png)

Zodra de failover wordt geactiveerd en de virtuele machines worden gemaakt in het netwerk herstel met het gewenste IP-adres, verbinding met dit netwerk kan worden gemaakt met behulp van een [Vnet-naar-Vnet-verbinding](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Indien nodig met deze actie kan scripts worden gebruikt.  Zoals in de vorige sectie over subnet failover, zelfs wanneer een failover naar Azure besproken, zou routes moeten op de juiste wijze worden gewijzigd naar aanleiding van dat die 192.168.1.0/24 is nu verplaatst naar Azure.

![Na een Failover van Subnet](./media/site-recovery-network-design/network-design9.png)

Na een failover

Als u een Azure Network geen zoals weergegeven in de bovenstaande afbeelding. U kunt een site-naar-site VPN-verbinding tussen uw 'Primaire Site' en de herstel-netwerk maken na de failover.  


#### <a name="fail-over-to-a-secondary-on-premises-site"></a>Failover naar een secundaire on-premises site
Laat het ons kijken een scenario waar we willen het IP-adres van elk van de virtuele machines behouden en failover van de volledige subnet samen. De primaire site zijn toepassingen in een subnet 192.168.1.0/24. Als de failover gebeurt, worden alle virtuele machines die deel van dit subnet uitmaken failover mogelijk is naar de site recovery en hun IP-adressen behouden. Routes moeten op de juiste wijze worden gewijzigd naar aanleiding van het feit dat alle virtuele machines die horen bij subnet 192.168.1.0/24 nu naar de site recovery verplaatst zijn.

De routes tussen primaire site en de site recovery, derde en primaire site en derde site en herstelsite moeten op de juiste wijze worden gewijzigd in de volgende afbeelding.

De volgende afbeeldingen tonen de subnetten voordat de failover. Subnet 192.168.0.1/24 actief is op de primaire Site voordat de failover wordt uitgevoerd en wordt na de failover actief van de Site Recovery

![Vóór de Failover](./media/site-recovery-network-design/network-design2.png)

Vóór de failover

De volgende afbeelding ziet netwerken en subnetten na een failover.

![Na een Failover](./media/site-recovery-network-design/network-design3.png)

Na een failover

Als uw secundaire site is op lokale en u een VMM-server gebruikt voor het beheren, klikt u vervolgens bij het inschakelen van beveiliging voor een specifieke virtuele machine, wordt de Site Recovery netwerkresources volgens de volgende werkstroom toewijzen:

* Site Recovery wijst een IP-adres voor elke netwerkinterface op de virtuele machine van de statische IP-adresgroep die is gedefinieerd in het relevante netwerk voor elk exemplaar van System Center VMM.
* Als de beheerder de dezelfde IP-adresgroep voor het netwerk op de herstelsite als die van de IP-adresgroep van het netwerk op de primaire site definieert bij het toewijzen van het IP-adres van de replica virtuele machine, zou Site Recovery hetzelfde IP-adres als toewijzen die van de primaire virtuele machine.  Het IP-adres is gereserveerd in VMM, maar niet ingesteld als het failover-IP-adres op de Hyper-v-host. Het failover-IP-adres op een Hyper-v-host is net voordat de failover ingesteld.


Als hetzelfde IP-adres niet beschikbaar is, wijst Site Recovery sommige andere beschikbaar IP-adres van de gedefinieerde IP-adresgroep.

Nadat de virtuele machine is ingeschakeld voor beveiliging kunt u volgende voorbeeldscript gebruiken om te controleren of het IP-adres dat is toegewezen aan de virtuele machine. Dezelfde IP zou worden ingesteld als Failover IP en toegewezen aan de virtuele machine op het moment van failover:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> In het scenario waar de virtuele machines DHCP gebruikt, is het beheer van IP-adressen volledig buiten de controle van Site Recovery. Een beheerder heeft om ervoor te zorgen dat de DHCP-server voor de IP-adressen op de site recovery uit hetzelfde bereik als die van de primaire site fungeren kan.
>
>



## <a name="option-2-changing-ip-addresses"></a>Optie 2: IP-adressen wijzigen
Deze aanpak lijkt te zijn de meest voorkomende op basis van wat wij hebben. Het duurt het formulier van het wijzigen van het IP-adres van elke VM die bij de failover betrokken is. Een nadeel van deze benadering is het binnenkomende netwerk naar 'informatie' de toepassing op IPx is is nu op IPy vereist. Zelfs als IPx en IPy logische namen, DNS-vermeldingen hebben doorgaans worden gewijzigd of verwijderd in het netwerk en vermeldingen in de netwerk-tabellen in cache moeten worden bijgewerkt of leeggemaakte, dus een uitvaltijd kan worden gezien, al naar gelang hoe de DNS-infrastructuur is instellen. Deze problemen kunnen worden verminderd door lage TTL-waarden in het geval van intranettoepassingen met en [Azure Traffic Manager met Site Recovery](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/), voor internet-toepassingen

### <a name="changing-the-ip-addresses---illustration"></a>Wijzigen van de IP-adressen - afbeelding
Laat het ons kijken naar het scenario waar u van plan bent te gebruiken verschillende IP-adressen voor de primaire en de herstellocatie. In het volgende voorbeeld wordt ook hebt een externe site van waar de toepassingen worden gehost op een primaire of herstel de site kan worden benaderd.

![Verschillende IP - vóór de Failover](./media/site-recovery-network-design/network-design10.png)


In de bovenstaande afbeelding zijn sommige toepassingen die worden gehost in een subnet 192.168.1.0/24 subnet op de primaire site en ze zijn geconfigureerd voor het actief op de site recovery in subnet 172.16.1.0/24 na een failover. VPN-verbindingen/netwerkroutes zijn op de juiste wijze geconfigureerd zodat alle drie sites toegang elkaar tot hebben.

Als de afbeelding hieronder bevat nadat ze zijn mislukt op een of meer toepassingen, wordt deze hersteld in het subnet van het herstel. Er zijn in dit geval niet beperkt tot het gehele subnet failover op hetzelfde moment. Er zijn geen wijzigingen zijn vereist voor het configureren van VPN- of netwerkbeheerder routes. Een failover en een aantal DNS-updates zorgt ervoor dat toepassingen toegankelijk blijven. Als de DNS-server is geconfigureerd voor dynamische updates toestaan wilt vervolgens de virtuele machines registreren met behulp van het nieuwe IP-adres zodra ze na een failover starten.

![Verschillende IP - na een Failover](./media/site-recovery-network-design/network-design11.png)


De replica virtuele machine mogelijk na het mislukken via een IP-adres dat is niet hetzelfde als het IP-adres van de primaire virtuele machine. Virtuele machines werkt u de DNS-server die ze gebruiken nadat ze zijn gestart. DNS-vermeldingen hebben doorgaans worden gewijzigd of verwijderd in het netwerk en vermeldingen in de netwerk-tabellen in cache moeten worden bijgewerkt of leeggemaakt, dus niet vaak worden geconfronteerd met uitvaltijd terwijl deze statuswijzigingen plaatsvinden. Dit probleem kan worden verholpen door:

* Met behulp van laag TTL-waarden voor intranettoepassingen.
* Met behulp van Azure Traffic Manager met Site Recovery voor internet-toepassingen.
* Met het volgende script in uw plan voor herstel bij te werken van de DNS-Server om te controleren of een tijdige update (het script is niet vereist als de dynamische DNS-registratie is geconfigureerd)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-the-ip-addresses--disaster-recovery-to-azure"></a>Wijzigen van de IP-adressen: herstel na noodgevallen naar Azure
De [Networking infrastructuur Setup voor Microsoft Azure als een herstel na noodgevallen Site](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) blogbericht wordt uitgelegd hoe u de vereiste Azure netwerkinfrastructuur ingesteld als IP-adressen behouden, geen vereiste is. Deze begint met het met een beschrijving van de toepassing en controleer vervolgens bij het instellen van netwerken on-premises en in Azure en vervolgens sluiting hoe een testfailover en een geplande failover.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [netwerktoewijzing](site-recovery-network-mapping.md).
