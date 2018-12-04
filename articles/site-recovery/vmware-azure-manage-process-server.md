---
title: Een processerver voor herstel na noodgevallen van virtuele VMware-machines en fysieke servers naar Azure met behulp van Azure Site Recovery beheren | Microsoft Docs
description: Dit artikel wordt beschreven voor het beheren van een processerver instellen voor herstel na noodgevallen van virtuele VMware-machines en fysieke server naar Azure met Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 05e77821ffe913a8c6669d58a3145635d230ff2b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846994"
---
# <a name="manage-process-servers"></a>Processervers beheren

De processerver die wordt gebruikt wanneer u virtuele VMware-machines of fysieke servers naar Azure repliceert wordt standaard geïnstalleerd op de servercomputer van de on-premises configuratie. Er zijn een aantal instanties waarin u moet voor het instellen van een afzonderlijk proces-server:

- Voor grote implementaties moet u mogelijk extra on-premises processervers dat moet capaciteit kunt schalen.
- Voor failback, moet u een tijdelijke processerver in Azure instellen. Wanneer failback is voltooid, kunt u deze virtuele machine verwijderen. 

In dit artikel bevat een overzicht van veelvoorkomende beheertaken voor deze extra processervers.

## <a name="upgrade-a-process-server"></a>Upgrade van een processerver

Een processerver die wordt uitgevoerd on-premises of in Azure (voor failback testdoeleinden), als volgt bijwerken:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Wanneer u de installatiekopie van de Azure-galerie gebruiken om u te maken van een processerver in Azure voor de doeleinden van failback, wordt dit meestal de meest recente beschikbare versie uitgevoerd. De Site Recovery teams release oplossingen en verbeteringen op gezette tijden, en we raden dat u processervers up-to-date houden.

## <a name="balance-the-load-on-process-server"></a>Verdeel de belasting op de processerver

Om taken te verdelen tussen twee processervers mogelijk

1. Navigeer naar **Recovery Services-kluis** > **beheren** > **infrastructuur voor Site Recovery** > **voor VMware en fysieke machines** > **configuratieservers**.
2. Klik op de configuratieserver waaraan de processervers zijn geregistreerd bij.
3. Lijst met processervers die zijn geregistreerd bij de van configuratieservers zijn beschikbaar op de pagina.
4. Klik op de processerver op die u wilt wijzigen van de werkbelasting.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

5. U kunt beide gebruiken **taakverdeling** of **Switch** opties, zoals wordt uitgelegd, volgens de vereisten.

### <a name="load-balance"></a>Verdeel de belasting

Via deze optie is, u kunt een of meer virtuele machines selecteren en kunt overbrengen naar een andere processerver.

1. Klik op **verdelen**, selecteer de doelprocesserver in de vervolgkeuzelijst omlaag. Klik op **OK**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Klik op **machines selecteren**, kiest u de virtuele machines die u wilt verplaatsen van de huidige processerver naar de doelprocesserver. Details van de gemiddelde gegevenswijziging worden weergegeven op basis van elke virtuele machine.
3. Klik op **OK**. De voortgang van de taak onder **Recovery Services-kluis** > **bewaking** > **Site Recovery-taken**.
4. Het duurt 15 minuten om de wijzigingen in overeenstemming met na voltooiing van deze bewerking of [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) voor onmiddellijk van kracht wordt.

### <a name="switch"></a>Switch

Hele werkbelasting die wordt beschermd onder een processerver wordt tot en met deze optie wordt verplaatst naar een andere processerver.

1. Klik op **Switch**, de doelprocesserver selecteren, klikt u op **OK**.

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

2. De voortgang van de taak onder **Recovery Services-kluis** > **bewaking** > **Site Recovery-taken**.
3. Het duurt 15 minuten om de wijzigingen in overeenstemming met na voltooiing van deze bewerking of [vernieuwen van de configuratieserver](vmware-azure-manage-configuration-server.md#refresh-configuration-server) voor onmiddellijk van kracht wordt.

## <a name="reregister-a-process-server"></a>Een processerver registreren

Als u wilt registreren van een processerver on-premises uitgevoerd of in Azure, met de configuratieserver, doet het volgende:

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

Als de processerver een proxy verbinding maken met Site Recovery in Azure, gebruikt u deze procedure als u nodig hebt om bestaande proxyinstellingen te wijzigen.

1. Meld u aan bij de server-machine proces. 
2. Open een opdrachtvenster Admin PowerShell en voer de volgende opdracht uit:
  ```powershell
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de volgende opdracht uit:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Een processerver verwijderen

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Antivirussoftware op de processervers beheren

Als de antivirussoftware op een zelfstandige processerver of de hoofddoelserver actief is, moet u de volgende mappen uitsluiten van antivirusprogramma's operations:


- C:\Program Files\Microsoft Azure Recovery Services-Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- De installatiemap proces, voorbeeld: C:\Program Files (x86) \Microsoft Azure Site Recovery

