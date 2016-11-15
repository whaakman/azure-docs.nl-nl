---
title: Aan de slag met het maken van een internetgerichte load balancer in het klassieke implementatiemodel voor cloudservices | Microsoft Docs
description: Meer informatie over het maken van een internetgerichte load balancer in het klassieke implementatiemodel voor cloudservices
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/17/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 171d5cd41d900b83c22e1db4bc514471a3d4b556

---

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Aan de slag met het maken van een internetgerichte load balancer voor cloudservices

[!INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. Hier vindt u [meer informatie over hoe u een internetgerichte load balancer maakt met Azure Resource Manager](load-balancer-get-started-internet-arm-cli.md).

Cloudservices worden automatisch geconfigureerd met een load balancer en kunnen worden aangepast via het servicemodel.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Een load balancer maken met behulp van het servicedefinitiebestand

U kunt gebruikmaken van de Azure SDK voor .NET 2.5 om uw cloudservice bij te werken. Instellingen van eindpunten voor cloudservices kunnen worden aangebracht in het bestand [servicedefinition](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef.

In het volgende voorbeeld ziet u hoe het bestand servicedefinition.csdef voor de implementatie van een cloud is geconfigureerd:

Als u het fragment voor het .csdef-bestand bekijkt dat is gegenereerd door een cloudimplementatie, kunt u zien dat het externe eindpunt is geconfigureerd voor het gebruik van HTTP-poorten op poort 10000, 10001 en 10002.

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

De load balancer combineert de gegevens van het eindpunt en de gegevens van de test om een URL met de volgende indeling te maken: http://{DIP van VM}:80/Probe.aspx. Deze kan worden gebruikt om de status van de service op te vragen.

De service detecteert periodieke tests van hetzelfde IP-adres. Dit is de aanvraag voor een statustest die afkomstig is van de host van het knooppunt waarop de virtuele machine wordt uitgevoerd. Als de service reageert met een HTTP 200-statuscode, gaat de load balancer ervan uit dat de service in orde is. Bij een andere HTTP-statuscode (bijvoorbeeld 503) wordt de virtuele machine direct uitgeschakeld.

De definitie van de test bepaalt ook de frequentie waarmee de test wordt uitgevoerd. In ons geval test de load balancer het eindpunt elke 5 seconden. Als er 10 seconden lang geen positief antwoord wordt ontvangen (twee testintervallen), wordt aangenomen dat de test negatief is en wordt de virtuele machine uitgeschakeld. Als de service is uitgeschakeld en er een positief antwoord wordt ontvangen, wordt de service direct weer in gebruik genomen. Als de status van de service steeds wisselt tussen goed en slecht, kan de load balancer de service tijdelijk uitgeschakeld houden totdat de status gedurende een aantal tests goed blijkt.

Controleer het schema van de servicedefinitie voor de [statustest](https://msdn.microsoft.com/library/azure/jj151530.aspx) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Nov16_HO2-->


