---
title: Een processerver die wordt gebruikt voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met Azure Site Recovery beheren | Microsoft Docs
description: Dit artikel wordt beschreven voor het beheren van een processerver instellen voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 2c27779719c73adf4d7fc1a61a0c77d03df71815
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925633"
---
# <a name="manage-process-servers"></a>Processervers beheren

Dit artikel worden algemene taken voor het beheren van de processerver van Site Recovery.

De processerver wordt gebruikt om te ontvangen, optimaliseren en het verzenden van gegevens van replicatie naar Azure. Het voert ook een push-installatie van de Mobility-service op virtuele VMware-machines en fysieke servers die u repliceren wilt, en wordt automatische detectie van on-premises machines uitgevoerd. Voor het repliceren van on-premises VMware-machines of fysieke servers naar Azure, is de processerver standaard geïnstalleerd op de servercomputer van de configuratie. 

- Voor grote implementaties moet u mogelijk extra on-premises processervers dat moet capaciteit kunt schalen.
- Voor de failback vanuit Azure naar on-premises, moet u een tijdelijke processerver in Azure instellen. Wanneer failback is voltooid, kunt u deze virtuele machine verwijderen. 

Meer informatie over de processerver.


## <a name="upgrade-a-process-server"></a>Upgrade van een processerver

Wanneer u implementeert een processerver on-premises of als een Azure-VM voor failback, de meest recente versie van de processerver is geïnstalleerd. De Site Recovery teams release oplossingen en verbeteringen op gezette tijden, en we raden dat u processervers up-to-date houden. U kunt een processerver als volgt bijwerken:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]


## <a name="move-vms-to-balance-the-process-server-load"></a>Verplaatsen van virtuele machines om te verdelen van de belasting van de proces-server

Verdelen door te verplaatsen van virtuele machines tussen twee processervers mogelijk als volgt:

1. In de kluis onder **beheren** klikt u op **Site Recovery-infrastructuur**. Onder **voor VMware en fysieke machines**, klikt u op **configuratieservers**.
2. Klik op de configuratieserver waarmee de processervers zijn geregistreerd.
3. Klik op de processerver waarvan u wilt om verkeer te verdelen.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

4. Klik op **verdelen**, selecteer de doelprocesserver waarnaar u wilt om machines te verplaatsen. Klik vervolgens op **OK**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Klik op **machines selecteren**, en kies de machines die u wilt verplaatsen van de huidige op de doelserver van het proces. Details van de gemiddelde gegevenswijziging worden weergegeven op basis van elke virtuele machine. Klik vervolgens op **OK**. 
3. In de kluis, bewaakt de voortgang van de taak onder **bewaking** > **Site Recovery-taken**.

Het duurt ongeveer 15 minuten voor wijzigingen in de portal worden weergegeven. Voor een snellere effect [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="switch-an-entire-workload-to-another-process-server"></a>Een hele werkbelasting overschakelen naar een andere processerver

De hele werkbelasting als volgt verwerkt door een processerver naar een andere processerver verplaatsen:

1. In de kluis onder **beheren** klikt u op **Site Recovery-infrastructuur**. Onder **voor VMware en fysieke machines**, klikt u op **configuratieservers**.
2. Klik op de configuratieserver waarmee de processervers zijn geregistreerd.
3. Klik op de processerver van waaruit u wilt overschakelen van de werkbelasting.
4. Klik op **Switch**, selecteer de doelprocesserver waarnaar u wilt verplaatsen van de werkbelasting. Klik vervolgens op **OK**

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

5. In de kluis, bewaakt de voortgang van de taak onder **bewaking** > **Site Recovery-taken**.

Het duurt ongeveer 15 minuten voor wijzigingen in de portal worden weergegeven. Voor een snellere effect [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server).



## <a name="reregister-a-process-server"></a>Een processerver registreren

Registreren van een processerver on-premises uitgevoerd of op een Azure-VM met de configuratieserver als volgt te werk:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Nadat u de instellingen hebt opgeslagen, het volgende doen:

1. Open een opdrachtprompt als administrator op de processerver.
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de opdracht uit:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Proxy-instellingen voor een on-premises processerver wijzigen

Als een on-premises processerver gebruikmaakt van een proxy verbinding maken met Azure, kunt u de proxy-instellingen als volgt wijzigen:

1. Meld u bij de server-machine proces. 
2. Open een opdrachtvenster Admin PowerShell en voer de volgende opdracht uit:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer deze opdracht uit:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Een processerver verwijderen

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="exclude-folders-from-anti-virus-software"></a>Mappen uitsluiten van antivirussoftware

Als antivirussoftware wordt uitgevoerd op een uitbreidbare processerver (of de hoofddoelserver), moet u de volgende mappen uitsluiten van antivirusprogramma's bewerkingen:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Map voor de installatie van proces server. Bijvoorbeeld: C:\Program Files (x86) \Microsoft Azure Site Recovery