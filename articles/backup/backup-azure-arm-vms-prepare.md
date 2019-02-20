---
title: Back-up van virtuele Azure-machines in een Recovery Services-kluis met Azure Backup
description: Hierin wordt beschreven hoe u back-up van virtuele Azure-machines in een Recovery Services-kluis met Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: 0f522897f3d3b3261045f1c14387af53ebf4ad9d
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429584"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Back-up van virtuele Azure-machines in een Recovery Services-kluis

In dit artikel wordt beschreven hoe u back-ups van virtuele Azure-machine met behulp van een [Azure Backup](backup-overview.md) door te implementeren en back-up in een Recovery Services-kluis in te schakelen. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Controleer of de ondersteunde scenario's en vereisten.
> * Azure virtuele machines voorbereiden. Installeer de Azure VM-agent indien nodig, en controleer of uitgaande toegang voor virtuele machines.
> * Een kluis maken.
> * Opslag voor de kluis instellen
> * VM's detecteren, instellingen voor back-up en het beleid configureren.
> * Back-up inschakelen voor virtuele Azure-machines


> [!NOTE]
   > In dit artikel wordt beschreven hoe u back-up van virtuele Azure-machines door het instellen van een kluis en virtuele machines naar back-up selecteren. Dit is handig als u wilt back-up van meerdere virtuele machines. U kunt ook [maakt u een back-up van een Azure-VM](backup-azure-vms-first-look-arm.md) rechtstreeks vanuit de instellingen van de virtuele machine.

## <a name="before-you-start"></a>Voordat u begint

Azure Backup back-ups van virtuele Azure-machines door het installeren van een extensie op de Azure VM-agent wordt uitgevoerd op de machine.

