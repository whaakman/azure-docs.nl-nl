---
title: Azure PowerShell-voorbeeldscript - Een Service Fabric-cluster maken | Microsoft Docs
description: 'Azure PowerShell-voorbeeldscript: een Service Fabric-testcluster met drie knooppunten maken.'
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/29/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: fd94a5dd9630cc65dedc180cdfd7aafea83c4866
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Een Service Fabric-testcluster met drie knooppunten maken

Met dit voorbeeldscript wordt een Service Fabric-testcluster met drie knooppunten gemaakt dat wordt beveiligd met een X.509-certificaat. De clusterconfiguratie met drie knooppunten wordt ondersteund voor ontwikkelen en testen omdat u veilig upgrades kunt uitvoeren en uitvallen van knooppunten kunt overleven (zolang ze niet tegelijk uitvallen). Productieclusters moeten vijf of meer knooppunten hebben om bestand te zijn tegen gelijktijdig uitvallen van knooppunten.  

Met de opdracht maakt u een zelfondertekend certificaat dat wordt geüpload naar een nieuwe sleutelkluis, die wordt gemaakt in dezelfde resourcegroep als het cluster. Het certificaat wordt ook gekopieerd naar een lokale map.  Stel de *-OS* parameter in om de versie van Windows of Linux die op de clusterknooppunten wordt uitgevoerd te kiezen.  Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Login-AzureRmAccount` uit om verbinding maken met Azure. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Nadat het voorbeeldscript is uitgevoerd, kan de volgende opdracht worden gebruikt om de resourcegroep, het cluster en alle gerelateerde resources te verwijderen.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [New-AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Hiermee wordt een nieuw Service Fabric-cluster gemaakt. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
