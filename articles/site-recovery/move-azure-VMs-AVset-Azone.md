---
title: Azure IaaS-VM's als aan een zone vastgemaakte VM's verplaatsen naar een andere Azure-regio met behulp van de Azure Site Recovery-service | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS-VM's als aan een zone vastgemaakte VM's te verplaatsen naar een andere Azure-regio.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: b6107211f49978bbacd1a827a9adc37ccef60a5b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57855405"
---
# <a name="move-azure-vms-into-availability-zones"></a>Virtuele Azure-machines verplaatsen naar beschikbaarheidszones
Beschikbaarheidszones in Azure kunt uw toepassingen en gegevens beschermen tegen storingen in datacenters. Elke beschikbaarheidszone bestaat uit een of meer datacenters die zijn voorzien van een onafhankelijke stroomvoorziening, koeling en netwerk. Om voor tolerantie te zorgen, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding tussen Beschikbaarheidszones binnen een regio beschermt toepassingen en gegevens tegen storingen in datacenters. Met Beschikbaarheidszones biedt Azure een service level agreement (SLA) van 99,99% voor actieve tijdsduur van virtuele machines (VM's). Beschikbaarheidszones worden ondersteund in bepaalde regio's, zoals vermeld in [wat zijn Beschikbaarheidszones in Azure?](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones).

In een scenario waarbij uw virtuele machines worden geïmplementeerd als *één exemplaar* in een bepaalde regio, en u de beschikbaarheid verbeteren, doordat u deze virtuele machines verplaatst naar een Beschikbaarheidszone wilt gebruiken, kunt u doen met behulp van Azure Site Recovery. Deze actie kan verder worden onderverdeeld in:

- Verplaatsen van virtuele machines van één exemplaar in Beschikbaarheidszones in een doelregio
- Virtuele machines in een beschikbaarheidsset in Beschikbaarheidszones in een doelregio verplaatsen

> [!IMPORTANT]
> Op dit moment Azure Site Recovery ondersteunt de verplaatsing van virtuele machines van de ene regio naar een andere, maar biedt geen ondersteuning voor het verplaatsen binnen een regio.

## <a name="check-prerequisites"></a>Controle van vereisten

- Controleer of de doelregio heeft [ondersteuning voor Beschikbaarheidszones](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones). Controleer of uw eigen keuze aan [bron regio/doel regio combinatie wordt ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support). Moet u een gefundeerde beslissing nemen over de doelregio.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).
- Controleer de machtigingen van de account. Als u uw gratis Azure-account hebt gemaakt, u kunt de beheerder van uw abonnement. Als u niet de beheerder van abonnement, kunt u werken met de beheerder vragen om de machtigingen die u nodig hebt. Replicatie inschakelen voor een virtuele machine en uiteindelijk gegevens kopiëren naar het doel met behulp van Azure Site Recovery, moet u het volgende hebben:

    1. Machtiging voor het maken van een virtuele machine in Azure-resources. De *Inzender voor virtuele machines* ingebouwde rol beschikt over deze machtigingen, waaronder:
        - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
        - Machtiging voor het maken van een virtuele machine in het geselecteerde virtuele netwerk
        - Machtigingen om naar het geselecteerde opslagaccount te schrijven

    2. Machtiging voor het beheren van Azure Site Recovery-taken. De *Site Recovery-Inzender* rol bevat alle machtigingen die zijn vereist voor het beheren van Site Recovery-acties in een Recovery Services-kluis.

## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

1. Uw virtuele machines moeten beheerde schijven gebruiken als u verplaatsen naar een Beschikbaarheidszone wilt met behulp van Site Recovery. U kunt bestaande Windows-VM's die gebruikmaken van niet-beheerde schijven gebruiken van beheerde schijven converteren. Volg de stappen in [een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks). Zorg ervoor dat de beschikbaarheidsset is geconfigureerd als *beheerd*.
2. Controleer of de meest recente basiscertificaten aanwezig zijn op de Azure-VM’s die u wilt verplaatsen. Als de meest recente basiscertificaten niet aanwezig zijn, kan het kopiëren van gegevens naar de doelregio kan niet worden ingeschakeld vanwege veiligheidsbeperkingen.

3. Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een omgeving zonder verbinding, volgt u de standaard Windows update en het certificaat standaardprocedures voor het bijwerken van uw organisatie.

4. Volg de richtlijnen bij uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de certificaatintrekkingslijst op de virtuele machine voor Linux-VM's.
5. Zorg ervoor dat u niet een verificatieproxy gebruikt voor het beheren van verbinding met het netwerk voor virtuele machines die u wilt verplaatsen.

6. Als de virtuele machine die u wilt verplaatsen, heeft geen toegang tot het internet en een firewallproxy gebruikt voor het beheren van uitgaande toegang, controleert u de vereisten die aan [ uitgaande netwerkconnectiviteit configureren](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

7. Identificeer de bron-indeling voor netwerken en de resources die u momenteel voor verificatie gebruikt, zoals netwerktaakverdelers, nsg's en openbaar IP-adres.

## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer dat uw Azure-abonnement kunt u virtuele machines maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Indien nodig, neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Als u Site Recovery gebruiken om gegevens te kopiëren naar het doel, hervat het even groot is of de dichtstbijzijnde grootte voor de doel-VM.

3. Maak een doelbron voor elk onderdeel geïdentificeerd in de bron-indeling voor netwerken. Deze actie zorgt ervoor dat als u naar de doelregio knippen, uw virtuele machines hebben de functionaliteit en functies die u in de bron hebt.

    > [!NOTE]
    > Azure Site Recovery wordt automatisch gedetecteerd en maakt u een virtueel netwerk- en storage-account als u replicatie voor de bron-VM schakelt. U kunt ook vooraf maken deze resources en toewijzen aan de virtuele machine als onderdeel van de stap van de replicatie inschakelen. Maar voor alle andere resources, zoals vermeld later, moet u ze handmatig maken in de doelregio.

     De volgende documenten vertellen hoe ze moeten de meest gebruikte netwerkresources die relevant voor u zijn, op basis van de bron-VM-configuratie maken.

    - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Voor andere netwerkonderdelen, raadpleegt u de netwerkondersteuning [documentatie](https://docs.microsoft.com/azure/#pivot=products&panel=network).

    > [!IMPORTANT]
    > Zorg ervoor dat u een zone-redundante load balancer in het doel. U vindt meer informatie in [Standard Load Balancer en Beschikbaarheidszones](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones).

4. Handmatig [maken van een niet-productie-netwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio als u testen van de configuratie wilt voordat u van naar de doelregio overzetten. We raden deze benadering omdat het ervoor zorgt slechts een minimale interferentie met de productie-omgeving dat.

## <a name="enable-replication"></a>Replicatie inschakelen
De volgende stappen uit begeleidt u bij het gebruik van Azure Site Recovery voor replicatie van gegevens naar de doelregio inschakelen voordat u ze uiteindelijk in Beschikbaarheidszones verplaatst.

> [!NOTE]
> Deze stappen zijn voor een enkele virtuele machine. U kunt dezelfde uitbreiden naar meerdere virtuele machines. Ga naar de Recovery Services-kluis, selecteer **+ repliceren**, en selecteer de relevante virtuele machines samen.

1. Selecteer in de Azure portal, **virtuele machines**, en selecteer de virtuele machine die u wilt verplaatsen in Beschikbaarheidszones.
2. In **Operations**, selecteer **herstel na noodgevallen**.
3. In **herstel na noodgevallen configureren** > **doelregio**, selecteert u de doelregio waarnaar u wilt repliceren. Zorg ervoor dat deze regio [ondersteunt](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) Beschikbaarheidszones.

    ![Selectie van doelregio](media/azure-vms-to-zones/enable-rep-1.PNG)

4. Selecteer **Volgende: Geavanceerde instellingen**.
5. Kies de juiste waarden voor het doelabonnement, de doelresourcegroep van de virtuele machine en het virtuele netwerk.
6. In de **beschikbaarheid** sectie, kiest u de Beschikbaarheidszone waarnaar u wilt verplaatsen van de virtuele machine. 
   > [!NOTE]
   > Als u de optie voor de beschikbaarheidsset of beschikbaarheid Zone niet ziet, zorg ervoor dat de [vereisten](#prepare-the-source-vms) wordt voldaan en de [voorbereiding](#prepare-the-source-vms) van bron-VM's is voltooid.
  
    ![Selecties voor het kiezen van een Beschikbaarheidszone](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Selecteer **Replicatie inschakelen**. Deze actie wordt een taak replicatie inschakelen voor de virtuele machine gestart.

## <a name="check-settings"></a>Instellingen controleren

Nadat de replicatietaak is voltooid, kunt u de replicatiestatus controleren, de replicatie-instellingen wijzigen en de implementatie testen.

1. Selecteer in het menu VM **herstel na noodgevallen**.
2. U kunt de replicatiestatus, de herstelpunten die zijn gemaakt en de bron en doelregio's op de kaart controleren.

   ![Replicatiestatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>De configuratie testen

1. Selecteer in het menu aan de virtuele machine **herstel na noodgevallen**.
2. Selecteer de **Testfailover** pictogram.
3. In **Testfailover**, selecteer een herstelpunt dat moet worden gebruikt voor de failover:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er sprake is van een lage RTO (Recovery Time Objective).
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM's naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het Azure-doeltestnetwerk waar u de Azure VM's naartoe wilt verplaatsen om de configuratie te testen. 

    > [!IMPORTANT]
    > U wordt aangeraden dat u een afzonderlijke Azure-VM-netwerk gebruiken voor de test mislukt en niet het productienetwerk in de doelregio waarnaar u wilt verplaatsen van uw virtuele machines.

4. Selecteer eerst de verplaatsing testen **OK**. Om de voortgang bijhouden, selecteert u de virtuele machine om de eigenschappen te openen. Of u kunt selecteren de **Testfailover** taak in de kluisnaam > **instellingen** > **taken** > **voorSiteRecovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Als u wilt verwijderen van de virtuele machine gemaakt als onderdeel van de verplaatsing testen, selecteert u **failovertest** op het gerepliceerde item. Leg in **Notities** eventuele opmerkingen over de test vast en sla deze op.

## <a name="move-to-the-target-region-and-confirm"></a>Ga naar de doelregio en bevestigen

1.  Selecteer in het menu aan de virtuele machine **herstel na noodgevallen**.
2. Selecteer de **Failover** pictogram.
3. Bij **Failover** selecteert u **Meest recente**. 
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**. 
5. Nadat de taak is voltooid, controleert u de virtuele machine wordt weergegeven in de doel-Azure-regio, zoals verwacht.
6. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Doorvoeren**. De verplaatsing naar de doelregio wordt voltooid. Wacht totdat de taak doorvoeren is voltooid.

## <a name="discard-the-resource-in-the-source-region"></a>Verwijder de resource in de bronregio

Ga naar de virtuele machine. Selecteer **replicatie uitschakelen**. Deze actie wordt het proces van het kopiëren van de gegevens voor de virtuele machine gestopt.  

> [!IMPORTANT]
> Voer de vorige stap om te voorkomen dat oninbaar voor Site Recovery-replicatie na de verplaatsing. De bronreplicatie-instellingen worden automatisch opgeschoond. Houd er rekening mee dat de extensie voor Site Recovery die is geïnstalleerd als onderdeel van de replicatie wordt niet verwijderd en moet handmatig worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, kunt u de beschikbaarheid van een Azure-VM verhoogd door te verplaatsen naar een beschikbaarheidsset of binnen een Beschikbaarheidszone. U kunt nu herstel na noodgevallen instellen voor de verplaatste virtuele machine.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)


