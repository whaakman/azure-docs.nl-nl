---
title: Azure PowerShell-Script steekproef - toepassing naar een cluster implementeren | Microsoft Docs
description: Azure PowerShell-Script voorbeeld - een toepassing implementeert op een Service Fabric-cluster.
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c81514fb4b1c1da483ebd55deae149caf22d4b63
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Een toepassing naar een Service Fabric-cluster implementeren

Dit voorbeeldscript toepassingspakketten kopieert naar een cluster image store, registreert het toepassingstype in het cluster, verwijdert u het onnodige toepassingspakket en maakt een toepassingsexemplaar van het toepassingstype.  Als alle services die standaard zijn gedefinieerd in het toepassingsmanifest van het type doeltoepassing, worden op dit moment die services gemaakt. Pas de parameters zo nodig aan. 

Installeer zo nodig de Service Fabric PowerShell-module met de [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Nadat het voorbeeldscript uitvoeren van het script in [een toepassing verwijderen](service-fabric-powershell-remove-application.md) kan worden gebruikt voor het verwijderen van exemplaar van de toepassing, hef de registratie van het toepassingstype en het toepassingspakket wissen van de image store.

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Maakt een verbinding met een Service Fabric-cluster. |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopieën opslaan voor een toepassingspakket op de installatiekopie van het cluster.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Registreert een toepassingstype en -versie op het cluster. |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Maakt een toepassing van een geregistreerde toepassingstype. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hiermee verwijdert u een Service Fabric-toepassingspakket uit de image store.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Service Fabric PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/service-fabric/?view=azureservicefabricps).

Aanvullende voorbeelden van Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
