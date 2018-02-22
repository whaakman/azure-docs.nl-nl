---
title: Voer een details voor Dr van Hyper-V-machines naar een secundaire site met Azure Site Recovery | Microsoft Docs
description: Informatie over het uitvoeren van een DR-herstelanalyse voor Hyper-V-machines in VMM-clouds naar een secundair datacenter met Azure Site Recovery.
services: site-recovery
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: ponatara
ms.openlocfilehash: a586eac3be39a4d3fb35dff7a4b1cc40f32f2720
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="run-a-dr-drill-for-hyper-v-vms-to-a-secondary-site"></a>Uitvoeren van een DR-herstelanalyse voor Hyper-V-machines naar een secundaire site


Dit artikel wordt beschreven hoe u een detailanalyse van disaster recovery (DR) voor Hyper-V virtuele machines die worden beheerd in System Center Virtual Machine Manager V(MM) clouds, met een secundaire on-premises site, met behulp van [Azure Site Recovery](site-recovery-overview.md).

U een testfailover voor het valideren van uw replicatiestrategie voor uitvoeren en uitvoeren van een details voor Dr zonder verlies van gegevens of uitvaltijd. Een testfailover heeft geen invloed op de lopende replicatie of op uw productieomgeving. 

## <a name="how-do-test-failovers-work"></a>Hoe test failovers werk?

U kunt een testfailover uitvoeren vanaf de primaire naar de secundaire site. Als u controleren wilt of een virtuele machine failover wordt uitgevoerd, kunt u een testfailover uitvoeren zonder iets instellen op de secundaire site. Als u controleren of de app failover werkt wilt zoals verwacht, moet u voor het instellen van netwerk- en -infrastructuur in de secundaire locatie.
- U kunt een testfailover uitvoeren op een enkele virtuele machine of op een [herstelplan](site-recovery-create-recovery-plans.md).
- U kunt een testfailover zonder een netwerk uitvoeren met een bestaand netwerk of met een automatisch gemaakte netwerk. Meer informatie over deze opties zijn beschikbaar in de onderstaande tabel.
    - U kunt een testfailover zonder een netwerk uitvoeren. Deze optie is handig als u wilt dat een virtuele machine kon failover, maar het niet mogelijk om te controleren of alle netwerkconfiguratie controleren.
    - Voer de failover met een bestaand netwerk. U wordt aangeraden dat u niet een productienetwerk gebruikt.
    - De failover uitvoert en laat u Site Recovery automatisch maken van een testnetwerk. Site Recovery wordt in dit geval automatisch maken van het netwerk en opschonen wanneer de testfailover voltooid is.
- U moet een herstelpunt voor de testfailover selecteren: 
    - **Meest recente verwerkte**: deze optie wordt een virtuele machine overgenomen door de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
    - **Meest recente app-consistente**: deze optie failover een virtuele machine naar de meest recente toepassingsconsistente herstelpunt verwerkt door Site Recovery. 
    - **Meest recente**: deze optie eerst alle gegevens die door een herstelpunt maken voor elke VM voordat deze is mislukt via het is verzonden naar Site Recovery-service verwerkt. Deze optie biedt de laagste RPO (beoogd herstelpunt), omdat de virtuele machine gemaakt nadat de failover moeten de gegevens die zijn gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
    - **Meest recente multi-VM verwerkt**: beschikbaar voor herstelplannen die een of meer VM's die multi-VM consistentie ingeschakeld hebben bevatten. Virtuele machines met de instelling is ingeschakeld failover via naar de meest recente algemene multi-VM consistent herstelpunt. Andere VM's failover via naar de meest recente verwerkte herstelpunt.
    - **Meest recente multi-VM-app-consistente**: deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM consistentie ingeschakeld hebben. Virtuele machines die deel van een replicatiegroep uitmaken failover via naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt. Andere VM's failover via naar de meest recente toepassingsconsistente herstelpunt.
    - **Aangepaste**: Gebruik deze optie om een specifieke virtuele machine naar een bepaald herstelpunt een failover.



## <a name="prepare-networking"></a>Netwerken voorbereiden

Wanneer u een failovertest uitvoert, wordt u gevraagd netwerkinstellingen voor test-replica-machines selecteren zoals samengevat in de tabel.

