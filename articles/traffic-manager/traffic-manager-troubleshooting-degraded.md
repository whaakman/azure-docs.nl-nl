---
title: Het oplossen van problemen gedegradeerd status op Azure Traffic Manager
description: Problemen met Traffic Manager-profielen wanneer deze wordt weergegeven als de status gedegradeerd.
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
ms.assetid: 8af0433d-e61b-4761-adcc-7bc9b8142fc6
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: kumud
ms.openlocfilehash: b1d00fb84695d2289f37647f55a7c56cf28c8c96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Het oplossen van problemen gedegradeerde status op Azure Traffic Manager

In dit artikel wordt beschreven hoe een Azure Traffic Manager-profiel dat wordt weergegeven een gedegradeerde status oplossen. Voor dit scenario kunt u een Traffic Manager-profiel die verwijst naar een deel van uw cloudapp.net gehoste services hebt geconfigureerd. Als de status van uw Traffic Manager wordt weergegeven in een **gedegradeerd** status en vervolgens de status van een of meer eindpunten mogelijk **gedegradeerd**:

![status van gedegradeerd endpoint](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Als de status van uw Traffic Manager wordt weergegeven in een **inactief** status en vervolgens beide eindpunten mogelijk **uitgeschakelde**:

![Inactieve Traffic Manager-status](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Understanding Traffic Manager-tests

* Traffic Manager overweegt een eindpunt worden ONLINE alleen wanneer de test een HTTP 200-respons van het pad van de test ontvangt. Een ander niet-200-antwoord is een fout.
* Een omleiding 30 x mislukt, zelfs als de omgeleide URL een 200 retourneert.
* Voor HTTPs-tests worden certificaatfouten genegeerd.
* De daadwerkelijke inhoud van het pad van de test maakt niet uit als een 200 wordt geretourneerd. Scannen van een URL naar een aantal statische inhoud, zoals ' / favicon.ico ' is een algemene methode. Dynamische inhoud, zoals de ASP-pagina's mogelijk niet altijd retourneren, 200, zelfs wanneer de toepassing in orde is.
* Er is een best practice naar het pad van de test ingesteld op iets wat onvoldoende logica om te bepalen heeft dat de site omhoog of omlaag is. In het vorige voorbeeld, door het pad in te stellen op ' / favicon.ico ', u alleen bent testen die w3wp.exe reageert. Deze test niet geeft aan dat uw webtoepassing in orde. Een betere optie zou zijn, zoals een pad op een iets instellen ' / Probe.aspx ' die logica voor het bepalen van de status van de site is. U kunt bijvoorbeeld prestatiemeteritems voor CPU-gebruik gebruiken of het aantal mislukte aanvragen. Of u toegang tot bronnen van de database of sessiestatus om ervoor te zorgen dat de webtoepassing werkt kan proberen.
* Als alle eindpunten in een profiel zijn gedegradeerd, behandelt Traffic Manager alle eindpunten als goed en routes verkeer naar alle eindpunten. Dit gedrag zorgt ervoor dat problemen met het mechanisme voor zoek niet in een volledige onderbreking van uw service resulteren.

## <a name="troubleshooting"></a>Problemen oplossen

Voor het oplossen van een test-fout, moet u een hulpprogramma waarmee de HTTP-statuscode geretourneerd van de test-URL. Er zijn veel hulpprogramma's beschikbaar die u de onbewerkte HTTP-antwoord tonen.

* [Fiddler](http://www.telerik.com/fiddler)
* [cURL](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Bovendien kunt u het tabblad netwerk van de F12-foutopsporing hulpprogramma's in Internet Explorer om de HTTP-antwoorden weer te geven.

In dit voorbeeld willen we zien van het antwoord van de WebTest-URL: http://watestsdp2008r2.cloudapp.net:80/test. De volgende PowerShell-voorbeeld ziet u het probleem.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Voorbeelduitvoer:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

U ziet dat er een omleidingsantwoord ontvangen. Zoals eerder gezegd, een StatusCode dan wordt 200 als een fout geïnterpreteerd. Traffic Manager verandert de Eindpuntstatus op Offline. Controleer de websiteconfiguratie om ervoor te zorgen dat de juiste StatusCode kan worden geretourneerd van het pad van de test het probleem op te lossen. Configureer opnieuw de Traffic Manager-test om te verwijzen naar een pad op dat een 200 retourneert.

Als uw test het HTTPS-protocol wordt gebruikt, moet u wellicht controleren SSL/TLS om fouten te voorkomen tijdens uw test certificaat uitschakelen. De volgende PowerShell-instructies uitschakelen validatie van het servercertificaat voor de huidige PowerShell-sessie:

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

[Wat is het Traffic Manager](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-Cmdlets][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx
