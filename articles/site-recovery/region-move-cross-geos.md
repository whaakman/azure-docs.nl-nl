---
title: Azure IaaS VM's verplaatsen tussen Azure Government en openbare regio's met de Azure Site Recovery-service | Microsoft Docs
description: Gebruik Azure Site Recovery op Azure IaaS VM's verplaatsen tussen Azure Government en openbare regio's.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: fe2620c7a07389b2a86d36420eadd2ef5883f5da
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62119622"
---
# <a name="move-azure-vms-between-azure-government-and-public-regions"></a>Azure-VM's verplaatsen tussen Azure Government en openbare regio 's 

U kunt uw IaaS-VM's verplaatsen tussen Azure Government en openbare regio's om de beschikbaarheid van uw bestaande virtuele machines verhogen, beheerbaarheid, of voor governance redenen, zoals beschreven [hier](azure-to-azure-move-overview.md).

Behalve dat u de service [Azure Site Recovery](site-recovery-overview.md) kunt gebruiken om herstel na noodgevallen van on-premises machines en Azure-VM’s te beheren en te organiseren met als doel de bedrijfscontinuïteit te waarborgen, en deze service kunt gebruiken voor herstel na een noodgeval (BCDR), kunt u Site Recovery ook gebruiken om Azure-VM’s te verplaatsen naar een secundaire regio.       

Deze zelfstudie leert u hoe u Azure-VM's verplaatsen tussen Azure Government en openbare regio's met Azure Site Recovery. Hetzelfde kan worden uitgebreid voor het verplaatsen van virtuele machines tussen gekoppelde regio's die zich niet binnen hetzelfde geografische cluster. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vereisten verifiëren
> * De bron-VM’s voorbereiden
> * De doelregio voorbereiden
> * Gegevens kopiëren naar de doelregio
> * De configuratie testen
> * De verplaatsing uitvoeren
> * De resources in de bronregio verwijderen

> [!IMPORTANT]
> Deze zelfstudie leert u hoe u Azure-VM's verplaatsen tussen Azure Government en openbare regio's, of tussen regio's die niet worden ondersteund door de oplossing voor reguliere noodherstel voor Azure VM's. In het geval, uw bron- en regio's overeenkomsten zijn [ondersteund](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support), Raadpleeg dit [document](azure-to-azure-tutorial-migrate.md) voor de verplaatsing. Als uw vereiste is beschikbaarheid verbeteren door het verplaatsen van virtuele machines in een beschikbaarheidsset om aan zone vastgemaakte virtuele machines in een andere regio, raadpleegt u de zelfstudie [hier](move-azure-VMs-AVset-Azone.md).

> [!IMPORTANT]
> Het is niet raadzaam deze methode gebruiken om te configureren van herstel na Noodgevallen tussen gekoppelde niet-ondersteunde regio's, zoals de paren gegevenslatentie houden in gedachten, wat van essentieel belang voor herstel na noodgevallen zijn gedefinieerd.

## <a name="verify-prerequisites"></a>Vereisten verifiëren

> [!NOTE]
> Zorg ervoor dat u begrijpt de [architectuur en onderdelen](physical-azure-architecture.md) voor dit scenario. Deze architectuur wordt gebruikt voor het verplaatsen van virtuele Azure-machines, **door de virtuele machines als fysieke servers te behandelen**.

