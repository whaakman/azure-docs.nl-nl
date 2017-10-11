---
title: Testfailover (VMM naar VMM) in Azure Site Recovery | Microsoft Docs
description: "Azure Site Recovery coördineert de replicatie, failovers en herstel van virtuele machines en fysieke servers. Meer informatie over failover naar Azure of een secundair datacenter."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: pratshar
ms.openlocfilehash: afc4790d5714ce7145c8f4291a05acc2e9882a9b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Testfailover (VMM naar VMM) in Site Recovery


In dit artikel bevat informatie en instructies voor het uitvoeren van een testfailover of een detailanalyse disaster recovery (DR) van virtuele machines (VM's) en fysieke servers die zijn beveiligd met Azure Site Recovery. Als de site recovery gebruikt u een System Center Virtual Machine Manager VMM beheerde on-premises site.

U uitvoeren een testfailover voor het valideren van uw replicatiestrategie voor of uitvoeren van een details voor Dr zonder verlies van gegevens of uitvaltijd. Een testfailover heeft geen invloed op de lopende replicatie of op uw productieomgeving. U kunt uitvoeren op een virtuele machine of een [herstelplan](site-recovery-create-recovery-plans.md). Wanneer u een testfailover activeren wilt, moet u opgeven van het netwerk dat de test-virtuele machines verbinding maken. U kunt de voortgang van de testfailover bijhouden op het **taken** pagina.  

Als u eventuele opmerkingen of vragen hebt, plaatst u deze aan de onderkant van dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-the-infrastructure-for-test-failover"></a>De infrastructuur voorbereiden voor testfailover
Als u een testfailover uitvoeren wilt met behulp van een bestaand netwerk, bereid u Active Directory, DHCP en DNS-in dit netwerk.

Als u een testfailover uitvoeren wilt met behulp van de optie voor het automatisch maken van VM-netwerken, moet u een handmatige stap voordat de groep 1 toevoegen in het herstelplan die u gaat gebruiken voor de testfailover. Vervolgens de infrastructuurresources toevoegen aan de automatisch gemaakte netwerk voordat u de testfailover uitvoert.

### <a name="things-to-note"></a>Let op
Als u naar een secundaire site repliceert, het type netwerk dat gebruikmaakt van de replicamachine moet niet overeenkomen met het type logische netwerk dat wordt gebruikt voor de testfailover, maar sommige combinaties werkt mogelijk niet. Als de replica maakt gebruik van DHCP- en isolatie op basis van een VLAN, hoeft het VM-netwerk voor de replica niet een statische IP-adresgroep. Met behulp van Windows-Netwerkvirtualisatie voor de testfailover werkt niet omdat geen-adresgroepen beschikbaar zijn. 

Bovendien werkt de testfailover niet als de replica-netwerk zonder isolatie en voor het testnetwerk Windows-Netwerkvirtualisatie gebruikt. Dit is omdat het netwerk zonder isolatie beschikt niet over de subnetten die zijn vereist voor het maken van een Windows-Netwerkvirtualisatie-netwerk.

Hoe gerepliceerde virtuele machines zijn verbonden met het VM-netwerken toegewezen nadat de failover is afhankelijk van hoe het VM-netwerk is geconfigureerd in de VMM-console.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>VM-netwerk is geconfigureerd met geen isolatie- of VLAN-isolatie
Als DHCP is gedefinieerd voor het VM-netwerk, wordt de replica virtuele machine verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk. De virtuele machine ontvangt het IP-adres van de beschikbare DHCP-server. 

U hoeft niet te definiëren van een statische IP-adresgroep voor de VM-netwerk van het doel. Als een statische IP-adresgroep wordt gebruikt voor het VM-netwerk, wordt de replica virtuele machine verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk.

De virtuele machine ontvangt het IP-adres van de groep die gedefinieerd voor het VM-netwerk. Als een statische IP-adresgroep is niet gedefinieerd voor het VM-netwerk van het doel, mislukt de toewijzing van IP-adres. De IP-adresgroep maken op de bron- en doel-VMM-servers die u voor beveiliging als herstel gebruiken wilt.

#### <a name="vm-network-with-windows-network-virtualization"></a>VM-netwerk met Windows-Netwerkvirtualisatie
Als u een VM-netwerk is geconfigureerd met Windows-Netwerkvirtualisatie, moet u een statische adresgroep voor de doel-VM-netwerk, ongeacht of de bron-VM-netwerk is geconfigureerd voor het gebruik van DHCP of een statisch IP-adresgroep definiëren. 

Als u DHCP definieert, wordt de doel-VMM-server fungeert als een DHCP-server en biedt een IP-adres van de groep die gedefinieerd voor het VM-netwerk van het doel. Als het gebruik van een statische IP-adresgroep is gedefinieerd voor de bronserver, wijst de doel-VMM-server een IP-adres van de groep. In beide gevallen mislukt toewijzing van IP-adres als een statische IP-adresgroep is niet gedefinieerd.


### <a name="prepare-dhcp"></a>DHCP voorbereiden
Als de virtuele machines die zijn betrokken bij de wordt testfailover gebruik van DHCP, maakt u een test-DHCP-server in de geïsoleerde netwerk omwille van de testfailover.

### <a name="prepare-active-directory"></a>Active Directory voorbereiden
Voor het uitvoeren van een testfailover voor de toepassing testen, moet u een kopie van de productie-omgeving met Active Directory in uw testomgeving. Raadpleeg voor meer informatie de [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>DNS voorbereiden
Een DNS-server voor de testfailover als volgt voorbereiden:

* **DHCP**: als virtuele machines DHCP gebruikt, het IP-adres van de test DNS moet worden bijgewerkt voor de test DHCP-server. Als u een met het netwerktype Windows-Netwerkvirtualisatie, wordt de VMM-server fungeert als de DHCP-server. Daarom moet het IP-adres van DNS worden bijgewerkt in het testfailovernetwerk. In dit geval wordt registreren de virtuele machines zichzelf voor de relevante DNS-server.
* **Statisch adres**: als virtuele machines met een statisch IP-adres, het IP-adres van de test DNS-server moet worden bijgewerkt in het testfailovernetwerk. Mogelijk moet u DNS bijwerken met het IP-adres van de test-virtuele machines. U kunt het volgende voorbeeldscript gebruiken voor dit doel:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Een testfailover uitvoeren
Deze procedure wordt beschreven hoe u een testfailover voor een herstelplan uitvoeren. U kunt ook de failover voor een enkele virtuele machine uitvoeren op de **virtuele Machines** tabblad.

![Failover-blade test](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover** > **Testfailover**.
1. Op de **Testfailover** blade opgeven hoe virtuele machines moeten zijn verbonden met netwerken na de testfailover. Zie voor meer informatie de [netwerkopties](#network-options-in-site-recovery).
1. Voortgang van de failover volgen op de **taken** tabblad.
1. Nadat de failover is voltooid, moet u controleren of de virtuele machines worden gestart.
1. Wanneer u bent klaar, klikt u op **testfailover opschonen** op het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. Hiermee verwijdert u de virtuele machines en de netwerken die zijn gemaakt tijdens de testfailover.


## <a name="network-options-in-site-recovery"></a>Netwerkopties in Site Recovery

Wanneer u een failovertest uitvoert, wordt u gevraagd om te selecteren van netwerkinstellingen voor test-replica-machines. U hebt meerdere mogelijkheden.  

| **Testen van failover-optie** | **Beschrijving** | **Controle van failover** | **Details** |
| --- | --- | --- | --- |
| **Failover naar een secundaire site VMM--zonder netwerk** |Selecteer een VM-netwerk niet. |Failover wordt gecontroleerd dat de test-machines worden gemaakt.<br/><br/>De virtuele testmachine wordt gemaakt op de host waarop de replica virtuele machine bestaat. Dit is niet toegevoegd aan de cloud waar de replica virtuele machine zich bevindt. |<p>De failover-machine is niet verbonden met een netwerk.<br/><br/>De machine kan worden verbonden met een VM-netwerk nadat deze is gemaakt. |
| **Failover naar een secundaire VMM-site--met het netwerk** |Selecteer een bestaand VM-netwerk. |Failover wordt gecontroleerd of de virtuele machines worden gemaakt. |De virtuele testmachine wordt gemaakt op de host waarop de replica virtuele machine bestaat. Dit is niet toegevoegd aan de cloud waar de replica virtuele machine zich bevindt.<br/><br/>Maak een VM-netwerk dat is geïsoleerd van uw productienetwerk.<br/><br/>Als u een VLAN-netwerk gebruikt, raden wij u een afzonderlijke logische netwerk (niet gebruikt in productie) maken in VMM voor dit doel. Dit logische netwerk wordt gebruikt voor het maken van VM-netwerken voor testfailovers.<br/><br/>Het logische netwerk moet worden gekoppeld aan ten minste één van de netwerkadapters van alle Hyper-V-servers die als van virtuele machines host.<br/><br/>Voor logische netwerken met VLAN moeten de netwerksites die u aan het logische netwerk toevoegt worden geïsoleerd.<br/><br/>Als u een logisch netwerk voor op basis van Windows-Netwerkvirtualisatie, maakt Azure Site Recovery automatisch geïsoleerde VM-netwerken. |
| **Failover naar een secundaire VMM-site--een netwerk maken** |Een tijdelijke testnetwerk is automatisch gemaakt op basis van de instelling die u opgeeft in **logisch netwerk** en de bijbehorende netwerksites. |Failover wordt gecontroleerd of de virtuele machines worden gemaakt. |Gebruik deze optie als u meer dan één VM-netwerk maakt gebruik van het herstelplan. Als u Windows-Netwerkvirtualisatie-netwerken gebruikt, kunt deze optie automatisch maken van VM-netwerken met dezelfde instellingen (subnetten en IP-adresgroepen) in het netwerk van de replica virtuele machine. Deze VM-netwerken zijn automatisch opgeschoond nadat de testfailover voltooid is.</p><p>De virtuele testmachine wordt gemaakt op de host waarop de replica virtuele machine bestaat. Dit is niet toegevoegd aan de cloud waar de replica virtuele machine zich bevindt. |

> [!TIP]
> Het IP-adres aan een virtuele machine wordt opgegeven tijdens de testfailover is hetzelfde IP-adres waarop de virtuele machine wilt ontvangen voor een geplande of niet-geplande failover (ervan uitgaande dat het IP-adres beschikbaar in het testfailovernetwerk is). Als hetzelfde IP-adres is niet beschikbaar in het testfailovernetwerk, krijgt de virtuele machine een ander IP-adres dat beschikbaar is in het testfailovernetwerk.
>
>


## <a name="test-failover-to-a-production-network-on-a-recovery-site"></a>Testfailover met een productienetwerk op een site recovery
Het is raadzaam dat als u een failovertest uitvoert, u een netwerk dat verschilt van het herstel site productienetwerk die u hebt opgegeven in [netwerktoewijzing](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Maar als u zeker valideren, end-to-end netwerkverbinding in een failover-virtuele machine wilt, noteert u de volgende punten:

* Zorg ervoor dat de primaire virtuele machine wordt afgesloten wanneer u de testfailover uitvoert. Als u dit niet doet, worden twee virtuele machines met dezelfde identiteit tegelijkertijd uitgevoerd in hetzelfde netwerk. Deze situatie kan leiden tot ongewenste gevolgen.
* Eventuele wijzigingen die u in de test failover virtuele machines aanbrengt gaan verloren wanneer u de test failover virtuele machines opruimen. Deze wijzigingen worden niet gerepliceerd naar de primaire virtuele machine.
* Deze werkwijze testen leidt tot uitvaltijd voor uw productietoepassing. Vraag gebruikers van de toepassing niet te gebruiken van de toepassing wanneer de details voor Dr uitgevoerd wordt.  


## <a name="next-steps"></a>Volgende stappen
Nadat u een testfailover met succes hebt uitgevoerd, kunt u proberen een [failover](site-recovery-failover.md).
