---
title: Een Dr-herstelanalyse van Hyper-V-machines worden uitgevoerd naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Leer hoe u een DR-oefening voor Hyper-V-machines in VMM-clouds worden uitgevoerd naar een secundaire on-premises datacenter met Azure Site Recovery.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: dc8deb16f7d124c5fb11568f25050eee99a245b8
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58096755"
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>Een DR-oefening uitvoeren van Hyper-V-machines naar een secundaire site


In dit artikel wordt beschreven hoe u een Dr-herstelanalyse (DR) voor Hyper-V-virtuele machines die worden beheerd in System Center Virtual Machine Manager V(MM)-clouds, naar een secundaire on-premises site, met behulp van [Azure Site Recovery](site-recovery-overview.md).

U voert een testfailover uit voor het valideren van uw replicatiestrategie voor en uitvoeren van een DR-oefening zonder gegevensverlies of uitvaltijd. Een testfailover heeft geen invloed op de voortdurende replicatie of op uw productieomgeving. 

## <a name="how-do-test-failovers-work"></a>Hoe test failovers werk?

U voert een testfailover uit van de primaire naar de secundaire site. Als u controleren op een virtuele machine failover wilt, kunt u een testfailover uitvoeren zonder iets instellen op de secundaire site. Als u controleren of de app failover werkt wilt zoals verwacht, moet u het instellen van netwerken en infrastructuur op de secundaire locatie.
- U kunt een testfailover uitvoeren op een enkele virtuele machine of op een [herstelplan](site-recovery-create-recovery-plans.md).
- U kunt een testfailover zonder een netwerk uitvoeren met een bestaand virtueel netwerk of met een automatisch gemaakte netwerk. Meer informatie over deze opties vindt u in de onderstaande tabel.
    - U kunt een testfailover zonder een netwerk uitvoeren. Deze optie is handig als u wilt dat een virtuele machine kan failover wilt uitvoeren, maar niet mogelijk om te controleren of de netwerkconfiguratie controleren.
    - Voer de failover met een bestaand virtueel netwerk. U wordt aangeraden dat u niet een productienetwerk gebruikt.
    - De failover uitvoert en laat u Site Recovery maakt automatisch een testnetwerk. Site Recovery wordt in dit geval automatisch maken van het netwerk en opschonen wanneer de test-failover voltooid is.
- U moet een herstelpunt voor de testfailover selecteren: 
    - **Laatst verwerkt**: Deze optie voert een virtuele machine failover uit naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
    - **Laatste toepassingsconsistente punt**: Deze optie is failover van een virtuele machine naar de meest recente toepassingsconsistente herstelpunt verwerkt door Site Recovery. 
    - **Laatste**: Deze optie worden eerst alle gegevens die is verzonden naar Site Recovery-service te maken van een herstelpunt voor elke virtuele machine voordat de failover wordt uitgevoerd naar het verwerkt. Deze optie biedt het laagste RPO (Recovery Point Objective), omdat de virtuele machine gemaakt nadat de failover heeft de gegevens die zijn gerepliceerd naar de Site Recovery wanneer de failover werd geactiveerd.
    - **Meest recente verwerkte multi-VM**: Beschikbaar voor herstelplannen met een of meer VM's die multi-VM's is ingeschakeld. Virtuele machines met de instelling is ingeschakeld, schakelt over naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt uitgevoerd. Andere VM's een failover naar de meest recente verwerkte herstelpunt.
    - **Nieuwste multi-VM-app-consistente**: Deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM's is ingeschakeld. Virtuele machines die deel van een replicatiegroep uitmaken schakelt over naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt. Andere VM's een failover naar de meest recente toepassingsconsistente herstelpunt.
    - **Aangepast**: Gebruik deze optie voor failover van een specifieke virtuele machine naar een bepaald herstelpunt.



## <a name="prepare-networking"></a>Voorbereiden van netwerken

Wanneer u een failovertest uitvoert, wordt u gevraagd om te selecteren van netwerkinstellingen voor test gerepliceerde machines, zoals samengevat in de tabel.

