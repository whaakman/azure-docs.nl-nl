---
title: Azure Virtual machines migreren tussen Azure-regio's met Azure Site Recovery | Microsoft Docs
description: Azure Site Recovery gebruiken voor het migreren van virtuele Azure IaaS-machines van de ene Azure-regio naar een andere.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 0460d595bc70ec09d492221485749ece32ec07df
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209705"
---
# <a name="migrate-azure-vms-to-another-region"></a>Virtuele Azure-machines migreren naar een andere regio

Behalve dat u de service [Azure Site Recovery](site-recovery-overview.md) kunt gebruiken om herstel na noodgevallen van on-premises machines en virtuele Azure-machines te beheren en te organiseren met als doel de bedrijfscontinuïteit te waarborgen, en deze service kunt gebruiken voor herstel na een noodgeval (BCDR), kunt u Site Recovery ook gebruiken om virtuele Azure-machines naar een secundaire regio te migreren. Als u virtuele Azure-machines wilt migreren, moet u replicatie voor deze machines inschakelen en er een failover voor uitvoeren naar de secundaire regio van uw keuze.

Deze zelfstudie laat zien hoe u virtuele Azure-machines naar een andere regio kunt migreren. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Recovery Services-kluis maken
> * Replicatie inschakelen voor een VM
> * Een failover uitvoeren om de virtuele machine te migreren

Voor deze zelfstudie wordt ervan uitgegaan dat u al een Azure-abonnement hebt. Als dat niet het geval is, moet u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken voordat u begint.

>[!NOTE]
>
> Replicatie van Site Recovery voor virtuele Azure-machines is momenteel in preview.



## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat u virtuele Azure-machines hebt in de Azure-regio waaruit u wilt migreren.
- Zorg ervoor dat u inzicht hebt in de [architectuur en onderdelen voor dit scenario](azure-to-azure-architecture.md).
- Controleer de [beperkingen en vereisten voor ondersteuning](azure-to-azure-support-matrix.md).



## <a name="before-you-start"></a>Voordat u begint

Voordat u replicatie instelt, moet u deze stappen voltooien.


### <a name="verify-target-resources"></a>Doelbronnen controleren

1. Controleer of u een Azure-abonnement hebt waarmee u in staat bent om virtuele machines te maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte.


### <a name="verify-account-permissions"></a>Accountmachtigingen controleren

Als u net pas uw gratis Azure-account hebt gemaakt, bent u de beheerder van uw abonnement. Als u niet de abonnementsbeheerder bent, neemt u contact op met de beheerder om de machtigingen te krijgen die u nodig hebt. Om replicatie in te kunnen schakelen voor een nieuwe virtuele machine, moet u het volgende hebben:

1. Machtigingen voor het maken van een virtuele machine in Azure-resources. De ingebouwde rol 'Inzender voor virtuele machines' beschikt over deze machtigingen, waaronder:
    - Machtiging voor het maken van een virtuele machine in de geselecteerde resourcegroep
    - Machtiging voor het maken van een virtuele machine in het geselecteerde virtuele netwerk
    - Machtigingen om naar het geselecteerde opslagaccount te schrijven

2. U moet ook zijn gemachtigd om Azure Site Recovery-bewerkingen te kunnen beheren. De rol 'Site Recovery-inzender' bevat alle machtigingen die nodig zijn om Site Recovery-bewerkingen in een Recovery Services-kluis te kunnen beheren.


### <a name="verify-vm-outbound-access"></a>Uitgaande toegang van de virtuele machine controleren

1. Zorg ervoor dat u geen verificatieproxy gebruikt om de verbinding met het netwerk te beheren voor virtuele machines die u wilt migreren. 
2. Voor deze zelfstudie gaan we ervan uit dat de virtuele machines die u wilt migreren toegang hebben tot internet en geen gebruik maken van een firewallproxy om de uitgaande toegang te beheren. Als dat wel het geval is, moet u de vereisten [hier](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity) controleren.

### <a name="verify-vm-certificates"></a>VM-certificaten controleren

Controleer of de meest recente basiscertificaten aanwezig zijn op de virtuele Azure-machines die u wilt migreren. Als de meest recente basiscertificaten niet aanwezig zijn, kan de virtuele machine vanwege veiligheidsbeperkingen niet bij Site Recovery worden geregistreerd.

- Voor Windows-VM’s moet u de meest recente Windows-updates op de VM installeren, zodat alle vertrouwde basiscertificaten op de machine aanwezig zijn. In een niet-verbonden omgeving moet u de standaardprocedures van Windows Update en de standaardprocedures voor het bijwerken van de certificaten van uw organisatie volgen.
- Voor Linux-VM’s volgt u de richtlijnen van de Linux-distributeur voor het verkrijgen van de meest recente basiscertificaten en de certificaatintrekkingslijst op de VM.



## <a name="create-a-vault"></a>Een kluis maken

Maak de kluis in elke gewenste regio, met uitzondering van de bronregio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
3. Bij **Naam** geeft u de beschrijvende naam **ContosoVMVault** op. Als u meer dan één abonnement hebt, selecteert u het gewenste abonnement.
4. Maak een resourcegroep met de naam **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Als u de kluis snel wilt kunnen openen via het dashboard, klikt u op **Vastmaken aan dashboard** en vervolgens op **Maken**.

   ![Nieuwe kluis](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

De nieuwe kluis wordt toegevoegd op het **Dashboard** onder **Alle resources** en op de hoofdpagina van **Recovery Services-kluizen**.






## <a name="select-the-source"></a>De bron selecteren

1. Klik in Recovery Services-kluizen op **ConsotoVMVault** > **+Repliceren**.
2. Bij **Bron** selecteert u **Azure**.
3. Bij **Bronlocatie** selecteert u de Azure-bronregio waar uw VM’s momenteel worden uitgevoerd.
4. Selecteer het Resource Manager-implementatiemodel. Selecteer vervolgens de **resourcegroep van de bron**.
5. Klik op **OK** om de instellingen op te slaan.


## <a name="enable-replication-for-azure-vms"></a>Replicatie voor virtuele Azure-machines inschakelen

In Site Recovery wordt een lijst opgehaald van de virtuele machines die zijn gekoppeld aan het abonnement en de resourcegroep.


1. Klik in Azure Portal op **Virtuele machines**.
2. Selecteer de virtuele machine die u wilt migreren. Klik vervolgens op **OK**.
3. Klik bij **Instellingen** op **Herstel na noodgeval**.
4. Selecteer in **Noodherstel configureren** > **Doelregio** de doelregio waarnaar u wilt repliceren.
5. Accepteer voor deze zelfstudie de overige standaardinstellingen.
6. Klik op **Replicatie inschakelen**. Hierdoor wordt een taak gestart voor het inschakelen van replicatie voor de VM.

    ![replicatie inschakelen](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="run-a-failover"></a>Een failover uitvoeren

1. Klik in **Instellingen** > **Gerepliceerde items** op de machine en klik vervolgens op **Failover**.
2. Bij **Failover** selecteert u **Meest recente**. De instelling voor de coderingssleutel is niet relevant in dit scenario.
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Controleer of de virtuele Azure-machine in Azure wordt weergegeven zoals verwacht.
5. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Doorvoeren**. Het migratieproces wordt voltooid.
6. Nadat het doorvoeren is voltooid, klikt u op **Replicatie uitschakelen**.  Hierdoor wordt de replicatie voor de virtuele machine beëindigd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een virtuele Azure-machine naar een andere Azure-regio gemigreerd. U kunt nu herstel na noodgevallen configureren voor de gemigreerde virtuele machine.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)

