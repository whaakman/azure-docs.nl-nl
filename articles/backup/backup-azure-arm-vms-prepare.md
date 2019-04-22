---
title: Back-up van virtuele Azure-machines in een Recovery Services-kluis met behulp van Azure Backup
description: Beschrijft hoe u back-up van virtuele Azure-machines in een Recovery Services-kluis met de Azure Backup
service: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: raynew
ms.openlocfilehash: 142ffdadf4adb1ee07f3592624cbdddfb310b580
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264553"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Back-up van virtuele Azure-machines in een Recovery Services-kluis

In dit artikel wordt beschreven hoe u back-up van virtuele Azure-machines in een Recovery Services-kluis met behulp van de [Azure Backup](backup-overview.md) service. 

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Azure virtuele machines voorbereiden.
> * Een kluis maken.
> * Virtuele machines detecteren en een back-upbeleid configureren.
> * Back-up inschakelen voor Azure VM's.
> * De eerste back-up uitvoeren.


> [!NOTE]
> In dit artikel wordt beschreven hoe u een sleutelkluis instellen en virtuele machines naar back-up selecteren. Dit is handig als u wilt back-up van meerdere virtuele machines. U kunt ook [maakt u een back-up van een enkele virtuele machine van Azure](backup-azure-vms-first-look-arm.md) rechtstreeks vanuit de instellingen van de virtuele machine.

## <a name="before-you-start"></a>Voordat u begint


