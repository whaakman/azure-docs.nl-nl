---
title: Instellen van IP-adressen als u wilt verbinden na een failover naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Beschrijft het instellen van IP-adressering voor de verbinding met virtuele machines na een failover naar een secundaire site met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: prateek9us
editor: 
ms.assetid: 67d73590-185c-49b2-a097-597bf54747a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: pratshar
ms.openlocfilehash: 6baeda08b1c41cc024a02f51ca27be2829c46962
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="set-up-ip-addressing-to-connect-after-failover-to-a-secondary-site"></a>Instellen van IP-adressen om na een failover naar een secundaire site verbinding te maken

Bekijk de vereisten voor implementatie en Lees dit artikel om te plannen wanneer repliceren van Hyper-V virtuele machines (VM's) wordt beheerd in System Center Virtual Machine Manager (VMM)-clouds, naar een secundaire site met netwerken [Azure Site Recovery](site-recovery-overview.md) in de Azure portal. 

Na het lezen van dit artikel kunt u onder aan dit artikel of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) opmerkingen of vragen plaatsen.

## <a name="overview"></a>Overzicht

Bij het plannen van uw replicatie en failoverstrategie is een van de belangrijke vragen verbinding maken met de replica na een failover. Er zijn een aantal opties: 

- **Gebruik een ander IP-adres**: U kunt selecteren om een ander IP-adres voor de gerepliceerde virtuele machine. In dit scenario wordt de virtuele machine een nieuw IP-adres na een failover en een DNS-update is vereist.
- **Hetzelfde IP-adres behouden**: U kunt hetzelfde IP-adres gebruiken voor de replica-VM. Houden dezelfde IP adressen het herstel vereenvoudigt door te beperken netwerkgerelateerde problemen na een failover. 

## <a name="retaining-ip-addresses"></a>IP-adressen behouden

Als u behouden van de IP-adressen van de primaire site na een failover naar de secundaire site wilt, kunt u een volledige subnet failover doen en bijwerken van routes om aan te geven van de nieuwe locatie van de IP-adressen of alternatieve implementeren een gespreide subnet tussen de primaire en de herstellocatie.

### <a name="stretched-subnet"></a>Gespreide subnet

In een gespreide subnet is het subnet beschikbaar tegelijkertijd in de primaire en secundaire site. Als u een server en de configuratie van de IP-(laag 3) naar de secundaire site verplaatst, stuurt het netwerk het verkeer naar de nieuwe locatie automatisch. 

Vanuit een perspectief Layer 2 (data link-laag) moet u netwerkapparatuur die een gespreide VLAN kunt beheren. Bovendien door het VLAN rekken, breidt de potentiële foutdomein bij beide sites, wordt in wezen een potentieel risico. Hoewel dit een onwaarschijnlijk, kan het gebeuren dat een broadcast-storm gestart en kan niet geïsoleerd worden. 


### <a name="subnet-failover"></a>Subnet failover

U kunt een failover subnet om te verkrijgen van de voordelen van het subnet van de gespreide zonder het daadwerkelijk uitrekken uitvoeren. In deze oplossing wordt een subnet beschikbaar zijn in de bron- of doelentiteit site, maar niet in beide tegelijk. Om te blijven van de IP-adresruimte in het geval van een failover, kunt u via een programma voor de infrastructuur van de router voor het verplaatsen van de subnetten van de ene site naar een andere rangschikken. Na wanneer failover plaatsvindt, subnetten, zou worden verplaatst met de bijbehorende virtuele machines. Het grootste nadeel is dat bij een storing, die u moet het hele subnet verplaatsen.

### <a name="example"></a>Voorbeeld

Hier volgt een voorbeeld van een volledige subnet failover. De primaire site zijn toepassingen in een subnet 192.168.1.0/24. Bij een failover alle VM's in dit subnet kan niet via de secundaire site en de IP-adressen behouden. Routes moeten worden gewijzigd naar aanleiding van het feit dat alle de virtuele machine virtuele machines die horen bij subnet 192.168.1.0/24 nu naar de secundaire site verplaatst zijn.

