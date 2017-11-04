---
title: Azure PowerShell-script voorbeeld - een beheerde toepassing implementeren | Microsoft Docs
description: Azure PowerShell-script steekproef - definitie van een beheerde toepassing implementeren
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: 2429d561beffed5bc171b9dbc2c2c9c88eba3313
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2017
---
# <a name="deploy-a-managed-application-for-a-service-catalog-with-powershell"></a>Implementeren van een beheerde toepassing voor een Servicecatalogus met PowerShell

Dit script implementeert een beheerde toepassingsdefinitie van de Servicecatalogus.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/managed-applications/create-application/create-application.ps1 "Create application")]


## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdracht om de beheerde toepassing te implementeren. Elke opdracht in de tabel is gekoppeld aan de opdracht specifieke documentatie bij.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmManagedApplication](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermmanagedapplication) | Maak een beheerde toepassing. Geef de roldefinitie-ID en de parameters voor de sjabloon. |


## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](../overview.md).
* Zie voor meer informatie over PowerShell [documentatie van Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
