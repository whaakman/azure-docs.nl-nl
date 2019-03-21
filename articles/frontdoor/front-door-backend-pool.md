---
title: Azure voordeur Service - back-ends en back-Endpools | Microsoft Docs
description: In dit artikel krijgt u inzicht in wat zijn de back-end en back-endpools voor in de voordeur configuratie.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 228ed5c54a382db7b47d19adacf9e5db398c53ae
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58123688"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Back-ends en back-end-pools in Azure voordeur Service
In dit artikel worden de verschillende concepten met betrekking tot hoe u de implementatie van uw toepassing met de voordeur kunt toewijzen. Ook wordt uitgelegd wat de verschillende voorwaarden voor configuratie van de deur over back-end toepassing aan de hand.

## <a name="backend-pool"></a>Back-endpool
Een back-endpool in Front Door verwijst naar de set gelijkwaardige back-ends die hetzelfde type verkeer voor de toepassing kunnen ontvangen. Met andere woorden, het is een logische groepering van uw toepassingsinstanties overal ter wereld die hetzelfde verkeer kunnen ontvangen en met het verwachte gedrag kunnen reageren. Deze back-ends zijn meestal geïmplementeerd in verschillende regio's of binnen dezelfde regio. Bovendien deze back-ends zich in de modus actief-actief-implementatie of anderszins kan worden gedefinieerd als een actief/passief-configuratie.

Back-endpool bepaalt ook hoe de verschillende back-ends moet alle worden geëvalueerd voor hun status via statuscontroles en dienovereenkomstig hoe de taakverdeling tussen de back-ends moet gebeuren.

### <a name="health-probes"></a>Statuscontroles
Voordeur verzendt aanvragen van de test periodieke HTTP/HTTPS naar elk van de geconfigureerde back-ends om te bepalen het nabijheid en de status van elke back-end laden saldo ontvangen aanvragen van uw eindgebruikers. Instellingen van de Health-test voor een back endpool definiëren hoe we polling voor de integriteitsstatus voor back-ends voor uw toepassing. De volgende instellingen zijn beschikbaar voor configuratie voor de taakverdeling:

1. **Pad**: URL-pad waar de test-aanvragen worden verzonden naar voor alle de back-ends via de back-endpool. Bijvoorbeeld, als een van uw back-ends `contoso-westus.azurewebsites.net` en het pad is ingesteld op `/probe/test.aspx`, en vervolgens de voordeur omgevingen, ervan uitgaande dat het protocol is ingesteld op HTTP, verzendt de health test aanvragen voor http://contoso-westus.azurewebsites.net/probe/test.aspx. 
2. **Protocol**: Bepaalt of de aanvragen van de health-test van voordeur naar uw back-ends worden verzonden via HTTP of HTTPS-protocol.
3. **Interval (seconden)**: Dit veld bepaalt de frequentie van statuscontroles uw back-ends, dat wil zeggen, de intervallen waarin elk van de voordeur omgevingen een test wordt verzonden. Opmerking: als u op zoek bent voor snellere failover wordt ingesteld in dit veld op een lagere waarde. Echter, des te lager de waarde meer de statustest volume dat uw back-ends ontvangt. Als u een idee van hoeveel test volume voordeur wordt gegenereerd op de back-ends, we nemen een voorbeeld. Laten we zeggen dat het interval is ingesteld op 30 seconden en er ongeveer 90 voordeur omgevingen of POP's wereldwijd. Zodat elk van uw back-ends ongeveer ontvangen over 3 tot 5 test aanvragen per seconde.

Lezen [statuscontroles](front-door-health-probes.md) voor meer informatie.

### <a name="load-balancing-settings"></a>Instellingen voor taakverdeling
De load balancing instellingen voor de back-endpool definiëren hoe we de statuscontroles voor het bepalen van de back-end op in orde worden geëvalueerd en ook hoe we moeten voor taakverdeling tussen het verkeer tussen de verschillende back-ends via de back-endpool. De volgende instellingen zijn beschikbaar voor configuratie voor de taakverdeling:

