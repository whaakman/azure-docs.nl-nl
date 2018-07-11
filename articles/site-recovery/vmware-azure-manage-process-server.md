---
title: Een processerver in Azure Site Recovery beheren | Microsoft Docs
description: Dit artikel wordt beschreven voor het beheren van een processerver instellen voor VMware-VM en fysieke servers repliceren in Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: ramamill
ms.openlocfilehash: df162177c1ff56c1d6fc34d2ce49925a2705a868
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37950784"
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
  ```
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


