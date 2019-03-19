---
title: Azure voordeur Service - Routering regel overeenkomende bewaking | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe Azure voordeur Service komt overeen met die regel voor doorsturen moet worden gebruikt voor een binnenkomende aanvraag
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
ms.openlocfilehash: eec99bde0ea73a99a9dc1345f938b821a95a7c05
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58111834"
---
# <a name="how-front-door-matches-requests-to-a-routing-rule"></a>Hoe voordeur aansluit bij aanvragen voor een regel voor doorsturen

Nadat een verbinding tot stand brengen en een SSL-handshake doen, wanneer een aanvraag terechtkomt op een voordeur omgeving een van de eerste dingen die voordeur is het vaststellen van alle configuraties, welke regel voor het doorsturen van bepaalde zodat deze overeenkomen met de aanvraag voor het en klik vervolgens met inachtneming de opgegeven actie. Het volgende document wordt uitgelegd hoe voordeur bepaalt welke configuratie Route te gebruiken bij het verwerken van een HTTP-aanvraag.

## <a name="structure-of-a-front-door-route-configuration"></a>Structuur van een configuratie van de route voordeur
Een voordeur routeringsconfiguratie regel uit twee hoofdonderdelen bestaat: een "links" en "rechts". We overeen met de binnenkomende aanvraag aan de linkerkant van de route, terwijl de rechterkant wordt gedefinieerd hoe we de aanvraag niet verwerken.

### <a name="incoming-match-left-hand-side"></a>Binnenkomende overeenkomst (linkerkant)
De volgende eigenschappen te bepalen of de inkomende aanvraag overeenkomt met de regel voor doorsturen (of de linkerkant):

* **HTTP-protocollen** (HTTP/HTTPS)
* **Hosts** (bijvoorbeeld, www\.foo.com, \*. bar.com)
* **Paden** (bijvoorbeeld /\*, /gebruikers/\*, /file.gif)

Deze eigenschappen zijn intern uit uitgebreid zodat elke combinatie van Protocol/Hostpad een mogelijke overeenkomst-set is.

### <a name="route-data-right-hand-side"></a>Routeren van gegevens (rechterkant)
De beslissing van de aanvraag te verwerken, is afhankelijk van of opslaan in cache is ingeschakeld of niet voor de specifieke route. Dus als we een antwoord in de cache voor de aanvraag niet hebt, we u de aanvraag doorsturen naar de juiste back-end in de geconfigureerde back-endpool.

## <a name="route-matching"></a>Route die overeenkomt met
In deze sectie wordt de nadruk gelegd op hoe we aan een opgegeven regel voor het doorsturen van voordeur overeenkomen. Het fundamentele concept is dat we altijd met overeenkomen de **meest specifieke overeenkomt met de eerste** uitziende alleen op 'aan de linkerkant'.  We eerst komt overeen met op basis van HTTP-protocol vervolgens Frontend-host en het pad.

### <a name="frontend-host-matching"></a>Frontend-host die overeenkomt met
Wanneer die overeenkomt met een front-hosts, gebruiken we de logica zoals hieronder:

1. Zoek naar eventuele routering met een exacte overeenkomst op de host.
2. Als er geen exacte frontend-hosts overeenkomen, wordt de aanvraag weigeren en verzenden van een 400-Ongeldige aanvraag-fout.

Voor een uitleg van dit proces verder, laten we bekijken een van de voorbeeldconfiguratie van voordeur routes (alleen linkerkant):

| Routeringsregel | Front-endhosts | Pad |
|-------|--------------------|-------|
| A | foo.contoso.com | /\* |
| B | foo.contoso.com | /gebruikers/\* |
| C | www\.fabrikam.com, foo.adventure-works.com  | /\*, /images/\* |

Als de volgende binnenkomende aanvragen naar de voordeur zijn verzonden, zouden ze overeenkomen met op basis van de volgende regels voor doorsturen van bovenstaande:

| Binnenkomende frontend-host | Overeenkomende routering (s) |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www\.fabrikam.com | C |
| images.fabrikam.com | Fout 400: Onjuiste aanvraag |
| foo.adventure-works.com | C |
| contoso.com | Fout 400: Onjuiste aanvraag |
| www\.adventure works.com | Fout 400: Onjuiste aanvraag |
| www\.northwindtraders.com | Fout 400: Onjuiste aanvraag |

