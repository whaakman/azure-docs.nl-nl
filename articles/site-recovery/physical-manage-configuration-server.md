---
title: De configuratieserver voor herstel na noodgevallen van on-premises fysieke servers naar Azure met Azure Site Recovery beheren | Microsoft-Docs
description: In dit artikel wordt beschreven hoe u de Azure Site Recovery-configuratieserver voor herstel van de fysieke server na noodgevallen naar Azure beheren.
services: site-recovery
author: mayurigupta13
ms.service: site-recovery
ms.topic: article
ms.date: 02/28/2019
ms.author: mayg
ms.openlocfilehash: 338c4a97bced7d9e524f96fcd82f19e5230ff143
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317337"
---
# <a name="manage-the-configuration-server-for-physical-server-disaster-recovery"></a>De configuratieserver voor herstel na noodgevallen van fysieke beheren

Instellen van een on-premises configuratieserver wanneer u de [Azure Site Recovery](site-recovery-overview.md) service voor herstel na noodgeval voor fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen on-premises machines en Azure, en beheert de gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.

## <a name="prerequisites"></a>Vereisten

De tabel bevat een overzicht van de vereisten voor het implementeren van de on-premises configuratie van server-computer.

| **Onderdeel** | **Vereiste** |
| --- |---|
| CPU-kernen| 8 |
| RAM | 16 GB|
| Aantal schijven | 3, met inbegrip van de OS-schijf, cacheschijf proces en bewaarstation voor failback |
| Vrije schijfruimte (cache van de processerver) | 600 GB
| Vrije schijfruimte (bewaarschijf) | 600 GB|
| Besturingssysteem  | Windows Server 2012 R2 <br> Windows Server 2016 |
| Landinstelling van het besturingssysteem | English (US)|
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server-functies | Deze rollen niet worden ingeschakeld: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| Groepsbeleid| Geen groepsbeleid voor deze inschakelen: <br> -Toegang tot de opdrachtprompt voorkomen <br> -Toegang tot registerbewerkingsprogramma's voorkomen <br> -Logica vertrouwen voor bestandsbijlagen <br> -Uitvoering van Script inschakelen <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
| IIS | -Er zijn geen bestaande standaardwebsite <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br> -Er is geen bestaande website/toepassing luistert op poort 443<br>|
| Type NIC | VMXNET3 (indien geïmplementeerd als een VMware-VM) |
| Type IP-adres | Statisch |
| Toegang tot het internet | De server moet toegang hebben tot deze URL's: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://management.azure.com <br> - *.services.visualstudio.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi (niet vereist voor Scale-out processervers) <br> - time.nist.gov <br> - time.windows.com |
| Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport)|

## <a name="download-the-latest-installation-file"></a>Download het recentste installatiebestand

