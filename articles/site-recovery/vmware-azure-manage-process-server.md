---
title: Een processerver beheren in Azure Site Recovery | Microsoft Docs
description: Dit artikel wordt beschreven beheren van een processerver instellen voor de VM VMware en fysieke server-replicatie in Azure Site Recovery.
services: site-recovery
author: AnoopVasudavan
manager: gauravd
editor: 
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 096b2890d41402448809ae87759fcd6b67bee2fe
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="manage-process-servers"></a>Processervers beheren

De processerver die wordt gebruikt wanneer u VMware-machines of fysieke servers naar Azure repliceert is standaard geïnstalleerd op de servercomputer van de lokale configuratie. Er zijn een aantal exemplaren waarin u moet een afzonderlijk processerver instellen:

- Voor grote implementaties moet u mogelijk aanvullende on-premises processervers voor het schalen van capaciteit.
- Failback, moet u een tijdelijke processerver in Azure hebt ingesteld. Wanneer de failback wordt uitgevoerd, kunt u deze virtuele machine verwijderen. 

In dit artikel bevat een overzicht van veelvoorkomende beheertaken voor deze aanvullende processen-servers.

## <a name="upgrade-a-process-server"></a>Een processerver bijwerken

De processerver van een met on-premises of in Azure (voor failback doeleinden), als volgt bijwerken:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
  Normaal gesproken wanneer u de installatiekopie van het Azure-galerie maken van een processerver in Azure voor de doeleinden van failback gebruikt, wordt de laatst beschikbare versie. Herstel van de Site teams release oplossingen en verbeteringen op regelmatige basis en we adviseren u processervers up-to-date.



## <a name="reregister-a-process-server"></a>Een processerver opnieuw registreren

Als u wilt registreren met lokale processerver, of in Azure, met de configuratieserver Doe het volgende:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Nadat u de instellingen hebt opgeslagen, het volgende doen:

1. Open een opdrachtprompt als administrator op de processerver.
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de opdracht:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Proxy-instellingen voor een on-premises processerver wijzigen

Als de processerver een proxy gebruikt verbinding maken met Site Recovery in Azure, gebruikt u deze procedure als u nodig hebt om bestaande proxyinstellingen te wijzigen.

1. Meld u bij de proces-servercomputer. 
2. Open een opdrachtvenster Admin PowerShell en voer de volgende opdracht:
  ```
  $pwd = ConvertTo-SecureString -String MyProxyUserPassword
  Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
  net stop obengine
  net start obengine
  ```
2. Blader naar de map **%PROGRAMDATA%\ASR\Agent**, en voer de volgende opdracht:
  ```
  cmd
  cdpcli.exe --registermt

  net stop obengine

  net start obengine

  exit
  ```


## <a name="remove-a-process-server"></a>Een processerver verwijderen

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]


