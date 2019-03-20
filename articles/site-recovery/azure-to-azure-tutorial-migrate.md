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
ms.openlocfilehash: 7619b8831d75ce639c6f6c773c7c7d491abc93e7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122026"
---
# <a name="move-azure-vms-to-another-region"></a>Virtuele Azure-machines verplaatsen naar een andere regio

Er zijn verschillende scenario's waarin u wilt verplaatsen van uw bestaande Azure IaaS virtuele machines (VM's) van de ene regio naar een andere. U wilt bijvoorbeeld meer betrouwbaarheid en beschikbaarheid van uw bestaande virtuele machines, beheerbaarheid, of omwille van de governance. Zie voor meer informatie de [Azure VM verplaatsen overzicht](azure-to-azure-move-overview.md). 

U kunt de [Azure Site Recovery](site-recovery-overview.md) service om te beheren en te organiseren van herstel na noodgevallen van on-premises machines en virtuele machines van Azure voor zakelijke continuïteit en herstel na noodgevallen (BCDR). U kunt Site Recovery ook gebruiken voor het beheren van de verplaatsing van virtuele Azure-machines naar een secundaire regio.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> 
> * Controleer of de vereisten voor het verplaatsen
> * De bron-VM's en de doelregio voorbereiden
> * Kopieert de gegevens en replicatie inschakelen
> * Test de configuratie en de verplaatsing uit te voeren
> * De resources in de bronregio verwijderen
> 
> [!NOTE]
> Deze zelfstudie leert u hoe u Azure-VM's verplaatsen van de ene regio naar een andere is. Als u wilt de beschikbaarheid verbeteren door het verplaatsen van virtuele machines in een beschikbaarheidsset om zone vastgemaakt virtuele machines in een andere regio, raadpleegt u de [Azure-VM's in Beschikbaarheidszones zelfstudie verplaatsen](move-azure-vms-avset-azone.md).

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat de Azure VM's zijn in de Azure-regio van waaruit u wilt verplaatsen.
- Controleren of uw eigen keuze aan [bronregio - doel regio combinatie wordt ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), en maakt u een gefundeerde beslissing nemen over de doelregio.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).
- Accountmachtigingen controleren. Als u uw gratis Azure-account hebt gemaakt, u kunt de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, werkt u met de beheerder om toe te wijzen de machtigingen die u nodig hebt. Replicatie inschakelen voor een virtuele machine en in wezen gegevens kopiëren met behulp van Azure Site Recovery, moet u het volgende hebben:

    * Machtigingen voor het maken van een virtuele machine in Azure-resources. De ingebouwde rol van inzender voor virtuele machines beschikt over deze machtigingen, waaronder:
        - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
        - Machtiging voor het maken van een virtuele machine in het geselecteerde virtuele netwerk
        - Machtigingen om naar het geselecteerde opslagaccount te schrijven

    * Machtigingen voor het beheren van Azure Site Recovery-bewerkingen. De rol Site Recovery-Inzender bevat alle machtigingen die vereist zijn voor het beheren van Site Recovery-bewerkingen in een Recovery Services-kluis.

## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

1. Zorg ervoor dat de meest recente basiscertificaten zijn op de Azure-VM's die u wilt verplaatsen. Als de meest recente basiscertificaten niet aanwezig op de virtuele machine zijn, wordt veiligheidsbeperkingen voorkomen dat het kopiëren van gegevens naar de doelregio.

    - Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een niet-verbonden omgeving moet u de standaardprocedures van Windows Update en de standaardprocedures voor het bijwerken van de certificaten van uw organisatie volgen.
    - Volg de richtlijnen bij uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de certificaatintrekkingslijst op de virtuele machine voor Linux-VM's.
