---
title: " Beheren van de configuratieserver voor noodherstel van de fysieke server met Azure Site Recovery | Microsoft Docs"
description: Dit artikel wordt beschreven hoe u een bestaande configuratieserver voor noodherstel fysieke server naar Azure, met de Azure Site Recovery-service beheert.
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 04/11/2018
ms.author: anoopkv
ms.openlocfilehash: 84969ff04684003a04f99b4fbf7f03be4140a277
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="manage-the-configuration-server-for-physical-server-disaster-recovery"></a>De configuratieserver voor noodherstel van de fysieke server beheren

Instellen van een on-premises configuratieserver wanneer u de [Azure Site Recovery](site-recovery-overview.md) service voor herstel na noodgevallen van fysieke servers naar Azure. De configuratieserver coördineert de communicatie tussen de on-premises machines en Azure en beheert gegevensreplicatie. In dit artikel bevat een overzicht van algemene taken voor het beheren van de configuratieserver nadat deze geïmplementeerd.

## <a name="prerequisites"></a>Vereisten

De tabel ziet u de prerequistes voor het implementeren van de lokale configuratie-servercomputer.

| **Onderdeel** | **Vereiste** |
| --- |---|
| CPU-kernen| 8 |
| RAM | 16 GB|
| Aantal schijven | 3, met inbegrip van de OS-schijf, proces server cache schijf en bewaarstation voor failback |
| Vrije schijfruimte (cache van de processerver) | 600 GB
| Vrije schijfruimte (bewaarschijf) | 600 GB|
| Besturingssysteem  | Windows Server 2012 R2 <br> Windows Server 2016 |
| Landinstelling van het besturingssysteem | English (US)|
| VMware vSphere PowerCLI-versie | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Windows Server-functies | Schakel niet in deze rollen: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V |
| Groepsbeleid| Geen groepsbeleid voor deze inschakelen: <br> -Voorkomen dat toegang tot de opdrachtprompt <br> -Toegang tot het register te bewerken van hulpprogramma's voorkomen <br> -Logica vertrouwen voor bestandsbijlagen <br> -Uitvoering van Script inschakelen <br> [Meer informatie](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
| IIS | -Er zijn geen bestaande standaardwebsite <br> -Inschakelen [anonieme verificatie](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Inschakelen [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) instelling  <br> -Niet bestaande reeds websitetoepassing geluisterd op poort 443<br>|
| NIC-type | VMXNET3 (indien geïmplementeerd als een VMware-VM) |
| Type IP-adres | Statisch |
| Toegang tot het internet | De server heeft toegang tot deze URL's: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - dc.services.visualstudio.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (niet vereist voor Scale-out proces Servers) <br> - time.nist.gov <br> - time.windows.com |
| Poorten | 443 (Orchestration-besturingselement)<br>9443 (Gegevenstransport)|

## <a name="download-the-latest-installation-file"></a>Download de meest recente installatiebestand

De meest recente versie van het configuratiebestand van de server-installatie is beschikbaar in de Site Recovery-portal. Bovendien kan worden gedownload rechtstreeks vanuit de [Microsoft Download Center](http://aka.ms/unifiedsetup).

1. Meld u aan bij de Azure portal en blader naar de Recovery Services-kluis.
2. Blader naar **Site Recovery-infrastructuur** > **configuratieservers** (onder voor VMware en fysieke Machines).
3. Klik op de **+ Servers** knop.
4. Op de **Server toevoegen** pagina, klikt u op de knop downloaden om te downloaden van de registratiesleutel. U kunt deze sleutel moet tijdens de installatie van de configuratieserver registreren met Azure Site Recovery-service.
5. Klik op de **downloaden van de Microsoft Azure Site Recovery Unified Setup** koppeling voor het downloaden van de nieuwste versie van de configuratieserver.

  ![Downloadpagina](./media/physical-manage-configuration-server/downloadcs.png)


## <a name="install-and-register-the-server"></a>Installeer en registreer de server

1. Voer het installatiebestand voor de geïntegreerde Setup uit.
2. In **voordat u begint**, selecteer **installeren van de configuratieserver en de processerver**.

    ![Voordat u begint](./media/physical-manage-configuration-server/combined-wiz1.png)

3. Klik bij **Licentievoorwaarden voor software van derden** op **Ik ga akkoord** om MySQL te downloaden en te installeren.
4. Geef bij **Internetinstellingen** op hoe de provider die op de configuratieserver wordt uitgevoerd, via internet verbinding moet maken met Azure Site Recovery. Zorg ervoor dat u hebt de gewenste URL's toegestaan.

    - Als u verbinding wilt maken met de proxy die momenteel is ingesteld op de computer, selecteer **verbinding maken met Azure Site Recovery via een proxyserver**.
    - Als u wilt dat de Provider rechtstreeks verbinding maakt, selecteert u **rechtstreeks verbinding maken met Azure Site Recovery zonder proxyserver**.
    - Als de bestaande proxy verificatie vereist is of als u wilt een aangepaste proxy gebruikt voor de providerverbinding, selecteer **verbinding maken met aangepaste proxyinstellingen**, en het adres, poort en referenties opgeven.
     ![Firewall](./media/physical-manage-configuration-server/combined-wiz4.png)
6. Tijdens Setup wordt in **Controle op vereisten** gecontroleerd of de installatie kan worden uitgevoerd. Als er een waarschuwing wordt weergegeven over **Synchronisatiecontrole voor algemene tijd**, moet u controleren of de tijd op de systeemklok (instellingen voor **datum en tijd**) overeenkomt met de tijdzone.

    ![Vereisten](./media/physical-manage-configuration-server/combined-wiz5.png)
7. Maak bij **MySQL-configuratie** referenties voor aanmelden bij de MySQL-serverinstantie die is geïnstalleerd.

    ![MySQL](./media/physical-manage-configuration-server/combined-wiz6.png)
8. Selecteer bij **Details van de omgeving** of u virtuele VMware-machines wilt repliceren. Als u bent, klikt u vervolgens controleert Setup of PowerCLI 6.0 is geïnstalleerd.
9. Selecteer bij **Installatielocatie** waar u de binaire bestanden wilt installeren en de cache wilt opslaan. Het station dat u selecteert, moet ten minste 5 GB vrije schijfruimte bevatten, maar wij raden u aan een cachestation te gebruiken met minstens 600 GB vrije ruimte.

    ![Installatielocatie](./media/physical-manage-configuration-server/combined-wiz8.png)
10. Geef bij **Netwerk selecteren** de listener op (netwerkadapter en SSL-poort) met behulp waarvan de configuratieserver replicatiegegevens verzendt en ontvangt. Poort 9443 is de standaardpoort voor het verzenden en ontvangen van replicatieverkeer, maar u kunt dit poortnummer aanpassen aan de vereisten van de omgeving. Naast poort 9443 wordt ook poort 443 geopend. Deze wordt door een webserver gebruikt om replicatiebewerkingen in te delen. Gebruik geen poort 443 voor het verzenden of ontvangen van replicatieverkeer.

    ![Netwerk selecteren](./media/physical-manage-configuration-server/combined-wiz9.png)


11. Lees de informatie bij **Samenvatting** en klik op **Installeren**. Wanneer de installatie is voltooid, wordt er een wachtwoordzin gegenereerd. U hebt deze nodig bij het inschakelen van de replicatie. Kopieer de wachtwoordzin daarom en bewaar deze op een veilige locatie.


Na voltooiing van de registratie wordt de server weergegeven op de blade **Instellingen** > **Servers** in de kluis.


## <a name="install-from-the-command-line"></a>Installeren vanaf de opdrachtregel

Voer het installatiebestand als volgt uit:

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Voorbeeld van gebruik
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="parameters"></a>Parameters

|Parameternaam| Type | Beschrijving| Waarden|
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

De parameter MySQLCredsFilePath Neem een bestand op als invoer. Het bestand maken met de volgende indeling en geven deze als invoerparameter MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-file-input-for-proxysettingsfilepath"></a>Bestandsinvoer voor ProxySettingsFilePath maken
De parameter ProxySettingsFilePath Neem een bestand als invoer. Het bestand maken met de volgende indeling en geven deze als invoerparameter ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modify-proxy-settings"></a>Proxy-instellingen wijzigen

Proxy-instellingen voor de configuratie van server-machine kunt u als volgt wijzigen:

1. Meld u bij de configuratieserver.
2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op uw.
3. Klik op de **kluis registratie** tabblad.
4. Download een nieuwe kluis registratie vanuit de portal en opgeven als invoer voor het hulpprogramma.

  ![register-configuratie-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
5. De nieuwe proxy-gegevens op en klik op de **registreren** knop.
6. Open een opdrachtvenster Admin PowerShell.
7. Voer de volgende opdracht uit:
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Als u aanvullende processen-servers die zijn gekoppeld aan de configuratieserver hebt, moet u [los van de proxy-instellingen op alle processervers van de scale-out](vmware-azure-manage-process-server.md#modify-proxy-settings-for-an-on-premises-process-server) in uw implementatie.

## <a name="reregister-a-configuration-server-with-the-same-vault"></a>Een configuratieserver met dezelfde kluis te registreren
  1. Aanmelden bij de configuratieserver van uw.
  2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
  3. Klik op de **kluis registratie** tabblad.
  4. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.
        ![register-configuratie-server](./media/physical-manage-configuration-server/register-csconfiguration-server.png)
  5. Geef de details van de proxyserver en klik op de **registreren** knop.  
  6. Open een opdrachtvenster Admin PowerShell.
  7. De volgende opdracht uitvoeren

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

  >[!WARNING]
  Als u meerdere processerver hebt, moet u [registreert](vmware-azure-manage-process-server.md#reregister-a-process-server).

## <a name="register-a-configuration-server-with-a-different-vault"></a>Een configuratieserver registreren bij een andere kluis

> [!WARNING]
> De configuratieserver uit de huidige kluis instelt, scheidt u de volgende stap en de replicatie van alle beveiligde virtuele machines onder de configuratieserver is gestopt.

1. Meld u aan bij de configuratieserver
2. Voer de opdracht vanaf een opdrachtprompt beheerder:

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
4. Klik op de **kluis registratie** tabblad.
5. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.
6. Geef de details van de proxyserver en klik op de **registreren** knop.  
7. Open een opdrachtvenster Admin PowerShell.
8. De volgende opdracht uitvoeren
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrade-a-configuration-server"></a>Een configuratieserver bijwerken

Updatepakketten voor het bijwerken van de configuratieserver worden uitgevoerd. Updates kunnen worden toegepast voor maximaal N-4 versies. Bijvoorbeeld:

- Als u werkt met 9.7, 9,8, 9,9 of 9.10 - kunt u rechtstreeks upgraden naar 9.11.
- Als u 9,6 of eerder uitvoert en u wilt upgraden naar 9.11, moet u eerst bijwerken naar versie 9.7. voordat u 9.11.

Koppelingen naar de updatepakketten voor het upgraden van alle versies van de configuratieserver zijn beschikbaar in de [wiki updates pagina](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

De server als volgt bijwerken:

1. Download het installatiebestand van de update voor de configuratieserver.
2. Dubbelklik erop om het installatieprogramma uitvoert.
3. Het installatieprogramma detecteert de huidige versie op de machine.
4. Klik op **OK** om te bevestigen en de upgrade uitvoert. 


## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

> [!WARNING]
> Zorg ervoor dat het volgende voordat u begint met het buiten gebruik stellen van de configuratieserver.
> 1. [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle virtuele machines op deze Server Configuration.
> 2. [Loskoppelen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) en [verwijderen](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) alle beleidsregels voor replicatie van de configuratieserver.
> 3. [Verwijder](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) alle Vcenter-servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Verwijderen van de configuratieserver via Azure-portal
1. Blader in de Azure-portal naar **Site Recovery-infrastructuur** > **configuratieservers** in het kluismenu.
2. Klik op de configuratieserver die u wilt buiten gebruik stellen.
3. Klik op de pagina met details van de Server van de configuratie op de **verwijderen** knop.
4. Klik op **Ja** het verwijderen van de server te bevestigen.

### <a name="uninstall-the-configuration-server-and-its-dependencies"></a>Verwijderen van de configuratieserver en de bijbehorende afhankelijkheden
  > [!TIP]
  Als u van plan bent om de configuratieserver met Azure Site Recovery opnieuw opnieuw te gebruiken, kunt klikt u vervolgens u doorgaan met stap 4 rechtstreeks

1. Meld u aan bij de configuratieserver als beheerder.
2. Open het Configuratiescherm > programma > programma's verwijderen
3. Verwijder de programma's in de volgende volgorde:
  * Microsoft Azure Recovery Services-agent
  * Microsoft Azure Site Recovery Mobility Service/hoofddoelserver
  * Microsoft Azure Site Recovery Provider
  * Microsoft Azure Site Recovery-configuratieserver Server-proces
  * Microsoft Azure Site Recovery configuratie Server afhankelijkheden
  * MySQL Server 5.5
4. Voer de volgende opdracht uit en admin-opdrachtprompt.
  ```
  reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
  ```

## <a name="delete-or-unregister-a-configuration-server-powershell"></a>Verwijderen of de registratie van een configuratieserver (PowerShell)

1. [Installeer](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) Azure PowerShell-module
2. Aanmelden bij aan uw Azure-account met de opdracht
    
    `Login-AzureRmAccount`
3. Selecteer het abonnement waaronder de kluis aanwezig is

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Nu uw kluis context instellen
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Selecteer uw configuratieserver ophalen

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. De configuratieserver verwijderen

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> De **-Force** optie in de Remove-AzureRmSiteRecoveryFabric kan worden gebruikt om af te dwingen de verwijdering/verwijdering van de configuratieserver.

## <a name="renew-ssl-certificates"></a>SSL-certificaten vernieuwen
De configuratieserver heeft een ingebouwde webserver, die de activiteiten van de Mobility-service, processervers en hoofddoelservers verbonden ingedeeld. Een SSL-certificaat de webserver gebruikmaakt om clients te verifiëren. Het certificaat verloopt na drie jaar en op elk gewenst moment kan worden vernieuwd.

### <a name="check-expiry"></a>Controleer de vervaldatum

Voor implementaties van configuration server vóór mei 2016, is verloopdatum voor certificaten ingesteld op één jaar. Als u hebt is een certificaat vereist, is verlopen, gebeurt het volgende:

- Wanneer de vervaldatum is twee maanden of minder, door de service wordt gestart verzenden van meldingen in de portal en per e-mail (als u zich hebt geabonneerd op Azure Site Recovery meldingen).
- Er verschijnt een meldingsbanner op de pagina van de resource kluis. Klik op de banner voor meer informatie.
- Als u ziet een **nu bijwerken** knop, dit geeft aan dat er een aantal onderdelen in uw omgeving die nog niet zijn bijgewerkt naar 9.4.xxxx.x of hogere versies. Onderdelen worden bijgewerkt voordat u het certificaat vernieuwen. Bij oudere versies kunt u niet vernieuwen.

### <a name="renew-the-certificate"></a>Het certificaat vernieuwen

1. Open in de kluis **Site Recovery-infrastructuur** > **configuratieserver**, en klik op de vereiste configuratie-server.
2. De vervaldatum weergegeven onder **configuratie-serverstatus**
3. Klik op **certificaten vernieuwen**. 




## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk de zelfstudies voor het instellen van herstel na noodgevallen van [fysieke servers](tutorial-physical-to-azure.md) naar Azure.

