---
title: Installeren van de Mobility-service voor VMware naar Azure replicatie | Microsoft Docs
description: Dit artikel wordt beschreven hoe u de Mobility-service-agent voor VMware naar Azure replicatie met de Azure Site Recovery-service installeert.
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
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a>Stap 10: Installeer de Mobility-service


Dit artikel wordt beschreven hoe u de bron en doel-instellingen configureren wanneer de lokale virtuele VMware-machines repliceren naar Azure, met behulp van de [Azure Site Recovery](site-recovery-overview.md) service in de Azure portal.

De Mobility-service worden vastgelegd op een machine weggeschreven en stuurt deze door naar de processerver. Het moet worden geïnstalleerd op elke machine die u wilt repliceren naar Azure.

U kunt de Mobility-service handmatig installeren met behulp van een push-installatie van de Site Recovery-processerver wanneer replicatie is ingeschakeld, of gebruik een hulpprogramma System Center Configuration Manager. Als u push-installatie gebruikt, wordt de service is geïnstalleerd op de virtuele machine wanneer replicatie is ingeschakeld.

Opmerkingen en vragen plaatsen onder aan dit artikel of op de [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>De installatie handmatig uitvoeren

1. Controleer de [vereisten](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) voor handmatige installatie.
2. Ga als volgt [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) voor handmatige installatie via de portal.
3. Als u liever installeren vanaf de opdrachtregel, voert u de [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-the-process-server"></a>Installeren vanaf de processerver

Als u de installatie van de Mobility-service van de processerver push wilt wanneer u replicatie voor een virtuele machine inschakelen, moet u een account dat toegang tot de VM kan worden gebruikt door de processerver. Het account wordt alleen gebruikt voor de pushinstallatie.

1. U moet beschikken over [een account gemaakt](vmware-walkthrough-prepare-vmware.md) die kunnen worden gebruikt voor de push-installatie. Vervolgens geeft u het account dat u gebruiken wilt bij het configureren van instellingen van de bronserver tijdens de implementatie van Site Recovery.
2. Volg [deze instructies](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) als u wilt pushen van de Mobility-service op Windows of Linux-machines.

## <a name="other-methods"></a>Andere methoden

Meer informatie over [installeren van de Mobility-service met Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), of met behulp van [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Volgende stappen

Ga naar [stap 11: replicatie inschakelen](vmware-walkthrough-enable-replication.md)
