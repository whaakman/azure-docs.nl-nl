---
title: Azure IaaS VM's verplaatsen naar een andere Azure-regio met behulp van de service Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery gebruiken om Azure IaaS-VM’s te verplaatsen van de ene Azure-regio naar een andere.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 0f73e68fd0c01d4323e8675d3fa12f7ca1051cdb
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192919"
---
# <a name="move-azure-vms-to-another-region"></a>Virtuele Azure-machines verplaatsen naar een andere regio

Het is raadzaam om te verplaatsen van Azure-infrastructuur als een service (IaaS) virtuele machines van de ene regio naar een andere voor het verbeteren van betrouwbaarheid, beschikbaarheid, beheer of governance. Deze zelfstudie leert u hoe u virtuele machines verplaatst naar een andere regio met behulp van Azure Site Recovery. U leert het volgende:

> [!div class="checklist"]
> * Vereisten verifiëren
> * De bron-VM’s voorbereiden
> * De doelregio voorbereiden
> * Gegevens kopiëren naar de doelregio
> * De configuratie testen
> * De verplaatsing uitvoeren
> * De resources van de bronregio negeren


> [!IMPORTANT]
> In dit artikel wordt beschreven hoe u Azure-VM's verplaatsen van de ene regio naar een andere *omdat*. Als het doel is om de beschikbaarheid van uw infrastructuur verbeteren door te verplaatsen van virtuele machines naar beschikbaarheidszones, Zie [Azure VM's verplaatsen naar Beschikbaarheidszones](move-azure-vms-avset-azone.md).

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u Azure-VM's in de bron van de Azure-regio die u wilt verplaatsen *van*.
- Controleren of uw eigen keuze aan [bron regio-target regio combinatie wordt ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), en kiest u zorgvuldig de doelregio.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).
- Accountmachtigingen controleren. Als u uw gratis Azure-account voor het zojuist hebt gemaakt *u* zijn van de beheerder van uw abonnement. Als u niet de beheerder, samen met de beheerder de machtigingen die u nodig hebt op te halen:
  -  Als u wilt inschakelen replicatie voor een virtuele machine en kopieert u gegevens naar het doel met behulp van Site Recovery, moet u machtigingen voor het maken van een virtuele machine in uw Azure-resources hebben. De ingebouwde rol Inzender voor virtuele machines beschikt over deze machtigingen. Met de machtigingen, kunt u het volgende doen:
        - Het maken van een VM in de geselecteerde resourcegroep.
        - Het maken van een VM in het geselecteerde virtuele netwerk.
        - Schrijven naar het geselecteerde opslagaccount.

  - U moet ook machtigingen voor het beheren van Site Recovery-bewerkingen. De rol Site Recovery-Inzender bevat alle machtigingen die vereist zijn voor het beheren van Site Recovery-bewerkingen in een Azure Recovery Services-kluis.

## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

1. Controleer of de Azure-VM's die u wilt verplaatsen van de meest recente basiscertificaten hebben. Als ze dit niet doet, kunt u gegevens kopiëren naar de doelregio vanwege veiligheidsbeperkingen niet inschakelen.

    - Voor Windows-VM's, de meest recente Windows-updates te installeren zodat alle vertrouwde basiscertificaten op de machine zijn. In een omgeving zonder verbinding, volgt u de standaard Windows Update en certificaat-update-processen voor uw organisatie.
    - Volg de richtlijnen van uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de certificaatintrekkingslijst voor virtuele Linux-machines.
2. Zorg ervoor dat u niet een verificatieproxy om netwerkconnectiviteit te beheren voor virtuele machines die u van plan bent om te verplaatsen.
3. Als een virtuele machine die u wilt verplaatsen heeft geen toegang tot het internet en is een firewallproxy uitgaande om toegang te beheren, Controleer de [vereisten](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).
4. Documenteer de bron-indeling voor netwerken en alle resources die u momenteel gebruikt, inclusief (maar niet beperkt tot) load balancers, netwerkbeveiligingsgroepen en openbare IP-adressen voor verificatie.

## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer of dat u virtuele machines kunt maken in de doelregio die wordt gebruikt voor herstel na noodgevallen in uw Azure-abonnement. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum indien nodig.

2. Zorg ervoor dat uw abonnement voldoende resources voor ondersteuning van uw bron-VM's heeft. Als u Site Recovery gebruikt om gegevens te kopiëren naar het doel, kiest deze het even groot of de dichtstbijzijnde beschikbare grootte voor de doel-VM's.

