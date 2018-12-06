---
title: Een Azure Load Balancer-regel voor een cluster maken
description: Een Azure Load Balancer voor het openen van poorten voor uw Azure Service Fabric-cluster configureren.
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: ryanwi
ms.openlocfilehash: e41e5d48141b120ec1f80b0faa899bda07a2934e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52962060"
---
# <a name="open-ports-for-a-service-fabric-cluster"></a>Poorten openen voor een Service Fabric-cluster

De load balancer geïmplementeerd met uw Azure Service Fabric-cluster zorgt ervoor dat verkeer naar uw app wordt uitgevoerd op een knooppunt. Als u uw app voor het gebruik van een andere poort wijzigt, moet u die poort (of een andere poort te routeren) in de Azure Load Balancer.

Wanneer u uw Service Fabric-cluster geïmplementeerd in Azure, wordt een load balancer is automatisch voor u gemaakt. Als u een load balancer niet hebt, raadpleegt u [een internetgerichte load balancer configureren](../load-balancer/load-balancer-get-started-internet-portal.md).

## <a name="configure-service-fabric"></a>Service fabric configureren

Uw Service Fabric-toepassing **ServiceManifest.xml** config-bestand definieert de eindpunten die uw toepassing verwacht te gebruiken. Het configuratiebestand is bijgewerkt voor het definiëren van een eindpunt, de load balancer moet worden bijgewerkt als u wilt weergeven die (of een andere) poort. Zie voor meer informatie over het maken van het service fabric-eindpunt [instellen van een eindpunt](service-fabric-service-manifest-resources.md).

## <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een Load Balancer-regel wordt de poort van een internetgerichte wordt geopend en verkeer naar het interne knooppunt-poort die wordt gebruikt door uw toepassing worden doorgestuurd. Als u een load balancer niet hebt, raadpleegt u [een internetgerichte load balancer configureren](../load-balancer/load-balancer-get-started-internet-portal.md).

Voor het maken van een Load Balancer-regel, moet u de volgende informatie verzamelen:

- Naam van load balancer.
- De resourcegroep van de load balancer en service fabric-cluster.
- Externe poort.
- Interne poort.

## <a name="azure-cli"></a>Azure-CLI
Dit duurt slechts één opdracht om te maken van een load balancer-regel met de **Azure CLI**. U hoeft alleen te weten van beide de naam van de load balancer en resource-groep om een nieuwe regel te maken.

>[!NOTE]
>Als u nodig hebt om te bepalen van de naam van de load balancer, moet u deze opdracht gebruiken om snel een lijst van alle load balancers en de bijbehorende resourcegroepen.
>
>`az network lb list --query "[].{ResourceGroup: resourceGroup, Name: name}"`
>


```azurecli
az network lb rule create --backend-port 40000 --frontend-port 39999 --protocol Tcp --lb-name LB-svcfab3 -g svcfab_cli -n my-app-rule
```

De Azure CLI-opdracht heeft een aantal parameters die in de volgende tabel worden beschreven:

| Parameter | Beschrijving |
| --------- | ----------- |
| `--backend-port`  | De poort van de Service Fabric-toepassing luistert. |
| `--frontend-port` | De poort van de load balancer wordt aangegeven voor externe verbindingen. |
| `-lb-name` | De naam van de load balancer te wijzigen. |
| `-g`       | De resourcegroep waaraan de load balancer en de Service Fabric-cluster. |
| `-n`       | De gewenste naam van de regel. |


>[!NOTE]
>Zie voor meer informatie over het maken van een load balancer met de Azure CLI [een load balancer maken met de Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="powershell"></a>PowerShell

PowerShell is iets gecompliceerder dan de Azure CLI. Volg deze algemene stappen voor het maken van een regel:

1. Haal de load balancer van Azure.
2. Maak een regel.
3. De regel toevoegen aan de load balancer.
4. Werk de load balancer.

>[!NOTE]
>Als u nodig hebt om te bepalen van de naam van de load balancer, moet u deze opdracht gebruiken om snel een lijst van alle load balancers en bijbehorende resourcegroepen.
>
>`Get-AzureRmLoadBalancer | Select Name, ResourceGroupName`

```powershell
# Get the load balancer
$lb = Get-AzureRmLoadBalancer -Name LB-svcfab3 -ResourceGroupName svcfab_cli

# Create the rule based on information from the load balancer.
$lbrule = New-AzureRmLoadBalancerRuleConfig -Name my-app-rule7 -Protocol Tcp -FrontendPort 39990 -BackendPort 40009 `
                                            -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
                                            -BackendAddressPool  $lb.BackendAddressPools[0] `
                                            -Probe $lb.Probes[0]

# Add the rule to the load balancer
$lb.LoadBalancingRules.Add($lbrule)

# Update the load balancer on Azure
$lb | Set-AzureRmLoadBalancer
```

Met betrekking tot de `New-AzureRmLoadBalancerRuleConfig` opdracht, de `-FrontendPort` vertegenwoordigt de poort voor externe verbindingen wordt aangegeven dat de load balancer en de `-BackendPort` vertegenwoordigt de service fabric-toepassing luistert op poort.

>[!NOTE]
>Zie voor meer informatie over het maken van een load balancer met PowerShell [een load balancer maken met PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [networking in Service Fabric](service-fabric-patterns-networking.md).