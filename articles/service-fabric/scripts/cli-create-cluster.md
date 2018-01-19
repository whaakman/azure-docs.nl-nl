---
title: Azure Service Fabric CLI Script voorbeeld implementeren
description: Een beveiligde Linux voor Service Fabric-cluster maken in Azure met behulp van de Azure Service Fabric-CLI.
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
ms.openlocfilehash: d49383b4f7b3b13beb9ea36ae725938e17ef1456
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Maken van een beveiligde Linux voor Service Fabric-cluster in Azure

Deze opdracht maakt u een zelfondertekend certificaat, voegt het toe aan een sleutelkluis en downloadt het certificaat lokaal.  Het nieuwe certificaat wordt gebruikt voor het beveiligen van het cluster wanneer het wordt geïmplementeerd.  U kunt ook een bestaand certificaat gebruiken in plaats van een nieuwe maken.  In beide gevallen moet de onderwerpnaam van het certificaat moet overeenkomen met het domein dat u gebruikt voor toegang tot de Service Fabric-cluster. Deze overeenkomst is vereist voor een met SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat kan niet verkrijgen van een Certificeringsinstantie voor de `.cloudapp.azure.com` domein. U moet een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt, moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam die u voor uw cluster gebruikt.

Installeer zo nodig de [Azure CLI 2.0](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Voorbeeld van een script

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Na het uitvoeren van het voorbeeldscript kan de volgende opdracht om te verwijderen van de resourcegroep, cluster en alle gerelateerde resources worden gebruikt.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Script uitleg

Dit script maakt gebruik van de volgende opdrachten. Elke opdracht in de tabel is gekoppeld aan de specifieke documentatie opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [AZ sf cluster maken](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest#az_sf_cluster_create) | Maakt een nieuwe Service Fabric-cluster.  |

## <a name="next-steps"></a>Volgende stappen

Extra Service Fabric CLI steekproeven voor de Azure Service Fabric vindt u in de [Service Fabric CLI voorbeelden](../samples-cli.md).
