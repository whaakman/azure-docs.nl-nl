---
title: Problemen oplossen in Azure Traffic Manager de status gedegradeerd
description: Problemen oplossen met Traffic Manager-profielen wanneer deze wordt weergegeven als de status gedegradeerd.
services: traffic-manager
documentationcenter: ''
author: chadmath
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: genli
ms.openlocfilehash: 6f539cb905c0ed401816c7f223918a1416f95d6c
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312271"
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Problemen oplossen in gedegradeerde status op Azure Traffic Manager

In dit artikel wordt beschreven hoe u een Azure Traffic Manager-profiel dat wordt weergegeven een gedegradeerde status oplossen. Voor dit scenario kunt u dat u een Traffic Manager-profiel dat verwijst naar een van uw cloudapp.net gehoste services hebt geconfigureerd. Als de status van uw Traffic Manager weergeeft: een **gedegradeerd** status en de status van een of meer eindpunten mogelijk **gedegradeerd**:

![status van gedegradeerd endpoint](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Als de status van uw Traffic Manager weergeeft: een **inactief** status en vervolgens beide eindpunten mogelijk **uitgeschakelde**:

![Niet-actieve status van Traffic Manager](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Informatie over Traffic Manager testen

* Traffic Manager rekening gehouden met een eindpunt te zijn van ONLINE alleen wanneer de test een HTTP 200-respons terug van het testpad ontvangt. Alle andere niet-200-respons is een fout.
* Een omleiding 30 x is mislukt, zelfs als de URL van de omgeleide een 200 retourneert.
* Voor HTTPs-tests, worden certificaatfouten genegeerd.
* De daadwerkelijke inhoud van het testpad niet van belang, zolang een 200 wordt geretourneerd. Scannen van een URL naar bepaalde statische inhoud, zoals ' / favicon.ico ' is een algemene techniek. Dynamische inhoud, zoals de ASP-pagina's mogelijk niet altijd 200 retourneren, zelfs wanneer de toepassing in orde.
* Een best practice is het instellen van het testpad naar iets dat onvoldoende logica heeft om te bepalen dat de site, omhoog of omlaag. In het vorige voorbeeld, door het pad in te stellen op ' / favicon.ico ", u alleen bent die w3wp.exe tests reageert. Deze test kan niet erop wijzen dat uw webtoepassing in orde is. Een betere optie is om in te stellen van een pad naar een iets zoals ' / Probe.aspx ' waarvoor logica om de status van de site te bepalen. Bijvoorbeeld, u prestatiemeteritems voor CPU-gebruik gebruiken of meten van het aantal mislukte aanvragen. Of u toegang tot database-resources of sessiestatus om ervoor te zorgen dat de web-App werkt kan proberen.
* Als alle eindpunten in een profiel worden gedegradeerd, behandelt Traffic Manager alle eindpunten als in orde en routeert verkeer naar alle eindpunten. Dit gedrag zorgt ervoor dat problemen met het testinterval mechanisme niet in een volledige uitval van uw service resulteren.

## <a name="troubleshooting"></a>Problemen oplossen

Voor het oplossen van een test mislukt, moet u een hulpprogramma waarmee HTTP-statuscode geretourneerd van de WebTest-URL. Er zijn veel hulpprogramma's beschikbaar waarin u het onbewerkte HTTP-antwoord.

* [Fiddler](https://www.telerik.com/fiddler)
* [curl](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Bovendien kunt u het tabblad netwerk van het gebied van F12 Tools foutopsporing in Internet Explorer om de HTTP-antwoorden weer te geven.

In dit voorbeeld willen we om te zien van het antwoord van de WebTest-URL: http:\//watestsdp2008r2.cloudapp.net:80/Probe. De volgende PowerShell-voorbeeld ziet u het probleem.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Voorbeelduitvoer:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

U ziet dat er een omleidingsantwoord is ontvangen. Zoals eerder gezegd, een StatusCode dan 200, wordt beschouwd als een fout. Traffic Manager de Eindpuntstatus gewijzigd in offline. Controleer de websiteconfiguratie om ervoor te zorgen dat de juiste StatusCode kan worden geretourneerd van het testpad het probleem op te lossen. De configuratie van de Traffic Manager-test om te verwijzen naar een pad dat een 200 retourneert.

Als uw test het HTTPS-protocol gebruikt, moet u mogelijk certificaat controleren SSL/TLS om fouten te voorkomen tijdens uw test uitschakelen. De volgende PowerShell-instructies uit te schakelen validatie van het servercertificaat voor de huidige PowerShell-sessie:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Volgende stappen

[Over verkeersrouteringsmethoden voor Traffic Manager](traffic-manager-routing-methods.md)

[Wat is Traffic Manager](traffic-manager-overview.md)

[Cloud Services](https://go.microsoft.com/fwlink/?LinkId=314074)

[Azure App Service](https://azure.microsoft.com/documentation/services/app-service/web/)

[Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets][1]

[1]: https://docs.microsoft.com/powershell/module/azurerm.trafficmanager