**Optie** | **Details** 
--- | --- 
Geen | De virtuele testmachine wordt gemaakt op de host waarop de replica-VM zich bevindt. Het is niet toegevoegd aan de cloud en is niet verbonden met een netwerk.<br/><br/> Nadat deze is gemaakt, kunt u de machine verbinden met een VM-netwerk.
**Gebruik bestaande** | De virtuele testmachine wordt gemaakt op de host waarop de replica-VM zich bevindt. Dit is niet toegevoegd aan de cloud.<br/><br/>Maak een VM-netwerk dat is geïsoleerd van uw productienetwerk.<br/><br/>Als u een VLAN-netwerk gebruikt, raden wij u een afzonderlijke logische netwerk (niet gebruikt in productie) maken in VMM voor dit doel. Dit logische netwerk wordt gebruikt voor het maken van VM-netwerken voor testfailovers.<br/><br/>Het logische netwerk moet worden gekoppeld aan ten minste één van de netwerkadapters van alle Hyper-V-servers die als van virtuele machines host.<br/><br/>Voor logische netwerken met VLAN moeten de netwerksites die u aan het logische netwerk toevoegt worden geïsoleerd.<br/><br/>Als u een logisch netwerk voor op basis van Windows-Netwerkvirtualisatie, maakt Azure Site Recovery automatisch geïsoleerde VM-netwerken. 
**Een netwerk maken** | Een tijdelijke testnetwerk is automatisch gemaakt op basis van de instelling die u opgeeft in **logisch netwerk** en de bijbehorende netwerksites.<br/><br/> Failover wordt gecontroleerd dat de virtuele machines worden gemaakt. |U moet deze optie gebruiken als een herstelplan gebruikmaakt van meer dan één VM-netwerk.<br/><br/> Als u Windows-Netwerkvirtualisatie-netwerken gebruikt, kunt deze optie automatisch maken van VM-netwerken met dezelfde instellingen (subnetten en IP-adresgroepen) in het netwerk van de replica virtuele machine. Deze VM-netwerken zijn automatisch opgeschoond nadat de testfailover voltooid is.<br/><br/> De test virtuele machine is gemaakt op de host waarop de replica virtuele machine bestaat. Dit is niet toegevoegd aan de cloud.

### <a name="best-practices"></a>Aanbevolen procedures

- Testen van een productienetwerk zorgt ervoor dat de uitvaltijd voor productieworkloads. Vraag uw gebruikers niet te gebruiken van gerelateerde apps wanneer het herstel na noodgevallen inzoomen uitgevoerd wordt.
- Het testnetwerk hoeft niet overeen met het VMM logisch netwerktype gebruikt voor de testfailover. Maar bepaalde combinaties werken niet: - als de replica maakt gebruik van DHCP- en isolatie op basis van VLAN VM-netwerk voor de replica niet een statische IP-adresgroep moet. Met behulp van Windows-Netwerkvirtualisatie voor de testfailover werkt niet omdat geen-adresgroepen beschikbaar zijn. 
        -Test failover werkt niet als de replica-netwerk zonder isolatie, en het testnetwerk Windows-Netwerkvirtualisatie gebruikt. Dit is omdat het netwerk zonder isolatie beschikt niet over de subnetten die zijn vereist voor het maken van een Windows-Netwerkvirtualisatie-netwerk.
- Het is raadzaam dat u het netwerk die u hebt geselecteerd voor netwerktoewijzing voor testfailover niet gebruikt.
- Hoe gerepliceerde virtuele machines zijn verbonden met het VM-netwerken toegewezen nadat de failover is afhankelijk van hoe het VM-netwerk is geconfigureerd in de VMM-console.

### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>VM-netwerk is geconfigureerd met geen isolatie- of VLAN-isolatie

Als u een VM-netwerk is geconfigureerd in VMM met geen isolatie- of VLAN-isolatie, moet u rekening houden met het volgende:

- Als DHCP is gedefinieerd voor het VM-netwerk, wordt de replica virtuele machine verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk. De virtuele machine ontvangt het IP-adres van de beschikbare DHCP-server.
- U hoeft niet te definiëren van een statische IP-adresgroep voor de VM-netwerk van het doel. Als een statische IP-adresgroep wordt gebruikt voor het VM-netwerk, wordt de replica virtuele machine verbonden met de VLAN-ID via de instellingen die zijn opgegeven voor de netwerksite in het gekoppelde logische netwerk.
- De virtuele machine ontvangt het IP-adres van de groep die gedefinieerd voor het VM-netwerk. Als een statische IP-adresgroep is niet gedefinieerd voor het VM-netwerk van het doel, mislukt de toewijzing van IP-adres. De IP-adresgroep maken op de bron- en doel-VMM-servers die u voor beveiliging als herstel gebruiken wilt.