De volgende afbeeldingen tonen de subnetten voor en na failover:

- Vóór de failover is subnet 192.168.0.1/24 actief op de bronsite worden actief op de secundaire site na een failover.
- De routes tussen primaire site en de site recovery, derde en primaire site en derde site en herstelsite moet op de juiste wijze worden gewijzigd.

**Vóór de failover**

![Vóór de Failover](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Na een failover**

![Na een Failover](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Na een failover moet u dit is wat er gebeurt:

- Site Recovery wijst een IP-adres voor elke netwerkinterface op de virtuele machine van de statische IP-adresgroep in het betreffende netwerk, voor elk exemplaar van VMM.
- Als de IP-adresgroep op de secundaire site hetzelfde als die op de bronsite is, wordt de Site Recovery hetzelfde IP-adres (van de bron-VM) op de replica virtuele machine. Het IP-adres is gereserveerd in VMM, maar het is niet ingesteld als het failover-IP-adres op de Hyper-V-host. Het failover-IP-adres op een Hyper-v-host is net voordat de failover ingesteld.
- Als hetzelfde IP-adres niet beschikbaar is, wijst Site Recovery een ander beschikbaar IP-adres van de groep.
- Als VMs DHCP gebruikt, beheren niet de IP-adressen in Site Recovery. U moet controleren of de DHCP-server op de secundaire site-adres uit hetzelfde bereik als de bronsite kunt toewijzen.

### <a name="validate-the-ip-address"></a>Valideren van het IP-adres

Nadat u beveiliging voor een virtuele machine inschakelt, kunt u volgende voorbeeldscript gebruiken om te controleren of het adres dat is toegewezen aan de virtuele machine. Hetzelfde IP-adres worden ingesteld als het failover-IP-adres, en toegewezen aan de virtuele machine op het moment van failover:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Het wijzigen van IP-adressen

In dit scenario zijn de IP-adressen van virtuele machines die een failover worden gewijzigd. Het nadeel van deze oplossing is het onderhoud vereist. Normaal gesproken worden DNS bijgewerkt nadat de replica-VM start. DNS-vermeldingen mogelijk moet worden gewijzigd of fluster in thenetwork en vermeldingen in cache bijgewerkt. Dit kan leiden tot uitvaltijd. Uitvaltijd kan worden opgevangen volgt als volgt:

- Lage TTL-waarden voor intranettoepassingen gebruiken.
- Gebruik het volgende script in een herstelplan Site Recovery om bij te werken van de DNS-server om te controleren of een tijdige update. U kunt het script niet nodig als u dynamische DNS-registratie.

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

We bekijken een scenario waarin u van plan bent te gebruiken verschillende IP-adressen voor de primaire en de herstellocatie. In dit voorbeeld hebben we verschillende IP-adressen op primaire en secundaire sites en er; s een externe site uit welke toepassingen die worden gehost op de primaire of herstel site toegankelijk zijn.

- Vóór de failover, apps 192.168.1.0/24 gehoste subnet op de primaire site zijn en zijn geconfigureerd als in het subnet 172.16.1.0/24 op de secundaire site na een failover.
- VPN-verbindingen/netwerkroutes zijn op de juiste wijze geconfigureerd zodat alle drie sites toegang elkaar tot hebben.
- Na een failover wordt in het subnet herstel apps worden hersteld. In dit scenario is niet nodig voor failover van het gehele subnet en er zijn geen wijzigingen nodig zijn om opnieuw te configureren van VPN- of netwerkbeheerder routes. Zorg ervoor dat toepassingen toegankelijk blijven de failover en een aantal DNS-updates.
- Als DNS is geconfigureerd voor dynamische updates toestaan, wordt klikt u vervolgens de virtuele machines geregistreerd zelf met behulp van het nieuwe IP-adres, wanneer ze na een failover wordt gestart.

**Vóór de failover**

![Verschillende IP - vóór de Failover](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Na een failover**

![Verschillende IP - na een Failover](./media/vmm-to-vmm-walkthrough-network/network-design11.png)




