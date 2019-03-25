---
title: Back-up van virtuele Azure-machines in een Recovery Services-kluis met Azure Backup
description: Hierin wordt beschreven hoe u back-up van virtuele Azure-machines in een Recovery Services-kluis met Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: raynew
ms.openlocfilehash: 3133f22a4d9ecd8a0ee4bff9f8b0be9c1f4eb705
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403660"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Back-up van virtuele Azure-machines in een Recovery Services-kluis

In dit artikel wordt beschreven hoe u back-up van virtuele Azure-machines in de Recovery Services-kluizen met de [Azure Backup](backup-overview.md) service. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Controleer of ondersteuning en vereisten voor back-up.
> * Azure virtuele machines voorbereiden. Installeer de Azure VM-agent indien nodig, en controleer of uitgaande toegang voor virtuele machines.
> * Een kluis maken.
> * Virtuele machines detecteren en een back-upbeleid configureren.
> * Back-up inschakelen voor Azure VM's.


> [!NOTE]
   > In dit artikel wordt beschreven hoe u een sleutelkluis instellen en virtuele machines naar back-up selecteren. Dit is handig als u wilt back-up van meerdere virtuele machines. U kunt ook [maakt u een back-up van een enkele virtuele machine van Azure](backup-azure-vms-first-look-arm.md) rechtstreeks vanuit de instellingen van de virtuele machine.

## <a name="before-you-start"></a>Voordat u begint


