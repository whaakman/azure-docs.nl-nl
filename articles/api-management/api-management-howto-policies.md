---
title: Beleid in Azure API Management | Microsoft Docs
description: Informatie over het maken, bewerken en configureren van beleid in API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: 99f756b5415811b3d4c2ee0167f98b31c905df1a
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793647"
---
# <a name="policies-in-azure-api-management"></a>Beleid in Azure API Management

In Azure API Management (APIM) zijn beleid een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of antwoord van een API. Populaire instructies omvatten Indelingsconversie van XML in JSON en aanroepfrequentielimiet-als u wilt beperken van de hoeveelheid inkomende aanroepen van een ontwikkelaar te beperken. Veel meer beleidsregels zijn gebruiksklaar beschikbaar.

Beleid wordt toegepast in de gateway die de consument API en de beheerde API kunnen uitwisselen. De gateway ontvangt alle aanvragen en normaal gesproken doorgestuurd naar de onderliggende API ongewijzigd. Een beleid kunt wel wijzigingen toepassen op zowel de inkomende aanvraag en het uitgaande antwoord.

Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de [controlestroom] [ Control flow] en [variabele instellen] [ Set variable] beleidsregels zijn gebaseerd op beleidsexpressies. Zie voor meer informatie, [Geavanceerde beleidsregels] [ Advanced policies] en [beleidsexpressies][Policy expressions].

## <a name="sections"> </a>Understanding beleid configureren

De beleidsdefinitie is een eenvoudig XML-document dat een reeks instructies voor binnenkomend en uitgaand beschrijft. Het XML-bestand kan worden bewerkt rechtstreeks in het definitievenster. Een lijst van de instructies wordt verstrekt aan de rechterkant en instructies van toepassing op het huidige bereik zijn ingeschakeld en is gemarkeerd.

Te klikken op een instructie ingeschakeld, wordt het juiste XML-bestand op de locatie van de cursor in het definitie-weergave toevoegen. 

> [!NOTE]
> Als het beleid dat u wilt toevoegen, is niet ingeschakeld, zorg ervoor dat u in het juiste bereik voor dat beleid. Elke instructie beleid is bedoeld voor gebruik in bepaalde bereiken en beleid voor secties. Als u wilt controleren in de secties van beleid en bereiken voor een beleid, Controleer de **gebruik** sectie voor dat beleid in de [Policy Reference][Policy Reference].
> 
> 

De configuratie is onderverdeeld in `inbound`, `backend`, `outbound`, en `on-error`. De reeks opgegeven beleidsverklaringen is wordt uitgevoerd in de order voor een aanvraag en een antwoord.

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

Als er een fout opgetreden tijdens het verwerken van een aanvraag, alle overige stappen in de `inbound`, `backend`, of `outbound` secties worden overgeslagen en uitvoering gaat u naar de instructies in de `on-error` sectie. Door beleidsinstructies in de `on-error` sectie kunt u de fout bekijken met behulp van de `context.LastError` eigenschap, controleren en aanpassen van de fout antwoord via de `set-body` beleid, en configureert u wat er gebeurt als er een fout optreedt. Er zijn foutcodes voor ingebouwde stappen en fouten die tijdens de verwerking van voor beleidsinstructies optreden. Zie voor meer informatie, [foutafhandeling in API Management-beleidsregels](/azure/api-management/api-management-error-handling-policies).

## <a name="scopes"> </a>Beleidsregels configureren

Zie voor meer informatie over het configureren van beleid [instellen of bewerken van beleid](set-edit-policies.md).

## <a name="policy-reference"></a>Naslaginformatie over beleid

Zie de [beleidsverwijzing](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen.

## <a name="policy-samples"></a>Voorbeelden van beleid

Zie [beleid voorbeelden](policy-samples.md) voor meer voorbeelden van code.

## <a name="examples"></a>Voorbeelden

### <a name="apply-policies-specified-at-different-scopes"></a>Beleidsregels die zijn opgegeven in verschillende bereiken toepassen

Als u een beleid op globaal niveau en een beleid is geconfigureerd voor een API hebt, klikt u vervolgens wanneer die bepaalde API wordt gebruikt beide beleidsregels toegepast. API Management kunnen voor het bestellen van deterministische van gecombineerde beleidsverklaringen via het base-element. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

In het bovenstaande voorbeeld beleidsdefinitie de `cross-domain` instructie uitgevoerd voordat een hogere beleid die op zijn beurt worden gevolgd door de `find-and-replace` beleid. 

### <a name="restrict-incoming-requests"></a>Beperken van binnenkomende aanvragen

Om toe te voegen een nieuwe instructie uit om te beperken van binnenkomende aanvragen naar de opgegeven IP-adressen, plaats de cursor alleen in de inhoud van de `inbound` XML-element en klikt u op de **beperken oproepende functie IP-adressen** instructie.

![Beleid voor softwarebeperking][policies-restrict]

Hiermee voegt u een XML-fragment voor de `inbound` bevat richtlijnen over het configureren van de instructie element.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Beperken van binnenkomende aanvragen en accepteert alleen die vanaf een IP-adres van de 1.2.3.4 het XML-bestand als volgt wijzigen:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie werken met beleidsregels voor:

+ [API's transformeren](transform-api.md)
+ [Naslaginformatie over beleid voor](api-management-policy-reference.md) voor een volledige lijst van beleidsverklaringen en de bijbehorende instellingen
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
