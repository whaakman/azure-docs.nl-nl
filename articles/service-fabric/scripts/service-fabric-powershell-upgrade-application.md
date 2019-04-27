---
title: Voorbeeld van Azure PowerShell-Script - Upgrade van een Service Fabric-toepassing | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - Upgrade van een Service Fabric-toepassing.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 2f896e0ff8d05d9c77a4a11c97afd9c0ffc17b7d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60621754"
---
# <a name="upgrade-a-service-fabric-application"></a>Een Service Fabric-toepassing upgraden

Met dit voorbeeldscript wordt een actief exemplaar van de Service Fabric-toepassing bijgewerkt naar versie 1.3.0. Het script het nieuwe toepassingspakket worden gekopieerd naar de installatiekopieopslag van het cluster, het toepassingstype registreert en verwijdert u het pakket onnodige toepassingen.  Het script wordt een upgrade van een bewaakte gestart en continu controleert de status van de upgrade totdat de upgrade is voltooid of teruggedraaid. Pas de parameters zo nodig aan. 

Installeer indien nodig de Service Fabric PowerShell-module met de [Service Fabric-SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Hiermee haalt u alle toepassingen in de Service Fabric-cluster of een specifieke toepassing.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Hiermee haalt u de status van de upgrade van een Service Fabric-toepassing. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Hiermee haalt u de Service Fabric-toepassingstypen geregistreerd op de Service Fabric-cluster. |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | De registratie van een type Service Fabric-toepassing.  |
| [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Een Service Fabric-toepassingspakket kopieert naar de installatiekopieopslag.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Registreert een type Service Fabric-toepassing. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Upgrade van een Service Fabric-toepassing naar de versie van de opgegeven type. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hiermee verwijdert u een Service Fabric-toepassingspakket uit de installatiekopieopslag.|


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Service Fabric PowerShell-module [documentatie over Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Meer PowerShell-voorbeelden voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
