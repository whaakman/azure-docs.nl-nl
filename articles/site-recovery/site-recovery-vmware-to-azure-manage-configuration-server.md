---
title: " Een configuratieserver beheren in Azure Site Recovery | Microsoft Docs"
description: In dit artikel wordt beschreven hoe instellen en beheren van een Server Configuration.
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 10/06/2017
ms.author: anoopkv
ms.openlocfilehash: e4740c96383468713976e5a98881bec13b0c1921
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-a-configuration-server"></a>Een configuratieserver beheren

Configuratieserver fungeert als een coördinator tussen de Site Recovery-services en uw on-premises infrastructuur. Dit artikel wordt beschreven hoe u kunt instellen, configureren en beheren van de configuratieserver.

> [!NOTE]
> [Capaciteitsplanning](site-recovery-capacity-planner.md) is een belangrijke stap om ervoor te zorgen dat u de configuratie-Server met een configuratie die suites implementeren uw vereisten voor het laden. Lees meer over [formaat van de vereisten voor een Server Configuration](#sizing-requirements-for-a-configuration-server).


## <a name="prerequisites"></a>Vereisten
Hier volgen de minimale hardware, software en netwerkconfiguratie is vereist voor het instellen van een Server Configuration.
> [!IMPORTANT]
> Bij het implementeren van een configuratie-Server voor het beveiligen van virtuele VMware-machines, raden wij aan dat u als implementeren een **maximaal beschikbare (HA)** virtuele machine.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-configuration-server-software"></a>Downloaden van de configuratieserver-software

1. Meld u aan bij de Azure portal en blader naar de Recovery Services-kluis.
2. Blader naar **Site Recovery-infrastructuur** > **configuratieservers** (onder voor VMware en fysieke Machines).

  ![Pagina Servers toevoegen](./media/site-recovery-vmware-to-azure-manage-configuration-server/AddServers.png)
3. Klik op de **+ Servers** knop.
4. Op de **Server toevoegen** pagina, klikt u op de knop downloaden om te downloaden van de registratiesleutel. U kunt deze sleutel moet tijdens de installatie van de configuratieserver registreren met Azure Site Recovery-service.
5. Klik op de **downloaden van de Microsoft Azure Site Recovery Unified Setup** koppeling voor het downloaden van de nieuwste versie van de configuratieserver.

  ![Downloadpagina](./media/site-recovery-vmware-to-azure-manage-configuration-server/downloadcs.png)

  > [!TIP]
  Meest recente versie van de configuratieserver kan worden gedownload rechtstreeks vanuit [downloadpagina van Microsoft Download Center](http://aka.ms/unifiedsetup)

## <a name="installing-and-registering-a-configuration-server-from-gui"></a>Installeren en registreren van een configuratieserver uit de gebruikersinterface
[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

## <a name="installing-and-registering-a-configuration-server-using-command-line"></a>Installeren en registreren van een configuratie-Server met behulp van de opdrachtregel

  ```
  UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
  ```

### <a name="sample-usage"></a>Voorbeeld van gebruik
  ```
  MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
  cd C:\Temp\Extracted
  UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "CS" /InstallLocation "D:\" /MySQLCredsFilePath "C:\Temp\MySQLCredentialsfile.txt" /VaultCredsFilePath "C:\Temp\MyVault.vaultcredentials" /EnvType "VMWare"
  ```


### <a name="configuration-server-installer-command-line-arguments"></a>Configuratie Server installatieprogramma opdrachtregelargumenten.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-mysql-credentials-file"></a>Maak een MySql-bestand voor referenties
De parameter MySQLCredsFilePath Neem een bestand als invoer. Het bestand maken met de volgende indeling en geven deze als invoerparameter MySQLCredsFilePath.
```
[MySQLCredentials]
MySQLRootPassword = "Password>"
MySQLUserPassword = "Password"
```
### <a name="create-a-proxy-settings-configuration-file"></a>Maak een configuratiebestand van de proxy-instellingen
De parameter ProxySettingsFilePath Neem een bestand als invoer. Het bestand maken met de volgende indeling en geven deze als invoerparameter ProxySettingsFilePath.

```
[ProxySettings]
ProxyAuthentication = "Yes/No"
Proxy IP = "IP Address"
ProxyPort = "Port"
ProxyUserName="UserName"
ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-configuration-server"></a>Proxy-instellingen voor de configuratieserver wijzigen
1. Aanmelden bij de configuratieserver van uw.
2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op uw.
3. Klik op de **kluis registratie** tabblad.
4. Een nieuw bestand voor de registratie van de kluis downloaden vanuit de portal en geef deze als invoer voor het hulpprogramma.

  ![register-configuratie-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
5. Geef de details op nieuwe Proxy-Server en klik op de **registreren** knop.
6. Open een opdrachtvenster Admin PowerShell.
7. De volgende opdracht uitvoeren
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```

  >[!WARNING]
  Als u Scale-out Process-servers die zijn gekoppeld aan deze Server configuratie hebt, moet u [los van de proxy-instellingen op alle processervers van de scale-out](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#modifying-proxy-settings-for-scale-out-process-server) in uw implementatie.

## <a name="modify-user-accounts-and-passwords"></a>Gebruikersaccounts en wachtwoorden wijzigen

De CSPSConfigTool.exe wordt gebruikt voor het beheren van de gebruikersaccounts die wordt gebruikt voor **automatische detectie van virtuele VMware-machines** en uit te voeren ** Push met het installeren van de Mobility-Service op beveiligde computers. 

1. Aanmelden bij de server van uw configuratie.
2. Start de CSPSConfigtool.exe door te klikken op de snelkoppeling beschikbaar op het bureaublad.
3. Klik op de **Accounts beheren** tabblad.
4. Selecteer het account waarvoor het wachtwoord moet worden gewijzigd en klik op de **bewerken** knop.
5. Voer het nieuwe wachtwoord en klikt u op **OK**


## <a name="re-register-a-configuration-server-with-the-same-recovery-services-vault"></a>Opnieuw registreren van een configuratie-Server met dezelfde Recovery Services-kluis
  1. Aanmelden bij de configuratieserver van uw.
  2. Start de cspsconfigtool.exe met behulp van de snelkoppeling op het bureaublad.
  3. Klik op de **kluis registratie** tabblad.
  4. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.
        ![register-configuratie-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
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
  Als u Scale-out Process-servers die zijn gekoppeld aan deze Server configuratie hebt, moet u [opnieuw registreren alle processervers van de scale-out](site-recovery-vmware-to-azure-manage-scaleout-process-server.md#re-registering-a-scale-out-process-server) in uw implementatie.

## <a name="registering-a-configuration-server-with-a-different-recovery-services-vault"></a>Een configuratie-Server registreren met een andere Recovery Services-kluis.

> [!WARNING]
> De volgende stap instelt, scheidt u de configuratie van de huidige kluis en de replicatie van alle beveiligde virtuele machines onder de configuratieserver is gestopt.

1. Aanmelden bij de configuratieserver van uw.
2. Voer de opdracht uit vanaf een opdrachtprompt admin

    ```
    reg delete HKLM\Software\Microsoft\Azure Site Recovery\Registration
    net stop dra
    ```
3. Start de cspsconfigtool.exe met behulp van de snelkoppeling op uw.
4. Klik op de **kluis registratie** tabblad.
5. Een nieuwe registratiebestand downloaden via de portal en geef deze als invoer voor het hulpprogramma.

    ![register-configuratie-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/register-csonfiguration-server.png)
6. Geef de details van de proxyserver en klik op de **registreren** knop.  
7. Open een opdrachtvenster Admin PowerShell.
8. De volgende opdracht uitvoeren
    ```
    $pwd = ConvertTo-SecureString -String MyProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
    ```

## <a name="upgrading-a-configuration-server"></a>Upgraden van een configuratieserver

> [!WARNING]
> Updates worden alleen tot de N-4th versie ondersteund. Bijvoorbeeld, als de nieuwste versie in de markt 9.11 is, kunt klikt u vervolgens u bijwerken van versie 9.10, 9,9, 9,8 of 9.7 rechtstreeks naar 9.11. Maar als u op elke versie moet kleiner dan of gelijk aan en 9,6 vervolgens bijwerken naar ten minste 9.7 voordat u kunt de meest recente updates die u aan bij de Server van uw configuratie toepassen. Downloaden van koppelingen voor de vorige versie kan worden gevonden in het [Azure Site Recovery service-updates](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx)

1. Download het installatieprogramma op de configuratieserver.
2. Het installatieprogramma starten door te dubbelklikken op het installatieprogramma.
3. Het installatieprogramma detecteert de versie van de Site Recovery-onderdelen aanwezig zijn op de computer en de prompt voor een bevestiging. 
4. Klik op OK om te bevestigen dat de & Doorgaan met de upgrade.


## <a name="delete-or-unregister-a-configuration-server"></a>Verwijderen of de registratie van een configuratieserver

> [!WARNING]
> Zorg ervoor dat het volgende voordat u begint met het buiten gebruik stellen van de configuratieserver.
> 1. [Schakel de beveiliging](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) voor alle virtuele machines op deze Server Configuration.
> 2. [Loskoppelen](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) en [verwijderen](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) alle beleidsregels voor replicatie van de configuratieserver.
> 3. [Verwijder](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) alle Vcenter-servers/vSphere-hosts die gekoppeld aan de configuratieserver zijn.


### <a name="delete-the-configuration-server-from-azure-portal"></a>Verwijderen van de configuratieserver via Azure-portal
1. Blader in de Azure-portal naar **Site Recovery-infrastructuur** > **configuratieservers** in het kluismenu.
2. Klik op de Server van de configuratie die u wilt buiten gebruik stellen.
3. Klik op de knop verwijderen op de pagina met details de configuratieserver.

  ![Delete-configuratie-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.PNG)
4. Klik op **Ja** het verwijderen van de server te bevestigen.

### <a name="uninstall-the-configuration-server-software-and-its-dependencies"></a>Verwijderen van de software van de configuratieserver en de bijbehorende afhankelijkheden
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
  * MySQL-Server 5.5
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

## <a name="renew-configuration-server-secure-socket-layerssl-certificates"></a>Configuratie Server Secure Socket Layer(SSL) certificaten vernieuwen
De configuratie-Server heeft een ingebouwde webserver, die de activiteiten van de Mobility-Service, proces-Servers en hoofddoel servers die zijn verbonden met de configuratieserver ingedeeld. De configuratieserver webserver maakt gebruik van een SSL-certificaat om de clients te verifiëren. Dit certificaat heeft een verloopdatum van drie jaar en op elk gewenst moment met de volgende methode kan worden vernieuwd:

> [!WARNING]
Verloopdatum voor certificaten kan alleen worden uitgevoerd op versie 9.4.XXXX. X of hoger. Alle Azure Site Recovery-onderdelen (configuratieserver, processerver, Hoofddoelserver, Mobility-Service) bijwerken voordat u de werkstroom certificaten vernieuwen.

1. Blader op de Azure-portal naar uw kluis > Site Recovery-infrastructuur > configuratieserver.
2. Klik op de Server van de configuratie die u moet het SSL-certificaat voor vernieuwen.
3. Onder de status configuratieserver ziet u de vervaldatum voor het SSL-certificaat.
4. Vernieuw het certificaat door te klikken op de **certificaten vernieuwen** actie, zoals wordt weergegeven in de volgende afbeelding:

  ![Delete-configuratie-server](./media/site-recovery-vmware-to-azure-manage-configuration-server/renew-cert-page.png)

### <a name="secure-socket-layer-certificate-expiry-warning"></a>Secure Socket Layer waarschuwing. certificaat verlopen

> [!NOTE]
De geldigheid van het SSL-certificaat voor alle installaties die hebben plaatsgevonden vóór mei 2016 is ingesteld op één jaar. u hebt gestart met het certificaat verlopen-meldingen weergegeven in de Azure-portal te zien.

1. Als de configuratieserver SSL-certificaat is verlopen in de volgende twee maanden, start de service voor het verwittigen van gebruikers via de Azure-portal & e-mailadres (u moet zich abonneren op meldingen van de Azure Site Recovery). U starten ziet een meldingsbanner op de kluis bronpagina.

  ![certificaat-melding](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-renew-notification.png)
2. Klik op de banner voor aanvullende details over de verloopdatum voor certificaten.

  ![details van certificaat](./media/site-recovery-vmware-to-azure-manage-configuration-server/ssl-cert-expiry-details.png)

  >[!TIP]
  Als in plaats van een **nu vernieuwen** knop u ziet een **nu bijwerken** knop. De knop Nu bijwerken geeft aan dat er een aantal onderdelen in uw omgeving die nog niet zijn bijgewerkt naar 9.4.xxxx.x of hogere versies.

## <a name="revive-a-configuration-server-if-the-secure-socket-layer-ssl-certificate-expired"></a>Een configuratieserver schud als de Secure Socket Layer (SSL)-certificaat is verlopen

1. Bijwerken van uw configuratie-Server de [meest recente versie](http://aka.ms/unifiedinstaller)
2. Als er geen processervers Scale-out, bijwerken hoofddoel Failback-servers, Failback processervers deze naar de meest recente versie
3. De Mobility-Service op de beveiligde virtuele machines bijwerken naar de nieuwste versie.
4. Aanmelden bij de configuratieserver en open een opdrachtprompt met beheerdersbevoegdheden.
5. Blader naar de map %ProgramData%\ASR\home\svsystems\bin
6. RenewCerts.exe om te vernieuwen van het SSL-certificaat op de configuratieserver worden uitgevoerd.
7. Als het proces is voltooid, ziet u het bericht 'certificaatvernieuwing is geslaagd'


## <a name="sizing-requirements-for-a-configuration-server"></a>Formaat van de vereisten voor een configuratie-Server

| **CPU** | **Geheugen** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- | --- |
| 8 vcpu's (2-sockets * @ 2,5 GHz-4 kernen) |16 GB |300 GB |500 GB of minder |Minder dan 100 machines repliceren. |
| 12 vcpu's (2-sockets * @ 2,5 GHz-6 kernen) |18 GB |600 GB |500 GB tot 1 TB |100 tot 150-machines repliceren. |
| 16 vcpu's (2-sockets * @ 2,5 GHz-8 kernen) |32 GB |1 TB |1 TB tot 2 TB |Repliceren tussen 150 tot 200-machines. |

  >[!TIP]
  Als uw dagelijkse gegevensverloop groter is dan 2 TB, of u wilt meer dan 200 virtuele machines repliceren, wordt het aanbevolen om aanvullende processen servers om taken te verdelen het replicatieverkeer te implementeren. Meer informatie over het implementeren van Scale-out proces-servers.


## <a name="common-issues"></a>Algemene problemen
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]
