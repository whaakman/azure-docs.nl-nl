---
title: Een internetgerichte load balancer maken voor Azure Cloud Services | Microsoft Docs
description: Meer informatie over het maken van een internetgerichte load balancer in het klassieke implementatiemodel voor cloudservices
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: b389d9a01db394b79d07ff9c3d6d1cd94e811472
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Aan de slag met het maken van een internetgerichte load balancer voor cloudservices

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Voordat u met Azure-resources gaat werken, is het belangrijk om te weten dat Azure momenteel twee implementatiemodellen heeft: Azure Resource Manager en het klassieke model. Zorg ervoor dat u begrijpt wat [implementatiemodellen en hulpprogramma's](../azure-classic-rm.md) zijn voordat u met een Azure-resource gaat werken. U kunt de documentatie voor verschillende hulpprogramma's bekijken door op de tabbladen boven aan dit artikel te klikken. Dit artikel is van toepassing op het klassieke implementatiemodel. Hier vindt u [meer informatie over hoe u een internetgerichte load balancer maakt met Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

Cloudservices worden automatisch geconfigureerd met een load balancer en kunnen worden aangepast via het servicemodel.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Een load balancer maken met behulp van het servicedefinitiebestand

U kunt gebruikmaken van de Azure SDK voor .NET 2.5 om uw cloudservice bij te werken. Instellingen van eindpunten voor cloudservices kunnen worden aangebracht in het bestand [servicedefinition](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef.

In het volgende voorbeeld ziet u hoe het bestand servicedefinition.csdef voor de implementatie van een cloud is geconfigureerd:

Als u het fragment voor het .csdef-bestand bekijkt dat is gegenereerd via een cloudimplementatie, kunt u zien dat het externe eindpunt is geconfigureerd voor het gebruik van HTTP-poorten op poort 10000, 10001 en 10002.

```xml
<ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
<Endpoints>
    <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
    <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
    <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

    <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

    <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
        <AllocatePublicPortFrom>
            <FixedPortRange min=“10110” max=“10120“  />
        </AllocatePublicPortFrom>
    </InstanceInputEndpoint>
    <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
</Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a>De status van een load balancer voor cloudservices controleren

Hier volgt een voorbeeld van een statustest:

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

De load balancer combineert de gegevens van het eindpunt en de gegevens van de test om een URL te maken met de indeling `http://{DIP of VM}:80/Probe.aspx`. Deze kan worden gebruikt om de status van de service op te vragen.

De service detecteert periodieke tests van hetzelfde IP-adres. Dit is de aanvraag voor een statustest die afkomstig is van de host van het knooppunt waarop de virtuele machine wordt uitgevoerd. Als de service reageert met een HTTP 200-statuscode, gaat de load balancer ervan uit dat de service in orde is. Bij een andere HTTP-statuscode (bijvoorbeeld 503) wordt de virtuele machine direct uitgeschakeld.

De definitie van de test bepaalt ook de frequentie waarmee de test wordt uitgevoerd. In ons geval test de load balancer het eindpunt elke 5 seconden. Als er 10 seconden lang geen positief antwoord wordt ontvangen (twee testintervallen), wordt aangenomen dat de test negatief is en wordt de virtuele machine uitgeschakeld. Als de service is uitgeschakeld en er een positief antwoord wordt ontvangen, wordt de service direct weer in gebruik genomen. Als de status van de service steeds wisselt tussen goed en slecht, kan de load balancer de service tijdelijk uitgeschakeld houden totdat de status gedurende een aantal tests goed blijkt.

Controleer het schema van de servicedefinitie voor de [statustest](https://msdn.microsoft.com/library/azure/jj151530.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)

