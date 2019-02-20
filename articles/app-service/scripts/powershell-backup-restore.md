---
title: 'Azure PowerShell-voorbeeldscript: Een web-app terugzetten vanuit een back-up | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: Een web-app terugzetten vanuit een back-up'
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: a2a27d94-d378-4c17-a6a9-ae1e69dc4a72
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 11/21/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: aca7f700dc7004f682430d0794f8bc9b024f41b3
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113495"
---
# <a name="restore-a-web-app-from-a-backup-using-azure-powershell"></a>Een web-app terugzetten vanuit een back-up met Azure PowerShell

Met dit voorbeeldscript haalt u een eerder voltooide back-up van een bestaande web-app op en zet u deze terug door de inhoud ervan te overschrijven. 

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Connect-AzAccount` uit om verbinding te maken met Azure. 

## <a name="sample-script"></a>Voorbeeldscript

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-azurepowershell-interactive[main](../../../powershell_scripts/app-service/backup-restore/backup-restore.ps1?highlight=1-2 "Restore a web app from a backup")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Als u de web-app niet meer nodig hebt, gebruikt u de volgende opdracht om de resourcegroep, web-app en alle gerelateerde resources te verwijderen.

```powershell
Remove-AzResourceGroup -Name $resourceGroupName -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-AzWebAppBackupList](/powershell/module/az.websites/get-azwebappbackuplist) | Hiermee vraagt u een lijst met back-ups op voor een web-app. |
| [Restore-AzWebAppBackup](/powershell/module/az.websites/restore-azwebappbackup) | Hiermee zet u een web-app terug vanuit een eerder voltooide back-up. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure App Service Web Apps vindt u in de [voorbeelden van Azure PowerShell](../samples-powershell.md).
