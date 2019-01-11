---
title: Voorbereiden op back-up van virtuele Azure-machines met Azure Backup
description: Beschrijft hoe u virtuele Azure-machines voorbereiden voor back-up met de Azure Backup-service
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: a7a2d8729e1abdafa89eff912faf84d8f247b442
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215436"
---
# <a name="prepare-to-back-up-azure-vms"></a>Voorbereiden op back-up van virtuele Azure-machines

In dit artikel wordt beschreven hoe u voorbereiden op back-ups van een virtuele Azure-machine met een [Azure Backup](backup-introduction-to-azure-backup.md) Recovery Services-kluis. Voorbereiden voor back-up bevat:


> [!div class="checklist"]
> * Voordat u begint, ondersteunde scenario's en beperkingen controleren.
> * Controleer de vereisten, met inbegrip van Azure VM-vereisten en de netwerkverbinding.
> * Een kluis maken.
> * Selecteer hoe opslag wordt gerepliceerd.
> * VM's detecteren, instellingen voor back-up en het beleid configureren.
> * Back-up voor de geselecteerde virtuele machines inschakelen


> [!NOTE]
   > In dit artikel wordt beschreven hoe u back-up van virtuele Azure-machines door het instellen van een kluis en virtuele machines naar back-up selecteren. Dit is handig als u wilt back-up van meerdere virtuele machines. U kunt ook back-up van een virtuele Azure-machine rechtstreeks vanuit de instellingen van de virtuele machine. [Meer informatie](backup-azure-vms-first-look-arm.md)

## <a name="before-you-start"></a>Voordat u begint

