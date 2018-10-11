---
title: IP-adressen voor de verbinding met een secundaire on-premises site na een failover met Azure Site Recovery instellen | Microsoft Docs
description: Beschrijft hoe u voor het instellen van het IP-adressen voor de verbinding met virtuele machines in een secundaire on-premises site na een failover van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: rayne
ms.openlocfilehash: dc28611f4b27d263e19f76ce4313f8d531a8cba2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49076998"
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>Instellen van IP-adressen voor de verbinding met een secundaire on-premises site na een failover

Nadat u Hyper-V virtuele machines in System Center Virtual Machine Manager (VMM)-clouds naar een secundaire site een failover, moet u kunnen verbinding maken met de replica-VM's. Dit artikel helpt u om dit te doen. 

## <a name="connection-options"></a>Opties voor verbinding

Na een failover, moet u er een aantal manieren voor het afhandelen van IP-adressering voor replica-VM's zijn: 

- **Hetzelfde IP-adres behouden na een failover**: In dit scenario wordt de gerepliceerde virtuele machine heeft hetzelfde IP-adres als de primaire virtuele machine. Dit vereenvoudigt het netwerk gerelateerde problemen na een failover, maar sommige infrastructuurtaken vereist.
- **Gebruik een ander IP-adres na een failover**: In dit scenario wordt de virtuele machine een nieuwe IP-adres na een failover. 
 

## <a name="retain-the-ip-address"></a>De IP-adres behouden

Als u behouden van de IP-adressen van de primaire site, na een failover naar de secundaire site wilt, kunt u het volgende doen:

- Implementeer een gespreide subnet tussen de primaire en secundaire sites.
- Voer een failover van de volledige subnet uit de primaire naar secundaire site. U moet bijwerken van routes om aan te geven van de nieuwe locatie van de IP-adressen.


### <a name="deploy-a-stretched-subnet"></a>Een gespreide subnet implementeren

In een stretch-configuratie is het subnet beschikbaar tegelijkertijd in de primaire en secundaire sites. In een stretch-subnet, wanneer u een virtuele machine en de (laag-3) IP-adresconfiguratie naar de secundaire site verplaatst, het netwerk automatisch het verkeer routeert naar de nieuwe locatie. 

- Vanuit een perspectief Layer 2 (data link-laag) moet u netwerkapparatuur die een gespreide VLAN kunt beheren.
- Door het VLAN rekken, vergroot de potentiële foutdomein voor beide sites. Dit is een single point of failure. Hoewel dit onwaarschijnlijk, in een dergelijk scenario u niet mogelijk om te isoleren van een incident, zoals een broadcast-storm. 


### <a name="fail-over-a-subnet"></a>Failover van een subnet

U kunt voor het hele subnet om op te halen van de voordelen van de gespreide subnet, zonder deze daadwerkelijk stretching failover. In deze oplossing maakt een subnet is beschikbaar in de bron- of doelabonnements-site, maar niet in beide tegelijkertijd.

- Als u wilt behouden de IP-adresruimte in het geval van een failover, kunt u via een programma voor de infrastructuur router subnetten verplaatsen van de ene site naar een andere rangschikken.
- Wanneer er een failover optreedt, wordt de subnetten met hun bijbehorende virtuele machines verplaatst.
- Het grootste nadeel van deze benadering is dat er een storing optreedt, moet u het hele subnet verplaatsen.

#### <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van volledige subnet voor failover. 

- De primaire site heeft voordat de failover, toepassingen die worden uitgevoerd in een subnet 192.168.1.0/24.
- Tijdens de failover, alle van de virtuele machines in dit subnet kan niet op de secundaire site en behouden hun IP-adressen. 
- Routes tussen alle sites moeten worden gewijzigd naar aanleiding van het feit dat alle virtuele machines in subnet 192.168.1.0/24 nu naar de secundaire site verplaatst zijn.

De volgende afbeeldingen illustreren de subnetten voor en na de failover.


**Voordat de failover**

![Voordat de failover](./media/hyper-v-vmm-networking/network-design2.png)

**Na een failover**

![Na een failover](./media/hyper-v-vmm-networking/network-design3.png)

Site Recovery wordt na een failover, een IP-adres toegewezen voor elke netwerkinterface op de virtuele machine. Het adres wordt toegewezen vanuit de statische IP-adresgroep in het betreffende netwerk, voor elk VM-exemplaar.

- Als de IP-adresgroep op de secundaire site hetzelfde als die op de bronsite is, Site Recovery hetzelfde IP-adres (van de bron-VM), toegewezen aan de replica-VM. Het IP-adres is gereserveerd in VMM, maar het is niet ingesteld als het failover-IP-adres op de Hyper-V-host. De failover-IP-adres op een Hyper-v-host is ingesteld op net vóór de failover.
- Als hetzelfde IP-adres beschikbaar is, wijst nog een beschikbaar IP-adres toe uit de groep van Site Recovery.
- Als VM's gebruik van DHCP, beheren niet de IP-adressen in Site Recovery. U moet controleren of de DHCP-server op de secundaire site adressen uit hetzelfde bereik als de bronsite toewijzen kunt.

### <a name="validate-the-ip-address"></a>Valideren van het IP-adres

Nadat u beveiliging voor een virtuele machine inschakelt, kunt u de volgende voorbeeldscript gebruiken om te controleren of het adres toegewezen aan de virtuele machine. Dit IP-adres is ingesteld als het failover-IP-adres en toegewezen aan de virtuele machine op het moment van failover:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Gebruik een ander IP-adres

In dit scenario, zijn de IP-adressen van virtuele machines die fungeren als failover gewijzigd. Het nadeel van deze oplossing is het onderhoud vereist.  DNS- en cache vermeldingen moeten mogelijk worden bijgewerkt. Dit kan resulteren in uitvaltijd kunt als volgt worden verholpen:

- Lage TTL-waarden gebruiken voor intranettoepassingen kunt maken.
- Gebruik het volgende script in een plan voor Site Recovery-herstel voor een tijdige update van de DNS-server. U hoeft geen het script als u dynamische DNS-registratie.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Voorbeeld 

In dit voorbeeld hebben we verschillende IP-adressen tussen primaire en secundaire sites en er is een derde site uit welke toepassingen die worden gehost op de primaire of herstel de site kan worden geopend.

- Apps zijn voordat de failover, gehoste subnet 192.168.1.0/24 op de primaire site.
- Apps worden na een failover geconfigureerd in subnet 172.16.1.0/24 op de secundaire site.
- Alle drie sites hebben toegang tot elkaar.
- Na een failover wordt apps in het subnet herstel worden hersteld.
- In dit scenario is het niet nodig voor failover van het gehele subnet en er zijn geen wijzigingen nodig zijn om te configureren van routes voor VPN of het netwerk. De failover, en sommige DNS-updates en ervoor te zorgen dat toepassingen toegankelijk blijven.
- Als DNS is geconfigureerd voor dynamische updates toestaan, klikt u vervolgens de virtuele machines, wordt geregistreerd zelf met behulp van het nieuwe IP-adres wanneer ze na een failover worden gestart.

**Voordat de failover**

![Verschillende IP-adres - voordat de failover](./media/hyper-v-vmm-networking/network-design10.png)

**Na een failover**

![Verschillende IP-adres - na een failover](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Volgende stappen

[Een failover uitvoeren](hyper-v-vmm-failover-failback.md)

