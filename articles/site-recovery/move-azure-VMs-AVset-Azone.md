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
ms.openlocfilehash: 5597f3c017ccf2dbb58b7b6b046720c8f49803c5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312281"
---
# <a name="move-azure-vms-into-availability-zones"></a>Virtuele Azure-machines verplaatsen naar beschikbaarheidszones
Beschikbaarheidszones in Azure beschermen uw toepassingen en gegevens tegen storingen in datacenters. Elke beschikbaarheidszone bestaat uit een of meer datacenters die zijn voorzien van een onafhankelijke stroomvoorziening, koeling en netwerk. Om voor tolerantie te zorgen, is er een minimum van drie afzonderlijke zones in alle ingeschakelde regio's. De fysieke scheiding tussen beschikbaarheidszones binnen een Azure-regio beschermt toepassingen en gegevens tegen storingen op zoneniveau. Met beschikbaarheidszones biedt Azure de beste uptime SLA voor VM’s van de branche, van 99,99%. Beschikbaarheidszones worden ondersteund in bepaalde regio's, zoals [hier](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) vermeld. 

In een scenario waarin u uw virtuele machines als één instantie in een bepaalde regio hebt vermeld, en u de beschikbaarheid wilt verbeteren door deze te verplaatsen naar een beschikbaarheidszone, kunt u dit doen met behulp van Azure Site Recovery. Dit kan verder worden onderverdeeld in:

- Verplaatsen van VM’s van één exemplaar naar beschikbaarheidszones in een doelregio
- Verplaatsen van VM’s naar beschikbaarheidszones in een doelregio

> [!IMPORTANT]
> Momenteel ondersteunt Azure Site Recovery de verplaatsing van VM’s van de ene regio naar een andere regio en biedt het geen ondersteuning voor het verplaatsen binnen een regio. 

## <a name="verify-prerequisites"></a>Vereisten verifiëren

- Controleer of de doelregio [ondersteuning voor beschikbaarheidszone](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones) heeft - controleer of uw [combinatie van bronregio en doelregio wordt ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) en neem een gefundeerde beslissing over de doelregio.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).
- Accountmachtigingen controleren: Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te schakelen voor een VM en uiteindelijk gegevens te kopiëren naar de doelserver met behulp van Azure Site Recovery, moet u het volgende hebben:

    1. Machtigingen voor het maken van een virtuele machine in Azure-resources. De ingebouwde rol 'Inzender voor virtuele machines' beschikt over deze machtigingen, waaronder:
        - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
        - Machtiging voor het maken van een virtuele machine in het geselecteerde virtuele netwerk
        - Machtigingen om naar het geselecteerde opslagaccount te schrijven

    2. U moet ook zijn gemachtigd om Azure Site Recovery-bewerkingen te kunnen beheren. De rol 'Site Recovery-inzender' bevat alle machtigingen die nodig zijn om Site Recovery-bewerkingen in een Recovery Services-kluis te kunnen beheren.

## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