3. Zorg ervoor dat u een doelbron voor elk onderdeel dat u hebt geïdentificeerd in de indeling van bron-netwerken maakt. Dit zorgt ervoor dat uw virtuele machines hebben de functionaliteit en functies in de doelregio die ze in de bronregio hebben.

   Azure Site Recovery wordt automatisch gedetecteerd en maakt u een virtueel netwerk- en storage-account als u replicatie voor de bron-VM schakelt. U kunt ook vooraf maken deze resources en deze toewijzen aan de virtuele machine als onderdeel van de stap replicatie inschakelen. Maar u moet alle andere resources handmatig maken in de doelregio. Raadpleeg de volgende documenten te maken van de meest gebruikte netwerkbronnen op basis van de configuratie van uw virtuele machine:

   - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
   - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
   Zie voor andere netwerkonderdelen, de [documentatie voor Azure networking](https://docs.microsoft.com/azure/#pivot=products&panel=network). 

4. Het testen van de configuratie voordat u de verplaatsing handmatig uitvoert [maken van een niet-productie-netwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio. Testen van de installatie maakt minimale interferentie met de productie-omgeving en wordt u aangeraden deze.
    
## <a name="copy-data-to-the-target-region"></a>Gegevens kopiëren naar de doelregio
De volgende stappen uit Azure Site Recovery gebruiken om gegevens te kopiëren naar de doelregio.

### <a name="create-the-vault-in-any-region-except-the-source"></a>Maak de kluis in andere regio's met uitzondering van de bron

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Selecteer **een resource maken** > **beheerhulpprogramma** > **back-up en siteherstel**.
3. Voor **naam**, geef de beschrijvende naam **ContosoVMVault**. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep met de naam **ContosoRG**.
5. Geef een Azure-regio op. Ondersteunde regio's, Zie [prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Selecteer voor de Recovery Services-kluizen **overzicht** > **ConsotoVMVault** > **+ repliceren**.
7. Voor **bron**, selecteer **Azure**.
8. Voor **bronlocatie**, selecteer de bron-Azure-regio waar uw VM's momenteel worden uitgevoerd.
9. Selecteer het Azure Resource Manager-implementatiemodel. Selecteer vervolgens de **abonnement van bron** en **bronresourcegroep**.
10. Selecteer **OK** de instellingen op te slaan.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Replicatie inschakelen voor Azure-VM's en beginnen met het kopiëren van de gegevens

Site Recovery haalt een lijst van de virtuele machines die gekoppeld aan het abonnement en resourcegroep zijn.

1. Selecteer de virtuele machine die u wilt verplaatsen, en selecteer vervolgens **OK**.
2. Voor **instellingen**, selecteer **herstel na noodgevallen**.
3. Voor **herstel na noodgevallen configureren** > **doelregio**, selecteer de doelregio die u naar repliceert.
4. Kiezen voor de doelresources standaard of die u vooraf hebt gemaakt.
5. Selecteer **inschakelen replicatie** om de taak te starten.

   ![Replicatie inschakelen](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>De configuratie testen


1. Ga naar de kluis. In **instellingen** > **gerepliceerde items**, selecteert u de virtuele machine die u wilt verplaatsen naar de doelregio. Selecteer **Testfailover**.
2. In **Testfailover**, selecteer een herstelpunt dat moet worden gebruikt voor de failover:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Er is geen tijd besteed aan het verwerken van gegevens, zodat u deze optie biedt een geringe beoogde hersteltijd (RTO).
   - **Laatste toepassingsconsistente punt**: Failover-schakeling alle VM's naar de meest recente toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het Azure-doelnetwerk waar u de Azure VM's naartoe wilt verplaatsen, om de configuratie te testen.

   > [!IMPORTANT]
   > U wordt aangeraden dat u een afzonderlijke Azure-VM-netwerk voor de testfailover, niet het productienetwerk in de doelregio gebruiken.

4. Selecteer eerst de verplaatsing testen **OK**. Om de voortgang volgen, selecteert u de virtuele machine en opent de **eigenschappen.** Of Selecteer de **Testfailover** taak in de kluis. Selecteer **instellingen** > **taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Selecteer wilt verwijderen van de virtuele machine die u hebt gemaakt voor het testen van **failovertest** op het gerepliceerde item. Van **opmerkingen bij de**, vastleggen en opslaan van eventuele opmerkingen met betrekking tot de test.

## <a name="perform-the-move-and-confirm"></a>Uitvoeren van de verplaatsing en bevestigen

1. Ga naar de kluis in **instellingen** > **gerepliceerde items**, selecteer de virtuele machine en selecteer vervolgens **Failover**.
1. Voor **Failover**, selecteer **nieuwste**. 
2. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery wordt geprobeerd de bron-VM afsluiten voordat de failover wordt geactiveerd. Maar de failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
3. Wanneer de taak is voltooid, controleert u de virtuele machine wordt weergegeven in de doel-Azure-regio, zoals verwacht.
4. In **gerepliceerde items**, met de rechtermuisknop op de virtuele machine en selecteert u **doorvoeren**. De verplaatsing wordt voltooid. Wacht totdat de taak doorvoeren is voltooid.

## <a name="discard-the-resources-from-the-source-region"></a>De resources van de bronregio negeren

- Ga naar de virtuele machine en selecteer **replicatie uitschakelen**. Hiermee wordt het proces van het kopiëren van de gegevens voor de VM beëindigd.

  > [!IMPORTANT]
  > Voer deze stap om te voorkomen dat in rekening gebracht voor Site Recovery-replicatie na de verplaatsing.

Als u niet van plan bent om een van de bron-resources opnieuw te gebruiken, volgt u deze stappen:

1. Verwijderen van alle relevante netwerkbronnen in de regio van de gegevensbron die u in stap 4 van [voorbereiden van de bron-VM's](#prepare-the-source-vms).
2. Verwijder het bijbehorende opslagaccount in de bronregio.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u Azure-VM's verplaatsen naar een andere Azure-regio. U kunt nu herstel na noodgevallen configureren voor deze virtuele machines.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)

