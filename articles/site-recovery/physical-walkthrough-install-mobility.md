---
title: Installeren van de Mobility-service voor de fysieke server naar Azure replicatie | Microsoft Docs
description: In dit artikel wordt beschreven hoe de Mobility-service-agent installeren op fysieke servers repliceren naar Azure met de Azure Site Recovery-service.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-install-the-mobility-service"></a>Stap 9: Installeer de Mobility-service


In dit artikel beschrijft het installeren van de Mobility-service zijn bij het repliceren van fysieke on-premises Windows of Linux-servers naar Azure met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

De Mobility-service worden vastgelegd op een machine weggeschreven en stuurt deze door naar de processerver. Het moet worden geïnstalleerd op elke server die u wilt repliceren naar Azure.

U kunt de Mobility-service handmatig te installeren of met behulp van een push-installatie van de Site Recovery-processerver wanneer replicatie is ingeschakeld, of gebruik een hulpprogramma zoals System Center Configuration Manager. Als u push-installatie gebruikt, wordt de service is geïnstalleerd op de server wanneer u replicatie inschakelt.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>De installatie handmatig uitvoeren

1. Controleer de [vereisten](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) voor handmatige installatie.
2. Ga als volgt [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) voor handmatige installatie via de portal.
3. Als u liever installeren vanaf de opdrachtregel, voert u de [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Installeren vanaf de processerver

Als u de installatie van de Mobility-service van de processerver push wilt wanneer u replicatie voor een machine inschakelt, moet u een account dat toegang tot het systeem kan worden gebruikt door de processerver. Het account wordt alleen gebruikt voor de pushinstallatie.

1. Als u een account dat nog niet hebt gemaakt, doen met behulp van deze richtlijnen:

    - U kunt een domein of lokale account gebruiken
    - Als u niet een domeinaccount voor Windows moet u externe gebruiker toegangsbeheer op de lokale computer uitschakelen. Om dit te doen, in het register onder **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, de DWORD-vermelding toevoegen **LocalAccountTokenFilterPolicy**, met een waarde van 1.
    - Als u wilt de registervermelding voor Windows uit een CLI toevoegen, typt u:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Voor Linux moet het account hoofdmap op de bronserver Linux zijn.

2. Volg [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) als u wilt pushen van de Mobility-service op Windows of Linux-machines.

## <a name="other-installation-methods"></a>Andere installatiemethoden

- [Meer informatie over](site-recovery-install-mobility-service-using-sccm.md) installeren van de Mobility-service met Configuration Manager
- [Meer informatie over](site-recovery-automate-mobility-service-install.md) installeren met Azure Automation DSC.


## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 10: replicatie inschakelen](physical-walkthrough-enable-replication.md)
