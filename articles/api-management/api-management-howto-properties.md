---
title: Het gebruik van eigenschappen in Azure API Management-beleidsregels
description: Informatie over het gebruik van eigenschappen in Azure API Management-beleidsregels.
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
ms.date: 01/25/2018
ms.author: apimpm
ms.openlocfilehash: e0559380f6d686a4e559779c4271ea85106558d6
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
ms.locfileid: "28197109"
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Het gebruik van eigenschappen in Azure API Management-beleidsregels
API Management-beleidsregels zijn een krachtige mogelijkheid van het systeem waarmee de Azure-portal om het gedrag van de API via configuratie te wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of het antwoord van een API. Beleidsverklaringen kunnen worden samengesteld met behulp van letterlijke tekstwaarden, beleidsexpressies en eigenschappen. 

Elk exemplaar van API Management-service heeft een verzameling eigenschappen van sleutel-waardeparen die algemeen in het service-exemplaar zijn. Deze eigenschappen kunnen worden gebruikt voor het beheren van constante string-waarden voor alle configuratie-API en beleidsregels. Elke eigenschap kan hebben de volgende kenmerken:

| Kenmerk | Type | Beschrijving |
| --- | --- | --- |
| Weergavenaam |tekenreeks |Een alfanumerieke tekenreeks die wordt gebruikt om naar de eigenschap te verwijzen in de beleidsregels. |
| Waarde |tekenreeks |De waarde van de eigenschap. Het is niet leeg zijn of alleen witruimte bevatten. |
|Geheim|booleaans|Bepaalt of de waarde een geheim is en moet worden versleuteld of niet.|
| Tags |Matrix van tekenreeks |Optioneel tags die de opgegeven voor het filteren van de lijst met eigenschappen kunnen worden gebruikt. |

![Benoemde waarden](./media/api-management-howto-properties/named-values.png)

Eigenschapswaarden letterlijke tekenreeksen kunnen bevatten en [beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx). De waarde van bijvoorbeeld `ExpressionProperty` is een beleidsexpressie die retourneert een tekenreeks met de huidige datum en tijd. De eigenschap `ContosoHeaderValue` is gemarkeerd als een geheim, zodat de waarde wordt niet weergegeven.

| Naam | Waarde | Geheim | Tags |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-add-and-edit-a-property"></a>Toe te voegen en een eigenschap bewerken

![Een eigenschap toevoegen](./media/api-management-howto-properties/add-property.png)

1. Selecteer **API's** uit onder **API MANAGEMENT**.
2. Selecteer **benoemde waarden**.
3. Druk op **+ toevoegen**.

  Naam en waarde zijn vereiste waarden. Als de waarde van deze eigenschap een geheim is, controleert u dat de dit is een geheime selectievakje. Voer een of meer optionele labels om te helpen bij het ordenen van uw eigenschappen en klikt u op opslaan.
4. Klik op **Create**.

Als de eigenschap is gemaakt, kunt u deze bewerken door te klikken op de eigenschap. Als u de naam van de eigenschap wijzigt, worden alle beleidsregels die verwijzen naar die eigenschap automatisch bijgewerkt voor het gebruik van de nieuwe naam.

Zie voor meer informatie over het bewerken van een eigenschap met de REST API [bewerken van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Een eigenschap te wijzigen

Als u wilt verwijderen van een eigenschap, klikt u op **verwijderen** naast de eigenschap te verwijderen.

> [!IMPORTANT]
> Als de eigenschap wordt verwezen door andere beleidsregels, kunt u zich niet met succes te verwijderen als u de eigenschap verwijdert uit alle beleidsregels die worden gebruikt.
> 
> 

Zie voor meer informatie over het verwijderen van een eigenschap met de REST API [verwijderen van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Om te zoeken en filteren eigenschappen

De **benoemde waarden** tabblad Zoeken en filteren van mogelijkheden voor het beheren van uw eigenschappen bevat. De eigenschappenlijst filteren door de naam van eigenschap voeren een zoekterm in het **eigenschap zoeken** textbox. Als u wilt weergeven van alle eigenschappen, schakelt u de **eigenschap zoeken** textbox en druk op enter.

Voer voor de eigenschappenlijst filteren op labelwaarden, een of meer labels in de **filteren op labels** textbox. Als u wilt weergeven van alle eigenschappen, schakelt u de **filteren op labels** textbox en druk op enter.

## <a name="to-use-a-property"></a>Als een eigenschap wilt gebruiken

Als u een eigenschap in een beleid, plaatst u de naam van de eigenschap binnen een paar dubbele accolades zoals `{{ContosoHeader}}`, zoals wordt weergegeven in het volgende voorbeeld:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

In dit voorbeeld `ContosoHeader` wordt gebruikt als de naam van een kop in een `set-header` beleid, en `ContosoHeaderValue` wordt gebruikt als de waarde van deze header. Wanneer dit beleid wordt beoordeeld tijdens een aanvraag of antwoord naar de API Management-gateway `{{ContosoHeader}}` en `{{ContosoHeaderValue}}` worden vervangen door hun respectieve eigenschapswaarden.

Eigenschappen kunnen worden gebruikt als volledige kenmerk of elementwaarden zoals weergegeven in het vorige voorbeeld, maar ze kunnen ook worden ingevoegd in of in combinatie met het onderdeel van een expressie letterlijke tekst, zoals wordt weergegeven in het volgende voorbeeld:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Eigenschappen bevatten ook beleidsexpressies. In het volgende voorbeeld wordt de `ExpressionProperty` wordt gebruikt.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Wanneer dit beleid wordt beoordeeld `{{ExpressionProperty}}` is vervangen door de waarde ervan: `@(DateTime.Now.ToString())`. Omdat de waarde een beleidsexpressie is, wordt de expressie wordt geëvalueerd en wordt het beleid wordt doorgegaan met de uitvoering.

U kunt deze functionaliteit in de portal voor ontwikkelaars testen door het aanroepen van een bewerking die een beleid met eigenschappen in het bereik heeft. In het volgende voorbeeld wordt een bewerking wordt aangeroepen met de twee vorige voorbeeld `set-header` beleid met eigenschappen. Houd er rekening mee dat het antwoord bevat twee aangepaste headers die zijn geconfigureerd met beleid van eigenschappen.

![ontwikkelaarsportal][api-management-send-results]

Als u naar kijkt de [API Inspector trace](api-management-howto-api-inspector.md) voor een aanroep met de twee vorige voorbeeld beleidsregels met eigenschappen, ziet u de twee `set-header` beleid met de waarden van de eigenschap ingevoegd en de evaluatie van de beleid-expressie voor de eigenschap die deel uitmaakt van de beleidsexpressie.

![API-Inspector tracering][api-management-api-inspector-trace]

Terwijl eigenschapswaarden beleidsexpressies bevatten kunnen, mag geen eigenschapswaarden andere eigenschappen bevatten. Als tekst met een eigenschapverwijzing wordt gebruikt voor de waarde van een eigenschap, zoals `Property value text {{MyProperty}}`, die eigenschapverwijzing won't worden vervangen en worden opgenomen als onderdeel van de waarde van eigenschap.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het werken met beleid
  * [Beleidsregels in API Management](api-management-howto-policies.md)
  * [Naslaginformatie over beleidsregels](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