- [Beoordeling](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM-back-architectuur.
- [Meer informatie over](backup-azure-vms-introduction.md) back-up van virtuele Azure-machine en de back-upextensie.
- [Bekijk de ondersteuningsmatrix](backup-support-matrix-iaas.md) voor Azure VM backup.


## <a name="prepare-azure-vms"></a>Azure virtuele machines voorbereiden

In sommige gevallen moet u mogelijk instellen met de Azure VM-agent op Azure Virtual machines of uitgaand verkeer expliciet toestaan op de virtuele machine.

### <a name="install-the-vm-agent"></a>De VM-agent installeren 

Azure Backup back-ups van virtuele Azure-machines door het installeren van een extensie op de Azure VM-agent wordt uitgevoerd op de machine. Als uw virtuele machine vanuit een Azure marketplace-installatiekopie is gemaakt, wordt de agent is geïnstalleerd en worden uitgevoerd. Als u een aangepaste VM maakt, of u een on-premises machine migreert, moet u mogelijk de agent handmatig installeren, zoals samengevat in de tabel.

**VM** | **Details**
--- | ---
**Virtuele Windows-machines** | 1. [Download en installeer](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) het agent-MSI-bestand.<br/><br/> 2. Installeren met beheerdersmachtigingen op de machine.<br/><br/> 3. Controleer of de installatie. In *C:\WindowsAzure\Packages* op de virtuele machine, met de rechtermuisknop op de WaAppAgent.exe > **eigenschappen**, > **Details** tabblad. **Versie van het product** moet 2.6.1198.718 of hoger.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerkingen worden uitgevoerd, en [Installeer de agent opnieuw](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Virtuele Linux-machines** | Installeren met behulp van een RPM- of DEB-pakket uit de opslagplaats voor uw distributie van pakket. Dit is de aanbevolen methode voor het installeren en bijwerken van de Azure Linux Agent. Alle de [distributie-providers die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën. De agent is beschikbaar op [GitHub](https://github.com/Azure/WALinuxAgent), maar wordt niet aanbevolen van daaruit installeren.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerkingen worden uitgevoerd en bijwerken van de binaire bestanden.


### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

De back-up-extensie die wordt uitgevoerd op de virtuele machine moet uitgaande toegang tot Azure openbare IP-adressen.

- In het algemeen moet u niet expliciet uitgaand netwerkverkeer toegang toestaan voor een Azure-VM om te communiceren met Azure Backup.
- Als u uitvoert in problemen met VM's verbinding maken, en als u een foutbericht weergegeven **ExtensionSnapshotFailedNoNetwork** bij een poging om verbinding te maken, moet u toegang expliciet toestaan, zodat de back-upextensie met de openbare Azure communiceren kan IP-adressen voor back-upverkeer.


#### <a name="explicitly-allow-outbound-access"></a>Uitgaand verkeer expliciet toestaan

Als uw virtuele machine geen verbinding met de Backup-service maken, kunt u expliciet uitgaande toegang met behulp van een van de methoden die in de tabel wordt samengevat.

**Optie** | **Actie** | **Details** 
--- | --- | --- 
**Instellen van NSG-regels** | Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). | U kunt een regel waarmee toegang tot de Azure Backup-service met behulp toevoegen in plaats van zodat en elk adresbereik beheert, een [servicetag](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). [Meer informatie](../virtual-network/security-overview.md#service-tags).<br/><br/> Geen extra kosten.<br/><br/> Eenvoudig te beheren met service-tags.
**Een proxy implementeren** | Implementeer een HTTP-proxy-server voor het routeren van verkeer. | Biedt toegang tot het geheel van Azure en niet alleen opslag.<br/><br/> Nauwkeurige controle over de URL's voor opslag is toegestaan.<br/><br/> Toegang tot één punt van internet voor VM's.<br/><br/> Extra kosten voor de proxy.
**Azure-Firewall instellen** | Verkeer via de Azure-Firewall op de virtuele machine, met behulp van een FQDN-tag voor de Azure Backup-service toestaan. |  Eenvoudig te gebruiken als u Azure-Firewall instellen in een VNet-subnet<br/><br/> U kan maken van uw eigen labels FQDN-naam, of FQDN-namen wijzigen in een tag.<br/><br/> Als u Azure Managed Disks gebruikt, moet u mogelijk een extra poort openen (poort 8443) op de firewalls.

##### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Instellen van een NSG-regel waarmee uitgaande toegang tot Azure

Als de VM-toegang wordt beheerd door een NSG, uitgaande toegang voor de back-upopslag, naar het vereiste bereik en de poorten toestaat.

1. In de VM-eigenschappen > **netwerken**, klikt u op **regel voor uitgaande poort toevoegen**.
2. In **uitgaande beveiligingsregel toevoegen**, klikt u op **Geavanceerd**.
3. In **bron**, selecteer **VirtualNetwork**.
4. In **poortbereiken van bron**, type in een sterretje (*) zodat uitgaande toegang vanaf een willekeurige poort.
5. In **bestemming**, selecteer **servicetag**. Selecteer in de lijst **Storage.region**. De regio is de regio waarin de kluis en de virtuele machines die u back wilt-up moet zich bevinden.
6. In **poortbereiken van doel**, selecteert u de poort.
    - Niet-beheerde virtuele machine met niet-versleuteld opslagaccount: 80
    - Niet-beheerde virtuele machine met versleutelde storage-account: 443 (standaardinstelling)
    - Beheerde virtuele machine: 8443.
7. In **Protocol**, selecteer **TCP**.
8. In **prioriteit**, Geef een prioriteitswaarde kleiner is dan een hoger weigeren regels.
   - Hebt u een regel voor weigeren van toegang, toestaan de nieuwe regel moet hoger zijn.
   - Als u hebt bijvoorbeeld een **Deny_All** regel instellen op prioriteit van 1000, de nieuwe regel moet worden ingesteld op minder dan 1000.
9. Geef een naam en beschrijving voor de regel en klikt u op **OK**.

U kunt de NSG-regel toepassen op meerdere virtuele machines om uitgaande toegang te verlenen. Deze video helpt u bij het proces.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


##### <a name="route-backup-traffic-through-a-proxy"></a>Back-verkeer routeren via een proxy

U kunt back-verkeer via een proxy omleiden en vervolgens de Proxytoegang geven tot de vereiste Azure bereiken. Configureer de proxy-VM om toe te staan de volgende:

- De Azure-VM moet alle afhankelijke voor het openbare internet via de proxy HTTP-verkeer routeren.
- De proxy moet binnenkomend verkeer toestaan van virtuele machines in het virtuele netwerk (VNet).
- De NSG **NSF-lockdown** moet een regel waarmee het uitgaande internetverkeer van de proxy-VM.

###### <a name="set-up-the-proxy"></a>De proxy niet instellen

Als u een systeemproxy-account hebt, stelt een als volgt:

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Voer **PsExec.exe -i -s cmd.exe** naar de opdrachtprompt met een systeemaccount worden uitgevoerd.
3. De browser wordt uitgevoerd in de systeemcontext. Bijvoorbeeld: **%PROGRAMFILES%\Internet Explorer\iexplore.exe** voor Internet Explorer.  
4. Definieer de proxyinstellingen.
   - Op Linux-machines:
     - Deze regel toe te voegen de **/etc/omgeving** bestand:
       - **gebruikt = http:\//proxy IP-adres: proxypoort**
     - Deze Voeg regels toe aan de **/etc/waagent.conf** bestand:
         - **HttpProxy.Host=proxy IP-adres**
         - **HttpProxy.Port=proxy port**
   - Opgeven dat een proxy moet worden gebruikt op Windows-machines in de browserinstellingen. Als u momenteel een proxy voor een gebruikersaccount gebruikt, kunt u dit script gebruiken om toe te passen van de instelling op het niveau van het systeem-account.
       ```powershell
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

       ```

###### <a name="allow-incoming-connections-on-the-proxy"></a>Binnenkomende verbindingen op de proxy toestaan

Binnenkomende verbindingen toestaan in de proxy-instellingen.

1, in de Windows-Firewall openen **Windows Firewall met geavanceerde beveiliging**.
2. Met de rechtermuisknop op **regels voor binnenkomende verbindingen** > **nieuwe regel**.
3. In **regeltype** Selecteer **aangepaste** > **volgende**.
4. In **programma**, selecteer **alle programma's** > **volgende**.
5. In **protocollen en poorten**:
   - Het type instellen op **TCP**
   - Stel **lokale poorten** naar **specifieke poorten**
   - Stel **externe poort** naar **alle poorten**.
  
6. Voltooi de wizard en geef een naam voor de regel.

###### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Voegt u een uitzonderingsregel toe aan de NSG voor de proxy

Op de NSG **NSF-lockdown**, verkeer van een willekeurige poort op 10.0.0.5 naar een internetadres op poort 80 (HTTP) of 443 (HTTPS) toestaan.

- De volgende PowerShell-script toont een voorbeeld van het verkeer wordt toegestaan.
- In plaats van toegestaan uitgaand naar alle openbare internet-adressen, kunt u een IP-adresbereik (-DestinationPortRange), of gebruik de servicetag storage.region.   

    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>Firewalltoegang met FQDN-naam tag toestaan

U kunt de Azure-Firewall instellen waarmee uitgaande toegang voor het netwerkverkeer naar back-up van Azure.

- [Meer informatie over](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Firewall van Azure implementeren.
- [Meer informatie over](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN tags.

## <a name="modify-storage-replication-settings"></a>Storage-replicatie-instellingen wijzigen

Uw kluis heeft standaard [geografisch redundante opslag (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

- We raden GRS voor uw primaire back-up.
- U kunt [lokaal redundante opslag (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor een goedkopere optie.

Opslagreplicatietype als volgt wijzigen:

1. Klik op de nieuwe kluis in de portal. Onder de **instellingen** sectie, klikt u op **eigenschappen**.
2. In **eigenschappen**onder **back-upconfiguratie**, klikt u op **Update**.
3. Selecteer het opslagtype voor replicatie en klikt u op **opslaan**.

      ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/full-blade.png)


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

- Een eerste back-up wordt uitgevoerd in overeenstemming met uw back-upschema.
- De Backup-service installeert de back-upextensie, ongeacht of de virtuele machine wordt uitgevoerd of niet.
    - Bij een actieve VM is de kans het grootst dat een toepassingsconsistent herstelpunt wordt verkregen.
    -  Echter, de virtuele machine is back-ups, zelfs als deze uitgeschakeld en de extensie kan niet worden geïnstalleerd. Dit staat bekend als een offline virtuele machine. In dit geval wordt het herstelpunt dat crash-consistente worden. [Meer informatie]() Houd er rekening mee dat Azure Backup automatische clock correctie biedt geen ondersteuning voor zomer-en wintertijd wijzigingen voor back-ups van virtuele Azure-machine. Back-upbeleid handmatig zo nodig wijzigen.

## <a name="run-the-initial-backup"></a>De eerste back-up uitvoeren

De eerste back-up wordt uitgevoerd volgens de planning, tenzij u deze handmatig onmiddellijk uitvoeren. Het handmatig uitvoeren als volgt te werk:

1. Klik in het kluismenu op **back-up items**.
2. In **back-Upitems** klikt u op **virtuele Azure-Machine**.
3. In de **back-Upitems** lijst, klikt u op het beletselteken **...** .
4. Klik op **back-up nu**.
5. In **nu back-up**, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag waarop het herstelpunt dat moet worden bewaard > **OK**.
6. Controleer de portal. U kunt de voortgang van de taak op het kluisdashboard bewaken > **back-uptaken** > **Bezig**. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.



## <a name="next-steps"></a>Volgende stappen

- Los eventuele problemen met [Azure VM agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) of [Azure VM backup](backup-azure-vms-troubleshoot.md).
- [Herstellen](backup-azure-arm-restore-vms.md) Azure VM's.

