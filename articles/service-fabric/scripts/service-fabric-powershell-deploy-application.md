---
title: Azure PowerShell-Script voorbeeld - App in een cluster implementeren | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - een toepassing implementeren in een Service Fabric-cluster.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 6de7a721a5042a20537d3f09ea9b8de1cc0e7169
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622030"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Een toepassing implementeren naar een Service Fabric-cluster

Met dit voorbeeldscript een toepassingspakket worden gekopieerd naar de installatiekopieopslag van een cluster, het toepassingstype registreert in het cluster, verwijdert u het pakket onnodige toepassingen en exemplaar van een toepassing maakt van het toepassingstype.  Als alle services die standaard zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing, worden deze services gemaakt op dit moment. Pas de parameters zo nodig aan. 

Installeer indien nodig de Service Fabric PowerShell-module met de [Service Fabric-SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Nadat het voorbeeldscript is uitgevoerd, het script in [verwijderen van een toepassing](service-fabric-powershell-remove-application.md) kan worden gebruikt om het toepassingsexemplaar te verwijderen en het toepassingspakket verwijderen uit de installatiekopieopslag registratie van het toepassingstype.

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Hiermee maakt u een verbinding met een Service Fabric-cluster. |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopieën opgeslagen voor een toepassingspakket op de installatiekopie van het cluster.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Registreert een toepassingstype en de versie op het cluster. |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Hiermee maakt u een toepassing van een geregistreerde toepassing. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hiermee verwijdert u een Service Fabric-toepassingspakket uit de installatiekopieopslag.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Service Fabric PowerShell-module [documentatie over Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Meer PowerShell-voorbeelden voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
