---
title: Azure PowerShell-Script steekproef - maken van een Service Fabric-cluster | Microsoft Docs
description: 'Azure PowerShell-Script voorbeeld: een drie-knooppunt test Service Fabric-cluster maken.'
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
ms.date: 12/12/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 03348efa4ebdaed987df73756c6b57da0cc76fb5
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="create-a-three-node-test-service-fabric-cluster"></a>Een drie-knooppunt test Service Fabric-cluster maken

Dit voorbeeldscript wordt een drie-knooppunt test Service Fabric-cluster beveiligd met een X.509-certificaat gemaakt. De configuratie van het cluster drie knooppunt wordt ondersteund voor het ontwikkelen en testen omdat kunt u veilig uitvoeren van upgrades en afzonderlijke knooppuntfouten overleven (zolang ze niet tegelijkertijd uitgevoerd). Productiecluster vijf of meer knooppunten zijn vereist om gelijktijdige fouten netwerkfouten.  

De opdracht maakt u een zelfondertekend certificaat en geüpload naar een nieuwe sleutelkluis, die wordt gemaakt in dezelfde resourcegroep bevinden als het cluster. Het certificaat wordt ook gekopieerd naar een lokale map.  Stel de *-OS* parameter in om de versie van Windows of Linux die op de clusterknooppunten wordt uitgevoerd te kiezen.  Pas de parameters zo nodig aan.

Installeer zo nodig de Azure PowerShell met de instructie gevonden in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Login-AzureRmAccount` geen verbinding maken met Azure. 

## <a name="sample-script"></a>Voorbeeld van een script

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-test-cluster/create-test-cluster.ps1 "Create a test Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie 

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht om te verwijderen van de resourcegroep, cluster en alle gerelateerde resources worden gebruikt.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Nieuwe AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Maakt een nieuwe Service Fabric-cluster. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module [documentatie van Azure PowerShell](/powershell/azure/overview).

Aanvullende voorbeelden van de Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
