---
title: Voorbeeld van Azure PowerShell-script - abonneren op resourcegroep | Microsoft Docs
description: Voorbeeld van Azure PowerShell-script - abonneren op resourcegroep
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
ms.openlocfilehash: db4be11286825a8a990918a80c687f86c2294c6d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58186728"
---
# <a name="subscribe-to-events-for-a-resource-group-with-powershell"></a>Abonneren op gebeurtenissen voor een resourcegroep met PowerShell

Met dit script maakt u een Event Grid-abonnement op de gebeurtenissen voor een resourcegroep.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Voor het voorbeeldscript van de preview is de Event Grid-module vereist. Voer `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery` uit om deze te installeren

## <a name="sample-script---stable"></a>Voorbeeldscript - stabiel

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group/subscribe-to-resource-group.ps1 "Subscribe to resource group")]

## <a name="sample-script---preview-module"></a>Voorbeeldscript - preview-module

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

[!code-powershell[main](../../../powershell_scripts/event-grid/subscribe-to-resource-group-preview/subscribe-to-resource-group-preview.ps1 "Subscribe to resource group")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script wordt de volgende opdracht gebruikt om het abonnement op de gebeurtenis te maken. Elke opdracht in de tabel is een koppeling naar opdracht-specifieke documentatie.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzEventGridSubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) | Hiermee wordt een Event Grid-abonnement gemaakt. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van door Azure beheerde toepassingen](../overview.md) voor algemene informatie over beheerde toepassingen.
* Zie [Documentatie over Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie over PowerShell.