1. **Voorbeeld van een grootte**: Deze eigenschap wordt aangegeven hoeveel exemplaren van statuscontroles we moet rekening houden met voor back-end evalueren.
2. **Geslaagde samplegrootte**: Deze eigenschap bepaalt dat van de grootte van het voorbeeld, zoals hierboven wordt beschreven hoeveel steekproeven we moeten om te controleren op slagen om aan te roepen van de back-end als in orde. 
</br>Bijvoorbeeld, stel voor de voordeur u hebt ingesteld met de statustest *interval* tot 30 seconden *voorbeeld grootte* is ingesteld op '5' en *geslaagde samplegrootte* is ingesteld op '3'. En wat deze configuratie betekent dat telkens wanneer we de statuscontroles voor uw back-end evalueren, kijken we de laatste vijf voorbeelden die zouden worden spanning laatste 150 seconden (= 5 * 30 s) en, tenzij er 3 of meer van deze tests geslaagde zijn declareert we de achtergrond einde niet in orde. Stel dat er slechts twee geslaagde tests en, zodat we de back-end als beschadigd wordt gemarkeerd. De volgende keer dat we de evaluatie uitgevoerd als we 3 lukt dit in de laatste vijf tests kunt vinden, markeert we vervolgens de back-end als in orde weer.
3. **Latentie gevoeligheid (extra latentie)**: Het veld latentie gevoeligheid definieert of u wilt dat de voordeur aan de aanvraag verzenden naar de back-ends die zich binnen het bereik van de gevoeligheid voor wat betreft latentie meting of de aanvraag naar de dichtstbijzijnde back-end. Lezen [minimale latentie op basis van de routeringsmethode](front-door-routing-methods.md#latency) voor voordeur voor meer informatie.

## <a name="backend"></a>Back-end
Een back-end is gelijk aan het exemplaar van de implementatie van een toepassing in een regio. Voordeur ondersteunt zowel Azure als niet-Azure-back-ends en dus de regio hier alleen is niet beperkt tot Azure-regio's, maar kan ook worden uw on-premises datacenter of een exemplaar van een toepassing in enkele andere cloud.

Back-ends verwijst in de context van voorgrond deuren, u naar de hostnaam of het openbare IP-adres van uw toepassing, die aanvragen van clients kan fungeren. Back-ends, dus moet niet worden verward met de databaselaag of uw storage-laag enz. maar in plaats daarvan moet worden weergegeven als het openbare eindpunt van de back-end van uw toepassing.

Wanneer u een back-end in een back-endpool van de voordeur toevoegt, moet u Vul de volgende gegevens:

1. **Back-end hosttype**: Het type resource dat u wilt toevoegen. Voordeur biedt ondersteuning voor automatische detectie van uw back-ends van appservice, cloudservice of opslag. Als u een andere resource in Azure of zelfs een niet-Azure back-end wilt, selecteert u 'Aangepaste host'. Opmerking: tijdens het configureren van dat de API's wordt niet gevalideerd of de back-end toegankelijk is vanaf de voordeur omgevingen, in plaats daarvan moet u ervoor zorgen is dat uw back-end kan worden bereikt door de voordeur. 
2. **Abonnement en de back-end hostnaam**: Als u niet hebt geselecteerd typt u 'Aangepaste host' voor back-end-host, en vervolgens moet u het bereik van omlaag en selecteert u uw back-end door het kiezen van het juiste abonnement en de bijbehorende back-end-hostnaam van de gebruikersinterface.
3. **Host-header van back-end**: De waarde van de Host-header is verzonden naar de back-end voor elke aanvraag. Lezen [host-header van back-end](#hostheader) voor meer informatie.
4. **Prioriteit**: Als u wilt een back-end van de primaire service gebruiken voor al het verkeer en back-ups opgeven als de primaire of de back-ends voor back-up niet beschikbaar zijn, kunt u prioriteiten toewijzen aan uw verschillende back-ends. Meer informatie over [prioriteit](front-door-routing-methods.md#priority).
5. **Gewicht**: Als u verkeer verspreiden over een set back-ends wilt, gelijkmatig of op basis van gewicht coëfficiënten, kunt u uw verschillende back-ends gewicht toewijzen. Meer informatie over [gewichten](front-door-routing-methods.md#weighted).


### <a name = "hostheader"></a>Back-end host-header

Aanvragen die door de voordeur doorgestuurd naar een back-end hebben een veld van de Host-header die de back-end gebruikt voor het ophalen van de betreffende resource. De waarde voor dit veld wordt meestal afkomstig is van de back-end van de URI en heeft de host en poort. Bijvoorbeeld: een verzoek voor `www.contoso.com` heeft de Host-header `www.contoso.com`. Als u uw back-end met behulp van Azure portal configureert, is de standaardwaarde voor dit veld wordt ingevuld de hostnaam van de back-end. Bijvoorbeeld, als uw back-end is `contoso-westus.azurewebsites.net`, wordt in Azure portal de waarde automatisch ingevuld voor back-end host-header `contoso-westus.azurewebsites.net`. 
</br>Echter, als u van Resource Manager-sjablonen gebruikmaakt of andere manier en u niet in dit veld expliciet instellen voordeur verzendt vervolgens de naam van de binnenkomende host als de waarde voor de Host-header. Bijvoorbeeld, als de aanvraag is gedaan voor `www.contoso.com`, en uw back-end `contoso-westus.azurewebsites.net` met het back-end host-header-veld als leeg, klikt u vervolgens voordeur wordt ingesteld als de Host-header `www.contoso.com`.

De meeste back-ends (zoals Web-Apps, Blob Storage en Cloud Services) vereisen dat de hostheader overeenkomt met het domein van de back-end. De frontend-host die u routes naar uw back-end heeft echter een andere hostnaam, zoals www\.contoso.azurefd.net. Als de back-end die u instelt, de hostheader overeenkomt met de hostnaam van de back-end moet, moet u ervoor zorgen dat de 'back-end host-header' ook heeft voor de hostnaam van de back-end.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Configureren van de back-end-host-header voor de back-end
Het veld 'Host-header van back-end' kan worden geconfigureerd voor een back-end in de sectie van de back-end-toepassingen.

1. Open uw voordeur-resource en klik op de back-endpool waarop de back-end moet worden geconfigureerd.

2. Als u niet een hebt toegevoegd, of een bestaand incident bewerkt, kunt u een back-end toevoegen. Het veld 'Host-header van back-end' kan worden ingesteld op een aangepaste waarde of leeg is, wat betekent dat de hostnaam voor de inkomende aanvraag wordt gebruikt als de waarde van de Host-header.



## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).