1. [Beoordeling](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM-back-architectuur.
[Meer informatie over](backup-azure-vms-introduction.md) back-up van virtuele Azure-machine en de back-upextensie.
2. [Bekijk de ondersteuningsmatrix](backup-support-matrix-iaas.md) voor Azure VM backup.
3. Azure virtuele machines voorbereiden. De VM-agent installeren als deze niet is geïnstalleerd en controleer of uitgaande toegang voor VM's die u back wilt-up.


## <a name="prepare-azure-vms"></a>Azure virtuele machines voorbereiden

Installeer de VM-agent indien nodig, en controleer of uitgaande toegang van virtuele machines.

### <a name="install-the-vm-agent"></a>De VM-agent installeren 
Installeer de agent als volgt, indien nodig.

**VM** | **Details**
--- | ---
**Virtuele Windows-machines** | [Download en installeer](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) het agent-MSI-bestand. Installeren met beheerdersmachtigingen op de machine.<br/><br/> Om te controleren of de installatie *C:\WindowsAzure\Packages* op de virtuele machine, met de rechtermuisknop op de WaAppAgent.exe > **eigenschappen**, > **Details** tabblad. **Versie van het product** moet 2.6.1198.718 of hoger.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerkingen worden uitgevoerd, en [Installeer de agent opnieuw](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Virtuele Linux-machines** | Installatie met behulp van een RPM- of DEB-pakket uit de opslagplaats voor uw distributie van pakket is de voorkeursmethode voor het installeren en upgraden van de Azure Linux Agent. Alle de [distributie-providers die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën. De agent is beschikbaar op [GitHub](https://github.com/Azure/WALinuxAgent), maar wordt niet aanbevolen van daaruit installeren.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerking wordt uitgevoerd, en bijwerken van de binaire bestanden. 


### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

De back-up-extensie die wordt uitgevoerd op de virtuele machine moet uitgaande toegang tot Azure openbare IP-adressen hebben.

- Er is geen expliciete uitgaande toegang is vereist voor virtuele Azure-machine om te communiceren met Azure Backup-Service.
- Echter bepaalde oudere virtuele machines mogelijk ervaart mogelijk problemen en mislukt met de fout **ExtensionSnapshotFailedNoNetwork** bij een poging om verbinding te maken. In dit geval, gebruik een van de volgende opties zodat de back-upextensie met Azure openbare IP-adressen voor back-upverkeer communiceren kan.

   **Optie** | **Actie** ** | **Voordelen** | **Nadelen**
   --- | --- | --- | ---
   **Instellen van NSG-regels** | Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/>  U kunt een regel waarmee toegang tot de Azure Backup-service met behulp toevoegen een [servicetag](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure), in plaats van afzonderlijk zodat en beheren van elk-adresbereik. [Meer informatie](../virtual-network/security-overview.md#service-tags) over service-tags. | Geen extra kosten. Eenvoudig te beheren met servicetags
   **Een proxy implementeren** | Implementeer een HTTP-proxy-server voor het routeren van verkeer. | Biedt toegang tot het geheel van Azure en niet alleen opslag. Nauwkeurige controle over de URL's voor opslag is toegestaan.<br/><br/> Toegang tot één punt van internet voor VM's.<br/><br/> Extra kosten voor de proxy.<br/><br/> 
   **Azure-Firewall instellen** | Verkeer via de Azure-Firewall op de virtuele machine, met behulp van een FQDN-tag voor de Azure Backup-service toestaan.|  Eenvoudig te gebruiken als u Azure-Firewall instellen in een VNet-subnet | Kan maken van uw eigen labels FQDN-naam, of FQDN-namen wijzigen in een tag.<br/><br/> Als u Azure Managed Disks gebruikt, moet u mogelijk een extra poort openen (poort 8443) op de firewalls.

#### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Instellen van een NSG-regel waarmee uitgaande toegang tot Azure

Als uw Azure-VM toegang tot die worden beheerd door een NSG heeft, uitgaande toegang voor de back-upopslag naar het vereiste bereik en de poorten toestaat.

1. In de virtuele machine > **netwerken**, klikt u op **regel voor uitgaande poort toevoegen**.
2. In **uitgaande beveiligingsregel toevoegen**, klikt u op **Geavanceerd**.
3. In **bron**, selecteer **VirtualNetwork**.
4. In **poortbereiken van bron**, type in een sterretje (*) zodat uitgaande toegang vanaf een willekeurige poort.
5. In **bestemming**, selecteer **servicetag**. Selecteer in de lijst **Storage.region**. De regio is de regio waarin de kluis en de virtuele machines die u back wilt-up moet zich bevinden.
6. In **poortbereiken van doel**, selecteert u de poort.
    - Niet-beheerde virtuele machine met niet-versleuteld opslagaccount: 80
    - Niet-beheerde virtuele machine met versleutelde storage-account: 443 (standaardinstelling)
    - Beheerde virtuele machine: 8443.
7. In **Protocol**, selecteer **TCP**.
8. In **prioriteit**, Geef een prioriteitswaarde kleiner is dan een hoger weigeren regels. Hebt u een regel voor weigeren van toegang, toestaan de nieuwe regel moet hoger zijn. Als u hebt bijvoorbeeld een **Deny_All** regel instellen op prioriteit van 1000, de nieuwe regel moet worden ingesteld op minder dan 1000.
9. Geef een naam en beschrijving voor de regel en klikt u op **OK**.

U kunt de NSG-regel toepassen op meerdere virtuele machines om uitgaande toegang te verlenen.

Deze video helpt u bij het proces.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


#### <a name="route-backup-traffic-through-a-proxy"></a>Back-verkeer routeren via een proxy

U kunt back-verkeer via een proxy omleiden en vervolgens de Proxytoegang geven tot de vereiste Azure bereiken.
U moet uw proxy-VM om toe te staan de volgende configureren:

- De Azure-VM moet alle afhankelijke voor het openbare internet via de proxy HTTP-verkeer routeren.
- De proxy moet binnenkomend verkeer toestaan van virtuele machines in het virtuele netwerk (VNet).
- De NSG **NSF-lockdown** moet een regel waarmee het uitgaande internetverkeer van de proxy-VM.

##### <a name="set-up-the-proxy"></a>De proxy niet instellen
Als u een systeemproxy-account hebt, stelt een als volgt:

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).

2. Voer **PsExec.exe -i -s cmd.exe** naar de opdrachtprompt met een systeemaccount worden uitgevoerd.
3. De browser wordt uitgevoerd in de systeemcontext. Bijvoorbeeld: **PROGRAMFILES%\Internet Explorer\iexplore.exe** for Internet Explorer.  
4. Definieer de proxyinstellingen.
    - Op Linux-machines:
        - Deze regel toe te voegen de **/etc/omgeving** bestand:
            - **gebruikt =http://proxy IP-adres: proxypoort**
        - Deze Voeg regels toe aan de **/etc/waagent.conf** bestand:
            - **HttpProxy.Host=proxy IP-adres**
            - **HttpProxy.Port=proxy port**
    - Opgeven dat een proxy moet worden gebruikt op Windows-machines in de browserinstellingen. Als u momenteel een proxy voor een gebruikersaccount gebruikt, kunt u dit script gebruiken om toe te passen van de instelling op het niveau van het systeem-account.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

##### <a name="allow-incoming-connections-on-the-proxy"></a>Binnenkomende verbindingen op de proxy toestaan

Binnenkomende verbindingen toestaan in de proxy-instellingen.

- Open bijvoorbeeld **Windows Firewall met geavanceerde beveiliging**.
    - Met de rechtermuisknop op **regels voor binnenkomende verbindingen** > **nieuwe regel**.
    - In **regeltype** Selecteer **aangepaste** > **volgende**.
    - In **programma**, selecteer **alle programma's** > **volgende**.
    - In **protocollen en poorten** het type instellen op **TCP**, **lokale poorten** naar **specifieke poorten**, en **externe poort**naar **alle poorten**.
    - Voltooi de wizard en geef een naam voor de regel.

##### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Voegt u een uitzonderingsregel toe aan de NSG voor de proxy

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

Een kluis back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen en slaat de back-upbeleid dat is gekoppeld aan back-ups van machines. Maak een kluis als volgt:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Op de **Hub** in het menu **Bladeren**, en het type **herstelservices**. Selecteer **Recovery Services-kluizen**.

    ![In het vak te typen en "Recovery Services-kluizen" in de resultaten te selecteren](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

3. Op de **Recovery Services-kluizen** in het menu **toevoegen**.

    ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    ![Deelvenster "Recovery Services-kluizen"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. In **Recovery Services-kluizen** >  **naam**, voer een beschrijvende naam voor het identificeren van de kluis.
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


## <a name="configure-a-backup-policy"></a>Een back-upbeleid configureren

VM's detecteren in het abonnement en back-up configureren.

1. In de kluis > **overzicht**, klikt u op **+ back-up**

   ![Back-knop](./media/backup-azure-arm-vms-prepare/backup-button.png)

   De **back-up** en **back-updoel** deelvensters openen.

2. In **back-updoel**> **waar wordt uw werkbelasting uitgevoerd?**, selecteer **Azure**. In **waarvan wilt u back-up maken?**, selecteer **virtuele machine** >  **OK**. Hiermee wordt de VM-extensie in de kluis geregistreerd.

   ![Deelvensters van de back-ups en back-updoel](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Deze stap wordt de VM-extensie bij de kluis geregistreerd. De **back-updoel** deelvenster wordt gesloten, en de **back-upbeleid** deelvenster wordt geopend.

3. In **back-upbeleid**, selecteert u het beleid dat u wilt koppelen aan de kluis. Klik vervolgens op **OK**.
    - De details van het standaardbeleid worden onder de vervolgkeuzelijst weergegeven.
    - Klik op **nieuw** om een beleid te maken. [Meer informatie](backup-azure-arm-vms-prepare.md#configure-a-backup-policy) over het definiëren van een beleid.
    

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

- Een eerste back-back-up wordt uitgevoerd in overeenstemming met uw back-upschema.
- De Backup-service installeert de back-upextensie, ongeacht of de virtuele machine wordt uitgevoerd of niet.
    - Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen.
    -  Echter, de virtuele machine is back-ups, zelfs als deze uitgeschakeld en de extensie kan niet worden geïnstalleerd. Dit staat bekend als *offline VM*. In dit geval is het herstelpunt *crashconsistent*.
    Houd er rekening mee dat Azure Backup automatische clock correctie biedt geen ondersteuning voor zomer-en wintertijd wijzigingen voor back-ups van virtuele Azure-machine. Back-upbeleid handmatig zo nodig wijzigen.
  
 ## <a name="run-the-initial-backup"></a>De eerste back-up uitvoeren

De eerste back-up wordt uitgevoerd volgens de planning, tenzij u deze handmatig onmiddellijk uitvoeren. Het handmatig uitvoeren als volgt te werk:

1. Klik in het kluismenu op **back-up items**.
2. In **back-Upitems** klikt u op **virtuele Azure-Machine**.
3. In de **back-Upitems** lijst, klikt u op het beletselteken **...** .
4. Klik op **back-up nu**.
5. In **nu back-up**, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag waarop het herstelpunt dat moet worden bewaard > **OK**.
6. Controleer de portal. U kunt de voortgang van de taak op het kluisdashboard bewaken > **back-uptaken** > **Bezig**. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.



## <a name="next-steps"></a>Volgende stappen

- Oplossen van problemen die optreden bij de [Azure VM agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) of [Azure VM backup](backup-azure-vms-troubleshoot.md).
- [Back-ups maken van Azure-VM's](backup-azure-vms-first-look-arm.md)