1. Uw virtuele machines moeten beheerde schijven gebruiken als u ze wilt verplaatsen naar een beschikbaarheidszone met Site Recovery. U kunt bestaande virtuele Windows-machines (VM's) die gebruikmaken van niet-beheerde schijven converteren, u kunt de VM’s voor het gebruik van beheerde schijven converteren door de stappen te volgen die [hier](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks) genoemd worden. Zorg ervoor dat de beschikbaarheidsset is geconfigureerd als Beheerd. 
2. Controleer of de meest recente basiscertificaten aanwezig zijn op de virtuele Azure-machines die u wilt verplaatsen. Als de meest recente basiscertificaten niet aanwezig zijn, kan het kopiëren van gegevens naar de doelregio niet worden ingeschakeld vanwege veiligheidsbeperkingen.

3. Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een niet-verbonden omgeving moet u de standaardprocedures van Windows Update en de standaardprocedures voor het bijwerken van de certificaten van uw organisatie volgen.

4. Voor Linux-VM’s volgt u de richtlijnen van de Linux-distributeur voor het verkrijgen van de meest recente basiscertificaten en de certificaatintrekkingslijst op de VM.
5. Zorg ervoor dat u geen verificatieproxy gebruikt om de verbinding met het netwerk te beheren voor de VM’s die u wilt verplaatsen.

6. Controleer [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) de vereisten als de virtuele machine die u wilt verplaatsen geen toegang tot internet heeft en een firewallproxy gebruikt voor het beheren van uitgaande toegang.

7. Identificeer de bron-indeling voor netwerken en alle resources die u momenteel gebruikt - zoals load balancers, NSG's, openbare IP, enz. voor uw verificatie.

## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer of u een Azure-abonnement hebt waarmee u in staat bent om virtuele machines te maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte om gegevens naar het doel te kopiëren.

3. Zorg ervoor dat u een doelresource maakt voor elk onderdeel dat is geïdentificeerd in de bronindeling voor netwerken. Het is belangrijk om ervoor te zorgen dat uw VM’s alle functionaliteit en functies hebben die u in de bron had, na het overknippen naar de doelregio.

    > [!NOTE]
    > Azure Site Recovery wordt automatisch gedetecteerd en maakt een virtueel netwerk en opslagaccount als u replicatie voor de bron-VM inschakelt. U kunt deze resources ook vooraf maken en aan de VM toewijzen als onderdeel van het inschakelen van de replicatie. Maar alle andere resources moet u zoals hieronder wordt beschreven handmatig maken in de doelregio.

     Raadpleeg de volgende documenten om de meest gebruikte netwerkresources te maken die relevant zijn voor u, op basis van de bron-VM-configuratie.

    - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials
        
     - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Voor andere netwerkonderdelen raadpleegt u de [netwerkondersteuningsdocumentatie.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

> [!IMPORTANT]
> Zorg ervoor dat u een zone-redundante load balancer in het doel gebruikt. Lees [hier](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones) meer informatie.

4. [Maak handmatig een niet-productienetwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio als u de configuratie wilt testen voordat u het uiteindelijke knippen naar de doelregio uitvoert. Hiermee ontstaat er slechts minimale verstoring in de productieomgeving. Dit wordt aanbevolen.


> [!NOTE]
> De onderstaande stappen zijn voor een enkele virtuele machine, maar u kunt dit uitbreiden naar meerdere virtuele machines door te navigeren naar de Recovery Services-kluis en te klikken op + Repliceren en de relevante virtuele machines tegelijk te selecteren.

## <a name="enable-replication"></a>Replicatie inschakelen
De onderstaande stappen tonen u hoe u met Azure Site Recovery de replicatie van gegevens naar de doelregio inschakelt voordat u ze uiteindelijk naar beschikbaarheidszones verplaatst.

1. Klik in de Azure Portal op **Virtuele machines** en selecteer de VM die u naar de beschikbaarheidszones wilt verplaatsen.
2. Klik bij **Bewerkingen** op **Herstel na noodgeval**.
3. Selecteer in **Noodherstel configureren** > **Doelregio** de doelregio waarnaar u wilt repliceren. Zorg ervoor dat deze regio beschikbaarheidszones [ondersteunt](https://docs.microsoft.com/azure/availability-zones/az-overview#regions-that-support-availability-zones).
![enable-rep-1.PNG](media/azure-vms-to-zones/enable-rep-1.PNG)

1. Selecteer **Volgende: Geavanceerde instellingen**
2. Kies de juiste waarden voor doelabonnement, doel-VM-resourcegroep & virtueel netwerk.
3. In de sectie **Beschikbaarheid** kiest u de beschikbaarheidszone waarnaar u de virtuele machine wilt verplaatsen. 
> [!NOTE]
> Als u de optie voor de beschikbaarheidsset of beschikbaarheidszone niet ziet, controleert u of aan de [vereisten](#prepare-the-source-vms) is voldaan en de [voorbereiding](#prepare-the-source-vms) van bron-VM's is voltooid.

   ![enable-rep-2.PNG](media/azure-vms-to-zones/enable-rep-2.PNG)

7. Klik op Replicatie inschakelen. Hierdoor wordt een taak gestart voor het inschakelen van replicatie voor de VM.

## <a name="verify-settings"></a>Instellingen controleren

Nadat de replicatietaak is voltooid, kunt u de replicatiestatus controleren, de replicatie-instellingen wijzigen en de implementatie testen.

1. Klik in het VM-menu op **Herstel na noodgeval**.
2. U kunt de replicatiestatus, de gemaakte herstelpunten, en de bron- en doelregio's op de kaart controleren.

   ![Replicatiestatus](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>De configuratie testen

1. Klik in het menu van de virtuele machine op **Herstel na noodgevallen**.
2. Klik op het **Failover testen**-pictogram.
3. Selecteer in **Failover testen** een herstelpunt om voor de failover te gebruiken:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM's naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het Azure-doeltestnetwerk waar u de Azure VM's naartoe wilt verplaatsen om de configuratie te testen. 

> [!IMPORTANT]
> U wordt aangeraden een afzonderlijk Azure-VM-netwerk te gebruiken voor de test en niet het productienetwerk in de doelregio waarnaar u uw virtuele machines uiteindelijk wilt verplaatsen.

4. Klik op **OK** om de verplaatsing te testen. Klik op de VM om de eigenschappen ervan te openen als u de voortgang wilt volgen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Als u de VM wilt verwijderen die is gemaakt als onderdeel van de verplaatsingstest, klikt u op **Failovertest wissen** op het gerepliceerde item. Leg in **Notities** eventuele opmerkingen over de test vast en sla deze op.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Voer de overstap naar de doelregio uit en bevestig dit.

1.  Klik in het menu van de virtuele machine op **Herstel na noodgevallen**.
2. Klik op het **Failover**-pictogram.
3. Bij **Failover** selecteert u **Meest recente**. 
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**. 
5. Nadat de taak is voltooid, controleert u of de VM zoals verwacht wordt weergegeven in de Azure-doelregio.
6. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Doorvoeren**. De verplaatsing naar de doelregio wordt voltooid. Wacht tot de doorvoertaak is voltooid.

## <a name="discard-the-resource-in-the-source-region"></a>Verwijder de resource in de bronregio 

1. Navigeer naar de VM.  Klik op **Replicatie uitschakelen**.  Hiermee wordt het proces van het kopiëren van de gegevens voor de VM beëindigd.  

> [!IMPORTANT]
> Het is belangrijk om de bovenstaande stap uit te voeren om te vermijden dat kosten in rekening worden gebracht voor Site Recovery-replicatie na de verplaatsing. De bronreplicatie-instellingen worden automatisch opgeschoond. Houd er rekening mee dat de Site Recovery-extensie die is geïnstalleerd als onderdeel van de replicatie, niet wordt verwijderd en handmatig moet worden verwijderd. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u de beschikbaarheid van een Azure-VM verhoogd door deze te verplaatsen naar een beschikbaarheidsset of een beschikbaarheidszone. U kunt nu herstel na noodgevallen configureren voor de verplaatste VM.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)


