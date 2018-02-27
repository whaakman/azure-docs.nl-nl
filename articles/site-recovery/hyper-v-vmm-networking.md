---
title: Instellen van IP-adressen voor verbinding met een secundaire on-premises site na een failover met Azure Site Recovery | Microsoft Docs
description: Beschrijft het instellen van IP-adressering voor de verbinding met virtuele machines in een secundaire on-premises site na een failover van Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: rayne
ms.openlocfilehash: 531705bc704b3366c1c670ecf07c809ade67bc55
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="set-up-ip-addressing-to-connect-to-a-secondary-on-premises-site-after-failover"></a>Instellen van IP-adressen voor verbinding met een secundaire on-premises site na een failover

Nadat u Hyper-V-machines in System Center Virtual Machine Manager (VMM)-clouds naar een secundaire site een failover, moet u kunnen verbinding maken met de replica virtuele machines. In dit artikel helpt u om dit te doen. 

## <a name="connection-options"></a>Opties voor verbinding

Na een failover zijn er verschillende manieren voor het afhandelen van IP-adressering voor replica-VM: 

- **Hetzelfde IP-adres behouden na een failover**: In dit scenario heeft de gerepliceerde virtuele machine hetzelfde IP-adres als de primaire virtuele machine. Dit vereenvoudigt het netwerk gerelateerde problemen na een failover, maar u moet ondernemen infrastructuur.
- **Gebruik een ander IP-adres na een failover**: In dit scenario wordt de virtuele machine een nieuw IP-adres na een failover. 
 

## <a name="retain-the-ip-address"></a>Het IP-adres behouden

Als u behouden van de IP-adressen van de primaire site na een failover naar de secundaire site wilt, kunt u:

- Implementeer een gespreide subnet tussen de primaire en secundaire sites.
- Voer een volledige subnet failover van de primaire naar secundaire site. U moet bijwerken van routes om aan te geven van de nieuwe locatie van de IP-adressen.


### <a name="deploy-a-stretched-subnet"></a>Een subnet gespreide implementeren

In een configuratie met gespreide is het subnet beschikbaar tegelijkertijd in de primaire en secundaire sites. In een subnet gespreide, wanneer u een machine en de adresconfiguratie (laag 3) naar de secundaire site verplaatst, stuurt het netwerk automatisch het verkeer naar de nieuwe locatie. 

- Vanuit een perspectief Layer 2 (data link-laag) moet u netwerkapparatuur die een gespreide VLAN kunt beheren.
- Door het VLAN rekken, breidt het potentiële foutdomein bij beide sites. Dit wordt een potentieel risico. Tijdens het onwaarschijnlijk, in een dergelijk scenario u niet mogelijk om te isoleren van een incident, zoals een broadcast-storm. 


### <a name="fail-over-a-subnet"></a>Failover van een subnet

U kunt het hele subnet om op te halen van de voordelen van het subnet van de gespreide zonder daadwerkelijk uitrekken deze failover. In deze oplossing een subnet is beschikbaar in de bron- of doelentiteit site, maar niet in beide tegelijk.

- Om te blijven van de IP-adresruimte in het geval van een failover, kunt u via een programma voor de infrastructuur router subnetten verplaatsen van de ene site naar een andere rangschikken.
- Als een failover optreedt, subnetten worden verplaatst met de bijbehorende gekoppelde virtuele machines.
- Het grootste nadeel van deze benadering is dat bij een storing, die u moet het hele subnet verplaatsen.

#### <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van een volledige subnet failover. 

- De primaire site heeft voordat de failover, toepassingen in een subnet 192.168.1.0/24.
- Tijdens de failover, alle van de virtuele machines in dit subnet kan niet via de secundaire site en de IP-adressen behouden. 
- Routes tussen alle sites moeten worden gewijzigd naar aanleiding van het feit dat alle virtuele machines in het subnet 192.168.1.0/24 nu naar de secundaire site verplaatst zijn.

De volgende afbeeldingen illustreren de subnetten voor en na een failover.


**Vóór de failover**

![Vóór de failover](./media/hyper-v-vmm-networking/network-design2.png)

**Na een failover**

![Na een failover](./media/hyper-v-vmm-networking/network-design3.png)

Site Recovery wijst een IP-adres na een failover toe voor elke netwerkinterface op de virtuele machine. Het adres is van de statische IP-adresgroep in het betreffende netwerk, voor elk exemplaar van de virtuele machine toegewezen.

- Als de IP-adresgroep op de secundaire site hetzelfde als die op de bronsite is, wijst Site Recovery hetzelfde IP-adres (van de bron-VM), naar de replica virtuele machine. Het IP-adres is gereserveerd in VMM, maar het is niet ingesteld als het failover-IP-adres op de Hyper-V-host. Het failover-IP-adres op een Hyper-v-host is net voordat de failover ingesteld.
- Als hetzelfde IP-adres niet beschikbaar is, wijst Site Recovery een ander beschikbaar IP-adres van de groep.
- Als VMs DHCP gebruikt, beheren niet de IP-adressen in Site Recovery. U moet controleren of de DHCP-server op de secundaire site adressen uit hetzelfde bereik als de bronsite toewijzen kan.

### <a name="validate-the-ip-address"></a>Valideren van het IP-adres

Nadat u beveiliging voor een virtuele machine inschakelt, kunt u volgende voorbeeldscript gebruiken om te controleren of het adres dat is toegewezen aan de virtuele machine. Dit IP-adres is ingesteld als het failover-IP-adres en toegewezen aan de virtuele machine op het moment van failover:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="use-a-different-ip-address"></a>Gebruik een ander IP-adres

In dit scenario zijn de IP-adressen van virtuele machines die een failover worden gewijzigd. Het nadeel van deze oplossing is het onderhoud vereist.  DNS- en cache vermeldingen moeten mogelijk worden bijgewerkt. Dit kan leiden tot uitvaltijd, die kan worden opgevangen volgt als volgt:

- Lage TTL-waarden voor intranettoepassingen gebruiken.
- Gebruik het volgende script in een herstelplan Site Recovery voor een tijdige update van de DNS-server. U kunt het script niet nodig als u dynamische DNS-registratie.

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

In dit voorbeeld hebben we verschillende IP-adressen op primaire en secundaire sites en er is een derde site welke toepassingen die worden gehost op de primaire of het herstel van site kan worden geopend.

- Apps zijn voordat de failover, gehoste subnet 192.168.1.0/24 op de primaire site.
- Na een failover worden de apps configureren in 172.16.1.0/24 subnet in de secundaire site.
- Alle drie sites hebben toegang tot elkaar.
- Na een failover wordt in het subnet herstel apps worden hersteld.
- In dit scenario is niet nodig voor failover van het gehele subnet en er zijn geen wijzigingen nodig zijn om opnieuw te configureren van VPN- of netwerkbeheerder routes. Zorg ervoor dat toepassingen toegankelijk blijven de failover en een aantal DNS-updates.
- Als DNS is geconfigureerd voor dynamische updates toestaan, wordt klikt u vervolgens de virtuele machines geregistreerd zelf met behulp van het nieuwe IP-adres, wanneer ze na een failover wordt gestart.

**Vóór de failover**

![Verschillende IP-adres - vóór de failover](./media/hyper-v-vmm-networking/network-design10.png)

**Na een failover**

![Verschillende IP-adres - na een failover](./media/hyper-v-vmm-networking/network-design11.png)


## <a name="next-steps"></a>Volgende stappen

[Een failover uitvoeren](hyper-v-vmm-failover-failback.md)

