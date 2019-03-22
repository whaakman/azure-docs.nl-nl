---
title: Ondersteuning voor het gebruik van Azure Site Recovery met Azure Backup | Microsoft Docs
description: Biedt een overzicht van hoe Azure Site Recovery en Azure Backup kunnen samen worden gebruikt.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: e902f70225ec0eb0caa98f7e19a16c87220cb6f9
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312883"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Ondersteuning voor het gebruik van Site Recovery met Azure Backup

In dit artikel bevat een overzicht van ondersteuning voor het gebruik van de [Site Recovery-service](site-recovery-overview.md) samen met de [Azure Backup-service](https://docs.microsoft.com/azure/backup/backup-overview).

**Actie** | **Ondersteuning voor site Recovery** | **Details**
--- | --- | ---
**Implementeer services tegelijk** | Ondersteund | Services compatibel zijn en kunnen samen worden geconfigureerd.
**Bestand back-up/herstellen** | Ondersteund | Wanneer back-up en replicatie zijn ingeschakeld voor een virtuele machine en back-ups worden gemaakt, is er geen probleem bij het terugzetten van bestanden aan de bron-VM's, of groep virtuele machines. Replicatie wordt voortgezet zoals gebruikelijk zonder veranderingen in de replicatiestatus.
**Schijf-back-up/herstellen** | Er is geen huidige ondersteuning | Als u een back-ups van schijf herstellen, moet u uitschakelen en weer inschakelen replicatie voor de virtuele machine opnieuw.
**VM-back-up/herstellen** | Er is geen huidige ondersteuning | Als u een back-up of herstellen van een virtuele machine of een groep virtuele machines, die u wilt uitschakelen en weer inschakelen replicatie voor de virtuele machine.  