| **Optie** | **Details** | |
| --- | --- | --- |
| **Geen** | De virtuele testmachine wordt gemaakt op de host waarop de replica-VM zich bevindt. Het is niet toegevoegd aan de cloud en is niet verbonden met een netwerk.<br/><br/> U kunt de machine verbinding maken met een VM-netwerk nadat deze is gemaakt.| |
| **Bestaande gebruiken** | De virtuele testmachine wordt gemaakt op de host waarop de replica-VM zich bevindt. Het is niet toegevoegd aan de cloud.<br/><br/>Maak een VM-netwerk dat is geïsoleerd van uw productienetwerk.<br/><br/>Als u een VLAN-netwerk gebruikt, raden wij u een afzonderlijke logisch netwerk (niet gebruikt in productie) maken in VMM voor dit doel. Dit logische netwerk wordt gebruikt voor het maken van VM-netwerken voor testfailovers.<br/><br/>Het logische netwerk moet worden gekoppeld aan ten minste één van de netwerkadapters van alle Hyper-V-servers die als van virtuele machines host.<br/><br/>Voor VLAN logische netwerken, moeten de netwerksites die u aan het logische netwerk toevoegt worden geïsoleerd.<br/><br/>Als u een Windows-Netwerkvirtualisatie op basis van logisch netwerk, maakt Azure Site Recovery automatisch geïsoleerde VM-netwerken. | |
| **Een netwerk maken** | Een tijdelijke testnetwerk is gemaakt op basis van de instelling die u opgeeft in automatisch **logisch netwerk** en de bijbehorende netwerksites.<br/><br/> Failover wordt gecontroleerd dat de virtuele machines worden gemaakt.<br/><br/> U moet deze optie gebruiken als een herstelplan maakt gebruik van meer dan één VM-netwerk.<br/><br/> Als u netwerken voor Windows-Netwerkvirtualisatie, kan deze optie automatisch maken van VM-netwerken met dezelfde instellingen (subnetten en IP-adresgroepen) in het netwerk van de replica virtuele machine. Deze VM-netwerken worden automatisch opgeschoond nadat de failovertest voltooid is.<br/><br/> De test virtuele machine is gemaakt op de host waar de replica virtuele machine zich bevindt. Het is niet toegevoegd aan de cloud.|

### <a name="best-practices"></a>Aanbevolen procedures

- Testen van een productienetwerk zorgt ervoor dat de downtime voor werkbelastingen voor productie. Vraag uw gebruikers geen gerelateerde om apps te gebruiken wanneer de Dr-herstelanalyse uitgevoerd wordt.

- Het testnetwerk hoeft niet overeen met de type van de VMM logisch netwerk dat wordt gebruikt voor test-failover. Maar bepaalde combinaties niet werken:

     - Als de replica maakt gebruik van DHCP- en op basis van een VLAN-isolatie, moet niet een statisch IP-adresgroep in het VM-netwerk voor de replica. Met behulp van Windows-Netwerkvirtualisatie voor de testfailover werkt niet omdat er geen-adresgroepen beschikbaar zijn. 
        
     - Test-failover werkt niet als de replica-netwerk zonder isolatie wordt gebruikt en het testnetwerk gebruikmaakt van Windows-Netwerkvirtualisatie. Dit is omdat het netwerk zonder isolatie beschikt niet over de subnetten die zijn vereist voor het maken van een Windows-Netwerkvirtualisatie-netwerk.
        
- U wordt aangeraden dat u het netwerk die u hebt geselecteerd niet gebruikt voor netwerktoewijzing voor test-failover.

- Hoe gerepliceerde virtuele machines zijn verbonden met het VM-netwerken toegewezen nadat failover is afhankelijk van hoe het VM-netwerk is geconfigureerd in de VMM-console.


### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>VM-netwerk zonder isolatie of VLAN-isolatie is geconfigureerd

Als een VM-netwerk is geconfigureerd in VMM met geen isolatie- of VLAN-isolatie, Let op het volgende:

- Als DHCP is gedefinieerd voor het VM-netwerk, wordt de replica virtuele machine is verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk. De virtuele machine ontvangt het IP-adres van de beschikbare DHCP-server.
- U hoeft niet te definiëren van een statische IP-adresgroep voor het doel-VM-netwerk. Als een statisch IP-adresgroep wordt gebruikt voor het VM-netwerk, wordt de replica virtuele machine is verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk.
- De virtuele machine ontvangt het IP-adres van de groep die gedefinieerd voor het VM-netwerk. Als een statisch IP-adresgroep is niet gedefinieerd in de doel-VM-netwerk, mislukt de toewijzing van IP-adressen. De IP-adresgroep maken op de bron- en doel-VMM-servers die u voor beveiliging en herstel gebruiken wilt.

### <a name="vm-network-with-windows-network-virtualization"></a>VM-netwerk met Windows-Netwerkvirtualisatie

Als een VM-netwerk is geconfigureerd in VMM met Windows-Netwerkvirtualisatie, Let op het volgende:

- U moet een statische adresgroep voor het doel-VM-netwerk, ongeacht of de bron-VM-netwerk is geconfigureerd voor het gebruik van DHCP of een statisch IP-adresgroep definiëren. 
- Als u DHCP definieert, wordt de doel-VMM-server fungeert als een DHCP-server en biedt een IP-adres van de groep die gedefinieerd voor de doel-VM-netwerk.
- Als het gebruik van een statische IP-adresgroep is gedefinieerd voor de bronserver, wijst de doel-VMM-server een IP-adres toe uit de groep. In beide gevallen mislukt IP-adrestoewijzing als een statisch IP-adresgroep is niet gedefinieerd.



## <a name="prepare-the-infrastructure"></a>De infrastructuur voorbereiden

