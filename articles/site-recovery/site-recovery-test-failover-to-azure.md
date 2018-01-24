---
title: Testen van failover naar Azure in Site Recovery | Microsoft Docs
description: Informatie over het uitvoeren van een testfailover van on-premises naar Azure
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
ms.date: 10/16/2017
ms.author: pratshar
ms.openlocfilehash: a4555b1cc758e2d4bdd11a16776dc3bb209adee8
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="test--failover-to-azure-in-site-recovery"></a>Testen van Failover naar Azure in Site Recovery



Dit artikel wordt beschreven hoe u een herstel na noodgevallen detailanalyse uitvoert naar Azure, met een testfailover van Site Recovery.  

U uitvoeren een testfailover voor het valideren van de replicatie en de strategie voor noodherstel zonder verlies van gegevens of uitvaltijd. Heeft geen invloed op een testfailover lopende replicatie of uw productieomgeving. U kunt een testfailover uitvoeren op een specifieke virtuele machine (VM) of op een [herstelplan](site-recovery-create-recovery-plans.md) met meerdere virtuele machines. 


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren
Deze procedure wordt beschreven hoe u een testfailover voor een herstelplan uitvoeren. 

![Failover testen](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. Klik in de Site Recovery in de Azure portal, op **herstelplannen** > *recoveryplan_name* > **Testfailover**.
2. Selecteer een **herstelpunt** waarvoor een failover. U kunt een van de volgende opties gebruiken:
    - **Meest recente verwerkte**: deze optie wordt overgenomen alle VM's in het plan naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Raadpleeg de meest recente herstelpunt voor een specifieke virtuele machine, **herstelpunten van de meest recente** in de VM-instellingen. Deze optie biedt een laag RTO (beoogde hersteltijd), omdat er geen tijd besteed aan het verwerken van niet-verwerkte gegevens.
    - **Meest recente app-consistente**: deze optie wordt overgenomen alle VM's in het plan naar de meest recente toepassingsconsistente herstelpunt verwerkt door Site Recovery. Raadpleeg de meest recente herstelpunt voor een specifieke virtuele machine, **herstelpunten van de meest recente** in de VM-instellingen. 
    - **Meest recente**: deze optie eerst alle gegevens die door een herstelpunt maken voor elke VM voordat deze is mislukt via het is verzonden naar Site Recovery-service verwerkt. Deze optie biedt de laagste RPO (beoogd herstelpunt), omdat de virtuele machine gemaakt nadat de failover moeten de gegevens die zijn gerepliceerd naar de Site Recovery wanneer de failover is geactiveerd.
    - **Meest recente multi-VM verwerkt**: deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM consistentie ingeschakeld hebben. Virtuele machines met de instelling is ingeschakeld failover via naar de meest recente algemene multi-VM consistent herstelpunt. Andere VM's failover via naar de meest recente verwerkte herstelpunt.  
    - **Meest recente multi-VM-app-consistente**: deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM consistentie ingeschakeld hebben. Virtuele machines die deel van een replicatiegroep uitmaken failover via naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt. Andere VM's failover via naar de meest recente toepassingsconsistente herstelpunt. 
    - **Aangepaste**: Gebruik deze optie om een specifieke virtuele machine naar een bepaald herstelpunt een failover.
3. Selecteer een Azure-netwerk waarin de test-virtuele machines worden gemaakt.

    - Site herstelpogingen maken VM's testen in een subnet met dezelfde naam en hetzelfde IP-adres als die beschikbaar is in de **berekening en netwerk** instellingen van de virtuele machine.
    - Als een subnet met dezelfde naam is niet beschikbaar in het Azure-netwerk dat is gebruikt voor de testfailover, wordt de test VM gemaakt in het eerste subnet alfabetische volgorde.
    - Als hetzelfde IP-adres niet beschikbaar in het subnet is, ontvangt de virtuele machine een ander beschikbaar IP-adres in het subnet. [Meer informatie](#creating-a-network-for-test-failover).
4. Als u bent failover wordt uitgevoerd naar Azure en gegevensversleuteling is ingeschakeld, in **versleutelingssleutel**, selecteert u het certificaat dat was uitgegeven wanneer u tijdens de installatie van de Provider is ingeschakeld. U kunt deze stap overslaan versleuteling is niet ingeschakeld.
5. Voortgang van de failover volgen op de **taken** tabblad. U moet mogelijk zijn om te zien van de test replica-machine in de Azure portal.
6. Voor het initiëren van een RDP-verbinding met de Azure VM, moet u [toevoegen van een openbaar IP-adres](site-recovery-monitoring-and-troubleshooting.md) op de netwerkinterface failover VM. 
7. Als alles werkt zoals verwacht, klikt u op **opschonen testfailover**. Hiermee verwijdert u de virtuele machines die zijn gemaakt tijdens de testfailover.
8. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. 


![Failover testen](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Wanneer een testfailover wordt geactiveerd, gebeurt het volgende:

1. **Vereisten**: controle van een vereisten wordt uitgevoerd om ervoor te zorgen dat alle voorwaarden die vereist is voor failover wordt voldaan.
2. **Failover**: de failover verwerkt en bereid de gegevens, zodat een virtuele machine van Azure kan worden gemaakt vanuit het.
3. **Meest recente**: als u de meest recente herstelpunt hebt gekozen, een herstelpunt wordt gemaakt van de gegevens die zijn verzonden naar de service.
4. **Start**: deze stap maakt u een virtuele machine van Azure met behulp van de gegevens die worden verwerkt in de vorige stap.

### <a name="failover-timing"></a>Timing van failover

In de volgende scenario's vereist failover een extra tussenstap die meestal ongeveer 8 tot en met 10 minuten duurt om te voltooien:

* Virtuele VMware-machines met een versie van de Mobility-service die ouder zijn dan 9,8
* Fysieke servers
* Virtuele VMware-Linux-machines
* Hyper-V VM beveiligd als fysieke servers
* VMware VM waarop opstartstuurprogramma's zijn niet in de volgende stuurprogramma's:
    * storvsc
    * vmbus
    * storflt
    * Intelide
    * atapi
* VMware VM die geen DHCP ingeschakeld, rrespective van of ze zijn geïnstalleerd via DHCP of statische IP-adressen.

In alle andere gevallen is er geen tussenliggende stap is niet vereist en failover aanzienlijk minder tijd nodig.


## <a name="create-a-network-for-test-failover"></a>Maken van een netwerk voor testfailover

Het is raadzaam dat voor de testfailover, u een netwerk dat is geïsoleerd van het herstel site productienetwerk specifieke in de **berekening en netwerk** instellingen voor elke virtuele machine. Standaard, wanneer u een Azure-netwerk maken is geïsoleerd van andere netwerken. Het testnetwerk moet uw productienetwerk nabootsen:

- Het testnetwerk moet hetzelfde aantal subnetten als uw productienetwerk hebben. Subnetten moeten dezelfde naam hebben.
- Het testnetwerk moet het dezelfde IP-adres rangek gebruiken.
- De DNS-server van het testnetwerk bijwerken met het IP-adres opgegeven voor de DNS-VM in **berekening en netwerk** instellingen. Lees [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations) voor meer informatie.


## <a name="test-failover-to-a-production-network-in-the-recovery-site"></a>Failover naar een productienetwerk te testen in de site recovery

Hoewel wordt aanbevolen dat u een afzonderlijk van uw productienetwerk testnetwerk als u wilt testen een detailanalyse van het herstel na noodgevallen in uw productienetwerk, Let op het volgende: 

- Zorg ervoor dat de primaire virtuele machine wordt afgesloten tijdens het uitvoeren van de testfailover. Otherewise er worden twee virtuele machines met dezelfde identiteit in hetzelfde netwerk worden uitgevoerd op hetzelfde moment. Dit kan leiden tot onverwachte gevolgen.
- Eventuele wijzigingen in virtuele machines die zijn gemaakt voor een testfailover gaan verloren wanneer u de failover opschonen. Deze wijzigingen worden niet gerepliceerd naar de primaire virtuele machine.
- Testen in uw productieomgeving leidt tot een uitvaltijd van uw productietoepassing. Gebruikers niet mogen gebruiken apps die worden uitgevoerd op virtuele machines als de testfailover uitgevoerd wordt.  



## <a name="prepare-active-directory-and-dns"></a>Active Directory en DNS voorbereiden

Voor het uitvoeren van een testfailover voor de toepassing testen, moet u een kopie van uw productieomgeving Active Directory in uw testomgeving. Lees [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations) voor meer informatie.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Als u verbinding maken met virtuele Azure-machines met RDP na een failover wilt, volgt u de vereisten die in de tabel samengevat.

**Failover** | **Locatie** | **Acties**
--- | --- | ---
**Azure VM waarop Windows wordt uitgevoerd** | Lokale machine vóór de failover | Voor toegang tot de Azure VM via internet, schakelt u RDP en zorg ervoor dat TCP en UDP-regels worden toegevoegd voor **openbare**, en dat is toegestaan voor alle profielen in RDP **Windows Firewall**  >  **Apps toegestaan**.<br/><br/> Voor toegang tot de Azure VM via een site-naar-site-verbinding, schakelt u RDP in op de machine en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en functies**, voor **Domein- en persoonlijke** netwerken.<br/><br/>  Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135).<br/><br/> Zorg ervoor dat er zijn geen Windows-updates in behandeling op de virtuele machine wanneer u een failover activeren. Windows update mogelijk gestart wanneer u een failover, en u niet aanmelden bij de virtuele machine totdat de update is voltooid. 
**Azure VM waarop Windows wordt uitgevoerd** | Azure virtuele machine na een failover |  [Een openbaar IP-adres toevoegen](site-recovery-monitoring-and-troubleshooting.md) voor de virtuele machine.<br/><br/> De netwerkbeveiligingsgroepen op failover VM (en de Azure-subnet waarmee deze is verbonden) moeten het toestaan van binnenkomende verbindingen voor de RDP-poort.<br/><br/> Controleer **opstarten diagnostics** om te controleren of een schermopname van de virtuele machine.<br/><br/> Als u geen verbinding maken, controleert u dat de virtuele machine wordt uitgevoerd en deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).
**Azure VM waarop Linux wordt uitgevoerd** | Lokale machine vóór de failover | Zorg ervoor dat de Secure Shell-service op de virtuele machine is ingesteld op automatisch starten op opstarten van het systeem.<br/><br/> Controleer of er in de firewallregels is ingesteld dat SSH-verbindingen zijn toegestaan.
**Azure VM waarop Linux wordt uitgevoerd** | Azure virtuele machine na een failover | De netwerkbeveiligingsgroepen op failover VM (en de Azure-subnet waarmee deze is verbonden) moeten het toestaan van binnenkomende verbindingen voor de SSH-poort.<br/><br/> [Een openbaar IP-adres toevoegen](site-recovery-monitoring-and-troubleshooting.md) voor de virtuele machine.<br/><br/> Controleer **opstarten diagnostics** voor een schermopname van de virtuele machine.<br/><br/>



## <a name="next-steps"></a>Volgende stappen
Nadat u een herstel na noodgevallen detailanalyse hebt voltooid, meer informatie over andere typen [failover](site-recovery-failover.md).
