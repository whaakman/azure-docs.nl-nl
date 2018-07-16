---
title: Testen van failover naar Azure in Azure Site Recovery | Microsoft Docs
description: Meer informatie over het uitvoeren van een testfailover van on-premises naar Azure met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 55fc1bf9d59c82abc76e40e834f67aa49942db44
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056695"
---
# <a name="test-failover-to-azure-in-site-recovery"></a>Testfailover naar Azure in Site Recovery


In dit artikel wordt beschreven hoe u een Dr-herstelanalyse uitvoeren naar Azure met behulp van een testfailover van Site Recovery.  

U voert een testfailover uit voor het valideren van de replicatie en de strategie voor noodherstel, zonder verlies van gegevens of downtime. Een testfailover niet invloed hebben op de doorlopende replicatie of uw productieomgeving. U kunt een testfailover uitvoeren op een specifieke virtuele machine (VM), of op een [herstelplan](site-recovery-create-recovery-plans.md) met meerdere VM's. 


## <a name="run-a-test-failover"></a>Een testfailover uitvoeren
Deze procedure wordt beschreven hoe u een testfailover uitvoeren voor een herstelplan uitvoeren. 

![Failover testen](./media/site-recovery-test-failover-to-azure/TestFailover.png)


1. In Site Recovery in Azure portal, klikt u op **herstelplannen** > *recoveryplan_name* > **Testfailover**.
2. Selecteer een **herstelpunt** waarnaar u failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
    - **Laatst verwerkte**: deze optie wordt over alle virtuele machines in het plan naar het meest recente herstelpunt dat is verwerkt door Site Recovery. Raadpleeg het meest recente herstelpunt voor een specifieke virtuele machine, **laatste herstelpunten** in de instellingen van de virtuele machine. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
    - **Laatste toepassingsconsistente**: deze optie wordt op alle virtuele machines in het plan naar de meest recente toepassingsconsistente herstelpunt verwerkt door Site Recovery. Raadpleeg het meest recente herstelpunt voor een specifieke virtuele machine, **laatste herstelpunten** in de instellingen van de virtuele machine. 
    - **Meest recente**: deze optie eerst alle gegevens die is verzonden naar Site Recovery-service te maken van een herstelpunt voor elke virtuele machine voordat de failover wordt uitgevoerd naar het verwerkt. Deze optie biedt het laagste RPO (Recovery Point Objective), omdat de virtuele machine gemaakt nadat de failover heeft de gegevens die zijn gerepliceerd naar de Site Recovery wanneer de failover werd geactiveerd.
    - **Meest recente verwerkte multi-VM**: deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM's is ingeschakeld. Virtuele machines met de instelling is ingeschakeld, schakelt over naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt uitgevoerd. Andere VM's een failover naar de meest recente verwerkte herstelpunt.  
    - **Nieuwste multi-VM-app-consistente**: deze optie is beschikbaar voor herstelplannen met een of meer VM's die multi-VM's is ingeschakeld. Virtuele machines die deel van een replicatiegroep uitmaken schakelt over naar de meest recente algemene multi-VM toepassingsconsistente herstelpunt. Andere VM's een failover naar de meest recente toepassingsconsistente herstelpunt. 
    - **Aangepaste**: Gebruik deze optie om de failover van een specifieke virtuele machine naar een bepaald herstelpunt.
