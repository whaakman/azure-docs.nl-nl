---
title: Voorbeeld van Azure PowerShell-script - abonneren op Blob Storage-account | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - abonneren op Blob Storage-account
services: event-grid
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: bc8726499f900602bf1dbfc58a7ea2574a483245
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273898"
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-powershell"></a>Abonneren op gebeurtenissen voor een Blob Storage-account met PowerShell

Met dit script maakt u een Event Grid-abonnement op de gebeurtenissen voor een Blob Storage-account.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script---stable"></a>Voorbeeldscript - stabiel

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.ps1 "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script wordt de volgende opdracht gebruikt om het abonnement op de gebeurtenis te maken. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Hiermee wordt een Event Grid-abonnement gemaakt. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van door Azure beheerde toepassingen](../overview.md) voor algemene informatie over beheerde toepassingen.
* Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie over PowerShell.