- Raadpleeg de [ondersteuningsvereisten](vmware-physical-secondary-support-matrix.md) voor alle onderdelen.
- Zorg ervoor dat de servers die u wilt repliceren, aan de voldoet [vereisten voor Azure VM's](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
- Een account voorbereiden voor automatische installatie van de Mobility-service op elke server die u wilt repliceren.

- Houd er rekening mee dat nadat u hebt de doelregio in Azure, mislukte aanmeldingen, u kunt niet rechtstreeks een failover naar de bronregio uitvoeren. U moet voor het instellen van replicatie weer terug naar het doel.

### <a name="verify-azure-account-permissions"></a>Controleer de machtigingen van de Azure-account

Zorg ervoor dat uw Azure-account heeft machtigingen voor replicatie van virtuele machines naar Azure.

- Controleer de [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) moet u virtuele machines repliceren naar Azure.
- Controleer en wijzig [op basis van de rol](../role-based-access-control/role-assignments-portal.md) machtigingen. 

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Instellen van een doel [Azure-netwerk](../virtual-network/quick-create-portal.md).

- Azure-VM's worden geplaatst in dit netwerk wanneer ze na een failover worden gemaakt.
- Het netwerk moet zich in dezelfde regio als de Recovery Services-kluis


### <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen

Instellen van een [Azure storage-account](../storage/common/storage-quickstart-create-account.md).

- Site Recovery repliceert on-premises machines naar Azure storage. Azure-VM's zijn gemaakt op basis van de opslag na failover.
- Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.


## <a name="prepare-the-source-vms"></a>De bron-VM’s voorbereiden

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke server die u wilt repliceren. Site Recovery installeert deze service automatisch wanneer u replicatie voor de server inschakelt. Voor het automatisch installeren, moet u een account voorbereiden waarmee Site Recovery wordt gebruikt voor toegang tot de server.

- U kunt een domein of lokale account gebruiken
- Voor Windows-VM's, als u niet een domeinaccount, schakel toegangsbeheer voor externe gebruikers op de lokale computer. Om dit te doen, in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, voegt u de DWORD-vermelding toe **LocalAccountTokenFilterPolicy**, met een waarde van 1.
- De registervermelding voor het uitschakelen van de instelling van een CLI toevoegen, typt u:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Voor Linux moet het account hoofdmap op de Linux-bronserver.


## <a name="prepare-the-target-region"></a>De doelregio voorbereiden

1. Controleer of u een Azure-abonnement hebt waarmee u in staat bent om virtuele machines te maken in de doelregio die wordt gebruikt voor herstel na noodgevallen. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

2. Zorg ervoor dat uw abonnement voldoende resources heeft die ondersteuning kunnen bieden voor VM’s met grootten die overeenkomen met uw bron-VM’s. Site Recovery kiest voor de doel-VM dezelfde of de dichtstbijzijnde grootte om gegevens naar het doel te kopiëren.

3. Zorg ervoor dat u een doelresource maakt voor elk onderdeel dat is geïdentificeerd in de bronindeling voor netwerken. Het is belangrijk om ervoor te zorgen dat, knippen boeken naar de doelregio, uw virtuele machines hebben de functionaliteit en functies die u in de bron hebt.

    > [!NOTE]
    > In Azure Site Recovery wordt automatisch een virtueel netwerk gedetecteerd en gemaakt wanneer u replicatie inschakelt voor de bron-VM. U kunt er ook voor kiezen om vooraf een netwerk te maken en dit toe te wijzen aan de VM in de gebruikersstroom om replicatie in te schakelen. Maar alle andere resources moet u zoals hieronder wordt beschreven handmatig maken in de doelregio.

     Raadpleeg de volgende documenten om de meest gebruikte netwerkresources te maken die relevant zijn voor u, op basis van de bron-VM-configuratie.

    - [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Load balancers](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    - [Openbare IP](https://docs.microsoft.com/azure/load-balancer/#step-by-step-tutorials)
    
    Voor andere netwerkonderdelen raadpleegt u de [netwerkondersteuningsdocumentatie.](https://docs.microsoft.com/azure/#pivot=products&panel=network) 

4. [Maak handmatig een niet-productienetwerk](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) in de doelregio als u de configuratie wilt testen voordat u het uiteindelijke knippen naar de doelregio uitvoert. Hiermee ontstaat er slechts minimale verstoring in de productie. Dit wordt aanbevolen.

## <a name="copy-data-to-the-target-region"></a>Gegevens kopiëren naar de doelregio
De onderstaande stappen begeleiden u bij het gebruik van Azure Site Recovery om gegevens te kopiëren naar de doelregio.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Maak de kluis in elke gewenste regio, met uitzondering van de bronregio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Klik op **Een resource maken** > **Beheerhulpprogramma's** > **Backup en Site Recovery**.
3. Bij **Naam** geeft u de beschrijvende naam **ContosoVMVault** op. Als u meer dan één abonnement hebt, selecteert u het juiste abonnement.
4. Maak een resourcegroep met de naam **ContosoRG**.
5. Geef een Azure-regio op. Zie Geografische beschikbaarheid in [Prijsinformatie voor Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/) om na te gaan welke regio's er worden ondersteund.
6. Klik in Recovery Services-kluizen op **overzicht** > **ConsotoVMVault** > **+ repliceren**
7. Selecteer **naar Azure** > **niet gevirtualiseerd/Overig**.

### <a name="set-up-the-configuration-server-to-discover-vms"></a>De configuratieserver instellen voor het detecteren van virtuele machines.


Instellen van de configuratieserver en VM's detecteren in de kluis te registreren.

1. Klik op **Site Recovery** > **infrastructuur voorbereiden** > **bron**.
2. Als u geen een configuratieserver, klikt u op **+ configuratieserver**.
3. In **-Server toevoegen**, controleert u of **configuratieserver** wordt weergegeven in **servertype**.
4. Download het installatiebestand van de geïntegreerde Setup van Site Recovery.
5. Download de registratiesleutel voor de kluis. U hebt deze nodig wanneer u geïntegreerde Setup uitvoert. De sleutel blijft vijf dagen na het genereren ervan geldig.

   ![Bron instellen](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>De configuratieserver in de kluis registreren

Het volgende doen voordat u begint: 

#### <a name="verify-time-accuracy"></a>Controleer of de nauwkeurigheid van de tijd
Op de configuratie van server, zorg ervoor dat de systeemklok is gesynchroniseerd met een [tijdserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Het moet overeenkomen. Als deze is 15 minuten voor of achter, de installatie mislukken.

#### <a name="verify-connectivity"></a>Controleer de verbinding
Zorg ervoor dat de computer toegang heeft tot deze URL's op basis van uw omgeving: 

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

IP-adressen gebaseerde firewallregels moeten communicatie met alle van de Azure-URL's die hierboven worden vermeld toestaan via HTTPS-poort (443). Om te vereenvoudigen en het IP-adresbereiken beperken, wordt aangeraden dat er een URL-filtering moet worden uitgevoerd.

- **Commerciële IP-adressen** -toestaan dat de [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653), en de poort HTTPS (443). Toestaan dat IP-adresbereiken voor de Azure-regio van uw abonnement voor de ondersteuning van de AAD, back-up, replicatie en URL's voor opslag.  
- **IP-adressen Government** -toestaan de [Azure Government IP-adresbereiken Datacenter](https://www.microsoft.com/en-us/download/details.aspx?id=57063), en de poort HTTPS (443) voor alle USGov-regio's (Virginia, Texas, Arizona en Iowa) voor de ondersteuning van AAD, back-up, replicatie en URL's voor opslag.  

#### <a name="run-setup"></a>Voer het installatieprogramma uit
Als een lokale beheerder voor het installeren van de configuratieserver voor geïntegreerde Setup uitgevoerd. De processerver en de hoofddoelserver worden ook standaard geïnstalleerd op de configuratieserver.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Nadat de registratie is voltooid, de configuratieserver wordt weergegeven op de **instellingen** > **Servers** pagina in de kluis.

### <a name="configure-target-settings-for-replication"></a>Doelinstellingen voor replicatie configureren

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het doel-implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

   ![Doel](./media/physical-azure-disaster-recovery/network-storage.png)


### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Klik op **Infrastructuur voor Site Recovery** > **Herstelbeleid** > **+Herstelbeleid** om een nieuw replicatiebeleid te maken.
2. Geef in **Replicatiebeleid maken** een beleidsnaam op.
3. Geef in **RPO-drempelwaarde** de limiet van de Recovery Point Objective (RPO) op. Deze waarde geeft aan hoe vaak gegevens herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
4. Geef in **Bewaarperiode van het herstelpunt** op hoelang (in uren) de bewaarperiode voor elk herstelpunt is. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster. Voor computers die worden gerepliceerd naar Premium Storage, wordt een bewaarperiode van maximaal 24 uur ondersteund, en 72 uur voor computers die naar Standard Storage worden gerepliceerd.
5. In **frequentie App-consistente momentopname**, opgeven hoe vaak (in minuten) de herstelpunten met toepassingsconsistente momentopnamen worden gemaakt. Klik op **OK** om het beleid te maken.

    ![Beleid voor replicatie](./media/physical-azure-disaster-recovery/replication-policy.png)


Het beleid wordt automatisch gekoppeld aan de configuratieserver. Standaard wordt automatisch een bijbehorend beleid gemaakt voor failback. Bijvoorbeeld, als het replicatiebeleid **repbeleid** vervolgens een failbackbeleid **repbeleid-failback** wordt gemaakt. Dit beleid wordt pas gebruikt als u een failback initieert vanuit Azure.

### <a name="enable-replication"></a>Replicatie inschakelen

- Site Recovery installeert de Mobility-service wanneer replicatie is ingeschakeld.
- Wanneer u replicatie voor een server inschakelt, duurt het 15 minuten of langer wijzigingen van kracht en wordt weergegeven in de portal.

1. Klik op **Toepassing repliceren** > **Bron**.
2. Selecteer in **Bron** de configuratieserver.
3. In **type Machine**, selecteer **fysieke machines**.
4. Selecteer de processerver (configuratieserver). Klik vervolgens op **OK**.
5. In **doel**, selecteer het abonnement en de resourcegroep waarin u wilt maken van de Azure VM's na een failover. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of resourcebeheer).
6. Selecteer het Azure-opslagaccount dat u wilt gebruiken voor het repliceren van gegevens. 
7. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
8. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren. 
9. In **fysieke Machines**, en klikt u op **fysieke machine**. Geef de naam en IP-adres. Selecteer het besturingssysteem van de computer die u wilt repliceren. Het duurt een paar minuten voor de servers die worden gedetecteerd en weergegeven. 

   > [!WARNING]
   > U moet het IP-adres van de Azure-VM die u van plan bent om te verplaatsen invoeren

10. In **eigenschappen** > **eigenschappen configureren**, selecteer het account dat wordt gebruikt door de processerver voor het installeren van de Mobility-service automatisch op de machine.
11. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**. 
12. Klik op **Replicatie inschakelen**. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.


Voor het controleren van servers die u toevoegt, kunt u de laatste detectietijd voor hen in controleren **configuratieservers** > **laatst Contact om**. Als u wilt toevoegen machines zonder te wachten op een tijdstip geplande detectie, markeert u de configuratieserver (klik er niet op), en klikt u op **vernieuwen**.

## <a name="test-the-configuration"></a>De configuratie testen


1. Ga naar de kluis, klik in **Instellingen** > **Gerepliceerde items** op de virtuele machine die u wilt verplaatsen naar de doelregio. Klik op het pictogram **+Failover testen**.
2. Selecteer in **Failover testen** een herstelpunt om voor de failover te gebruiken:

   - **Laatst verwerkt**: voert een failover van de VM uit naar het laatste herstelpunt dat is verwerkt door de Site Recovery-service. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is
   - **Laatste toepassingsconsistente punt**: met deze optie wordt er een failover uitgevoerd van alle VM's naar het laatste toepassingsconsistente herstelpunt. Het tijdstempel wordt weergegeven.
   - **Aangepast**: selecteer een herstelpunt.

3. Selecteer het Azure-doelnetwerk waar u de Azure VM's naartoe wilt verplaatsen, om de configuratie te testen. 

   > [!IMPORTANT]
   > U wordt aangeraden dat u een afzonderlijke Azure-VM-netwerk voor de testfailover en niet het productienetwerk waarnaar u wilt uw virtuele machines verplaatsen uiteindelijk die is ingesteld gebruiken toen u replicatie inschakelde.

4. Klik op **OK** om de verplaatsing te testen. Klik op de VM om de eigenschappen ervan te openen als u de voortgang wilt volgen. U kunt ook op de taak **Failover testen** klikken in kluisnaam > **Instellingen** > **Taken** > **Site Recovery-taken**.
5. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de VM draait, de juiste grootte heeft en aangesloten is op het juiste netwerk.
6. Als u de VM wilt verwijderen die is gemaakt als onderdeel van de verplaatsingstest, klikt u op **Failovertest wissen** op het gerepliceerde item. Leg in **Notities** eventuele opmerkingen over de test vast en sla deze op.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Voer de overstap naar de doelregio uit en bevestig dit.

1. Ga naar de kluis, klik in **Instellingen** > **Gerepliceerde items** op de virtuele machine en klik vervolgens op **Failover**.
2. Bij **Failover** selecteert u **Meest recente**. 
3. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery sluit de virtuele bronmachine af voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**. 
4. Nadat de taak is voltooid, controleert u of de VM zoals verwacht wordt weergegeven in de Azure-doelregio.
5. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Doorvoeren**. De verplaatsing naar de doelregio wordt voltooid. Wacht tot de doorvoertaak is voltooid.

## <a name="discard-the-resource-in-the-source-region"></a>Verwijder de resource in de bronregio 

- Navigeer naar de VM.  Klik op **Replicatie uitschakelen**.  Hiermee wordt het proces van het kopiëren van de gegevens voor de VM beëindigd.  

   > [!IMPORTANT]
   > Het is belangrijk dat u deze stap uitvoert om te vermijden dat kosten in rekening worden gebracht voor ASR-replicatie.

Als u van plan bent om een of meer van de bronresources opnieuw te gebruiken, gaat u verder met de volgende reeks stappen.

1. Ga verder met het verwijderen van alle relevante netwerkresources in de bronregio die u hebt vermeld als onderdeel van stap 4 in [De bron-VM’s voorbereiden](#prepare-the-source-vms) 
2. Verwijder het bijbehorende opslagaccount in de bronregio.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure-VM verplaatst naar een andere Azure-regio. U kunt nu herstel na noodgevallen configureren voor de verplaatste VM.

> [!div class="nextstepaction"]
> [Herstel na noodgeval instellen na een migratie](azure-to-azure-quickstart.md)
