---
title: Azure Virtual machines migreren tussen Azure-regio's met Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery gebruiken voor het migreren van Azure IaaS VM's van een Azure-regio naar een andere.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Virtuele Azure-machines migreren naar een andere regio

Naast het gebruik van de [Azure Site Recovery](site-recovery-overview.md) service te beheren en te organiseren herstel na noodgevallen van on-premises machines en virtuele Azure-machines voor de doeleinden van zakelijke continuïteit en herstel na noodgevallen (BCDR) kunt u de Site ook gebruiken Herstel voor het beheren van de migratie van virtuele Azure-machines met een secundaire regio. Replicatie inschakelen voor deze om te migreren virtuele Azure-machines, en ze via failover van de primaire regio naar de secundaire regio van uw keuze.

Deze zelfstudie laat zien hoe u virtuele Azure-machines migreren naar een andere regio. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery services-kluis maken
> * Replicatie inschakelen voor een virtuele machine
> * Voer een failover voor het migreren van de virtuele machine

Deze zelfstudie wordt ervan uitgegaan er al een Azure-abonnement. Als u dit niet doet, maakt een [gratis account](https://azure.microsoft.com/pricing/free-trial/) voordat u begint.

>[!NOTE]
>
> Replicatie van site Recovery voor Azure VM's is momenteel in preview.



## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt voltooid, moet u virtuele Azure-machines in een Azure-regio van waaruit u wilt migreren. Bovendien zijn er een aantal instellingen die u controleren moet voordat u begint.


### <a name="verify-target-resources"></a>Controleer of de doelresources

1. Controleer of uw Azure-abonnement kunt u virtuele machines in de doelregio die wordt gebruikt voor herstel na noodgevallen maken. Neem contact op met ondersteuning voor het inschakelen van de quota voor vereist.

2. Zorg ervoor dat uw abonnement heeft onvoldoende bronnen ter ondersteuning van virtuele machines met grootten die overeenkomen met uw virtuele bronmachines. Site Recovery kiest even groot of de dichtstbijzijnde grootte voor het VM-doel.


### <a name="verify-account-permissions"></a>Controleer de accountmachtigingen

Als u hebt zojuist hebt gemaakt met uw gratis Azure-account bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, samen met de beheerder u moet machtigingen toewijzen. Om replicatie inschakelen voor een nieuwe virtuele machine, moet u het volgende hebben:

1. Machtigingen voor het maken van een virtuele machine in Azure-resources. De ingebouwde rol 'Virtual Machine Contributor' beschikt over deze machtigingen, waaronder:
    - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
    - Machtiging voor het maken van een virtuele machine in de geselecteerde virtuele netwerk
    - Machtigingen voor schrijven naar het geselecteerde opslagaccount

2. U moet ook de machtiging voor het beheren van Azure Site Recovery-bewerkingen. De rol 'Inzender Site Recovery' heeft alle machtigingen nodig voor het beheren van Site Recovery-bewerkingen in een Recovery Services-kluis.


### <a name="verify-vm-outbound-access"></a>De VM uitgaande toegang controleren

1. Zorg ervoor dat u een verificatieproxy niet gebruikt om verbinding met het netwerk voor virtuele machines die u wilt migreren te regelen. 
2. Voor deze zelfstudie gaan we ervan uit dat de virtuele machines die u wilt migreren toegang heeft tot internet en geen gebruik maakt van een een firewallproxy om uitgaande toegang te beheren. Als u bent, controleert u de vereisten [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Controleer of de VM-certificaten

Controleer of de meest recente basiscertificaten aanwezig zijn op de Azure VM's die u wilt migreren. Als de meest recente basiscertificaten niet, kan niet de virtuele machine vanwege veiligheidsbeperkingen Site Recovery worden geregistreerd.

- Installeren de meest recente Windows updates voor Windows-VM's op de virtuele machine, zodat de vertrouwde basiscertificaten op de machine zijn. Volg de standaard Windows Update en de processen die certificaat updates voor uw organisatie in een omgeving zonder verbinding.
- Volg de richtlijnen bij uw Linux-leverancier om op te halen van de meest recente vertrouwde basiscertificaten en de certificaatintrekkingslijst op de virtuele machine voor Linux VM's.



## <a name="create-a-vault"></a>Een kluis maken

Maak de kluis in elke regio, met uitzondering van de bron-regio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Nieuw** > **Bewaking en beheer** > **Backup en Site Recovery**.
3. In **naam**, geef de beschrijvende naam **ContosoVMVault**. Als u meer dan één abonnement hebt, selecteert u het juiste bestand.
4. Een resourcegroep maken **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Voor snelle toegang tot de kluis vanuit het dashboard, klikt u op **vastmaken aan dashboard** en klik vervolgens op **maken**.

   ![Nieuwe kluis](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

De nieuwe kluis wordt toegevoegd aan de **Dashboard** onder **alle resources**, en klik op de hoofdblade **Recovery Services-kluizen** pagina.






## <a name="select-the-source"></a>Selecteer de gegevensbron

1. Klik in de Recovery Services-kluizen op **ConsotoVMVault** > **+ repliceren**.
2. In **bron**, selecteer **Azure - PREVIEW**.
3. In **bronlocatie**, selecteer de bron van Azure-regio waarin uw virtuele machines worden uitgevoerd.
4. Selecteer het Resource Manager-implementatiemodel. Selecteer vervolgens de **resourcegroep**.
5. Klik op **OK** om de instellingen op te slaan.


## <a name="enable-replication-for-azure-vms"></a>Replicatie inschakelen voor Azure Virtual machines

Site Recovery haalt een lijst van de virtuele machines die zijn gekoppeld aan het abonnement en resourcegroep.


1. Klik in de Azure-portal op **virtuele machines**.
2. Selecteer de virtuele machine die u wilt migreren. Klik vervolgens op **OK**.
3. In **instellingen**, klikt u op **herstel na noodgevallen (preview)**.
4. In **configureren van herstel na noodgevallen** > **doelregio** Selecteer het de doelregio waarnaar u zult repliceren.
5. Accepteer de standaardinstellingen voor deze zelfstudie.
6. Klik op **replicatie inschakelen**. Een taak voor het inschakelen van replicatie voor de virtuele machine wordt gestart.

    ![Replicatie inschakelen](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Replicatie van Azure VM's met beheerde schijven wordt momenteel niet ondersteund. 

## <a name="run-a-failover"></a>Een failover uitvoeren

1. In **instellingen** > **gerepliceerde items**, klik op de machine en klik vervolgens op **Failover**.
2. In **Failover**, selecteer **nieuwste**. De instelling voor de versleuteling van sleutel is niet relevant zijn voor dit scenario.
3. Selecteer **machine afsluiten voordat u begint met failover**. Site Recovery probeert de bron-VM afsluiten voordat de failover. Failover wordt voortgezet zelfs als afsluiten is mislukt. U kunt de voortgang van de failover volgen op de **taken** pagina.
4. Controleer de Azure VM in Azure wordt weergegeven zoals verwacht.
5. In **gerepliceerde items**, met de rechtermuisknop op de virtuele machine > **volledige migratie**. Dit het migratieproces is voltooid en de replicatie voor de virtuele machine stopt.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een Azure-virtuele machine gemigreerd naar een ander Azure-regio. U kunt nu herstel na noodgevallen configureren voor de gemigreerde virtuele machine.

> [!div class="nextstepaction"]
> [Herstel na noodgevallen instellen na migratie](azure-to-azure-quickstart.md)

