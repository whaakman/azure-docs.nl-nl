---
title: " Een Scale-out processerver beheren in Azure Site Recovery | Microsoft Docs"
description: In dit artikel wordt beschreven hoe instellen en beheren van een Scale-out processerver in Azure Site Recovery.
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
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: e5c01de19917235c34c035415df86291b9152bf0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="manage-a-scale-out-process-server"></a>Een Scale-out processerver beheren

Scale-out processerver fungeert als een coördinator voor overdracht van gegevens tussen de Site Recovery-services en uw on-premises infrastructuur. Dit artikel wordt beschreven hoe u kunt instellen, configureren en beheren van de processerver van een Scale-out.

## <a name="prerequisites"></a>Vereisten
Hier volgen de aanbevolen hardware, software en netwerkconfiguratie voor het instellen van de processerver van een Scale-out is vereist.

> [!NOTE]
> [Capaciteitsplanning](site-recovery-capacity-planner.md) is een belangrijke stap om ervoor te zorgen dat u de processerver Scale-out met een configuratie die suites implementeert uw vereisten voor het laden. Lees meer over [kenmerken schalen voor een Scale-out processerver](#sizing-requirements-for-a-configuration-server).

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="downloading-the-scale-out-process-server-software"></a>Downloaden van de processerver Scale-out-software
1. Meld u aan bij de Azure portal en blader naar de Recovery Services-kluis.
2. Blader naar **Site Recovery-infrastructuur** > **configuratieservers** (onder voor VMware en fysieke Machines).
3. Selecteer uw configuratieserver Inzoomen op de pagina details van de configuratieserver.
4. Klik op de **+ processerver** knop.
5. In de **toevoegen processerver** pagina **implementeren een Scale-out processerver lokale** optie van de **kiezen waar u wilt implementeren de processerver** vervolgkeuzelijst.

  ![Pagina Servers toevoegen](./media/site-recovery-vmware-to-azure-manage-scaleout-process-server/add-process-server.png)
6. Klik op de **downloaden van de Microsoft Azure Site Recovery Unified Setup** koppeling voor het downloaden van de meest recente versie van de installatie van de processerver Scale-out.

  > [!WARNING]
  De versie van de processerver Scale-out moet gelijk zijn aan of kleiner zijn dan de configuratieserver versie uitvoert in uw omgeving. Een eenvoudige manier om ervoor te zorgen versiecompatibiliteit is het gebruik van dezelfde installatieprogramma bits waarmee u recent uw configuratieserver installeren/bijwerken.

## <a name="installing-and-registering-a-scale-out-process-server-from-gui"></a>Installeren en registreren van de processerver Scale-out van de gebruikersinterface
Als u wilt schalen van uw implementatie voorbij 200 bronmachines of een totale dagelijkse verloop van meer dan 2 TB, moet u extra processervers voor de verwerking van het netwerkverkeer.

Controleer de [grootte aanbevelingen voor het processervers](#size-recommendations-for-the-process-server), en volg deze instructies voor het instellen van de processerver. Na het instellen van de server, moet u bronmachines voor het gebruik van deze migreren.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="installing-and-registering-a-scale-out-process-server-using-command-line"></a>Installeren en registreren van een Scale-out processerver met opdrachtregel

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

### <a name="sample-usage"></a>Voorbeeld van gebruik
```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```

### <a name="scale-out-process-server-installer-command-line-arguments"></a>Scale-out processerver installatieprogramma opdrachtregelargumenten.
[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]


### <a name="create-a-proxy-settings-configuration-file"></a>Maak een configuratiebestand van de proxy-instellingen
De parameter ProxySettingsFilePath Neem een bestand als invoer. Bestand maken met behulp van de volgende indeling en geven deze als invoerparameter ProxySettingsFilePath.
```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```
## <a name="modifying-proxy-settings-for-scale-out-process-server"></a>Proxy-instellingen voor de processerver Scale-out wijzigen
1. Meld u aan bij de processerver van uw Scale-out.
2. Open een opdrachtvenster Admin PowerShell.
3. De volgende opdracht uitvoeren
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
4. Naast Blader naar de map **%PROGRAMDATA%\ASR\Agent** en voer de volgende opdracht
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```

## <a name="re-registering-a-scale-out-process-server"></a>Een Scale-out processerver opnieuw te registreren
[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

* Naast open een opdrachtprompt beheerder.
* Ga naar de map **%PROGRAMDATA%\ASR\Agent** en voer de opdracht

```
cdpcli.exe --registermt

net stop obengine

net start obengine
```

## <a name="upgrading-a-scale-out-process-server"></a>Upgraden van een Scale-out processerver
[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

## <a name="decommissioning-a-scale-out-process-server"></a>Buiten gebruik stellen van een Scale-out processerver
1. Zorg ervoor dat:
  - Status van de verbinding van de configuratieserver wordt weergegeven als **verbonden** in de Azure portal
  - De processerver is nog steeds kunnen communiceren met de configuratieserver.
2. Meld u aan bij de processerver als beheerder
3. Open het Configuratiescherm > programma > programma's verwijderen
4. Verwijder de programma's in de gegeven volgorde te volgen:
  * Microsoft Azure Site Recovery-configuratieserver Server-proces
  * Microsoft Azure Site Recovery configuratie Server afhankelijkheden
  * Microsoft Azure Recovery Services-agent

Het kan maximaal 15 minuten duren voordat de verwijdering van de processerver om weer te geven in de Azure portal.

  > [!NOTE]
  Als de processerver kan niet communiceren met de configuratieserver is (status in de portal verbinding wordt verbroken), moet u de volgende stappen voor het opschonen van de configuratieserver.

## <a name="unregistering-a-disconnected-scale-out-process-server-from-a-configuration-server"></a>De registratie van een niet-verbonden processerver van Scale-out van een configuratie-Server

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="sizing-requirements-for-a-scale-out-process-server"></a>Vereisten voor een Scale-out processerver schaling

| **Aanvullende processerver** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- |
|4 vcpu's (2-sockets * 2 kernen @ 2,5 GHz), 8 GB geheugen |300 GB |250 GB of minder |85 of minder machines repliceren. |
|8 vcpu's (2-sockets * 4 kernen @ 2,5 GHz), 12 GB geheugen |600 GB |250 GB tot 1 TB |Repliceren tussen 85 150-machines. |
|12 vcpu's (2-sockets * 6 kernen @ 2,5 GHz) 24 GB geheugen |1 TB |1 TB tot 2 TB |Repliceren tussen 150 225 machines. |
