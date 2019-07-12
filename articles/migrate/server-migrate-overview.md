---
title: Selecteer een optie voor het migreren van VMware met Azure Migrate-servermigratie | Microsoft Docs
description: Biedt een overzicht van opties voor VMware-VM's migreren naar Azure met Azure Migrate-servermigratie
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f8bfbe26dc4c6ddbcf622f91938ba060de00b2ec
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811568"
---
# <a name="select-a-vmware-migration-option"></a>Selecteer een optie voor het migreren van VMware

U kunt virtuele VMware-machines migreren naar Azure met behulp van het hulpprogramma voor migratie van Azure-Server migreren. Dit hulpprogramma biedt een aantal opties voor VMware-VM-migratie:

- Migratie met behulp van zonder agents replicatie. Virtuele machines migreren zonder dat u hoeft te installeren op deze.
- Migratie met een agent voor replicatie. Een agent installeren op de virtuele machine voor replicatie.


Hoewel de zonder agent replicatie vanuit het oogpunt van de implementatie eenvoudiger is heeft momenteel een aantal beperkingen.

## <a name="agentless-migration-limitations"></a>Zonder agent migratiebeperkingen

Beperkingen zijn als volgt:

- **Gelijktijdige replicatie**: Een maximum van 50 virtuele machines kan tegelijkertijd worden gerepliceerd van een vCenter-Server.<br/> Als u meer dan 50 virtuele machines voor de migratie hebt, maakt u meerdere batches van VM's.<br/> Meer gerepliceerd in één keer is van invloed op prestaties.
- **VM-schijven**: Een virtuele machine die u wilt migreren, moet 60 of minder schijven hebben.
- **VM-besturingssystemen**: In het algemeen een Windows Server of Linux-besturingssysteem Azure Migrate kunt migreren, maar deze wijzigingen op virtuele machines mogelijk, zodat ze in Azure uitvoeren kunnen. Met Azure Migrate kunt de wijzigingen automatisch voor deze besturingssystemen:
    - Red Hat Enterprise Linux 6.5+, 7.0+
    - CentOS 6.5+, 7.0+
    - SUSE Linux Enterprise Server 12 SP1+
    - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS
    - Debian 7,8
    - Voor andere besturingssystemen moet u aanpassingen handmatig vóór de migratie. De [zelfstudie gemigreerd](tutorial-migrate-vmware.md) wordt uitgelegd hoe u dit doet.
- **Linux-boot**: Als er zich bevinden op een specifieke partitie, moet zich bevinden op de besturingssysteemschijf en niet worden verdeeld over meerdere schijven.<br/> Als bevinden maakt deel uit van de hoofdpartitie (/), moet klikt u vervolgens de partitie '/' zich op de schijf met het besturingssysteem en andere schijven zijn niet verdeeld.
- **Opstarten via UEFI**: Virtuele machines met opstarten via UEFI worden niet ondersteund voor migratie.
- **Versleutelde schijven/volumes (BitLocker, cryptfs)** : Virtuele machines met versleutelde schijven/volumes worden niet ondersteund voor migratie.
- **RDM/passthrough-schijven**: Als virtuele machines RDM of passthrough-schijven hebben, wordt niet deze schijven gerepliceerd naar Azure
- **NFS**: NFS-volumes die zijn gekoppeld als volumes op de virtuele machines wordt niet worden gerepliceerd.
- **Doelopslag**: U kunt alleen virtuele VMware-machines migreren naar virtuele Azure-machines met beheerde schijven (HDD Standard, Premium SSD).



## <a name="deployment-steps-comparison"></a>Vergelijking van de implementatie stappen

Bekijk de beperkingen en informatie over de stappen voor het implementeren van elke oplossing mogelijk kunt u bepalen welke optie te kiezen.

**Taak** | **Details** |**Zonder agent** | **Agent-based**
--- | --- | --- | ---
**VMware-servers en virtuele machines voorbereiden voor migratie** | Een aantal instellingen configureren voor VMware-servers en VM's. | Verplicht | Verplicht
**Het hulpprogramma voor migratie van de Server toevoegen** | Het hulpprogramma voor migratie van Azure-Server migreren in de Azure Migrate-project toevoegen. | Verplicht | Verplicht
**Implementeren van het apparaat Azure Migrate** | Instellen van een lichtgewicht apparaat op een VMware-VM voor VM-detectie en evaluatie. | Verplicht | Niet vereist.
**De Mobility-service installeren op virtuele machines** | De Mobility-service installeren op elke virtuele machine die u wilt repliceren | Niet vereist | Verplicht
**Het apparaat Azure Migrate-servermigratie-replicatie implementeren** | Instellen van een apparaat op een VMware-VM voor het detecteren van virtuele machines en brug tussen de Mobility-service die wordt uitgevoerd op virtuele machines en Azure Migrate-servermigratie | Niet vereist | Verplicht
**Virtuele machines repliceren**. Schakel replicatie van virtuele machines. | Replicatie-instellingen configureren en selecteer virtuele machines repliceren | Verplicht | Verplicht
**Een testfailover uitvoeren** | Een testmigratie om te controleren of dat alles werkt zoals verwacht worden uitgevoerd. | Verplicht | Verplicht
**Een volledige migratie uitvoeren** | Migreer de virtuele machines. | Verplicht | Verplicht




## <a name="next-steps"></a>Volgende stappen

[Virtuele VMware-machines migreren](tutorial-migrate-vmware.md) met migratie zonder agent.