### <a name="vm-network-with-windows-network-virtualization"></a>VM-netwerk met Windows-Netwerkvirtualisatie

Als een VM-netwerk in VMM met Windows-Netwerkvirtualisatie is geconfigureerd, moet u rekening houden met het volgende:

- U moet een statische adresgroep voor de doel-VM-netwerk, ongeacht of de bron-VM-netwerk is geconfigureerd voor het gebruik van DHCP of een statisch IP-adresgroep definiëren. 
- Als u DHCP definieert, wordt de doel-VMM-server fungeert als een DHCP-server en biedt een IP-adres van de groep die gedefinieerd voor het VM-netwerk van het doel.
- Als het gebruik van een statische IP-adresgroep is gedefinieerd voor de bronserver, wijst de doel-VMM-server een IP-adres van de groep. In beide gevallen mislukt toewijzing van IP-adres als een statische IP-adresgroep is niet gedefinieerd.



## <a name="prepare-the-infrastructure"></a>De infrastructuur voorbereiden

Als u controleren wilt of een virtuele machine failover, kunt u een testfailover zonder een infrastructuur kunt uitvoeren. Als u een volledige details voor Dr app failover testen wilt, moet u de infrastructuur op de secundaire site voorbereiden:

- Als u een testfailover met een bestaande netwerk uitvoeren, bereid u Active Directory, DHCP en DNS-in dit netwerk.
- Als u een testfailover uitgevoerd met de optie voor het automatisch maken van een VM-netwerk, moet u de infrastructuurresources toevoegen aan de automatisch gemaakte netwerk, voordat u de testfailover uitvoert. In een herstelplan, kunt u dit vereenvoudigen door een handmatige stap voordat de groep 1 toe te voegen in het herstelplan die u gaat gebruiken voor de testfailover. Vervolgens de infrastructuurresources toevoegen aan de automatisch gemaakte netwerk voordat u de testfailover uitvoert.


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

1. Selecteer **herstelplannen** > *recoveryplan_name*. Klik op **Failover** > **Testfailover**.
2. Op de **Testfailover** blade opgeven hoe replica VMs moet zijn verbonden met netwerken na de testfailover.
3. Voortgang van de failover volgen op de **taken** tabblad.
4. Nadat de failover is voltooid, moet u controleren of de virtuele machines worden gestart.
5. Wanneer u bent klaar, klikt u op **testfailover opschonen** op het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. Hiermee verwijdert u alle virtuele machines en de netwerken die zijn gemaakt door Site Recovery tijdens de testfailover. 

![Testfailover](./media/hyper-v-vmm-test-failover/TestFailover.png)
 


> [!TIP]
> Het IP-adres aan een virtuele machine wordt opgegeven tijdens de testfailover is hetzelfde IP-adres waarop de virtuele machine wilt ontvangen voor een geplande of niet-geplande failover (ervan uitgaande dat het IP-adres beschikbaar in het testfailovernetwerk is). Als hetzelfde IP-adres is niet beschikbaar in het testfailovernetwerk, krijgt de virtuele machine een ander IP-adres dat beschikbaar is in het testfailovernetwerk.



### <a name="run-a-test-failover-to-a-production-network"></a>Een testfailover uitvoeren naar een productienetwerk

Het is raadzaam dat u een testfailover naar het herstel site productienetwerk die u hebt opgegeven tijdens de netwerktoewijzing niet uitvoert. Maar als u valideren, end-to-end netwerkverbinding in een VM-failover wilt, noteert u de volgende punten:

* Zorg ervoor dat de primaire virtuele machine wordt afgesloten wanneer u de testfailover uitvoert. Als u dit niet doet, worden twee virtuele machines met dezelfde identiteit tegelijkertijd uitgevoerd in hetzelfde netwerk. Deze situatie kan leiden tot ongewenste gevolgen.
* Alle wijzigingen die u in de testfailover aanbrengt VM's gaan verloren wanneer u de test failover virtuele machines opruimen. Deze wijzigingen worden niet gerepliceerd naar de primaire virtuele machines.
* Als volgt testen leidt tot uitvaltijd voor uw productietoepassing. Vraag gebruikers van de toepassing niet te gebruiken van de toepassing wanneer de details voor Dr uitgevoerd wordt.  


## <a name="next-steps"></a>Volgende stappen
Nadat u hebt een details voor Dr met succes uitgevoerd, kunt u [een volledige failover uitvoert](site-recovery-failover.md).



