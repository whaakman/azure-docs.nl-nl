---
title: Azure Site Recovery - back-interoperabiliteit | Microsoft Docs
description: Biedt een overzicht van hoe Azure Site Recovery en Azure Backup kunnen samen worden gebruikt.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/26/2019
ms.author: sideeksh
ms.openlocfilehash: 6658ab8c967c70ac1deaeba3d1dfeac602515591
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57731864"
---
# <a name="about-site-recovery-and-backup-interoperability"></a>Over Site Recovery en back-interoperabiliteit

In dit artikel bevat richtlijnen voor het met behulp van Azure IaaS VM Backup en Azure-VM-noodherstel.

## <a name="azure-backup"></a>Azure Backup

Azure Backup beschermt gegevens voor on-premises servers, virtuele machines, gevirtualiseerde workloads, SQL-servers, SharePoint-servers en meer. Azure Site Recovery coördineert en beheert herstel na noodgevallen voor Azure-VM's, on-premises machines en fysieke servers.

## <a name="azure-site-recovery"></a>Azure Site Recovery

Het is mogelijk het configureren van zowel Azure Backup en Azure Site Recovery op een virtuele machine of een groep virtuele machines. Beide producten werken samen. Een aantal scenario's waarbij de interoperabiliteit tussen de back-up- en Azure Site Recovery belangrijk is zijn als volgt:

### <a name="file-backuprestore"></a>Bestand back-up/herstellen

Als back-up en replicatie zijn beide ingeschakeld en een back-up is gemaakt, is er geen probleem is met het herstellen van een of meer bestanden op de bron-side-virtuele machine of de groep van virtuele machines. Replicatie wordt geen wijziging in de replicatiestatus van gebruikelijke manier voortgezet.

### <a name="disk-backuprestore"></a>Schijf-back-up/herstellen

Als u een schijf vanaf de back-up herstellen heeft de beveiliging van de virtuele machine opnieuw wordt ingeschakeld.

### <a name="vm-backuprestore"></a>VM-back-up/herstellen

Back-up en herstel van een virtuele machine of een groep virtuele machines wordt niet ondersteund. Beveiliging moet opnieuw worden ingeschakeld zodat deze werkt.

**Scenario** | **Ondersteund door Azure Site Recovery?** | **Tijdelijke oplossing, indien van toepassing**  
--- | --- | ---
Back-up van bestanden/mappen | Ja | Niet van toepassing
Back-up van schijf | Op dit moment niet | Uitschakelen en inschakelen van de beveiliging
VM-back-up | Nee | Uitschakelen en inschakelen van de beveiliging
