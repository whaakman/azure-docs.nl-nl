---
title: Azure IaaS-VM's verplaatsen naar een andere Azure-regio met behulp van de Azure Site Recovery-service | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS-VM’s te verplaatsen van de ene Azure-regio naar een andere.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: bbede01844f20c0240b154fd319b818a43463131
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824489"
---
# <a name="move-azure-vms-to-another-region"></a>Virtuele Azure-machines verplaatsen naar een andere regio

Er zijn verschillende redenen waarom u bestaande Azure IaaS-VM’s mogelijk wilt verplaatsen van de ene regio naar de andere: om de betrouwbaarheid en beschikbaarheid van uw bestaande VM’s te verbeteren, om de beheerbaarheid te verbeteren, of vanwege beheerredenen, enzovoort, zoals wordt beschreven. 

Deze zelfstudie laat zien hoe u Azure-VM’s naar een andere regio verplaatst met behulp van Azure Site Recovery. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * [Vereisten controleren](#verify-prerequisites)
> * [De bron-VM’s voorbereiden](#prepare-the-source-vms)
> * [De doelregio voorbereiden](#prepare-the-target-region)
> * [Gegevens kopiëren naar de doelregio](#copy-data-to-the-target-region)
> * [De configuratie testen](#test-the-configuration)
> * [De verplaatsing uitvoeren ](#perform-the-move-to-the-target-region-and-confirm)
> * [Verwijder de resource in de bronregio ](#discard-the-resource-in-the-source-region)

> [!IMPORTANT]
> Dit document biedt hulp bij het ongewijzigd verplaatsen van Azure-VM’s van de ene regio naar een andere regio. Raadpleeg deze zelfstudie als u de beschikbaarheid van uw infrastructuur wilt verbeteren door VM’s naar beschikbaarheidszones te verplaatsen.

## <a name="verify-prerequisites"></a>Vereisten controleren

- Zorg ervoor dat de Azure-VM’s zich bevinden in de Azure-bronregio waaruit u wilt verplaatsen.
- Controleer of uw [combinatie van bronregio en doelregio wordt ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support) en neem een gefundeerde beslissing over de doelregio.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).
- Accountmachtigingen controleren: Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te schakelen voor een VM en uiteindelijk gegevens te kopiëren naar de doelserver met behulp van Azure Site Recovery, moet u het volgende hebben:

    1. Machtigingen voor het maken van een virtuele machine in Azure-resources. De ingebouwde rol 'Inzender voor virtuele machines' beschikt over deze machtigingen, waaronder:
        - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
        - Machtiging voor het maken van een virtuele machine in het geselecteerde virtuele netwerk
        - Machtigingen om naar het geselecteerde opslagaccount te schrijven

    2. U moet ook zijn gemachtigd om Azure Site Recovery-bewerkingen te kunnen beheren. De rol 'Site Recovery-inzender' bevat alle machtigingen die nodig zijn om Site Recovery-bewerkingen in een Recovery Services-kluis te kunnen beheren.

## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

1. Controleer of de meest recente basiscertificaten aanwezig zijn op de Azure-VM’s die u wilt verplaatsen. Als de meest recente basiscertificaten niet aanwezig zijn, kan het kopiëren van gegevens naar de doelregio niet worden ingeschakeld vanwege veiligheidsbeperkingen.

    - Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een niet-verbonden omgeving moet u de standaardprocedures van Windows Update en de standaardprocedures voor het bijwerken van de certificaten van uw organisatie volgen.
    - Voor Linux-VM’s volgt u de richtlijnen van de Linux-distributeur voor het verkrijgen van de meest recente basiscertificaten en de certificaatintrekkingslijst op de VM.
2. Zorg ervoor dat u geen verificatieproxy gebruikt om de verbinding met het netwerk te beheren voor de VM’s die u wilt verplaatsen.
3. Controleer [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) de vereisten als de VM die u wilt verplaatsen, geen toegang tot internet heeft en een firewallproxy gebruikt voor het beheren van uitgaande toegang.
4. Identificeer en noteer de bronindeling voor netwerken en alle resources die u momenteel gebruikt - inclusief maar niet beperkt tot load balancers, NSG's, openbare IP, enzovoort, ter controle.

## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer of u een Azure-abonnement hebt waarmee u in staat bent om virtuele machines te maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte om gegevens naar het doel te kopiëren.

3. Zorg ervoor dat u een doelresource maakt voor elk onderdeel dat is geïdentificeerd in de bronindeling voor netwerken. Het is belangrijk om ervoor te zorgen dat uw VM’s alle functionaliteit en functies hebben die u in de bron had, na het overknippen naar de doelregio.

    > [!NOTE]
    > Azure Site Recovery wordt automatisch gedetecteerd en maakt een virtueel netwerk en opslagaccount als u replicatie voor de bron-VM inschakelt. U kunt deze resources ook vooraf maken en aan de VM toewijzen als onderdeel van het inschakelen van de replicatie. Maar alle andere resources moet u zoals hieronder wordt beschreven handmatig maken in de doelregio.

     Raadpleeg de volgende documenten om de meest gebruikte netwerkresources te maken die relevant zijn voor u, op basis van de bron-VM-configuratie.

    - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Voor andere netwerkonderdelen raadpleegt u de [netwerkondersteuningsdocumentatie.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

4. [Maak handmatig een niet-productienetwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio als u de configuratie wilt testen voordat u het uiteindelijke knippen naar de doelregio uitvoert. Hiermee ontstaat er slechts minimale verstoring in de productieomgeving. Dit wordt aanbevolen.
    
## <a name="copy-data-to-the-target-region"></a>Gegevens kopiëren naar de doelregio
De onderstaande stappen begeleiden u bij het gebruik van Azure Site Recovery om gegevens te kopiëren naar de doelregio.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Maak de kluis in elke gewenste regio, met uitzondering van de bronregio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Beheerhulpprogramma's** > **Backup en Site Recovery**.
3. Bij **Naam** geeft u de beschrijvende naam **ContosoVMVault** op. Als u meer dan één abonnement hebt, selecteert u het juiste abonnement.
4. Maak een resourcegroep met de naam **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Klik in Recovery Services-kluizen op **Overzicht** > **ConsotoVMVault** > **+Repliceren**.
7. Bij **Bron** selecteert u **Azure**.
8. Bij **Bronlocatie** selecteert u de Azure-bronregio waar uw VM’s momenteel worden uitgevoerd.
9. Selecteer het Resource Manager-implementatiemodel. Selecteer vervolgens het **Bronabonnement** en de **Bronresourcegroep**.
10. Klik op **OK** om de instellingen op te slaan.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Schakel replicatie voor Azure-VM's in en begin met het kopiëren van de gegevens.

In Site Recovery wordt een lijst opgehaald van de virtuele machines die zijn gekoppeld aan het abonnement en de resourcegroep.

1. In de volgende stap, Selecteer de VM die u wilt verplaatsen. Klik vervolgens op **OK**.
3. Klik bij **Instellingen** op **Herstel na noodgeval**.
4. Selecteer in **Noodherstel configureren** > **Doelregio** de doelregio waarnaar u wilt repliceren.
5. U kunt ervoor kiezen om de standaarddoelresources te gebruiken of de doelresources die u vooraf hebt gemaakt.
6. Klik op **Replicatie inschakelen**. Hierdoor wordt een taak gestart voor het inschakelen van replicatie voor de VM.

    ![replicatie inschakelen](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>De configuratie testen


1. Ga naar de kluis, klik in **Instellingen** > **Gerepliceerde items** op de virtuele machine die u wilt verplaatsen naar de doelregio. Klik op het pictogram **+Failover testen**.
2. Selecteer in **Failover testen** een herstelpunt om voor de failover te gebruiken:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM's naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het Azure-doelnetwerk waar u de Azure VM's naartoe wilt verplaatsen, om de configuratie te testen. 

> [!IMPORTANT]
> U wordt aangeraden een afzonderlijk Azure-VM-netwerk te gebruiken voor de test en niet het productienetwerk in de doelregio waarnaar u uw VM’s uiteindelijk wilt verplaatsen.

4. Klik op **OK** om de verplaatsing te testen. Klik op de VM om de eigenschappen ervan te openen als u de voortgang wilt volgen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Als u de VM wilt verwijderen die is gemaakt als onderdeel van de verplaatsingstest, klikt u op **Failovertest wissen** op het gerepliceerde item. Leg in **Notities** eventuele opmerkingen over de test vast en sla deze op.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Voer de overstap naar de doelregio uit en bevestig dit.

1.  Ga naar de kluis, klik in **Instellingen** > **Gerepliceerde items** op de virtuele machine en klik vervolgens op **Failover**.
2. Bij **Failover** selecteert u **Meest recente**. 
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**. 
4. Nadat de taak is voltooid, controleert u of de VM zoals verwacht wordt weergegeven in de Azure-doelregio.
5. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Doorvoeren**. De verplaatsing naar de doelregio wordt voltooid. Wacht tot de doorvoertaak is voltooid.

## <a name="discard-the-resource-in-the-source-region"></a>Verwijder de resource in de bronregio 

1. Navigeer naar de VM.  Klik op **Replicatie uitschakelen**.  Hiermee wordt het proces van het kopiëren van de gegevens voor de VM beëindigd.  

> [!IMPORTANT]
> Het is belangrijk om de bovenstaande stap uit te voeren om te vermijden dat na de verplaatsing kosten in rekening worden gebracht voor Site Recovery-replicatie.

Als u van plan bent om een of meer van de bronresources opnieuw te gebruiken, gaat u verder met de volgende reeks stappen.

1. Ga verder met het verwijderen van alle relevante netwerkresources in de bronregio die u hebt vermeld als onderdeel van stap 4 in [De bron-VM’s voorbereiden](#prepare-the-source-vms) 
2. Verwijder het bijbehorende opslagaccount in de bronregio.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure-VM verplaatst naar een andere Azure-regio. U kunt nu herstel na noodgevallen configureren voor de verplaatste VM.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)