- [Beoordeling](backup-architecture.md#architecture-direct-backup-of-azure-vms) de architectuur van Azure VM-back-up.
- [Meer informatie over](backup-azure-vms-introduction.md) back-up van virtuele Azure-machine en de back-upextensie.
- [Bekijk de ondersteuningsmatrix](backup-support-matrix-iaas.md) voordat u back-up configureren.

Er zijn bovendien een aantal dingen die u moet uitvoeren in sommige gevallen:

- **De VM-agent installeren op de virtuele machine**: Azure Backup back-ups van virtuele Azure-machines door het installeren van een extensie op de Azure VM-agent wordt uitgevoerd op de machine. Als uw virtuele machine vanuit een Azure marketplace-installatiekopie is gemaakt, wordt de agent is geïnstalleerd en worden uitgevoerd. Als u een aangepaste VM maakt, of u een on-premises machine migreert, moet u mogelijk [de agent handmatig installeren](#install-the-vm-agent).
- **Uitgaand verkeer expliciet toestaan**: Over het algemeen moet u niet expliciet uitgaand netwerkverkeer toegang toestaan voor een Azure-VM om te communiceren met Azure Backup. Echter, enkele VM's mogelijk ondervindt problemen met verbindingen, met de **ExtensionSnapshotFailedNoNetwork** fout bij de poging om verbinding te maken. Als dit gebeurt, moet u [uitgaand verkeer expliciet toestaan](#explicitly-allow-outbound-access), zodat de Azure Backup-extensie met Azure openbare IP-adressen voor back-upverkeer communiceren kan.


## <a name="create-a-vault"></a>Een kluis maken

 Een kluis back-ups en herstelpunten die zijn gemaakt na verloop van tijd worden opgeslagen en slaat de back-upbeleid dat is gekoppeld aan back-ups van machines. Maak een kluis als volgt:    

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).    
2. In het zoekvak, typ **herstelservices**. Onder **Services**, klikt u op **Recovery Services-kluizen**.   

     ![Zoeken naar Recovery Services-kluizen](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/> 

3. In **Recovery Services-kluizen** menu, klikt u op **+ toevoegen**.    

     ![Een Recovery Services-kluis maken, stap 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)   

4. In **Recovery Services-kluis**, typ een beschrijvende naam om u te identificeren van de kluis.   
    - De naam moet uniek zijn voor het Azure-abonnement.   
    - 2 en 50 tekens kan bevatten.    
    - Deze moet beginnen met een letter en mag alleen letters, cijfers en afbreekstreepjes bevatten.   
5. Selecteer de Azure-abonnement, resourcegroep en geografische regio waarin de kluis moet worden gemaakt. Klik vervolgens op **Maken**.    
    - Het kan even duren voor de kluis is gemaakt.  
    - Houd de statusmeldingen in de rechterbovenhoek van de portal.   


 Nadat de kluis is gemaakt, wordt deze weergegeven in de lijst met Recovery Services-kluizen. Als u uw kluis niet ziet, selecteert u **vernieuwen**.
 
![Lijst met back-upkluizen](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)    

### <a name="modify-storage-replication"></a>Wijzigen van de storage-replicatie

Standaard-kluizen gebruiken [geografisch redundante opslag (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

- Als de kluis uw primaire back-mechanisme is, raden wij dat u GRS gebruikt.
- U kunt [lokaal redundante opslag (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor een goedkopere optie.

Opslagreplicatietype als volgt wijzigen:

1. Klik in de nieuwe kluis op **eigenschappen** in de **instellingen** sectie.
2. In **eigenschappen**onder **back-upconfiguratie**, klikt u op **Update**.
3. Selecteer het opslagtype voor replicatie en klikt u op **opslaan**.

      ![De opslagconfiguratie voor nieuwe kluis instellen](./media/backup-try-azure-backup-in-10-mins/full-blade.png)
> [!NOTE]
   > U kunt het opslagtype voor de replicatie niet wijzigen nadat de kluis is ingesteld en back-up items bevat. Als u dit wilt doen moet u de kluis opnieuw. 

## <a name="apply-a-backup-policy"></a>Een back-upbeleid toepassen

Configureer een back-upbeleid voor de kluis.

1. Klik in de kluis op **+ back-up** in de **overzicht** sectie.

   ![Back-knop](./media/backup-azure-arm-vms-prepare/backup-button.png)


2. In **back-updoel** > **waar wordt uw werkbelasting uitgevoerd?** Selecteer **Azure**. In **waarvan wilt u back-up maken?** Selecteer **virtuele machine** >  **OK**. Hiermee wordt de VM-extensie in de kluis geregistreerd.

   ![Deelvensters van de back-ups en back-updoel](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. In **back-upbeleid**, selecteert u het beleid dat u wilt koppelen aan de kluis. 
    - Het standaardbeleid voor back-ups van de virtuele machine eenmaal per dag. De dagelijkse back-ups worden gedurende 30 dagen bewaard. Momentopnamen voor Instant herstel worden de twee dagen bewaard.
    - Als u niet wilt gebruiken van het standaardbeleid, selecteert u **nieuw**, en een aangepast beleid maken, zoals beschreven in de volgende procedure.

      ![Standaard back-upbeleid](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. In **virtuele machines selecteren**, selecteert u de virtuele machines die u back wilt-up met behulp van het beleid. Klik vervolgens op **OK**.

   - De geselecteerde virtuele machines worden gevalideerd.
   - U kunt alleen virtuele machines selecteren in dezelfde regio als de kluis.
   - VM's kunnen alleen worden gemaakt op een enkele kluis.

     ![Deelvenster 'Virtuele machines selecteren'](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. In **back-up**, klikt u op **back-up inschakelen**. Dit wordt het beleid geïmplementeerd naar de kluis en de virtuele machines en de back-upextensie op de VM-agent die wordt uitgevoerd op de Azure-VM wordt geïnstalleerd.
     
     ![Knop 'Back-up inschakelen'](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Na het inschakelen van back-up:

- De Backup-service installeert de back-upextensie, ongeacht of de virtuele machine wordt uitgevoerd of niet.
- Een eerste back-up wordt uitgevoerd in overeenstemming met uw back-upschema.
- Wanneer back-ups uitvoert, houd er rekening mee dat:
    - Een virtuele machine die wordt uitgevoerd, hebben de grootste kans op voor het vastleggen van een toepassingsconsistente herstelpunt.
    - Echter, zelfs als de virtuele machine is uitgeschakeld, wordt wordt deze back-ups. Een virtuele machine wordt ook wel een offline virtuele machine. In dit geval wordt het herstelpunt dat crash-consistente worden.
    

### <a name="create-a-custom-policy"></a>Aangepast beleid maken

Als u hebt geselecteerd om te maken van een nieuwe back-upbeleid, vult u de beleidsinstellingen.

1. In **beleidsnaam**, Geef een beschrijvende naam.
2. In **back-upschema** opgeven wanneer de back-ups moeten worden genomen. U kunt dagelijkse of wekelijkse back-ups uitvoeren voor virtuele Azure-machines.
2. In **direct herstellen**, opgeven hoe lang u wilt behouden momentopnamen lokaal voor direct terugzetten.
    - Wanneer u herstelt, back-up van VM schijven worden gekopieerd uit de opslag, via het netwerk naar de opslaglocatie voor herstel. Met instant herstel, kunt u gebruikmaken van lokaal opgeslagen momentopnamen tijdens een back-uptaak, zonder te wachten op back-upgegevens worden overgedragen naar de kluis.
    - U kunt momentopnamen voor direct terugzetten voor tussen één tot vijf dagen bewaren. Twee dagen is de standaardinstelling.
3. In **bewaartermijn**, opgeven hoe lang u wilt behouden van uw dagelijkse of wekelijkse back-uppunten.
4. In **maandelijkse back-uppunt bewaren**, opgeven of u wilt blijven een maandelijks back-up van uw back-ups dagelijks of wekelijks. 
5. Klik op **OK** het beleid op te slaan.

    ![Nieuwe back-upbeleid](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup biedt geen ondersteuning voor automatische clock correctie voor zomer-en wintertijd wijzigingen voor back-ups van virtuele Azure-machine. Als er wijzigingen optreden, back-beleid handmatig zo nodig wijzigen.

## <a name="trigger-the-initial-backup"></a>De eerste back-up activeren

De eerste back-up wordt uitgevoerd volgens de planning, maar u kunt deze onmiddellijk als volgt uitvoeren:

1. Klik in het kluismenu op **back-up items**.
2. In **back-Upitems** klikt u op **virtuele Azure-Machine**.
3. In de **back-Upitems** lijst, klikt u op het beletselteken (...).
4. Klik op **back-up nu**.
5. In **nu back-up**, het kalenderbesturingselement gebruiken om te selecteren van de laatste dag waarop het herstelpunt dat moet worden bewaard. Klik vervolgens op **OK**.
6. Controleer de portal. U kunt de voortgang van de taak op het kluisdashboard bewaken > **back-uptaken** > **Bezig**. Afhankelijk van de grootte van de virtuele machine kan het maken van de eerste back-up even duren.

## <a name="optional-steps-install-agentallow-outbound"></a>Optionele stappen (installeren agent/uitgaand verkeer toestaan)
### <a name="install-the-vm-agent"></a>De VM-agent installeren

Azure Backup back-ups van virtuele Azure-machines door het installeren van een extensie op de Azure VM-agent wordt uitgevoerd op de machine. Als uw virtuele machine vanuit een Azure Marketplace-installatiekopie is gemaakt, wordt de agent is geïnstalleerd en worden uitgevoerd. Als u een aangepaste VM maakt, of u een on-premises machine migreert, moet u mogelijk de agent handmatig installeren, zoals samengevat in de tabel.

**VM** | **Details**
--- | ---
**Windows** | 1. [Download en installeer](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) het agent-MSI-bestand.<br/><br/> 2. Installeren met beheerdersmachtigingen op de machine.<br/><br/> 3. Controleer of de installatie. In *C:\WindowsAzure\Packages* op de virtuele machine, met de rechtermuisknop op **WaAppAgent.exe** > **eigenschappen**. Op de **Details** tabblad **productversie** moet 2.6.1198.718 of hoger.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerkingen worden uitgevoerd, en [Installeer de agent opnieuw](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Installeren met een RPM- of DEB-pakket in uw distributie van pakket-opslagplaats. Dit is de aanbevolen methode voor het installeren en bijwerken van de Azure Linux agent. Alle de [distributie-providers die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) het pakket van Azure Linux agent integreren in hun opslagplaatsen en installatiekopieën. De agent is beschikbaar op [GitHub](https://github.com/Azure/WALinuxAgent), maar wordt niet aanbevolen van daaruit installeren.<br/><br/> Als u de agent bijwerkt, zorg ervoor dat er geen back-upbewerkingen worden uitgevoerd en bijwerken van de binaire bestanden.

### <a name="explicitly-allow-outbound-access"></a>Uitgaand verkeer expliciet toestaan

De Backup-extensie die wordt uitgevoerd op de virtuele machine moet uitgaande toegang tot Azure openbare IP-adressen.

- In het algemeen moet u niet expliciet uitgaand netwerkverkeer toegang toestaan voor een Azure-VM om te communiceren met Azure Backup.
- Als u uitvoert in problemen met VM's verbinding te maken, of als u een foutbericht weergegeven **ExtensionSnapshotFailedNoNetwork** bij een poging om verbinding te maken, moet u toegang expliciet toestaan, zodat de back-upextensie met Azure openbaar IP-adres communiceren kan adressen voor back-upverkeer. Toegang tot methoden worden samengevat in de volgende tabel.


**Optie** | **Actie** | **Details** 
--- | --- | --- 
**Instellen van NSG-regels** | Toestaan dat de [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/> U kunt een regel waarmee toegang tot de Azure Backup-service met behulp toevoegen in plaats van zodat en elk adresbereik beheert, een [servicetag](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). | [Meer informatie](../virtual-network/security-overview.md#service-tags) over service-tags.<br/><br/> Services tags Vereenvoudig het toegangsbeheer van en geen extra kosten in rekening worden gebracht.
**Een proxy implementeren** | Implementeer een HTTP-proxy-server voor het routeren van verkeer. | Biedt toegang tot het geheel van Azure en niet alleen opslag.<br/><br/> Nauwkeurige controle over de URL's voor opslag is toegestaan.<br/><br/> Toegang tot één punt van internet voor VM's.<br/><br/> Extra kosten voor de proxy.
**Azure-Firewall instellen** | Toestaan van verkeer via de Azure-Firewall op de virtuele machine, met behulp van een FQDN-tag voor de Azure Backup-service | Eenvoudig te gebruiken als u Azure-Firewall instellen in een VNet-subnet.<br/><br/> U kan maken van uw eigen labels FQDN-naam, of FQDN-namen wijzigen in een tag.<br/><br/> Als uw virtuele Azure-machines beheerde schijven hebt, moet u mogelijk een extra openen (8443)-poort op de firewalls.

#### <a name="establish-network-connectivity"></a>Netwerkverbinding tot stand brengen

Verbinding met NSG, via een proxy of via de firewall

##### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Instellen van een NSG-regel waarmee uitgaande toegang tot Azure

Als een NSG de toegang tot de virtuele machine beheert, uitgaande toegang voor de back-upopslag naar het vereiste bereik en de poorten toestaat.

1. In de VM-eigenschappen > **netwerken**, selecteer **regel voor uitgaande poort toevoegen**.
2. In **uitgaande beveiligingsregel toevoegen**, selecteer **Geavanceerd**.
3. In **bron**, selecteer **VirtualNetwork**.
4. In **poortbereiken van bron**, geeft u een sterretje (*) voor uitgaande toegang van een willekeurige poort.
5. In **bestemming**, selecteer **servicetag**. Selecteer in de lijst **Storage.region**. De regio is waar de kluis en de virtuele machines die u back wilt-up moet zich bevinden.
6. In **poortbereiken van doel**, selecteert u de poort.
    - Niet-beheerde virtuele machine met niet-versleuteld opslagaccount: 80
    - Niet-beheerde virtuele machine met versleutelde storage-account: 443 (standaardinstelling)
    - Beheerde virtuele machine: 8443.
7. In **Protocol**, selecteer **TCP**.
8. In **prioriteit**, Geef een prioriteitswaarde kleiner is dan een hoger weigeren regels.
   
   Hebt u een regel die de toegang weigert, toestaan de nieuwe regel moet hoger zijn. Als u hebt bijvoorbeeld een **Deny_All** regel instellen op prioriteit van 1000, de nieuwe regel moet worden ingesteld op minder dan 1000.
9. Geef een naam en beschrijving voor de regel op en selecteer **OK**.

U kunt de NSG-regel toepassen op meerdere virtuele machines om uitgaande toegang te verlenen. Deze video helpt u bij het proces.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


##### <a name="route-backup-traffic-through-a-proxy"></a>Back-verkeer routeren via een proxy

U kunt back-verkeer via een proxy omleiden en vervolgens de Proxytoegang geven tot de vereiste Azure bereiken. Configureer de proxy-VM om toe te staan de volgende:

- De Azure-VM moet alle afhankelijke voor het openbare internet via de proxy HTTP-verkeer routeren.
- De proxy moet binnenkomend verkeer toestaan van virtuele machines in het virtuele netwerk dat van toepassing.
- De NSG **NSF-lockdown** moet een regel waarmee het uitgaande internetverkeer van de proxy-VM.

###### <a name="set-up-the-proxy"></a>De proxy niet instellen

Als u een systeemproxy-account hebt, stelt een als volgt:

1. Download [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Voer **PsExec.exe -i -s cmd.exe** naar de opdrachtprompt met een systeemaccount worden uitgevoerd.
3. De browser wordt uitgevoerd in de systeemcontext. Gebruik bijvoorbeeld **%PROGRAMFILES%\Internet Explorer\iexplore.exe** voor Internet Explorer.  
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

1. Open in Windows Firewall, **Windows Firewall met geavanceerde beveiliging**.
2. Met de rechtermuisknop op **regels voor binnenkomende verbindingen** > **nieuwe regel**.
3. In **regeltype**, selecteer **aangepaste** > **volgende**.
4. In **programma**, selecteer **alle programma's** > **volgende**.
5. In **protocollen en poorten**:
   - Het type instellen op **TCP**.
   - Stel **lokale poorten** naar **specifieke poorten**.
   - Stel **externe poort** naar **alle poorten**.
  
6. Voltooi de wizard en geef een naam voor de regel.

###### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Voegt u een uitzonderingsregel toe aan de NSG voor de proxy

Op de NSG **NSF-lockdown**, verkeer van een willekeurige poort op 10.0.0.5 naar een internetadres op poort 80 (HTTP) of 443 (HTTPS) toestaan.

De volgende PowerShell-script toont een voorbeeld van het verkeer wordt toegestaan.
In plaats van toegestaan uitgaand naar alle openbare internet-adressen, kunt u een IP-adresbereik (`-DestinationPortRange`), of gebruik de servicetag storage.region.   

```powershell
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

##### <a name="allow-firewall-access-with-an-fqdn-tag"></a>Firewalltoegang met een FQDN-tag toestaan

U kunt de Firewall van Azure instellen waarmee uitgaande toegang voor het netwerkverkeer naar back-up van Azure.

- [Meer informatie over](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Firewall van Azure implementeren.
- [Meer informatie over](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN tags.



## <a name="next-steps"></a>Volgende stappen

- Los eventuele problemen met [Azure VM agents](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) of [Azure VM backup](backup-azure-vms-troubleshoot.md).
- [Herstellen](backup-azure-arm-restore-vms.md) Azure VM's.