De meest recente versie van het configuratiebestand van de server-installatie is beschikbaar in de Site Recovery-portal. Bovendien kan worden gedownload rechtstreeks vanuit de [Microsoft Download Center](https://aka.ms/unifiedsetup).

1. Meld u aan bij de Azure-portal en blader naar de Recovery Services-kluis.
2. Blader naar **infrastructuur voor Site Recovery** > **configuratieservers** (onder voor VMware en fysieke Machines).
3. Klik op de **+ Servers** knop.
4. Op de **-Server toevoegen** pagina, klikt u op de knop downloaden om te downloaden van de registratiesleutel. Deze sleutel moet u tijdens de installatie van de configuratieserver registreren bij Azure Site Recovery-service.
5. Klik op de **downloaden van de Microsoft Azure Site Recovery geïntegreerde Setup** koppeling om te downloaden van de meest recente versie van de configuratieserver.

   ![Downloadpagina](./media/physical-manage-configuration-server/downloadcs.png)


## <a name="install-and-register-the-server"></a>Installeer en registreer de server

1. Voer het installatiebestand voor de geïntegreerde Setup uit.
2. In **voordat u begint**, selecteer **de configuratieserver en processerver installeren**.

    ![Voordat u begint](./media/physical-manage-configuration-server/combined-wiz1.png)

3. Klik bij **Licentievoorwaarden voor software van derden** op **Ik ga akkoord** om MySQL te downloaden en te installeren.
4. Geef bij **Internetinstellingen** op hoe de provider die op de configuratieserver wordt uitgevoerd, via internet verbinding moet maken met Azure Site Recovery. Zorg ervoor dat u hebt de vereiste URL's toegestaan.

    - Als u wilt verbinding maken met de proxy die momenteel ingesteld op de machine, selecteer **verbinding maken met Azure Site Recovery via een proxyserver**.
    - Als u wilt dat de Provider rechtstreeks verbinding maakt, selecteert u **rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
    - Als de bestaande proxy verificatie is vereist of als u wilt een aangepaste proxy gebruikt voor verbinding met de Provider, selecteer **verbinding maken met aangepaste proxyinstellingen**, en geeft u het adres, poort en referenties.
     ![Firewall](./media/physical-manage-configuration-server/combined-wiz4.png)
6. Tijdens Setup wordt in **Controle op vereisten** gecontroleerd of de installatie kan worden uitgevoerd. Als er een waarschuwing wordt weergegeven over **Synchronisatiecontrole voor algemene tijd**, moet u controleren of de tijd op de systeemklok (instellingen voor **datum en tijd**) overeenkomt met de tijdzone.

    ![Vereisten](./media/physical-manage-configuration-server/combined-wiz5.png)
7. Maak bij **MySQL-configuratie** referenties voor aanmelden bij de MySQL-serverinstantie die is geïnstalleerd.

    ![MySQL](./media/physical-manage-configuration-server/combined-wiz6.png)
8. Selecteer bij **Details van de omgeving** of u virtuele VMware-machines wilt repliceren. Als u bent, klikt u vervolgens controleert Setup of PowerCLI 6.0 is geïnstalleerd.
9. Selecteer bij **Installatielocatie** waar u de binaire bestanden wilt installeren en de cache wilt opslaan. Het station dat u selecteert, moet ten minste 5 GB vrije schijfruimte bevatten, maar wij raden u aan een cachestation te gebruiken met minstens 600 GB vrije ruimte.

    ![Installatielocatie](./media/physical-manage-configuration-server/combined-wiz8.png)
10. In **Netwerkselectie**, selecteert u eerst de NIC die gebruikmaakt van de ingebouwde processerver voor detectie en push-installatie van de mobility-service op de bronmachines, en selecteer vervolgens de NIC die gebruikmaakt van de configuratieserver voor connectiviteit met Azure. Poort 9443 is de standaardpoort voor het verzenden en ontvangen van replicatieverkeer, maar u kunt dit poortnummer aanpassen aan de vereisten van de omgeving. Naast poort 9443 wordt ook poort 443 geopend. Deze wordt door een webserver gebruikt om replicatiebewerkingen in te delen. Gebruik poort 443 niet voor het verzenden of ontvangen van replicatieverkeer.

    ![Netwerk selecteren](./media/physical-manage-configuration-server/combined-wiz9.png)


11. Lees de informatie bij **Samenvatting** en klik op **Installeren**. Wanneer de installatie is voltooid, wordt er een wachtwoordzin gegenereerd. U hebt deze nodig bij het inschakelen van de replicatie. Kopieer de wachtwoordzin daarom en bewaar deze op een veilige locatie.


Na voltooiing van de registratie wordt de server weergegeven op de blade **Instellingen** > **Servers** in de kluis.


## <a name="install-from-the-command-line"></a>Installeren vanaf de opdrachtregel

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
MySQLRootPassword = "Password"
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
## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Proxy-instellingen voor de configuratie van server-machine kunt u als volgt wijzigen:

1. Aanmelden bij de configuratieserver.
2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
3. Klik op de **kluis registratie** tabblad.
4. Een nieuwe kluis registratie-bestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.

   ![register-configuratie-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Geef de nieuwe proxy-informatie op en klik op de **registreren** knop.
6. Open een opdrachtvenster Admin PowerShell.
7. Voer de volgende opdracht uit:

   ```PowerShell
   $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
   net stop obengine
   net start obengine
   ```

   > [!WARNING]
   > Als u extra processervers die is gekoppeld aan de configuratieserver hebt, moet u [los van de proxy-instellingen op alle processervers van de scale-out](vmware-azure-manage-process-server.md#modify-proxy-settings-for-an-on-premises-process-server) in uw implementatie.

## <a name="reregister-a-configuration-server-with-the-same-vault"></a>Een configuratieserver bij dezelfde kluis registreren
1. Meld u aan bij uw configuratieserver.
2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
3. Klik op de **kluis registratie** tabblad.
4. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.
      ![register-configuration-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. Geef de details van de proxyserver en klikt u op de **registreren** knop.  
6. Open een opdrachtvenster Admin PowerShell.
7. De volgende opdracht uitvoeren

    ```PowerShell
    $Pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $Pwd
    net stop obengine
    net start obengine
    ```

   > [!WARNING]
   > Als u meerdere processerver hebt, moet u [registreert](vmware-azure-manage-process-server.md#reregister-a-process-server).

## <a name="register-a-configuration-server-with-a-different-vault"></a>Een configuratieserver registreren bij een andere kluis

> [!WARNING]
> De volgende stap de configuratieserver in de huidige kluis schaduwkopieën verbroken en de replicatie van alle beveiligde virtuele machines onder de configuratieserver is gestopt.

1. Meld u aan bij de configuratieserver
2. Voer de opdracht uit vanaf een beheerprompt:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
4. Klik op de **kluis registratie** tabblad.
5. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.
6. Geef de details van de proxyserver en klikt u op de **registreren** knop.  
7. Open een opdrachtvenster Admin PowerShell.
8. De volgende opdracht uitvoeren
    ```powershell
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrade-a-configuration-server"></a>Een configuratieserver upgraden

U uitvoeren updatepakketten voor het bijwerken van de configuratieserver. Updates kunnen worden toegepast voor maximaal N-4 versies. Bijvoorbeeld:

- Als u werkt met 9.7, 9,8, 9,9 of 9.10 - kunt u rechtstreeks naar 9.11 upgraden.
- Als u 9,6 of een eerdere versie uitvoert en u wilt upgraden naar 9.11, moet u eerst upgraden naar versie 9.7. voordat u 9.11.

Koppelingen naar de updatepakketten voor het upgraden van alle versies van de configuratieserver zijn beschikbaar in de [wiki updates pagina](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

De server als volgt bijwerken:

1. Download het installatiebestand van de update naar de configuratieserver.
2. Dubbelklik erop om het installatieprogramma uitvoert.
3. Het installatieprogramma detecteert de huidige versie die wordt uitgevoerd op de machine.
4. Klik op **OK** om te bevestigen en de upgrade uitvoert. 


## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

> [!WARNING]
> Controleer het volgende voordat u begint met het buiten gebruik stellen van uw configuratieserver.
> 1. [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle virtuele machines onder deze configuratieserver.
> 2. [Loskoppelen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) en [verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) alle beleidsregels voor replicatie vanaf de configuratieserver.
> 3. [Verwijder](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) alle vCenters servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.


### <a name="delete-the-configuration-server-from-azure-portal"></a>De configuratieserver verwijderen uit Azure portal
1. Blader in Azure portal naar **Site Recovery-infrastructuur** > **configuratieservers** in het menu kluis.
2. Klik op de configuratieserver die u wilt buiten gebruik stellen.
3. Klik op de pagina details van de configuratieserver, de **verwijderen** knop.
4. Klik op **Ja** om de verwijdering van de server te bevestigen.

### <a name="uninstall-the-configuration-server-and-its-dependencies"></a>Verwijderen van de configuratieserver en de bijbehorende afhankelijkheden
> [!TIP]
>   Als u van plan bent om de configuratieserver met Azure Site Recovery opnieuw opnieuw te gebruiken, kunt u overslaan naar stap 4 rechtstreeks

1. Meld u aan bij de configuratieserver als beheerder.
2. Open het Configuratiescherm > programma > programma's verwijderen
3. Verwijder de programma's in de volgende volgorde:
   * Microsoft Azure Recovery Services-agent
   * Microsoft Azure Site Recovery Mobility Service/Master Target server
   * Microsoft Azure Site Recovery-Provider
   * Microsoft Azure Site Recovery-configuratieserver/Server-proces
   * Microsoft Azure Site Recovery Configuration Serverafhankelijkheden
   * MySQL Server 5.5
4. Voer de volgende opdracht uit en een opdrachtprompt met beheerdersrechten.
   ```
   reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
   ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Verwijderen of de registratie van een configuratieserver (PowerShell)

1. [Installeer](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell-module
2. Meld u aan bij uw Azure-account met de opdracht
    
    `Connect-AzureRmAccount`
3. Selecteer het abonnement waarin de kluis aanwezig is

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Nu de context van uw kluis instellen
    
    ```PowerShell
    $Vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $Vault
    ```
4. Selecteer uw configuratieserver ophalen

    `$Fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. De configuratieserver verwijderen

    `Remove-AzureRmSiteRecoveryFabric -Fabric $Fabric [-Force] `

> [!NOTE]
> De **-forceren** optie in de Remove-AzureRmSiteRecoveryFabric kan worden gebruikt om af te dwingen de verwijdering/verwijdering van de configuratieserver.

## <a name="renew-ssl-certificates"></a>SSL-certificaten vernieuwen
De configuratieserver heeft een ingebouwde webserver, die regelt de activiteiten van de Mobility-service, processervers en hoofddoelservers die zijn verbonden. De webserver wordt een SSL-certificaat gebruikt om clients te verifiëren. Het certificaat verloopt na drie jaar en kan op elk gewenst moment worden vernieuwd.

### <a name="check-expiry"></a>Controleer de vervaldatum

Configuratie van implementaties van de server voordat mei 2016, is verlopen van het certificaat ingesteld op één jaar. Als u gaat een certificaat is verlopen, gebeurt het volgende:

- Wanneer de vervaldatum is twee maanden of minder, start de service verzenden van meldingen in de portal en via e-mail (als u zich hebt geabonneerd op meldingen voor Azure Site Recovery).
- Een meldingsbanner wordt weergegeven op de pagina van de kluis-resource. Klik op de banner voor meer informatie.
- Als u ziet een **nu bijwerken** knop, dit geeft aan dat er bepaalde onderdelen in uw omgeving die nog niet hebt bijgewerkt naar 9.4.xxxx.x of hogere versies. Onderdelen bijwerken voordat u het certificaat vernieuwen. Kunt u vernieuwen niet in oudere versies.

### <a name="renew-the-certificate"></a>Het certificaat vernieuwen

1. Open in de kluis **Site Recovery-infrastructuur** > **configuratieserver**, en klik op de vereiste configuratie-server.
2. De vervaldatum wordt weergegeven onder **configuratie-serverstatus**
3. Klik op **certificaten vernieuwen**. 




## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [fysieke servers](tutorial-physical-to-azure.md) naar Azure.

