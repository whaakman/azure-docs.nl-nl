---
title: De configuratieserver voor VMware en fysieke server-noodherstel met Azure Site Recovery beheren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een bestaande configuratieserver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery beheren.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: ramamill
ms.openlocfilehash: 93e05390d28b9e9998d84935417121696d2963cc
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877224"
---
# <a name="manage-the-configuration-server-for-vmware-vm-disaster-recovery"></a>De configuratieserver voor VMware-VM-noodherstel beheren

Instellen van een on-premises configuratieserver wanneer u [Azure Site Recovery](site-recovery-overview.md) voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen on-premises VMware- en Azure en beheert de gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.

## <a name="access-configuration-server"></a>Configuratieserver voor toegang

De configuratieserver kunt u als volgt openen:

* Aanmelden bij de virtuele machine waarop deze geïmplementeerd, en Start **Azure Site Recovery Configuration Manager** van de snelkoppeling op het bureaublad.
* U kunt ook openen de configuratieserver op afstand van https://*ConfigurationServerName*/:44315 /. Meld u aan met beheerdersreferenties.

## <a name="modify-vmware-server-settings"></a>Instellingen voor VMware-server wijzigen

1. Om te koppelen van een andere VMware-server met de configuratieserver na [aanmelden](#access-configuration-server), selecteer **vCenter Server/vSphere ESXi-server toevoegen**.
2. Voer de details in en selecteer vervolgens **OK**.

## <a name="modify-credentials-for-automatic-discovery"></a>Referenties wijzigen voor automatische detectie

1. De referenties waarmee verbinding wordt gemaakt met de VMware-server voor automatische detectie van virtuele VMware-machines na het bijwerken [aanmelden](#access-configuration-server), kiest u het account en klikt u op **bewerken**.
2. Geef de nieuwe referenties op en selecteer vervolgens **OK**.

    ![Wijzigen van VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

U kunt ook de referenties via CSPSConfigtool.exe wijzigen.

1. Meld u aan bij de configuratieserver en start CSPSConfigtool.exe
2. Kiezen welk account u wilt wijzigen en klik op **bewerken**.
3. De gewijzigde referenties invoeren en klikt u op **Ok**

## <a name="modify-credentials-for-mobility-service-installation"></a>Referenties wijzigen voor de installatie van Mobility Service

Wijzig de referenties die worden gebruikt voor het installeren van Mobility Service automatisch op de VMware-VM's die u voor replicatie inschakelen.

1. Na [aanmelden](#access-configuration-server), selecteer **referenties voor virtuele machine beheren**
2. Kiezen welk account u wilt wijzigen en klik op **bewerken**
3. Geef de nieuwe referenties op en selecteer vervolgens **OK**.

    ![Referenties van de Mobility-Service wijzigen](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

U kunt ook referenties via CSPSConfigtool.exe wijzigen.

1. Meld u aan bij de configuratieserver en start CSPSConfigtool.exe
2. Kiezen welk account u wilt wijzigen en klik op **bewerken**
3. De nieuwe referenties invoeren en klikt u op **Ok**.

## <a name="add-credentials-for-mobility-service-installation"></a>Referenties voor de installatie van de Mobility-service toevoegen

Als u referenties toevoegen tijdens de implementatie van de configuratieserver, OVF hebt gemist

1. Na [aanmelden](#access-configuration-server), selecteer **referenties voor virtuele machine beheren**.
2. Klik op **referenties voor virtuele machine toevoegen**.
    ![toevoegen-mobility-referenties](media/vmware-azure-manage-configuration-server/add-mobility-credentials.png)
3. Geef de nieuwe referenties op en klik op **toevoegen**.

U kunt ook referenties via CSPSConfigtool.exe toevoegen.

1. Meld u aan bij de configuratieserver en start CSPSConfigtool.exe
2. Klik op **toevoegen**, voert u de nieuwe referenties en klikt u op **Ok**.

## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Wijzig de proxy-instellingen die worden gebruikt door de configuratie van server-machine voor internettoegang tot Azure. Als u een proces-servercomputer naast de standaard-processerver die wordt uitgevoerd op de servercomputer van de configuratie hebt, kunt u de instellingen op beide computers wijzigen.

1. Na [aanmelden](#access-configuration-server) naar de configuratieserver, selecteert u **verbindingen beheren**.
2. Werk de proxy-waarden. Selecteer vervolgens **opslaan** om de instellingen te werken.

## <a name="add-a-network-adapter"></a>Een netwerkadapter toevoegen

De Open Virtualization Format (OVF)-sjabloon implementeert de configuratieserver VM met één netwerkadapter.

- U kunt [een extra adapter toevoegen aan de virtuele machine](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), maar moet u deze toevoegen voordat u de configuratieserver in de kluis registreert.
- Om toe te voegen een adapter nadat u de configuratieserver in de kluis registreert, moet u de adapter toevoegen in de VM-eigenschappen. Moet u [opnieuw registreren](#reregister-a-configuration-server-in-the-same-vault) de server in de kluis.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Een configuratieserver in dezelfde kluis registreren

Als u wilt, kunt u de configuratieserver in dezelfde kluis registreren. Hebt u een extra proces server virtuele machine, naast de standaard-processerver die wordt uitgevoerd op de servercomputer van de configuratie opnieuw registreren op beide machines.


1. Open in de kluis **beheren** > **Site Recovery-infrastructuur** > **configuratieservers**.
2. In **Servers**, selecteer **registratiesleutel downloaden** voor het downloaden van het bestand met kluisreferenties.
3. Aanmelden bij de configuratie van server-machine.
4. In **%ProgramData%\ASR\home\svsystems\bin**, open **cspsconfigtool.exe**.
5. Op de **kluis registratie** tabblad **Bladeren**, en Ga naar het bestand met kluisreferenties die u hebt gedownload.
6. Indien nodig, bieden u proxy-server-gegevens. Selecteer vervolgens **Registreren**.
7. Open een admin PowerShell-opdrachtvenster en voer de volgende opdracht uit:
   ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
   ```

    >[!NOTE]
    >Om **ophalen van de meest recente certificaten** Voer de opdracht uit vanaf de configuratieserver naar uitbreidbare processerver *"< installatie Drive\Microsoft Azure Site Recovery\agent\cdpcli.exe >"--registermt uit*

8. Ten slotte de obengine opnieuw door de volgende opdracht wordt uitgevoerd.
   ```
        net stop obengine
        net start obengine
   ```


## <a name="register-a-configuration-server-with-a-different-vault"></a>Een configuratieserver registreren bij een andere kluis

> [!WARNING]
> De volgende stap de configuratieserver in de huidige kluis schaduwkopieën verbroken en de replicatie van alle beveiligde virtuele machines onder de configuratieserver is gestopt.

1. Meld u aan bij de configuratieserver.
2. Open een admin PowerShell-opdrachtvenster en voer de volgende opdracht uit:

    ```
    reg delete "HKLM\Software\Microsoft\Azure Site Recovery\Registration"
    net stop dra
    ```
3. Starten van de configuratie van server toestel browser portal met behulp van de snelkoppeling op het bureaublad.
4. Voer de registratie stappen uit die vergelijkbaar is met een nieuwe configuratieserver [registratie](vmware-azure-tutorial.md#register-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>De configuratieserver upgraden

U uitvoeren updatepakketten voor het bijwerken van de configuratieserver. Updates kunnen worden toegepast voor maximaal N-4 versies. Bijvoorbeeld:

- Als u 9.7, 9,8, 9,9 of 9.10 uitvoert, kunt u rechtstreeks naar 9.11 bijwerken.
- Als u 9,6 of een eerdere versie uitvoert en u wilt upgraden naar 9.11, moet u eerst upgraden naar versie 9.7. voordat u 9.11.

Raadpleeg voor gedetailleerde richtlijnen voor het Azure Site Recovery-onderdelen ondersteuningsverklaring [hier](https://aka.ms/asr_support_statement).
Vindt u koppelingen naar de updatepakketten voor het upgraden van alle versies van de configuratieserver [hier](https://aka.ms/asr_update_rollups).

> [!IMPORTANT]
> Bij elke nieuwe versie "n" van een Azure Site Recovery-onderdeel dat wordt uitgebracht, alle versies hieronder 'N-4' wordt beschouwd als ondersteuning vervalt. Het is altijd verstandig om te upgraden naar de nieuwste beschikbare versies.</br>
> Raadpleeg voor gedetailleerde richtlijnen voor het Azure Site Recovery-onderdelen ondersteuningsverklaring [hier](https://aka.ms/asr_support_statement).

De server als volgt bijwerken:

1. Ga in de kluis naar **beheren** > **Site Recovery-infrastructuur** > **configuratieservers**.
2. Als een update beschikbaar is, wordt een koppeling weergegeven in de **Agentversie** > kolom.
    ![Update](./media/vmware-azure-manage-configuration-server/update2.png)
3. Download het installatiebestand van de update naar de configuratieserver.

    ![Update](./media/vmware-azure-manage-configuration-server/update1.png)

4. Dubbelklik erop om het installatieprogramma uitvoert.
5. Het installatieprogramma detecteert de huidige versie die wordt uitgevoerd op de machine. Klik op **Ja** bij te werken.
6. Wanneer de upgrade is voltooid valideert de configuratie van de server.

    ![Update](./media/vmware-azure-manage-configuration-server/update3.png)

7. Klik op **voltooien** te sluiten van het installatieprogramma.
8. Als u de rest van de Site Recovery-onderdelen upgraden, raadpleegt u onze [richtlijnen voor upgrades](https://aka.ms/asr_vmware_upgrades).

## <a name="upgrade-configuration-serverprocess-server-from-the-command-line"></a>Serverproces/configuratieserver upgraden vanaf de opdrachtregel

Voer het bestand voor installatie als volgt uit:

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Voorbeeld van gebruik
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="parameters"></a>Parameters

|Parameternaam| Type | Description| Waarden|
|-|-|-|-|
| /ServerMode|Vereist|Hiermee wordt aangegeven of zowel de configuratieserver als de processerver moet worden geïnstalleerd, of alleen de processerver|CS<br>PS|
|/InstallLocation|Vereist|De map waarin de onderdelen worden geïnstalleerd| Een map op de computer|
|/MySQLCredsFilePath|Vereist|Het bestandspad waarin de referenties voor de MySQL-server worden opgeslagen|Het bestand moet de indeling hebben die hieronder wordt aangegeven|
|/VaultCredsFilePath|Vereist|Het pad naar het bestand met kluisreferenties|Geldig bestandspad|
|/EnvType|Vereist|Type omgeving die u wilt beveiligen |VMware<br>NonVMware|
|/PSIP|Vereist|IP-adres van de NIC dat wordt gebruikt voor de overdracht van replicatiegegevens| Een geldig IP-adres|
|/CSIP|Vereist|Het IP-adres van de NIC waarop de configuratieserver luistert| Een geldig IP-adres|
|/PassphraseFilePath|Vereist|Het volledige pad naar het bestand met de wachtwoordzin|Geldig bestandspad|
|/BypassProxy|Optioneel|Hiermee wordt aangegeven dat de configuratieserver zonder proxy verbinding moet maken met Azure|Deze waarde moet van Venu worden opgehaald|
|/ProxySettingsFilePath|Optioneel|Proxy-instellingen (de standaardproxy vereist verificatie of een aangepaste proxy)|Het bestand moet de indeling hebben die hieronder wordt aangegeven|
|DataTransferSecurePort|Optioneel|Poortnummer van de PSIP dat wordt gebruikt voor replicatiegegevens| Geldig poortnummer (standaardwaarde is 9433)|
|/SkipSpaceCheck|Optioneel|Hiermee wordt aangegeven dat de controle op vrije ruimte in de cacheschijf moet worden overgeslagen| |
|/AcceptThirdpartyEULA|Vereist|Een vlag impliceert acceptatie van de gebruiksrechtovereenkomst van derden| |
|/ShowThirdpartyEULA|Optioneel|Hiermee wordt de gebruiksrechtovereenkomst van derden weergegeven. Indien opgegeven als invoer, worden alle andere parameters genegeerd| |



### <a name="create-file-input-for-mysqlcredsfilepath"></a>Bestandsinvoer voor MYSQLCredsFilePath maken

De parameter MySQLCredsFilePath wordt een bestand gebruikt als invoer. Maak het bestand met de volgende notatie en geven deze als invoerparameter MySQLCredsFilePath.
```ini
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-file-input-for-proxysettingsfilepath"></a>Bestandsinvoer voor ProxySettingsFilePath maken
ProxySettingsFilePath parameter wordt een bestand gebruikt als invoer. Maak het bestand met de volgende notatie en geven deze als invoerparameter ProxySettingsFilePath.

```ini
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```

## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

1. [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle virtuele machines onder de configuratieserver.
2. [Loskoppelen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) en [verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) alle replicatiebeleidsregels voor vanaf de configuratieserver.
3. [Verwijder](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) alle vCenter-servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.
4. Open in de kluis **Site Recovery-infrastructuur** > **configuratieservers**.
5. Selecteer de configuratieserver die u wilt verwijderen. Klik vervolgens op de **Details** weergeeft, schakelt **verwijderen**.

    ![Configuratieserver verwijderen](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)


### <a name="delete-with-powershell"></a>Verwijderen met PowerShell

U kunt eventueel de configuratieserver verwijderen met behulp van PowerShell.

1. [Installeer](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) de Azure PowerShell-module.
2. Meld u aan uw Azure-account met de volgende opdracht:

    `Connect-AzureRmAccount`
3. Selecteer het kluisabonnement.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Stel de context van de kluis.

    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. De configuratieserver worden opgehaald.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. De configuratieserver verwijderen.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force]`

> [!NOTE]
> U kunt de **-Force** optie in Remove-AzureRmSiteRecoveryFabric voor geforceerde verwijdering van de configuratieserver.

## <a name="generate-configuration-server-passphrase"></a>Configuratieserver wachtwoordzin genereren

1. Meld u aan bij de configuratieserver en open een opdrachtpromptvenster als beheerder.
2. Als u de map naar de bin-map, geeft u de opdracht **cd %ProgramData%\ASR\home\svsystems\bin**
3. Voor het genereren van het bestand wachtwoordzin uitvoeren **genpassphrase.exe - v > MobSvc.passphrase**.
4. De wachtwoordzin worden opgeslagen in het bestand dat zich bevindt in **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## <a name="renew-ssl-certificates"></a>SSL-certificaten vernieuwen

De configuratieserver heeft een ingebouwde webserver, die regelt de activiteiten van de Mobility-Service, processervers, hoofddoelservers die zijn verbonden. De webserver wordt een SSL-certificaat gebruikt om clients te verifiëren. Het certificaat verloopt na drie jaar en kan op elk gewenst moment worden vernieuwd.

### <a name="check-expiry"></a>Controleer de vervaldatum

Configuratie van implementaties van de server voordat mei 2016, is verlopen van het certificaat ingesteld op één jaar. Als u een certificaat dat verloopt hebben, gebeurt het volgende:

- Wanneer de vervaldatum is twee maanden of minder, start de service verzenden van meldingen in de portal en via e-mail (als u zich hebt geabonneerd op meldingen voor Site Recovery).
- Een meldingsbanner wordt weergegeven op de pagina van de kluis-resource. Selecteer de banner voor meer informatie.
- Als u ziet een **nu bijwerken** knop, betekent dit dat sommige onderdelen in uw omgeving nog niet hebt bijgewerkt naar 9.4.xxxx.x of hogere versies. De onderdelen upgraden voordat u het certificaat vernieuwen. Kunt u vernieuwen niet in oudere versies.

### <a name="renew-the-certificate"></a>Het certificaat vernieuwen

1. Open in de kluis **Site Recovery-infrastructuur** > **configuratieserver**. Selecteer de vereiste configuratie-server.
2. De vervaldatum wordt weergegeven onder **configuratieserver health**.
3. Selecteer **certificaten vernieuwen**.

## <a name="refresh-configuration-server"></a>Configuratieserver vernieuwen

1. In de Azure-portal, gaat u naar **Recovery Services-kluis** > **beheren** > **Site Recovery-infrastructuur**  >   **Voor VMware en fysieke machines** > **configuratieservers**
2. Klik op de configuratieserver die u wilt vernieuwen.
3. Klik op de blade met details van de gekozen configuratieserver **meer** > **Server vernieuwen**.
4. De voortgang van de taak onder **Recovery Services-kluis** > **bewaking** > **Site Recovery-taken**.

## <a name="update-windows-license"></a>Bijwerken van Windows-licentie

De licentie die is opgegeven met het OVF-sjabloon is een van de evaluatielicentie is geldig gedurende 180 dagen. Voor ononderbroken gebruik, moet u Windows activeren met een geleverde licentie.

## <a name="failback-requirements"></a>Vereisten voor failback

Tijdens het opnieuw beveiligen en failback moet de on-premises configuratieserver actief is en in een verbonden status heeft. Voor succesvolle failback wordt bestaan de virtuele machine wordt een failback in de database van de configuratieserver.

Zorgen ervoor dat u regelmatig geplande back-ups van uw configuratieserver. Als zich een noodgeval voordoet en de configuratieserver verbroken is, moet u eerst de configuratieserver herstellen vanuit een back-up en ervoor te zorgen dat de herstelde configuratieserver hetzelfde IP-adres dat is geregistreerd bij de kluis. Failback werkt niet als een ander IP-adres wordt gebruikt voor de herstelde configuratieserver.

## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [virtuele VMware-machines](vmware-azure-tutorial.md) naar Azure.
