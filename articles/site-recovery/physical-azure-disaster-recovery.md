---
title: Instellen van herstel na noodgevallen naar Azure voor fysieke on-premises servers met Azure Site Recovery | Microsoft Docs
description: Meer informatie over het instellen van herstel na noodgevallen naar Azure voor on-premises Windows en Linux-servers, met de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 6f2b4a83d4c13dcb866737d27c33b4f5a95c2f94
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078610"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Herstel na noodgevallen naar Azure voor on-premises fysieke servers instellen

De [Azure Site Recovery](site-recovery-overview.md)-service draagt bij aan uw strategie voor herstel na noodgevallen door de replicatie, failover en failback van on-premises machines en virtuele Azure-machines te beheren en in te delen.

Deze zelfstudie leert u hoe u herstel na noodgevallen van on-premises fysieke Windows en Linux-servers naar Azure instelt. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Azure en on-premises vereisten instellen
> * Maakt een Recovery Services-kluis voor Site Recovery 
> * Instellen van de bron en doel van de replicatie-omgevingen
> * Een replicatiebeleid maken
> * Replicatie inschakelen voor een server

[architectuur](concepts-hyper-v-to-azure-architecture.md) voor dit noodherstelscenario.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Zorg ervoor dat u begrijpt de [architectuur en onderdelen](physical-azure-architecture.md) voor dit scenario.
- Raadpleeg de [ondersteuningsvereisten](vmware-physical-secondary-support-matrix.md) voor alle onderdelen.
- Zorg ervoor dat de servers die u wilt repliceren, aan de voldoet [vereisten voor Azure VM's](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
- Azure voorbereiden. U hebt een Azure-abonnement, een Azure-netwerk en een storage-account nodig.
- Een account voorbereiden voor automatische installatie van de Mobility-service op elke server die u wilt repliceren.

Voordat u begint, houd er rekening mee dat:

- Na een failover naar Azure, kunnen geen fysieke servers naar on-premises fysieke computers mogelijk is. U kunt alleen een failback naar virtuele VMware-machines. 
- In deze zelfstudie stelt u de fysieke server-noodherstel naar Azure met de meest eenvoudige instellingen. Als u meer informatie over andere opties wilt, leest u via onze richtlijnen How To:
    - Instellen van de [replicatiebron](physical-azure-set-up-source.md), met inbegrip van de Site Recovery-configuratieserver.
    - Stel het [replicatiedoel](physical-azure-set-up-target.md) in.
    - Configureer een [replicatiebeleid](vmware-azure-set-up-replication.md) en schakel [replicatie in](vmware-azure-enable-replication.md).


### <a name="set-up-an-azure-account"></a>Een Azure-account instellen

Ophalen van een Microsoft [Azure-account](http://azure.microsoft.com/).

- U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
- Meer informatie over [prijzen voor Site Recovery](site-recovery-faq.md#pricing), en ontvang [prijsinformatie](https://azure.microsoft.com/pricing/details/site-recovery/).
- Ontdek welke [regio's worden ondersteund](https://azure.microsoft.com/pricing/details/site-recovery/) voor Site Recovery.

### <a name="verify-azure-account-permissions"></a>Controleer de machtigingen van de Azure-account

Zorg ervoor dat uw Azure-account heeft machtigingen voor replicatie van virtuele machines naar Azure.

- Controleer de [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) moet u virtuele machines repliceren naar Azure.
- Controleer en wijzig [op basis van de rol](../role-based-access-control/role-assignments-portal.md) machtigingen. 



### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Instellen van een [Azure-netwerk](../virtual-network/quick-create-portal.md).

- Azure-VM's worden geplaatst in dit netwerk wanneer ze na een failover worden gemaakt.
- Het netwerk moet zich in dezelfde regio als de Recovery Services-kluis


## <a name="set-up-an-azure-storage-account"></a>Een Azure-opslagaccount instellen

Instellen van een [Azure storage-account](../storage/common/storage-quickstart-create-account.md).

- Site Recovery repliceert on-premises machines naar Azure storage. Azure-VM's zijn gemaakt op basis van de opslag na failover.
- Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis.
- Het opslagaccount kan standaard worden of [premium](../virtual-machines/windows/premium-storage.md).
- Als u een premium-account hebt ingesteld, moet u ook een extra standard-account voor de logboekgegevens.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op elke server die u wilt repliceren. Site Recovery installeert deze service automatisch wanneer u replicatie voor de server inschakelt. Voor het automatisch installeren, moet u een account voorbereiden waarmee Site Recovery wordt gebruikt voor toegang tot de server.

- U kunt een domein of lokale account gebruiken
- Voor Windows-VM's, als u niet een domeinaccount, schakel toegangsbeheer voor externe gebruikers op de lokale computer. Om dit te doen, in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, voegt u de DWORD-vermelding toe **LocalAccountTokenFilterPolicy**, met een waarde van 1.
- De registervermelding voor het uitschakelen van de instelling van een CLI toevoegen, typt u:       ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Voor Linux moet het account hoofdmap op de Linux-bronserver.


## <a name="create-a-vault"></a>Een kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Een beveiligingsdoel selecteren

Selecteer wat om te repliceren en deze naar te repliceren.

1. Klik op **Recovery Services-kluizen** > kluis.
2. Klik in het resourcemenu op **Site Recovery** > **Infra structuur voorbereiden** > **Beveiligingsdoel**.
3. In **beveiligingsdoel**, selecteer **naar Azure** > **niet gevirtualiseerd/Overig**.

## <a name="set-up-the-source-environment"></a>De bronomgeving instellen

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

#### <a name="run-setup"></a>Het installatieprogramma uitvoert
Als een lokale beheerder voor het installeren van de configuratieserver voor geïntegreerde Setup uitgevoerd. De processerver en de hoofddoelserver worden ook standaard geïnstalleerd op de configuratieserver.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Nadat de registratie is voltooid, de configuratieserver wordt weergegeven op de **instellingen** > **Servers** pagina in de kluis.

## <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. Klik op **Infrastructuur voorbereiden** > **Doel** en selecteer het Azure-abonnement dat u wilt gebruiken.
2. Geef het doel-implementatiemodel.
3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.

   ![Doel](./media/physical-azure-disaster-recovery/network-storage.png)


## <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. Klik op om een nieuw replicatiebeleid **infrastructuur voor Site Recovery** > **replicatiebeleid** > **+ replicatiebeleid**.
2. In **replicatiebeleid maken**, een beleidsnaam opgeven.
3. In **RPO-drempelwaarde**, de objective (RPO) herstelpuntlimiet opgeven. Deze waarde geeft aan hoe vaak gegevens herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
4. In **bewaarperiode voor herstelpunten**, opgeven hoe lang (in uren) de bewaarperiode voor elk herstelpunt. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster. Bewaarperiode van 24 uur maximaal wordt ondersteund voor computers die worden gerepliceerd naar premium storage en 72 uur voor standard-opslag.
5. In **frequentie App-consistente momentopname**, opgeven hoe vaak (in minuten) de herstelpunten met toepassingsconsistente momentopnamen worden gemaakt. Klik op **OK** om het beleid te maken.

    ![Beleid voor replicatie](./media/physical-azure-disaster-recovery/replication-policy.png)


Het beleid wordt automatisch gekoppeld aan de configuratieserver. Standaard wordt automatisch een bijbehorend beleid gemaakt voor failback. Bijvoorbeeld, als het replicatiebeleid **repbeleid** vervolgens een failbackbeleid **repbeleid-failback** wordt gemaakt. Dit beleid wordt pas gebruikt als u een failback initieert vanuit Azure.

## <a name="enable-replication"></a>Replicatie inschakelen

Schakel replicatie voor elke server.

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
10. In **eigenschappen** > **eigenschappen configureren**, selecteer het account dat wordt gebruikt door de processerver voor het installeren van de Mobility-service automatisch op de machine.
11. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**. 
12. Klik op **Replicatie inschakelen**. U kunt de voortgang van de taak **Beveiliging inschakelen** volgen via **Instellingen** > **Taken** > **Site Recovery-taken**. Nadat de taak **Beveiliging voltooien** is uitgevoerd, is de machine klaar voor een mogelijke failover.


Voor het controleren van servers die u toevoegt, kunt u de laatste detectietijd voor hen in controleren **configuratieservers** > **laatst Contact om**. Als u wilt toevoegen machines zonder te wachten op een tijdstip geplande detectie, markeert u de configuratieserver (klik er niet op), en klikt u op **vernieuwen**.

## <a name="next-steps"></a>Volgende stappen

[Een Dr-herstelanalyse uitvoeren](tutorial-dr-drill-azure.md).