3. Selecteer een Azure-netwerk waarin de test-VM's worden gemaakt.

    - Site Recovery probeert te maken van test-VM's in een subnet met dezelfde naam en hetzelfde IP-adres als die beschikbaar is in de **berekening en netwerk** instellingen van de virtuele machine.
    - Als een subnet met dezelfde naam is niet beschikbaar in de Azure-netwerk gebruikt voor test-failover, klikt u vervolgens de test virtuele machine wordt gemaakt in het eerste subnet op alfabetische volgorde.
    - Als hetzelfde IP-adres is niet beschikbaar in het subnet, ontvangt de virtuele machine nog een beschikbaar IP-adres in het subnet. [Meer informatie](#create-a-network-for-test-failover).
4. Als u bent Failover-overschakeling uitvoeren naar Azure en gegevensversleuteling is ingeschakeld, in **versleutelingssleutel**, selecteer het certificaat dat is uitgegeven wanneer u versleuteling tijdens de installatie van de Provider ingeschakeld. U kunt deze stap overslaan versleuteling is niet ingeschakeld.
5. Voortgang van de failover volgen op de **taken** tabblad. U zou het mogelijk om te zien van de test replica-machine in Azure portal.
6. Als u wilt een RDP-verbinding met de Azure-VM hebt gestart, moet u [een openbaar IP-adres toevoegen](https://aka.ms/addpublicip) op de netwerkinterface van de failover VM. 
7. Als alles werkt zoals verwacht, klikt u op **failovertest**. Hiermee verwijdert u de virtuele machines die zijn gemaakt tijdens de testfailover.
8. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op. 


![Failover testen](./media/site-recovery-test-failover-to-azure/TestFailoverJob.png)

Wanneer een test-failover wordt geactiveerd, gebeurt het volgende:

1. **Vereisten**: een controle uitgevoerd om ervoor te zorgen dat alle vereiste voorwaarden voor failover wordt voldaan.
2. **Failover**: de failover verwerkt en de gegevens voorbereid, zodat een Azure-VM kan worden gemaakt op basis van deze.
3. **Meest recente**: als u de meest recente herstelpunt hebt gekozen, een herstelpunt wordt gemaakt van de gegevens die zijn verzonden naar de service.
4. **Start**: deze stap maakt u een virtuele machine van Azure met behulp van de gegevens verwerkt in de vorige stap.

### <a name="failover-timing"></a>Timing van failover

In de volgende scenario's vereist de failover een extra tussenstap die meestal ongeveer 8 tot en met 10 minuten duurt om te voltooien:

* Virtuele VMware-machines met een versie van de Mobility-service die ouder zijn dan 9,8
* Fysieke servers
* VMware Linux-VM 's
* Hyper-V-VM beveiligd als fysieke servers
* VMware-VM waarbij opstartstuurprogramma's zijn niet in de volgende stuurprogramma's:
    * storvsc
    * VMBus
    * storflt
    * Intelide
    * ATAPI
* VMware-VM die geen DHCP ingeschakeld hebben, ongeacht of ze zijn gebruikmaakt van DHCP- of statische IP-adressen.

In alle andere gevallen, geen tussenstap is niet vereist en failover aanzienlijk minder tijd in beslag neemt.


## <a name="create-a-network-for-test-failover"></a>Maken van een netwerk voor test-failover

Het wordt aangeraden dat voor de testfailover, u een netwerk dat is geïsoleerd van de productie-site-herstelnetwerk specifieke in de **berekening en netwerk** instellingen voor elke virtuele machine. Standaard bij het maken van een Azure-netwerk, is het geïsoleerd van andere netwerken. Het testnetwerk moet uw productienetwerk nabootsen:

- Het testnetwerk moet hetzelfde aantal subnetten als uw productienetwerk hebben. Subnetten moeten dezelfde naam hebben.
- Het testnetwerk moet hetzelfde IP-adresbereik gebruiken.
- De DNS-server van het testnetwerk bijwerken met het IP-adres dat is opgegeven voor de DNS-VM in **berekening en netwerk** instellingen. Lezen [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations) voor meer informatie.


## <a name="test-failover-to-a-production-network-in-the-recovery-site"></a>Een failovertest uitvoeren naar een productienetwerk in de site recovery

Hoewel het wordt aangeraden dat u een testnetwerk gescheiden van uw productienetwerk gebruiken als u wilt testen van een Dr-herstelanalyse naar uw productienetwerk, Let op het volgende: 

- Zorg ervoor dat de primaire virtuele machine wordt afgesloten wanneer u de testfailover uitvoert. Anders zullen er twee virtuele machines met dezelfde identiteit, in hetzelfde netwerk wordt uitgevoerd op hetzelfde moment. Dit kan leiden tot onverwachte gevolgen.
- Eventuele wijzigingen in virtuele machines die zijn gemaakt voor test-failover gaan verloren als u de failover opschonen. Deze wijzigingen worden niet gerepliceerd naar de primaire virtuele machine.
- Testen in uw productieomgeving leidt tot een uitvaltijd van uw productietoepassing. Gebruikers al dan niet mogen gebruiken apps die op virtuele machines worden uitgevoerd wanneer de testfailover uitgevoerd wordt.  



## <a name="prepare-active-directory-and-dns"></a>Voorbereiden van Active Directory en DNS

Als u wilt uitvoeren van een testfailover uitvoeren voor de toepassing testen, moet u een kopie van uw Active Directory-omgeving voor productie in uw testomgeving. Lezen [testfailover-overwegingen voor Active Directory](site-recovery-active-directory.md#test-failover-considerations) voor meer informatie.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Als u verbinding maken met virtuele Azure-machines na een failover met RDP wilt, volgt u de vereisten die worden samengevat in de tabel.

**Failover** | **Locatie** | **Acties**
--- | --- | ---
**Virtuele Azure-machine waarop Windows wordt uitgevoerd** | On-premises computer voordat de failover | Voor toegang tot de Azure-VM via internet, schakelt u RDP in en zorg ervoor dat TCP en UDP-regels zijn toegevoegd voor **openbare**, en die RDP is toegestaan voor alle profielen in **Windows Firewall**  >  **Toegestane Apps**.<br/><br/> Voor toegang tot de Azure-VM via een site-naar-site-verbinding, schakelt u RDP in op de machine en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en functies**, voor **Domein en privé** netwerken.<br/><br/>  Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135).<br/><br/> Zorg ervoor dat er geen Windows-updates in behandeling zijn op de virtuele machine wanneer u een failover activeert. Windows update kan worden gestart wanneer u een failover uitvoert, en kunt u zich niet aanmelden bij de virtuele machine totdat de update is voltooid. 
**Virtuele Azure-machine waarop Windows wordt uitgevoerd** | Virtuele Azure-machine na een failover |  [Voeg een openbaar IP-adres toe](https://aka.ms/addpublicip) voor de VM.<br/><br/> De regels voor netwerkbeveiligingsgroepen op de virtuele machine (en de Azure-subnet waarmee deze is verbonden) moeten binnenkomende verbindingen met de RDP-poort worden toegestaan.<br/><br/> Controleer **diagnostische gegevens over opstarten** om te controleren of een schermopname van de virtuele machine.<br/><br/> Als u geen verbinding kunt maken, controleert u dat de virtuele machine wordt uitgevoerd, en bekijk deze [tips voor probleemoplossing](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).
**Virtuele Azure-machine waarop Linux wordt uitgevoerd** | On-premises computer voordat de failover | Zorg ervoor dat de Secure Shell-service op de virtuele machine is ingesteld op automatisch wordt gestart bij het opstarten van het systeem.<br/><br/> Controleer of er in de firewallregels is ingesteld dat SSH-verbindingen zijn toegestaan.
**Virtuele Azure-machine waarop Linux wordt uitgevoerd** | Virtuele Azure-machine na een failover | De regels voor netwerkbeveiligingsgroepen op de virtuele machine (en de Azure-subnet waarmee deze is verbonden) moeten binnenkomende verbindingen aan de SSH-poort worden toegestaan.<br/><br/> [Voeg een openbaar IP-adres toe](https://aka.ms/addpublicip) voor de VM.<br/><br/> Controleer **diagnostische gegevens over opstarten** voor een schermopname van de virtuele machine.<br/><br/>



## <a name="next-steps"></a>Volgende stappen
Nadat u een Dr-herstelanalyse hebt voltooid, meer informatie over andere typen [failover](site-recovery-failover.md).
