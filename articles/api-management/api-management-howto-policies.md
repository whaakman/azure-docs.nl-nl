---
title: Beleid in Azure API Management | Microsoft Docs
description: Informatie over het maken, bewerken en configureren van beleid in API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: 8576d590568a24df4c3320ec283edba7a28ab032
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="policies-in-azure-api-management"></a>Beleid in Azure API Management

In Azure API Management (APIM) zijn-beleidsregels een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of antwoord van een API. Populaire instructies omvatten Indelingsconversie van XML in JSON en snelheidsbeperking als u wilt beperken, de hoeveelheid inkomende aanroepen van een ontwikkelaar aanroepen. Veel meer beleidsregels zijn gebruiksklaar beschikbaar.

Beleidsregels worden toegepast binnen de gateway die de consument API en de beheerde API tussen. De gateway ontvangt van alle aanvragen en meestal doorsturen ervan naar de onderliggende API ongewijzigd. Een beleid kan echter wijzigingen toepassen op zowel de binnenkomende aanvraag en het uitgaande antwoord.

Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de [transportbesturing] [ Control flow] en [variabele instellen] [ Set variable] beleid is gebaseerd op beleidsexpressies. Zie voor meer informatie [Geavanceerde beleidsregels] [ Advanced policies] en [beleidsexpressies][Policy expressions].

## <a name="sections"></a>Understanding beleidsconfiguratie

De beleidsdefinitie is een eenvoudige XML-document dat wordt een reeks binnenkomend en uitgaand instructies beschreven. Het XML-bestand kan worden bewerkt rechtstreeks in het definitievenster. Een lijst van de instructies vindt u aan de rechterkant en instructies van toepassing op het huidige bereik zijn ingeschakeld en gemarkeerd.

Te klikken op een instructie ingeschakeld, wordt de juiste XML op de locatie van de cursor in de weergave definitie toevoegen. 

> [!NOTE]
> Als het beleid dat u wilt toevoegen is niet ingeschakeld, zorg ervoor dat u in het juiste bereik voor dat beleid. Elke instructie beleid is ontworpen voor gebruik in bepaalde scopes en beleid-secties. Als u wilt controleren in de secties van beleid en de bereiken voor een beleid, Controleer de **gebruik** sectie voor dat beleid in de [beleidsverwijzing][Policy Reference].
> 
> 

De configuratie is onderverdeeld in `inbound`, `backend`, `outbound`, en `on-error`. De reeks beleidsverklaringen opgegeven is in de volgorde voor een aanvraag en een antwoord wordt uitgevoerd.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Als er een fout opgetreden tijdens het verwerken van een aanvraag, alle overige stappen in de `inbound`, `backend`, of `outbound` secties worden overgeslagen en uitvoering gaat u naar de instructies in de `on-error` sectie. Door het plaatsen van beleidsverklaringen in de `on-error` sectie kunt u de fout bekijken met behulp van de `context.LastError` eigenschap, controleren en aanpassen van de fout antwoord via de `set-body` -beleid, en configureer wat er gebeurt als een fout optreedt. Er zijn foutcodes voor ingebouwde stappen en voor fouten die tijdens de verwerking van beleid voor instructies optreden. Zie voor meer informatie [foutafhandeling in API Management-beleidsregels](https://msdn.microsoft.com/library/azure/mt629506.aspx).

## <a name="scopes"></a>Beleid configureren

Zie voor meer informatie over het configureren van beleid [instellen of bewerken van beleid](set-edit-policies.md).

## <a name="policy-reference"></a>Naslaginformatie over beleid

Zie de [beleidsverwijzing](api-management-policy-reference.md) voor een volledige lijst met beleidsverklaringen en de bijbehorende instellingen.

## <a name="policy-samples"></a>Voorbeelden van beleid

Zie [beleid voorbeelden](policy-samples.md) voor meer voorbeelden van de programmacode.

## <a name="examples"></a>Voorbeelden

### <a name="apply-policies-specified-at-different-scopes"></a>Beleidsregels die zijn opgegeven bij verschillende bereiken

Als u een beleid op het globale niveau en een beleid dat is geconfigureerd voor een API hebt, wordt klikt u vervolgens wanneer die bepaalde API wordt gebruikt beide beleidsregels toegepast. API Management kunt u deterministische ordening van gecombineerde beleidsverklaringen via het base-element. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

In het bovenstaande voorbeeld beleidsdefinitie de `cross-domain` instructie zou worden uitgevoerd voordat een hogere beleid die op zijn beurt worden gevolgd door de `find-and-replace` beleid. 

### <a name="restrict-incoming-requests"></a>Beperken van binnenkomende aanvragen

Als u wilt toevoegen van een nieuwe statusverklaring om te beperken van binnenkomende aanvragen tot het opgegeven IP-adressen, plaats de cursor net binnen de inhoud van de `inbound` XML-element en klik op de **beperken aanroeper IP-adressen** instructie.

![Beleid voor softwarebeperking][policies-restrict]

Hiermee voegt u toe een XML-fragment naar de `inbound` element dat biedt richtlijnen voor het configureren van de instructie.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Beperken van binnenkomende aanvragen en accepteert alleen die uit een IP-adres van 1.2.3.4 het XML-bestand als volgt wijzigen:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, werken met beleid:

+ [Transformeren API 's](transform-api.md)
+ [Naslaginformatie over beleid](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en hun instellingen
+ [Voorbeelden van beleid](policy-samples.md)   

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
