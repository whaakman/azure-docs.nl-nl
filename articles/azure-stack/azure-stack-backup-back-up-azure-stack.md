---
title: Maak een back-up van Azure-Stack | Microsoft Docs
description: Een back-up van op aanvraag uitvoeren op Azure-Stack met back-up in plaats.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: df1f4c6fadd08b17a1a1eb8bbe41ab71ae4729fc
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="back-up-azure-stack"></a>Back-up van Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Een back-up van op aanvraag uitvoeren op Azure-Stack met back-up in plaats. Als u moet de infrastructuur back-up-Service wilt inschakelen, Zie [back-up inschakelen voor Azure-Stack van de beheerportal](azure-stack-backup-enable-backup-console.md).

## <a name="start-azure-stack-backup"></a>Azure Stack back-up starten

Open Windows PowerShell met een opdrachtprompt en voer de volgende opdrachten:

   ```powershell
   Start-AzSBackup -Location $location
   ```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Back-up is voltooid in de beheerportal bevestigen

1. Open de Stack van Azure-beheerportal op [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Selecteer **meer services** > **infrastructuur back-up**. Kies **configuratie** in de **infrastructuur back-up** blade.
3. Zoek de **naam** en **datum voltooid** van de back-up in **beschikbare back-ups** lijst.
4. Controleer of de **status** is **geslaagd**.

U kunt ook de back-up ingevuld op basis van de beheerportal bevestigen. Navigeer naar`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de werkstroom voor het herstellen van het gegevensverlies. Zie [herstellen vanaf onherstelbaar gegevensverlies](azure-stack-backup-recover-data.md).