### <a name="path-matching"></a>Overeenkomende pad
Na bepaling van de specifieke front-host en mogelijke routeringsregels om alleen de routes met die host frontend te filteren, filtert de voordeur vervolgens de routeringsregels toe op basis van het Aanvraagpad. We gebruiken een soortgelijke logica als front-hosts:

1. Zoek naar een regel met een exacte overeenkomst in het pad voor het doorsturen
2. Als er geen exacte overeenkomst paden, zoekt u naar routeringsregels met een jokerteken pad die overeenkomt met
3. Als er geen regels voor doorsturen worden gevonden met een overeenkomende pad, de aanvraag weigeren en een 400 geretourneerd: Ongeldige aanvraag fout HTTP-antwoord.

>[!NOTE]
> Alle paden zonder een jokerteken worden beschouwd als exacte overeenkomst paden. Zelfs als het pad op een slash eindigt, wordt nog steeds exacte overeenkomst beschouwd.

Voor een nadere uitleg, kunt u een andere set voorbeelden laten we kijken:

| Routeringsregel | Frontend-host    | Pad     |
|-------|---------|----------|
| A     | www\.contoso.com | /        |
| B     | www\.contoso.com | /\*      |
| C     | www\.contoso.com | /ab      |
| D     | www\.contoso.com | /abc     |
| E     | www\.contoso.com | /ABC/    |
| F     | www\.contoso.com | /ABC/\*  |
| G     | www\.contoso.com | /abc/def |
| H     | www\.contoso.com | /Path/   |

Opgegeven dat de configuratie, het volgende voorbeeld van de overeenkomende tabel, is het resultaat:

| Inkomende aanvraag    | Overeenkomende Route |
|---------------------|---------------|
| www\.contoso.com/            | A             |
| www\.contoso.com/a           | B             |
| www\.contoso.com/ab          | C             |
| www\.contoso.com/abc         | D             |
| www\.contoso.com/abzzz       | B             |
| www\.contoso.com/abc/        | E             |
| www\.contoso.com/abc/d       | F             |
| www\.contoso.com/abc/def     | G             |
| www\.contoso.com/abc/defzzz  | F             |
| www\.contoso.com/abc/def/ghi | F             |
| www\.contoso.com/path        | B             |
| www\.contoso.com/path/       | H             |
| www\.contoso.com/path/zzz    | B             |

>[!WARNING]
> </br> Als er geen regels voor doorsturen voor een exacte overeenkomst frontend-host met een catch-all routeren pad (`/*`), wordt er niet overeenkomen met een regel voor doorsturen.
>
> Van de voorbeeldconfiguratie:
>
> | Route | Host             | Pad    |
> |-------|------------------|---------|
> | A     | profile.contoso.com | /API/\* |
>
> Overeenkomende tabel:
>
> | Inkomende aanvraag       | Overeenkomende Route |
> |------------------------|---------------|
> | profile.domain.com/other | Geen. Fout 400: Onjuiste aanvraag |

### <a name="routing-decision"></a>Routering besluit
Zodra we aan een regel voor het doorsturen van voordeur één hebt gekoppeld, moeten we vervolgens kiezen hoe u de aanvraag niet verwerken. Als voor de overeenkomende routeringsregel voordeur een antwoord in de cache beschikbaar heeft opgehaald terug naar de client hetzelfde zijn geleverd. Anders wordt het volgende wat die wordt geëvalueerd, is of u hebt geconfigureerd [herschrijven van URL's (aangepaste doorsturen via het pad)](front-door-url-rewrite.md) regel voor de overeenkomende routering of niet. Als er geen een aangepaste doorsturen pad dat is gedefinieerd, wordt de aanvraag doorgestuurd naar de juiste back-end in de geconfigureerde back endpool is. Anders het Aanvraagpad wordt bijgewerkt volgens de [aangepaste doorsturen pad](front-door-url-rewrite.md) gedefinieerd en vervolgens doorsturen naar de back-end.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