1. [Bekijk een overzicht](backup-azure-vms-introduction.md) Azure back-up voor virtuele Azure-machines.
2. Bekijk details van bestandsondersteuning en beperkingen die hieronder.

   **Ondersteuning voor/beperking** | **Details**
   --- | ---
   **Windows-besturingssysteem** | Windows Server 2008 R2 64-bits of hoger.<br/><br/> Client voor Windows 7, 64-bits of hoger.
   **Linux-besturingssysteem** | U kunt back-up van Linux-distributies, 64-bits [ondersteund door Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), met uitzondering van CoreOS Linux.<br/><br/> Bekijk de Linux-besturingssystemen die [ondersteuning voor bestanden herstellen](backup-azure-restore-files-from-vm.md#for-linux-os).<br/><br/> Andere Linux-distributies mogelijk werken, als de VM-agent beschikbaar op de virtuele machine is en ondersteuning voor Python aanwezig. Maar worden niet deze distributies ondersteund.
   **Regio** | U kunt back-up van virtuele Azure-machines in alle [ondersteunde regio's](https://azure.microsoft.com/regions/#services). Als een regio niet ondersteund wordt, kunt u zich niet om deze te selecteren wanneer u de kluis maakt.<br/><br/> U kunt geen back-up en herstellen van Azure-regio's. Alleen binnen één regio.
   **Limiet van schijf** | U kunt geen back-up van virtuele machines met meer dan 16 gegevensschijven.
   **Gedeelde opslag** | U kunt beter geen back-ups van virtuele machines CSV- of Scale-Out bestandsserver. CSV-schrijvers zijn waarschijnlijk zal mislukken.
   **Linux-versleuteling** | Back-ups van virtuele Linux-machines met Linux Unified sleutel instellen (LUKS) versleuteld wordt niet ondersteund.
   **VM-consistentie** | Azure Backup biedt geen ondersteuning voor meerdere VM's.
   **Netwerken** | Back-ups van gegevens bevat geen gekoppeld netwerkstations die zijn gekoppeld aan een virtuele machine.<br/><br/>
   **Momentopnamen** | Maken van momentopnamen op een schijf write accelerator is ingeschakeld wordt niet ondersteund. Azure back-up van het maken van een app-consistente momentopname van alle VM-schijven worden geblokkeerd.
   **PowerShell** | Er zijn een aantal acties die alleen beschikbaar met PowerShell zijn:<br/><br/> -Herstellen van virtuele machines worden beheerd door interne of externe load balancers of met meerdere gereserveerde IP-adressen of adapters. [Meer informatie](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)<br/><br/> -Herstellen van een domeincontroller-VM in een configuratie van meerdere domeincontrollers. [Meer informatie](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
   **Wanneer het systeem** | Azure Backup biedt geen ondersteuning voor automatische clock correctie voor zomer-en wintertijd wijzigingen voor back-ups van virtuele Azure-machine. Back-upbeleid handmatig zo nodig wijzigen.
   **Opslagaccounts** | Als u een opslagaccount van het netwerk beperkte, zorgt u ervoor dat u inschakelt **vertrouwde Microsoft-services voor toegang tot dit storage-account toestaan** zodat Azure Backup-service toegang heeft tot het account. Herstel op itemniveau wordt niet ondersteund voor het netwerk beperkte opslagaccounts.<br/><br/> Controleer of in een storage-account **Firewalls en virtuele netwerken** instellingen zodat toegang vanaf **alle netwerken**.


## <a name="prerequisites"></a>Vereisten

- U moet de kluis in dezelfde regio als de Azure VM's die u wilt dat back-up te maken.
- Voordat u begint, controleert u de virtuele machine van Azure-regio's.
    - Als u virtuele machines in meerdere regio's hebt, maakt u een kluis in elke regio.
    - U hoeft niet te geven van de storage-accounts voor het opslaan van de back-upgegevens. De kluis en de Azure Backup-service worden verwerkt die automatisch.
- Controleren of de VM-agent is geïnstalleerd op Azure VM's die u back wilt-up.



### <a name="install-the-vm-agent"></a>De VM-agent installeren

Om in te schakelen back-up, installeert Azure Backup een Backup-extensie (VM-momentopname of VM-momentopname Linux) voor de VM-agent die wordt uitgevoerd op de virtuele machine van Azure.
    -  De Azure VM-Agent is standaard geïnstalleerd op een Windows-VM die door een Azure Marketplace-installatiekopie. Wanneer u een Azure Marketplace-installatiekopie uit de portal, PowerShell, CLI of een Azure Resource Manager-sjabloon implementeert, wordt ook de Azure VM-Agent geïnstalleerd.
    - Als u een virtuele machine hebt gemigreerd van on-premises, wordt de agent is niet geïnstalleerd en u moet deze installeren voordat u back-up voor de virtuele machine inschakelen kunt.

Installeer de agent als volgt, indien nodig.

**VM** | **Details**
--- | ---
**Virtuele Windows-machines** | [Download en installeer](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) de agent met beheerdersmachtigingen op de machine.<br/><br/> Om te controleren of de installatie *C:\WindowsAzure\Packages* op de virtuele machine, met de rechtermuisknop op de WaAppAgent.exe > **eigenschappen**, > **Details** tabblad. **Versie van het product** moet 2.6.1198.718 of hoger.
**Virtuele Linux-machines** | Installatie met behulp van een RPM- of DEB-pakket uit de opslagplaats voor uw distributie van pakket is de voorkeursmethode voor het installeren en upgraden van de Azure Linux Agent. Alle de [distributie-providers die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën. De agent is beschikbaar op [GitHub](https://github.com/Azure/WALinuxAgent), maar wordt niet aanbevolen van daaruit installeren.
Als u problemen met back-ups van de Azure-VM hebt, gebruikt u de volgende tabel om te controleren of de Azure VM-agent correct is geïnstalleerd op de virtuele machine. De tabel bevat aanvullende informatie over de VM-agent voor Windows en Linux-machines.

### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

De back-up-extensie die wordt uitgevoerd op de virtuele machine moet uitgaande toegang tot Azure openbare IP-adressen hebben. Om toegang te verlenen, kunt u het volgende doen:


- **NSG-regels**: Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). U kunt een regel waarmee toegang tot de Azure Backup-service met behulp toevoegen een [servicetag](../virtual-network/security-overview.md#service-tags), in plaats van afzonderlijk zodat elk-adresbereik, en het beheer na verloop van tijd.
- **Proxy**: Implementeer een HTTP-proxy-server voor het routeren van verkeer.
- **Firewall van Azure**: Verkeer via de Azure-Firewall op de virtuele machine, met behulp van een FQDN-tag voor de Azure Backup-service toestaan.

Wanneer u beslist tussen de opties, houd rekening met het gebruik van systeembronnen.

**Optie** | **Voordelen** | **Nadelen**
--- | --- | ---
**NSG** | Geen extra kosten. Eenvoudig te beheren met servicetags | Biedt toegang tot het geheel van Azure en niet alleen opslag. |
**HTTP-proxy** | Nauwkeurige controle over de URL's voor opslag is toegestaan.<br/><br/> Toegang tot één punt van internet voor VM's.<br/><br/> Extra kosten voor de proxy.
**FQDN-tags** | Eenvoudig te gebruiken als u Azure-Firewall instellen in een VNet-subnet | Kan maken van uw eigen labels FQDN-naam, of FQDN-namen wijzigen in een tag.



Als u Azure Managed Disks gebruikt, moet u mogelijk een extra poort openen (poort 8443) op de firewalls.



### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Instellen van een NSG-regel waarmee uitgaande toegang tot Azure

Als uw Azure-VM toegang tot die worden beheerd door een NSG heeft, uitgaande toegang voor de back-upopslag naar het vereiste bereik en de poorten toestaat.



1. In de virtuele machine > **netwerken**, klikt u op **regel voor uitgaande poort toevoegen**.
- Hebt u een regel voor weigeren van toegang, toestaan de nieuwe regel moet hoger zijn. Als u hebt bijvoorbeeld een **Deny_All** regel instellen op prioriteit van 1000, de nieuwe regel moet worden ingesteld op minder dan 1000.
2. In **uitgaande beveiligingsregel toevoegen**, klikt u op **Geavanceerd**.
3. Selecteer in de bron, **VirtualNetwork**.
4. In **poortbereiken van bron**, type in een sterretje (*) zodat uitgaande toegang vanaf een willekeurige poort.
5. In **bestemming**, selecteer **servicetag**. Selecteer de opslag in de lijst. <region>. De regio is de regio waarin de kluis en de virtuele machines die u back wilt-up moet zich bevinden.
6. In **poortbereiken van doel**, selecteert u de poort.

    - Virtuele machine met niet-beheerde schijven en niet-versleuteld opslagaccount: 80
    - Virtuele machine met niet-beheerde schijven en versleutelde storage-account: 443 (standaardinstelling)
    - Beheerde virtuele machine: 8443.
1. In **Protocol**, selecteer **TCP**.
2. In **prioriteit**, wijs hieraan een prioriteitswaarde kleiner is dan een hoger weigeren regels.
3. Geef een naam en beschrijving voor de regel en klikt u op **OK**.

U kunt de NSG-regel toepassen op meerdere virtuele machines waarmee uitgaande toegang tot Azure voor Azure Backup.

Deze video helpt u bij het proces.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]



### <a name="route-backup-traffic-through-a-proxy"></a>Back-verkeer routeren via een proxy

U kunt back-verkeer via een proxy omleiden en vervolgens de Proxytoegang geven tot de vereiste Azure bereiken.

U moet uw proxy-VM om toe te staan de volgende configureren:

- De Azure-VM moet alle afhankelijke voor het openbare internet via de proxy HTTP-verkeer routeren.
- De proxy moet binnenkomend verkeer toestaan van virtuele machines in het virtuele netwerk (VNet).
- De NSG **NSF-lockdown** moet een regel waarmee het uitgaande internetverkeer van de proxy-VM.

Dit is hoe u nodig hebt voor het instellen van de proxy. Wij gebruiken voorbeeldwaarden. U moet deze vervangen door uw eigen.

#### <a name="set-up-a-system-account-proxy"></a>Een systeem-account-proxy niet instellen
Als u een systeemproxy-account hebt, stelt een als volgt:

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Voer **PsExec.exe -i -s cmd.exe** naar de opdrachtprompt met een systeemaccount worden uitgevoerd.
3. De browser wordt uitgevoerd in de systeemcontext. Bijvoorbeeld: **ProgramFiles%\Internet Explorer\iexplore.exe** voor Internet Explorer.  
4. Definieer de proxyinstellingen.
    - Op Linux-machines:
        - Deze regel toe te voegen de **/etc/omgeving** bestand:
            - **gebruikt =http://proxy IP-adres: proxypoort**
        - Deze Voeg regels toe aan de **/etc/waagent.conf** bestand:
            - **HttpProxy.Host=proxy IP-adres**
            - **HttpProxy.Port=proxy poort**
    - Opgeven dat een proxy moet worden gebruikt op Windows-machines in de browserinstellingen. Als u een proxy die momenteel op een gebruikersaccount dat is gebruikt, kunt u dit script gebruiken om toe te passen van de instelling op het niveau van het systeem-account.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

#### <a name="allow-incoming-connections-on-the-proxy"></a>Binnenkomende verbindingen op de proxy toestaan

1. Binnenkomende verbindingen toestaan in de proxy-instellingen.
2. Open bijvoorbeeld **Windows Firewall met geavanceerde beveiliging**.
    - Met de rechtermuisknop op **regels voor binnenkomende verbindingen** > **nieuwe regel**.
    - In **regeltype** Selecteer **aangepaste** > **volgende**.
    - In **programma**, selecteer **alle programma's** > **volgende**.
    - In **protocollen en poorten** het type instellen op **TCP**, **lokale poorten** naar **specifieke poorten**, en **externe poort**naar **alle poorten**.
    - Voltooi de wizard en geef een naam voor de regel.

#### <a name="add-an-exception-rule-to-the-nsg"></a>Een uitzonderingsregel toevoegen aan de NSG

Op de NSG **NSF-lockdown**, verkeer van een willekeurige poort op 10.0.0.5 naar een internetadres op poort 80 (HTTP) of 443 (HTTPS) toestaan.

- De volgende PowerShell-script toont een voorbeeld van het verkeer wordt toegestaan.
- In plaats van toegestaan uitgaand naar alle openbare internet-adressen, kunt u een IP-adresbereik (-DestinationPortRange), of gebruik de servicetag storage.region.   

    ```
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```
### <a name="allow-firewall-access-with-fqdn-tag"></a>Firewalltoegang met FQDN-naam tag toestaan

U kunt de Azure-Firewall instellen waarmee uitgaande toegang voor het netwerkverkeer naar back-up van Azure.

- [Meer informatie over](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Firewall van Azure implementeren.
- [Meer informatie over](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN tags.


## <a name="create-a-vault"></a>Een kluis maken

Een Recovery Services-kluis voor back-up back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen en slaat de back-upbeleid dat is gekoppeld aan back-ups van machines. Maak een kluis als volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Op de **Hub** in het menu **Bladeren**, en het type **herstelservices**. Als u te typen begint, filtert de invoer van de lijst met resources. Selecteer **Recovery Services-kluizen**.

    ![In het vak te typen en "Recovery Services-kluizen" in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    De lijst met Recovery Services-kluizen wordt weergegeven.
3. Op de **Recovery Services-kluizen** in het menu **toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    De **Recovery Services-kluizen** deelvenster wordt geopend. Deze gevraagd om informatie voor **naam**, **abonnement**, **resourcegroep**, en **locatie**.

    ![Deelvenster "Recovery Services-kluizen"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden.
    - De naam moet uniek zijn voor het Azure-abonnement.
    - 2 en 50 tekens kan bevatten.
    - Deze moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.
5. Selecteer **abonnement** om te zien van de lijst met beschikbare abonnementen. Als u niet zeker weet welk abonnement u moet gebruiken, gebruikt u de standaard (of voorgestelde) abonnement. Er zijn meerdere opties alleen als uw werk of school-account is gekoppeld aan meerdere Azure-abonnementen.
6. Selecteer **resourcegroep** om te zien van de beschikbare lijst met resourcegroepen, of selecteer **nieuw** om een nieuwe resourcegroep te maken. [Meer informatie](../azure-resource-manager/resource-group-overview.md) over resourcegroepen.
7. Selecteer **locatie** om te selecteren van de geografische regio voor de kluis. De kluis *moet* zich in dezelfde regio als de virtuele machines die u back wilt-up.
8. Selecteer **Maken**.
    - Het kan even duren voor de kluis is gemaakt.
    - Houd de statusmeldingen in de rechterbovenhoek van de portal.
    ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Nadat de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.

## <a name="set-up-storage-replication"></a>Storage-replicatie instellen

Uw kluis heeft standaard [geografisch redundante opslag (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). GRS wordt aanbevolen voor uw primaire back-up, maar u kunt[lokaal redundante opslag](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor een goedkopere optie. 

Storage-replicatie als volgt wijzigen:

1. In de kluis > **back-upinfrastructuur**, klikt u op **back-upconfiguratie**

   ![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/full-blade.png)

2. In **back-upconfiguratie**, wijzigen van de methode van de redundantie opslag als vereist, en selecteer **opslaan**.


## <a name="configure-backup"></a>Back-up configureren

VM's detecteren in het abonnement en back-up configureren.

1. In de kluis > **overzicht**, klikt u op **+ back-up**

   ![Back-knop](./media/backup-azure-arm-vms-prepare/backup-button.png)

   De **back-up** en **back-updoel** deelvensters openen.

2. In **back-updoel**> **waar wordt uw werkbelasting uitgevoerd?**, selecteer **Azure**. In **waarvan wilt u back-up maken?**, selecteer **virtuele machine** >  **OK**. Hiermee wordt de VM-extensie in de kluis geregistreerd.

   ![Deelvensters van de back-ups en back-updoel](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Deze stap wordt de VM-extensie bij de kluis geregistreerd. De **back-updoel** deelvenster wordt gesloten, en de **back-upbeleid** deelvenster wordt geopend.

3. In **back-upbeleid**, selecteert u het beleid dat u wilt koppelen aan de kluis. Klik vervolgens op **OK**.
    - De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven.
    - Klik op **nieuw** om een beleid te maken. [Meer informatie](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) over het definiëren van een beleid.

    !["Backup" en "Back-upbeleid" deelvensters](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. In **virtuele machines selecteren** deelvenster, selecteert u de virtuele machines met het opgegeven back-upbeleid > **OK**.

    - De geselecteerde virtuele machine wordt gevalideerd.
    - U kunt alleen virtuele machines selecteren in dezelfde regio als de kluis. VM's kunnen alleen worden gemaakt op een enkele kluis.

   ![Deelvenster 'Virtuele machines selecteren'](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. In **back-up**, selecteer **back-up inschakelen**.

   - Dit wordt het beleid geïmplementeerd naar de kluis en de virtuele machines en de back-upextensie op de VM-agent die wordt uitgevoerd op de Azure-VM wordt geïnstalleerd.
   - Deze stap maken niet het eerste herstelpunt voor de virtuele machine.

   ![Knop 'Back-up inschakelen'](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Na het inschakelen van back-up:

- Het back-upbeleid wordt uitgevoerd in overeenstemming met uw back-upschema.
- De Backup-service installeert de back-upextensie, ongeacht of de virtuele machine wordt uitgevoerd of niet.
    - Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen.
    -  Echter, de virtuele machine is back-ups, zelfs als deze uitgeschakeld en de extensie kan niet worden geïnstalleerd. Dit staat bekend als *offline VM*. In dit geval is het herstelpunt *crashconsistent*.
- Als u wilt genereren van een on-demand back-up voor de virtuele machine onmiddellijk in **back-Upitems**, klikt u op het weglatingsteken (...) naast de virtuele machine > **back-up nu**.


## <a name="next-steps"></a>Volgende stappen

- Oplossen van problemen die optreden bij de [Azure VM agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) of [Azure VM backup](backup-azure-vms-troubleshoot.md).
- [Back-ups maken van Azure-VM's](backup-azure-vms-first-look-arm.md)
