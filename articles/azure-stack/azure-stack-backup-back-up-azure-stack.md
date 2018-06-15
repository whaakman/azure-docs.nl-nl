---
title: Maak een back-up van Azure-Stack | Microsoft Docs
description: Een op aanvraag back-up uitvoeren op Azure-Stack met back-up in plaats.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/08/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: c2a6727692a7a74b3e5fe32de8800722a9ed91b5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075184"
---
# <a name="back-up-azure-stack"></a>Back-up van Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een op aanvraag back-up uitvoeren op Azure-Stack met back-up in plaats. Als u moet de infrastructuur back-up-Service wilt inschakelen, Zie [back-up inschakelen voor Azure-Stack van de beheerportal](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Zie voor instructies over het configureren van de PowerShell-omgeving [Installeer PowerShell voor Azure-Stack ](azure-stack-powershell-install.md).

## <a name="start-azure-stack-backup"></a>Azure-Stack back-up starten

Open Windows PowerShell met een opdrachtprompt in de operator-omgeving en voer de volgende opdrachten:

```powershell
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Back-up is voltooid in de beheerportal bevestigen

1. Open de Stack van Azure-beheerportal op [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.
3. Zoek de **naam** en **datum voltooid** van de back-up in **beschikbare back-ups** lijst.
4. Controleer of de **status** is **geslaagd**.

<!-- You can also confirm the backup completed from the administration portal. Navigate to `\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

In ‘Confirm backup completed’ section, the path at the end doesn’t make sense (ie relative to what, datetime format, etc?)
\MASBackup\<datetime>\<backupid>\BackupInfo.xml -->


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de werkstroom voor het herstellen van het gegevensverlies. Zie [herstellen vanaf onherstelbaar gegevensverlies](azure-stack-backup-recover-data.md).