Als u wilt dat een virtuele machine failover kunt controleren, kunt u een testfailover zonder een infrastructuur uitvoeren. Als u een volledige DR-oefening voor het testen van failover van de app doet wilt, moet u de infrastructuur op de secundaire site voorbereiden:

- Als u een testfailover met behulp van een bestaand virtueel netwerk uitvoert, moet Active Directory, DHCP en DNS-voorbereiden in dat netwerk.
- Als u een testfailover met de optie uitvoeren voor het automatisch maken van een VM-netwerk, moet u beschikken over infrastructurele resources toevoegen aan de automatisch gemaakte netwerk, voordat u de testfailover uitvoert. In een herstelplan, kunt u dit vereenvoudigen door het toevoegen van een handmatige stap vóór groep 1 in het herstelplan te gaan die u wilt gebruiken voor de testfailover. Vervolgens voegt u de infrastructuurresources met de automatisch gemaakte netwerk voordat u de testfailover uitvoert.


### <a name="prepare-dhcp"></a>Prepare DHCP
Als de virtuele machines die zijn betrokken bij de wordt testfailover DHCP gebruiken, maken van een test-DHCP-server in het geïsoleerde netwerk ten behoeve van test-failover.


### <a name="prepare-active-directory"></a>Active Directory voorbereiden
Als u wilt uitvoeren van een testfailover uitvoeren voor de toepassing testen, moet u een kopie van de productie-omgeving met Active Directory in uw testomgeving. Raadpleeg voor meer informatie de [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>DNS voorbereiden
Een DNS-server voor de testfailover als volgt voorbereiden:

* **DHCP**: Als virtuele machines gebruik van DHCP, moet de IP-adres van de DNS-test worden bijgewerkt op de DHCP-testserver. Als u een netwerktype van Windows-Netwerkvirtualisatie, wordt de VMM-server fungeert als de DHCP-server. Daarom moet het IP-adres van DNS worden bijgewerkt in het testfailovernetwerk. In dit geval wordt registreren de virtuele machines zichzelf voor de relevante DNS-server.
* **Statisch adres**: Als virtuele machines met een statisch IP-adres, moet de IP-adres van de test DNS-server worden bijgewerkt in het testfailovernetwerk. Mogelijk moet u DNS bijwerken met het IP-adres van de virtuele testmachines. U kunt het volgende voorbeeldscript gebruiken voor dit doel:

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

Deze procedure wordt beschreven hoe u een testfailover uitvoeren voor een herstelplan uitvoeren. U kunt ook de failover voor een enkele virtuele machine uitvoeren op de **virtuele Machines** tabblad.

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover** > **Testfailover**.
2. Op de **Testfailover** blade opgeven hoe replica-VM's moet zijn verbonden met netwerken na de testfailover.
3. Voortgang van de failover volgen op de **taken** tabblad.
4. Nadat failover voltooid is, controleert u of de virtuele machines is gestart.
5. Wanneer u klaar bent, klikt u op **failovertest** op het herstelplan te gaan. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op. Deze stap worden verwijderd voor alle VM's en netwerken die zijn gemaakt door Site Recovery tijdens de testfailover. 

![Testfailover](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> Het IP-adres dat is opgegeven met een virtuele machine tijdens de testfailover is hetzelfde IP-adres dat de virtuele machine ontvangt voor een geplande of niet-geplande failover (ervan uitgaande dat het IP-adres beschikbaar in het testfailovernetwerk is). Als hetzelfde IP-adres is niet beschikbaar in het testfailovernetwerk, ontvangt de virtuele machine een ander IP-adres dat beschikbaar is in het testfailovernetwerk.



### <a name="run-a-test-failover-to-a-production-network"></a>Een testfailover uitvoeren naar een productienetwerk

Het is raadzaam dat u een testfailover niet uitgevoerd om uw productie-site herstelnetwerk die u hebt opgegeven tijdens de netwerktoewijzing. Maar als u valideren, end-to-end netwerkverbinding in een failover-VM wilt, let dan op de volgende punten:

* Zorg ervoor dat de primaire virtuele machine wordt afgesloten wanneer u de testfailover uitvoert. Als u dit niet doet, worden twee virtuele machines met dezelfde identiteit tegelijkertijd uitgevoerd in hetzelfde netwerk. Deze situatie kan leiden tot ongewenste gevolgen.
* Alle wijzigingen die u in de testfailover aanbrengt VM's gaan verloren wanneer u de migratiegegevens van de test-failover-virtuele machines. Deze wijzigingen worden niet gerepliceerd naar de primaire virtuele machines.
* Testen, zoals dit leidt tot downtime voor uw productietoepassing. Vraag gebruikers van de toepassing niet voor het gebruik van de toepassing wanneer de DR-oefening uitgevoerd wordt.  


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt een DR-oefening is uitgevoerd, kunt u [een volledige failover uitvoeren](site-recovery-failover.md).



