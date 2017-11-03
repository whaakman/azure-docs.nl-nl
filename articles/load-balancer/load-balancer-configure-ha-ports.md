---
title: Hoge beschikbaarheid poorten configureren voor Azure Load Balancer | Microsoft Docs
description: Informatie over het gebruik van poorten voor hoge beschikbaarheid voor interne verkeer op alle poorten voor taakverdeling
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: 7256548b988812c64ca9a9f8a84fec377646635d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-high-availability-ports-for-internal-load-balancer"></a>Het configureren van poorten voor hoge beschikbaarheid voor de interne Load Balancer

Dit artikel bevat een Voorbeeldimplementatie van hoge beschikbaarheid (HA) poorten op een interne Load Balancer. Voor specifieke configuraties van virtuele netwerkapparaten, raadpleegt u de bijbehorende provider websites.

>[!NOTE]
> Hoge beschikbaarheid poorten functie is momenteel in Preview. Tijdens de preview heeft de functie mogelijk niet dezelfde beschikbaarheid en betrouwbaarheid als functies die al algemeen beschikbaar zijn. Zie [Microsoft Azure Supplemental Terms of Use for Microsoft Azure Previews (Microsoft Azure Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Afbeelding 1 ziet u de volgende configuratie van de voorbeeld-implementatie in dit artikel wordt beschreven:
- De NVAs worden geïmplementeerd in de groep back-end van een interne Load Balancer achter de configuratie van de HA-poorten. 
- De UDR toegepast op de DMZ subnetroutes al het verkeer op de <>? door het maken van de volgende hop als de interne Load Balancer virtueel IP-adres. 
- Interne Load Balancer wordt het verkeer naar een van de actieve NVAs volgens de algoritme van de Load Balancer.
- NVA het verkeer wordt verwerkt en stuurt het naar de oorspronkelijke doelhost in het back-end-subnet.
- Het retourtype pad kan ook dezelfde route duren als een bijbehorende UDR is geconfigureerd in het back-end-subnet. 

![ha poorten Voorbeeldimplementatie](./media/load-balancer-configure-ha-ports/haports.png)

Afbeelding 1: virtuele netwerkapparaten geïmplementeerd achter een interne Load Balancer met hoge beschikbaarheid poorten 

## <a name="preview-sign-up"></a>Preview-registratie

Als u wilt deelnemen aan de evaluatieversie van de functie van de HA-poorten in Load Balancer standaard SKU Registreer uw abonnement om met PowerShell of Azure CLI 2.0 toegang te krijgen.

- Aanmelden met behulp van PowerShell

   ```powershell
   Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```

- Aanmelden met Azure CLI 2.0

    ```cli
  az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network  
    ```

## <a name="configuring-ha-ports"></a>HA-poorten configureren

De configuratie van de HA-poorten omvat het instellen van een interne Load Balancer, met de NVAs in de endadresgroep back-van een overeenkomende health test taakverdelingsconfiguratie NVA gezondheid en de Load Balancer-regel met HA-poorten te detecteren. De algemene configuratie van de Load Balancer-gerelateerde wordt beschreven in [aan de slag](load-balancer-get-started-ilb-arm-portal.md). In dit artikel ziet u de configuratie van de HA-poorten.

De configuratie in wezen worden van de frontend-poort en back-end van de waarde van de poort **0**, en de protocolwaarde op **alle**. In dit artikel wordt beschreven hoe Azure-portal, PowerShell en Azure CLI 2.0 met hoge beschikbaarheid-poorten configureren.

### <a name="configure-ha-ports-load-balancer-rule-with-the-azure-portal"></a>Regel voor load balancer voor HA poorten configureren met de Azure-portal

De Azure-portal bevat de **HA poorten** optie via een selectievakje voor deze configuratie. Wanneer u selecteert, wordt de bijbehorende poort en protocol configuration automatisch ingevuld. 

![ha poorten configuratie via de Azure-portal](./media/load-balancer-configure-ha-ports/haports-portal.png)

Afbeelding 2: HA poortconfiguratie via de Portal

### <a name="configure-ha-ports-load-balancer-rule-with-powershell"></a>Regel voor load balancer voor HA-poorten configureren met PowerShell

Gebruik de volgende opdracht de HA-poorten Load Balancer-regel maken tijdens het maken van de interne Load Balancer met PowerShell:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-ha-ports-load-balancer-rule-with-azure-cli-20"></a>Regel voor load balancer voor HA-poorten configureren met Azure CLI 2.0

Bij stap 4 van [maken van een interne Load Balancer ingesteld](load-balancer-get-started-ilb-arm-cli.md), gebruik de volgende opdracht maakt u de HA-poorten Load Balancer-regel.

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [poorten voor hoge beschikbaarheid](load-balancer-ha-ports-overview.md)
