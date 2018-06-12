---
title: Instellen van een processerver in Azure voor VMware-VM en failback van de fysieke server met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe een processerver in Azure, voor failback virtuele Azure-machines naar VMware instellen.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 06/10/2018
ms.author: raynew
ms.openlocfilehash: 3e53954341136a293052f9af755515a5552432fe
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300844"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Aanvullende processen servers voor schaalbaarheid instellen

Wanneer u repliceert VMware-machines of fysieke servers naar Azure worden verkregen met [siteherstel](site-recovery-overview.md), een processerver is geïnstalleerd op de servercomputer van de configuratie en wordt gebruikt voor het coördineren van gegevensoverdracht tussen Site Recovery en uw on-premises infrastructuur. U kunt aanvullende zelfstandige processervers toevoegen voor een verhoging van de capaciteit en scale-out-implementatie van uw replicatie. Dit artikel wordt beschreven hoe u dit doet.

## <a name="before-you-start"></a>Voordat u begint

### <a name="capacity-planning"></a>Capaciteitsplanning

Zorg ervoor dat u hebt uitgevoerd [capaciteitsplanning](site-recovery-plan-capacity-vmware.md) voor VMware-replicatie. Dit helpt u om te bepalen hoe en wanneer moet u extra processervers implementeren.

### <a name="sizing-requirements"></a>Vereisten voor schaling 

Controleer of de sizing vereisten samengevat in de tabel. Als u hebt voor het schalen van uw implementatie om meer dan 200 bronmachines of hebt u een totaal dagelijks verloop in verhouding van meer dan 2 TB, moet u in het algemeen aanvullende processen servers voor de verwerking van het netwerkverkeer.

| **Aanvullende processerver** | **Cachegrootte van de schijf** | **Snelheid van de gegevens wijzigen** | **Beveiligde machines** |
| --- | --- | --- | --- |
|4 vcpu's (2-sockets * 2 kernen @ 2,5 GHz), 8 GB geheugen |300 GB |250 GB of minder |85 of minder machines repliceren. |
|8 vcpu's (2-sockets * 4 kernen @ 2,5 GHz), 12 GB geheugen |600 GB |250 GB tot 1 TB |Repliceren tussen 85 150-machines. |
|12 vcpu's (2-sockets * 6 kernen @ 2,5 GHz) 24 GB geheugen |1 TB |1 TB tot 2 TB |Repliceren tussen 150 225 machines. |

### <a name="prerequisites"></a>Vereisten

De vereisten voor de aanvullende processerver worden samengevat in de volgende tabel.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Download het installatiebestand

Download het installatiebestand voor de processerver als volgt:

1. Meld u aan bij de Azure portal en blader naar de Recovery Services-kluis.
2. Open **Site Recovery-infrastructuur** > **VMWare en fysieke Machines** > **configuratieservers** (onder voor VMware en fysieke Machines).
3. Selecteer de configuratieserver voor inzoomen op gegevens van de server. Klik vervolgens op **+ processerver**.
4. In **toevoegen processerver** >  **kiezen waar u wilt implementeren de processerver**, selecteer **implementeren een Scale-out processerver lokale**.

  ![Pagina Servers toevoegen](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Klik op **downloaden van de Microsoft Azure Site Recovery Unified Setup**. Hiermee downloadt u de nieuwste versie van het bestand voor installatie.

  > [!WARNING]
  De versie van de processerver installatie moet hetzelfde zijn als, of een eerdere, de configuratieversie van de server die u hebt uitgevoerd. Een eenvoudige manier om ervoor te zorgen versiecompatibiliteit is het gebruik van hetzelfde installatieprogramma, waarmee u meest recent installeren of bijwerken van de configuratieserver.

## <a name="install-from-the-ui"></a>Installeren via de gebruikersinterface

Als volgt installeren. Na het instellen van de server, moet u bronmachines voor het gebruik van deze migreren.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Installeren vanaf de opdrachtregel

Installeer de volgende opdracht uit te voeren:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Waar opdrachtregelparameters zijn als volgt:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Bijvoorbeeld:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Maken van een proxybestand-instellingen

Als u nodig hebt voor het instellen van een proxy, neem de parameter ProxySettingsFilePath een bestand op als invoer. U kunt als volgt het bestand te maken en geven deze als invoerparameter ProxySettingsFilePath.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [Serverinstellingen beheren verwerken](vmware-azure-manage-process-server.md)
