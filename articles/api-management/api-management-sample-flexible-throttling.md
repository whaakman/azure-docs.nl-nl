---
title: Geavanceerde aanvraagbeperking met Azure API Management
description: Informatie over het maken en toepassen van flexibele quota's en snelheidsbeperking beleid met Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: fc813a65-7793-4c17-8bb9-e387838193ae
ms.service: api-management
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 356f98aec072a1295915ae0701a3e3cd793aba07
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="advanced-request-throttling-with-azure-api-management"></a>Geavanceerde aanvraagbeperking met Azure API Management
Om te beperken van binnenkomende aanvragen kunnen is een belangrijke functie van Azure API Management. Een door de snelheid van aanvragen of de totale aanvragen/gegevensoverdracht te beheren, kunt API Management API-providers voor hun-API's beveiligen tegen misbruik en waarde maken voor de verschillende lagen voor API-product.

## <a name="product-based-throttling"></a>Op basis van product-beperking
Op dit moment, de frequentie waarmee beperking zijn mogelijkheden beperkt tot wordt binnen het bereik van een bepaald Product-abonnement (in wezen een sleutel), gedefinieerd in de publicatieportal van API Management. Dit is handig voor de API-provider beperkingen toepassen op de ontwikkelaars die zich hebben geregistreerd hun API gebruiken, echter dat niet helpt, bijvoorbeeld in een beperking van afzonderlijke eindgebruikers van de API. Het is mogelijk dat voor één gebruiker van de toepassing van de ontwikkelaar het hele quotum gebruiken en vervolgens te voorkomen dat andere klanten van de ontwikkelaar kan de toepassing gebruiken. Enkele klanten een groot aantal aanvragen genereren kunnen kunnen ook toegang tot incidentele gebruikers te beperken.

## <a name="custom-key-based-throttling"></a>Aangepaste sleutel op basis van beperking
De nieuwe [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -beleid biedt u een aanzienlijk meer flexibele oplossing voor beheer van netwerkverkeer. Dit nieuwe beleid kunt u voor het definiëren van expressies voor het aanduiden van de sleutels die worden gebruikt voor het bijhouden van gebruik van verkeer. De manier waarop die dit werkt wordt easiest weergegeven met een voorbeeld. 

## <a name="ip-address-throttling"></a>Beperking van IP-adres
De volgende beleidsregels voor beperken een IP-adres van één client tot alleen 10 aanroepen elke minuut, met een totaal van 1.000.000 aanroepen en 10.000 kilobytes van bandbreedte per maand. 

```xml
<rate-limit-by-key  calls="10"
          renewal-period="60"
          counter-key="@(context.Request.IpAddress)" />

<quota-by-key calls="1000000"
          bandwidth="10000"
          renewal-period="2629800"
          counter-key="@(context.Request.IpAddress)" />
```

Als alle clients op het Internet een uniek IP-adres gebruikt, dit wordt mogelijk een effectieve manier voor het beperken van gebruiksgegevens door de gebruiker. Het is echter wel waarschijnlijk dat meerdere gebruikers delen één openbaar IP-adres omdat ze toegang tot Internet via een NAT-apparaat wordt. Ondanks dit voor de API's waarmee niet-geverifieerde toegang tot de `IpAddress` mogelijk de beste optie.

## <a name="user-identity-throttling"></a>Gebruiker identity-beperking
Als een eindgebruiker is geverifieerd, wordt een bandbreedteregeling sleutel kan worden gegenereerd op basis van informatie die een unieke identificatie van een die gebruiker.

```xml
<rate-limit-by-key calls="10"
    renewal-period="60"
    counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />
```

In dit voorbeeld we uitpakken van de autorisatie-header, converteert u deze naar `JWT` object en het onderwerp van het token gebruiken om te bepalen van de gebruiker en gebruiken als de frequentie waarmee de sleutel te beperken. Als de identiteit van de gebruiker is opgeslagen in de `JWT` als een van de andere claims vervolgens dat de waarde kan worden gebruikt in plaats daarvan.

## <a name="combined-policies"></a>Gecombineerde beleid
Hoewel de nieuwe bandbreedteregeling beleidsregels meer controle dan de bestaande beleidsregels voor bandbreedteregeling bieden, is er nog steeds waarde combinatie van beide mogelijkheden. Beperkingen door abonnement productcode ([aanroepfrequentie per abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) en [gebruiksquotum per abonnement instellen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) is een uitstekende manier om in te schakelen van een API monetizing in rekening gebracht op basis van gebruik niveaus op. De betere resultaten gedetailleerde besturingselement niet in staat om te beperken door de gebruiker is een aanvulling en gedrag van een gebruiker wordt voorkomen dat de ervaring van een andere beïnvloeden. 

## <a name="client-driven-throttling"></a>Client aangestuurd beperking
Als de bandbreedteregeling sleutel is gedefinieerd met behulp van een [beleidsexpressie](https://msdn.microsoft.com/library/azure/dn910913.aspx), dan is de API-provider die is kiezen hoe de beperking ligt binnen het bereik. Echter, een ontwikkelaar mogelijk wilt bepalen hoe ze frequentielimiet hun eigen klanten. Dit kan worden ingeschakeld door de API-provider door de introductie van een aangepaste header om toe te staan van de ontwikkelaar de clienttoepassing communiceren de sleutel voor de API.

```xml
<rate-limit-by-key calls="100"
          renewal-period="60"
          counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>
```

Hierdoor kunnen de clienttoepassing van de ontwikkelaar te kiezen hoe ze willen maken van de sleutel van de snelheidsbeperking. Met een beetje draaien kan een ontwikkelaar client hun eigen niveaus tarief maken door sets van sleutels toewijzen aan gebruikers en het sleutelgebruik draaien.

## <a name="summary"></a>Samenvatting
Azure API Management biedt snelheid en aanhalingsteken beperking zowel wilt beveiligen en waarde toevoegen aan uw API-service. De nieuwe bandbreedteregeling beleidsregels met aangepaste bereik regels kunnen u betere resultaten gedetailleerde controle over deze beleidsregels waarmee uw klanten om nog betere toepassingen te bouwen. De voorbeelden in dit artikel worden het gebruik van dit nieuwe beleid door productie snelheidsbeperking-sleutels met client-IP-adressen, gebruikers-id en waarden van de client gegenereerd. Er zijn echter veel andere onderdelen van het bericht dat kan worden gebruikt zoals gebruikersagent, fragmenten voor URL-pad, de grootte van het bericht.

## <a name="next-steps"></a>Volgende stappen
Geef ons uw feedback in de Disqus-thread voor dit onderwerp. Het normaal zou zijn ideaal voor informatie over andere mogelijke sleutelwaarden die een logische keuze in uw scenario's zijn gekomen.

## <a name="watch-a-video-overview-of-these-policies"></a>Bekijk een video-overzicht van deze beleidsregels
Voor meer informatie over de [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) en [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) beleidsregels die in dit artikel, bekijk Neem de volgende video.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Advanced-Request-Throttling-with-Azure-API-Management/player]
> 
> 

