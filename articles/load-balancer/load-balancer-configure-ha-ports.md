---
title: Hoge beschikbaarheid poorten configureren voor Azure Load Balancer | Microsoft Docs
description: Informatie over het gebruik van hoge beschikbaarheid van poorten voor interne verkeer op alle poorten voor taakverdeling
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/20178
ms.author: kumud
ms.openlocfilehash: 7f7f8e254e0ed0556446e7b08eaf46ec59977f62
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Hoge beschikbaarheid poorten configureren voor een interne load balancer

Dit artikel bevat een voorbeeld van een implementatie met hoge beschikbaarheid poorten op een interne load balancer. Zie de bijbehorende provider websites voor meer informatie over specifieke configuraties voor virtuele apparaten (NVAs).

>[!NOTE]
>Azure Load Balancer ondersteunt twee verschillende typen: Basic en Standard. Dit artikel wordt standaard Load Balancer. Zie voor meer informatie over Basic Load Balancer [overzicht van de Load Balancer](load-balancer-overview.md).

De afbeelding ziet de volgende configuratie van de voorbeeld-implementatie in dit artikel wordt beschreven:

- De NVAs worden geïmplementeerd in de groep back-end van een interne load balancer achter de configuratie voor hoge beschikbaarheid van poorten. 
- De gebruiker gedefinieerde route (UDR) toegepast op de DMZ subnetroutes al het verkeer op de NVAs door het maken van de volgende hop als de interne load balancer virtueel IP-adres. 
- De interne load balancer wordt het verkeer naar een van de actieve NVAs volgens de load balancer-algoritme.
- De NVA het verkeer wordt verwerkt en stuurt het naar de oorspronkelijke doelhost in het back-end-subnet.
- Het retourtype pad kan dezelfde route duren als een bijbehorende UDR is geconfigureerd in het back-end-subnet. 

![Hoge beschikbaarheid poorten Voorbeeldimplementatie](./media/load-balancer-configure-ha-ports/haports.png)


## <a name="preview-sign-up"></a>Preview-registratie

Registreer uw abonnement om toegang te krijgen met behulp van Azure CLI 2.0 of PowerShell als u wilt deelnemen aan de evaluatieversie van de functie voor hoge beschikbaarheid van poorten in standaard Azure Load Balancer. Registreren van uw abonnement voor [standaard Load Balancer Preview](https://aka.ms/lbpreview#preview-sign-up).

>[!NOTE]
>De registratie van de standaard Load Balancer Preview kan een uur duren.

## <a name="configure-high-availability-ports"></a>Poorten voor hoge beschikbaarheid configureren

Hoge beschikbaarheid als poorten wilt configureren, instellen van een interne load balancer met de NVAs in de groep back-end. Een overeenkomende health test taakverdelingsconfiguratie instellen voor het detecteren van NVA status en de regel voor load balancer met hoge beschikbaarheid van poorten. De algemene load balancer-gerelateerde configuratie wordt beschreven in [aan de slag](load-balancer-get-started-ilb-arm-portal.md). In dit artikel ziet u de configuratie voor hoge beschikbaarheid van poorten.

De configuratie in wezen omvat het instellen van de front-endpoort en de waarde van de back-end-poort naar **0**. Stel de protocolwaarde op **alle**. Dit artikel wordt beschreven hoe u hoge beschikbaarheid om poorten te configureren met behulp van de Azure-portal, PowerShell en Azure CLI 2.0.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Een load balancer-regel van hoge beschikbaarheid poorten configureren met de Azure-portal

Hoge beschikbaarheid als poorten wilt configureren met behulp van de Azure-portal, selecteer de **HA poorten** selectievakje. Wanneer u selecteert, wordt de bijbehorende poort en protocol configuration automatisch ingevuld. 

![Configuratie voor hoge beschikbaarheid poorten via de Azure-portal](./media/load-balancer-configure-ha-ports/haports-portal.png)


### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Een hoge beschikbaarheid poorten taakverdeling regel via de Resource Manager-sjabloon configureren

U kunt de hoge beschikbaarheid poorten configureren met behulp van de 2017-08-01-API-versie voor Microsoft.Network/loadBalancers in de Load Balancer-resource. De volgende JSON-fragment ziet u de wijzigingen in de load balancer-configuratie voor hoge beschikbaarheid poorten via de REST-API:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>Een load balancer-regel van hoge beschikbaarheid poorten configureren met PowerShell

Gebruik de volgende opdracht voor het maken van de load balancer-regel van poorten voor hoge beschikbaarheid bij het maken van de interne load balancer met PowerShell:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli-20"></a>Een load balancer-regel van hoge beschikbaarheid poorten configureren met Azure CLI 2.0

In stap 4 van [maken van een interne load balancer-set](load-balancer-get-started-ilb-arm-cli.md), gebruik de volgende opdracht voor het maken van de load balancer-regel van hoge beschikbaarheid poorten:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [hoge beschikbaarheid poorten](load-balancer-ha-ports-overview.md).
