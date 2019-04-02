---
title: Geavanceerde aanvraagbeperking met Azure API Management
description: Informatie over het maken en toepassen van flexibele quotum en dat beleid met Azure API Management te beperken.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2018
ms.author: apimpm
ms.openlocfilehash: 22c3987121e2ab3479274c89c359c679f5f1135e
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793342"
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Geavanceerde aanvraagbeperking met Azure API Management
De mogelijkheid om te beperken van binnenkomende aanvragen is een belangrijke rol van Azure API Management. Een door de snelheid van aanvragen of het totale aantal aanvragen/gegevens overgebracht te beheren, met API Management kunnen API-providers voor hun API's beveiligen tegen misbruik en maken van de waarde voor de verschillende lagen van de API-product.

## <a name="product-based-throttling"></a>Op basis van een product beperking
Tot op heden, de snelheid zijn bandbreedtebeperking mogelijkheden die wordt toegewezen aan een bepaald Product-abonnement, gedefinieerd in de Azure-portal. Dit is handig voor de API-provider beperkingen toepassen op de ontwikkelaars die zich hebben aangemeld hun API gebruikt, maar deze niet helpt, bijvoorbeeld in de beperking van afzonderlijke eindgebruikers van de API. Het is mogelijk dat voor één gebruiker van de ontwikkelaar van de toepassing om te gebruiken van het gehele quotum en vervolgens wordt het gebruik van de toepassing te voorkomen dat andere klanten van de ontwikkelaar. Verschillende klanten die een groot aantal aanvragen kunnen genereren mogelijk ook toegang tot incidentele gebruikers te beperken.

## <a name="custom-key-based-throttling"></a>Aangepaste sleutel op basis van beperking
De nieuwe [rate-limit-by-key](/azure/api-management/api-management-access-restriction-policies#LimitCallRateByKey) en [quota-by-key](/azure/api-management/api-management-access-restriction-policies#SetUsageQuotaByKey) -beleid biedt u een meer flexibele oplossing voor beheer van netwerkverkeer. Deze nieuwe beleidsregels kunnen u voor het definiëren van expressies voor het identificeren van de sleutels die worden gebruikt voor het bijhouden van verkeer. De manier waarop die dit werkt wordt easiest weergegeven met een voorbeeld. 

## <a name="ip-address-throttling"></a>Beperking van de IP-adres
De volgende beleidsregels beperken een IP-adres van één client tot maximaal 10 aanroepen per minuut, met een totaal van 1.000.000 aanroepen en 10.000 kilobytes aan bandbreedte per maand. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Als alle clients op Internet een uniek IP-adres gebruikt, is dit mogelijk een effectieve manier gebruik door de gebruiker te beperken. Het is echter waarschijnlijk dat meerdere gebruikers delen een enkel openbaar IP-adres omdat ze toegang tot Internet via een NAT-apparaat. Ondanks dat dit voor API's waarmee niet-geverifieerde toegang tot de `IpAddress` mogelijk de beste optie.

## <a name="user-identity-throttling"></a>Beperking van gebruikers-id
Als een eindgebruiker is geverifieerd, kan klikt u vervolgens een beperking sleutel worden gegenereerd op basis van informatie die een unieke identificatie van die gebruiker.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

In dit voorbeeld laat zien hoe de autorisatie-header ophalen, converteert u deze naar `JWT` object en het onderwerp van het token gebruiken om te bepalen van de gebruiker en als de frequentie sleutel. Als de identiteit van de gebruiker wordt opgeslagen in de `JWT` als een van de andere claims, wordt die waarde kan worden gebruikt in plaats daarvan.

## <a name="combined-policies"></a>Gecombineerde beleid
Hoewel de nieuwe beperking beleidsregels meer controle te hebben dan de bestaande beleidsregels voor beperking bieden, is er nog steeds waarde beide mogelijkheden te combineren. Beperking van de productcode van het abonnement ([aanroepfrequentie per abonnement beperken](/azure/api-management/api-management-access-restriction-policies#LimitCallRate) en [gebruiksquotum per abonnement instellen](/azure/api-management/api-management-access-restriction-policies#SetUsageQuota)) is een uitstekende manier om in te schakelen van een API merkbekendheid in rekening gebracht op basis van het gebruiksniveau. De wilt korrelig controle over de mogelijkheid om te beperken door de gebruiker is een aanvulling en voorkomt u dat gedrag van een gebruiker vernederen van de ervaring van een andere. 

## <a name="client-driven-throttling"></a>Client gestuurde beperking
Als de bandbreedteregeling sleutel is gedefinieerd met behulp van een [beleidsexpressie](/azure/api-management/api-management-policy-expressions), dan is de API-provider die is te kiezen hoe de beperking is afgestemd. Echter, een ontwikkelaar verstandig om te bepalen hoe ze hebben gescoord limiet hun eigen klanten. Dit kan worden ingeschakeld door de API-provider door de introductie van een aangepaste header om toe te staan van de ontwikkelaar-clienttoepassing voor de communicatie van de sleutel voor de API.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Hierdoor kunnen client-toepassing om te kiezen hoe ze willen maken van de frequentie sleutel van de ontwikkelaar. De client-ontwikkelaars kunnen hun eigen snelheid lagen maken door het instellen van sleutels te wijzen aan gebruikers en het gebruik van de sleutel draaien.

## <a name="summary"></a>Samenvatting
Met Azure API Management biedt de snelheid en offerte beperking zowel wilt beveiligen en waarde toevoegen aan uw API-service. De nieuwe beperking beleidsregels met aangepaste scoping regels kunnen u wilt dat controle over deze beleidsregels kunnen uw klanten nog betere toepassingen te bouwen. De voorbeelden in dit artikel worden het gebruik van het nieuwe beleid door productie frequentie sleutels met client-IP-adressen, gebruikers-id en waarden van de client gegenereerd. Er zijn echter veel andere onderdelen van het bericht dat kan worden gebruikt, zoals de gebruikersagent, URL-pad fragmenten, grootte van het bericht.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in het Disqus-thread voor dit onderwerp. Het is fantastisch om te weten andere mogelijke sleutelwaarden die een logische keuze in uw scenario's zijn.

