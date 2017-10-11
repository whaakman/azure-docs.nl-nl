---
title: Beleid in Azure API Management | Microsoft Docs
description: Informatie over het maken, bewerken en configureren van beleid in API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7c1f235343074ec11c635097f2b094a10f3fe781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="policies-in-azure-api-management"></a>Beleid in Azure API Management
In Azure API Management zijn-beleidsregels een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of antwoord van een API. Populaire instructies omvatten Indelingsconversie van XML in JSON en snelheidsbeperking als u wilt beperken, de hoeveelheid inkomende aanroepen van een ontwikkelaar aanroepen. Veel meer beleidsregels zijn gebruiksklaar beschikbaar.

Zie de [beleidsverwijzing] [ Policy Reference] voor een volledige lijst met beleidsverklaringen en de bijbehorende instellingen.

Beleidsregels worden toegepast binnen de gateway die de consument API en de beheerde API tussen. De gateway ontvangt van alle aanvragen en meestal doorsturen ervan naar de onderliggende API ongewijzigd. Een beleid kan echter wijzigingen toepassen op zowel de binnenkomende aanvraag en het uitgaande antwoord.

Beleidsexpressies kunnen worden gebruikt als kenmerkwaarden of tekstwaarden in API Management-beleidsregels, tenzij het beleid iets anders aangeeft. Sommige beleidsregels, zoals de [transportbesturing] [ Control flow] en [variabele instellen] [ Set variable] beleid is gebaseerd op beleidsexpressies. Zie voor meer informatie [Geavanceerde beleidsregels] [ Advanced policies] en [beleidsexpressies][Policy expressions].

## <a name="scopes"></a>Beleid configureren
Beleid kan worden geconfigureerd globaal of in het bereik van een [Product][Product], [API] [ API] of [bewerking][Operation]. Een beleid wilt configureren, gaat u naar de beleid-editor in de publicatieportal.

![Menu beleid][policies-menu]

De editor beleid bestaat uit drie gedeelten: het beleidsbereik (boven), de beleidsdefinitie waarbij beleid (links) worden bewerkt en de instructies lijst (rechts):

![Beleid-editor][policies-editor]

U moet het bereik waarvoor het beleid moet toepassen selecteren om te beginnen met een beleid te configureren. In de onderstaande schermafbeelding de **Starter** product is geselecteerd. Houd er rekening mee dat het vierkante symbool naast de naam van het beleid geeft een beleid is al toegepast op dit niveau.

![Bereik][policies-scope]

Omdat er al een beleid is toegepast, wordt de configuratie wordt weergegeven in de weergave van de definitie.

![Configureren][policies-configure]

Het beleid wordt weergegeven in de alleen-lezen in eerste instantie. Om te bewerken van het definitie klikt u op de **beleid configureren** in te grijpen.

![Bewerken][policies-edit]

De beleidsdefinitie is een eenvoudige XML-document dat wordt een reeks binnenkomend en uitgaand instructies beschreven. Het XML-bestand kan worden bewerkt rechtstreeks in het definitievenster. Een lijst van de instructies vindt u aan de rechterkant en instructies van toepassing op het huidige bereik zijn ingeschakeld en geselecteerd. zoals blijkt uit de **limiet aanroepen snelheid** -instructie in de bovenstaande schermafbeelding.

Te klikken op een instructie ingeschakeld, wordt de juiste XML op de locatie van de cursor in de weergave definitie toevoegen. 

> [!NOTE]
> Als het beleid dat u wilt toevoegen is niet ingeschakeld, zorg ervoor dat u in het juiste bereik voor dat beleid. Elke instructie beleid is ontworpen voor gebruik in bepaalde scopes en beleid-secties. Als u wilt controleren in de secties van beleid en de bereiken voor een beleid, Controleer de **gebruik** sectie voor dat beleid in de [beleidsverwijzing][Policy Reference].
> 
> 

Een volledige lijst met beleidsverklaringen en hun instellingen zijn beschikbaar in de [beleidsverwijzing][Policy Reference].

Bijvoorbeeld, als u wilt toevoegen van een nieuwe statusverklaring om te beperken van binnenkomende aanvragen tot het opgegeven IP-adressen, plaats de cursor net binnen de inhoud van de `inbound` XML-element en klik op de **beperken aanroeper IP-adressen** instructie.

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

![Opslaan][policies-save]

Als u klaar voor het configureren van de instructies voor het beleid, klikt u op **opslaan** en de wijzigingen wordt doorgegeven aan de API Management-gateway van onmiddellijk.

## <a name="sections"></a>Understanding beleidsconfiguratie
Een beleid is een reeks instructies die worden uitgevoerd om een antwoord en aanvragen. De configuratie op de juiste wijze is onderverdeeld in `inbound`, `backend`, `outbound`, en `on-error` zoals weergegeven in de configuratie van de volgende secties.

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

Aangezien beleidsregels kunnen worden opgegeven op verschillende niveaus (globale, product, api en bewerking) biedt de configuratie een manier om de volgorde waarin de beleidsdefinitie-instructies met betrekking tot het beleid van de bovenliggende uitvoeren opgeven. 

Beleid scopes worden geëvalueerd in de volgende volgorde.

1. Globaal bereik
2. Product-bereik
3. API-bereik
4. Bewerkingsbereik

De instructies in deze worden geëvalueerd op basis van de plaatsing van de `base` element, indien aanwezig. Globaal beleid heeft geen bovenliggend element beleid en het gebruik van de `<base>` -element in het heeft geen effect.

Bijvoorbeeld, als u een beleid op het globale niveau en een beleid dat is geconfigureerd voor een API hebt, klikt u vervolgens wanneer die bepaalde API wordt gebruikt beide beleidsregels gelden. API Management kunt u deterministische ordening van gecombineerde beleidsverklaringen via het base-element. 

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

Klik op een overzicht van het beleid in het huidige bereik in de beleidseditor **herberekenen effectief beleid voor de geselecteerde scope**.

## <a name="next-steps"></a>Volgende stappen
Bekijk na video op beleidsexpressies.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