1. Zorg ervoor dat u niet een verificatieproxy om netwerkconnectiviteit te beheren voor virtuele machines die u wilt verplaatsen.
1. Als de virtuele machine die u probeert te verplaatsen heeft geen toegang tot het internet of een firewallproxy gebruikt voor het beheren van uitgaande toegang [de vereisten controleren](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).
1. Identificeer de bron-indeling voor netwerken en alle resources die u momenteel gebruikt. Dit omvat, maar is niet beperkt tot taakverdelers, netwerkbeveiligingsgroepen (nsg's) en openbare IP-adressen.

## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer of uw Azure-abonnement kunt u virtuele machines maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

1. Zorg ervoor dat uw abonnement voldoende resources voor ondersteuning van VM's met grootten die overeenkomen met uw bron-VM's heeft. Als u Site Recovery gebruikt om gegevens te kopiëren naar het doel, wordt met Site Recovery kiest even groot is of de dichtstbijzijnde grootte voor de doel-VM.

1. Zorg ervoor dat u een doelbron voor elk onderdeel die wordt geïdentificeerd in de indeling van bron-netwerken maakt. Deze stap is belangrijk om ervoor te zorgen dat uw virtuele machines hebben de functionaliteit en functies in de doelregio die u in de bronregio hebt.

    > [!NOTE]
    > Azure Site Recovery wordt automatisch gedetecteerd en maakt een virtueel netwerk wanneer u replicatie voor de bron-VM inschakelt. U kunt ook vooraf maken een netwerk en wijs deze toe aan de virtuele machine in het beleid voor replicatie inschakelen. Zoals verderop vermeld, moet u handmatig maken van alle andere resources in de doelregio.

     Doorgaans maakt u het meest gebruikt netwerkbronnen die relevant voor u op basis van de bron-VM-configuratie zijn, raadpleegt u de volgende documentatie:

   - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
   - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
     Zie voor andere netwerkonderdelen, de [netwerken documentatie](https://docs.microsoft.com/azure/#pivot=products&panel=network).

1. Handmatig [maken van een niet-productie-netwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio als u testen van de configuratie wilt voordat u de uiteindelijke overgang naar de doelregio uitvoert. We raden deze stap omdat Hiermee zorgt u ervoor minimale interferentie met het productienetwerk.

## <a name="copy-data-to-the-target-region"></a>Gegevens kopiëren naar de doelregio
De volgende stappen laten zien hoe u Azure Site Recovery gebruiken om gegevens te kopiëren naar de doelregio.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Maak de kluis in andere regio's, met uitzondering van de bronregio

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
1. Selecteer **een resource maken** > **beheerhulpprogramma** > **back-up en siteherstel**.
1. Bij **Naam** geeft u de beschrijvende naam **ContosoVMVault** op. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
1. Maak de resourcegroep **ContosoRG**.
1. Geef een Azure-regio op. Ondersteunde regio's Zie geografische beschikbaarheid in [Azure Site Recovery prijsinformatie](https://azure.microsoft.com/pricing/details/site-recovery/).
1. In **Recovery Services-kluizen**, selecteer **overzicht** > **ContosoVMVault** > **+ repliceren**.
1. Bij **Bron** selecteert u **Azure**.
1. Bij **Bronlocatie** selecteert u de Azure-bronregio waar uw VM’s momenteel worden uitgevoerd.
1. Selecteer het Resource Manager-implementatiemodel. Selecteer vervolgens het **Bronabonnement** en de **Bronresourcegroep**.
1. Selecteer **OK** de instellingen op te slaan.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Replicatie inschakelen voor Azure-VM's en beginnen met het kopiëren van de gegevens

Site Recovery haalt een lijst van de virtuele machines die gekoppeld aan het abonnement en resourcegroep zijn.

1. Selecteer de virtuele machine die u wilt verplaatsen, en selecteer vervolgens in de volgende stap **OK**.
1. In **instellingen**, selecteer **herstel na noodgevallen**.
1. In **herstel na noodgevallen configureren** > **doelregio**, selecteert u de doelregio waarnaar u wilt repliceren.
1. Accepteer voor deze zelfstudie de overige standaardinstellingen.
1. Selecteer **inschakelen replicatie**. Deze stap wordt een taak replicatie inschakelen voor de virtuele machine gestart.

    ![Replicatie inschakelen](media/tutorial-migrate-azure-to-azure/settings.png)

## <a name="test-the-configuration"></a>De configuratie testen

1. Ga naar de kluis. In **instellingen** > **gerepliceerde items**, selecteer de virtuele machine die u wilt verplaatsen naar de doelregio en selecteer vervolgens **+ Testfailover**.
1. In **Testfailover**, selecteer een herstelpunt dat moet worden gebruikt voor de failover:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage Recovery Time Objective (RTO).
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM's naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

1. Selecteer het Azure-doelnetwerk waar u de Azure VM's naartoe wilt verplaatsen, om de configuratie te testen.
    > [!IMPORTANT]
    > U wordt aangeraden dat u een afzonderlijke Azure-VM-netwerk voor de testfailover gebruiken. Gebruik niet het productienetwerk die is ingesteld toen u replicatie en dat u wilt verplaatsen van uw virtuele machines in uiteindelijk ingeschakeld.
1. Klik op **OK** om de verplaatsing te testen. Klik op de VM om de eigenschappen ervan te openen als u de voortgang wilt volgen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** > **Site Recovery-taken**.
1. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in Azure Portal > **Virtuele machines**. Zorg ervoor dat de virtuele machine wordt uitgevoerd, het juiste formaat is en is verbonden met het juiste netwerk.
1. Als u wilt verwijderen de tha VM is gemaakt als onderdeel van de verplaatsing testen, klikt u op **failovertest** op het gerepliceerde item. In **opmerkingen bij de**, vastleggen en opslaan van eventuele opmerkingen die gekoppeld aan de test zijn.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>De overstap naar de doelregio uitvoeren en controleren

1. Ga naar de kluis. In **instellingen** > **gerepliceerde items**, selecteer de virtuele machine en selecteer vervolgens **Failover**.
1. Bij **Failover** selecteert u **Meest recente**.
1. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
1. Nadat de taak is voltooid, controleert u de virtuele machine wordt weergegeven in de doel-Azure-regio, zoals verwacht.
1. In **gerepliceerde items**, selecteer met de rechtermuisknop de virtuele machine > **doorvoeren**. Deze stap is het verplaatsingsproces naar de doelregio voltooid. Wacht totdat de taak doorvoeren is voltooid.

## <a name="delete-the-resource-in-the-source-region"></a>De resource in de bronregio verwijderen

Ga naar de virtuele machine. Selecteer **replicatie uitschakelen**. Deze stap wordt het proces van het kopiëren van de gegevens voor de virtuele machine gestopt.

> [!IMPORTANT]
> Het is belangrijk om uit te voeren van deze stap om te voorkomen dat in rekening gebracht voor Azure Site Recovery-replicatie.

Als u geen plannen om opnieuw te gebruiken een van de bron-resources hebt, volgt u deze stappen:

1. Verwijderen van alle relevante netwerkbronnen in de regio van de gegevensbron die u worden vermeld als deel van stap 4 in [voorbereiden van de bron-VM's](#prepare-the-source-vms).
1. Verwijder het bijbehorende opslagaccount in de bronregio.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt verplaatst u een Azure-VM naar een andere Azure-regio. U kunt nu herstel na noodgevallen configureren voor de virtuele machine die u verplaatst.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)

