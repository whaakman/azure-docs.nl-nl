---
title: Over het gebruik van waarden met de naam in Azure API Management-beleidsregels
description: Informatie over het gebruik van waarden met de naam in Azure API Management-beleidsregels.
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
ms.openlocfilehash: 829d6bc6cb3f8e78d065d7aaca4937634e7349c8
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437062"
---
# <a name="how-to-use-named-values-in-azure-api-management-policies"></a>Over het gebruik van waarden met de naam in Azure API Management-beleidsregels
API Management-beleidsregels zijn een krachtige mogelijkheid van het systeem waarmee de Azure-portal om het gedrag van de API via configuratie te wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of het antwoord van een API. Beleidsinstructies kunnen worden samengesteld met behulp van letterlijke waarden, beleidsexpressies, en met de naam waarden. 

Elk exemplaar van API Management-service heeft een verzameling eigenschappen van sleutel/waarde-paren, die wordt aangeroepen met de naam waarden, die zijn van toepassing op het service-exemplaar. Deze waarden met de naam kan worden gebruikt voor het beheren van constante tekenreekswaarden met alle configuratie-API en beleidsregels. Elke eigenschap kan de volgende kenmerken hebben:

| Kenmerk | Type | Beschrijving |
| --- | --- | --- |
| Weergavenaam |tekenreeks |Een alfanumerieke tekenreeks die wordt gebruikt om naar de eigenschap te verwijzen in de beleidsregels. |
| Waarde |tekenreeks |De waarde van de eigenschap. Het kan niet leeg zijn of alleen uit witruimte bestaan. |
|Geheim|booleaans|Bepaalt of de waarde is van een geheim en moet worden versleuteld of niet.|
| Tags |matrix van tekenreeks |Optioneel tags die de opgegeven kan worden gebruikt voor het filteren van de lijst met eigenschappen. |

![Benoemde waarden](./media/api-management-howto-properties/named-values.png)

Eigenschapswaarden letterlijke tekenreeksen kunnen bevatten en [beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx). Bijvoorbeeld, de waarde van `ExpressionProperty` is een beleidsexpressie die retourneert een tekenreeks met de huidige datum en tijd. De eigenschap `ContosoHeaderValue` is gemarkeerd als een geheim, zodat de waarde wordt niet weergegeven.

| Naam | Waarde | Geheim | Tags |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-add-and-edit-a-property"></a>Toe te voegen en een eigenschap bewerken

![Een eigenschap toevoegen](./media/api-management-howto-properties/add-property.png)

1. Selecteer **API's** bij **API MANAGEMENT**.
2. Selecteer **benoemde waarden**.
3. Druk op **+ toevoegen**.

  Naam en waarde worden de vereiste waarden. Als de waarde van deze eigenschap een geheim is, controleert u dat de dit is een geheim selectievakje. Voer een of meer optionele labels om te helpen bij het organiseren van uw naamwaarden en klikt u op opslaan.
4. Klik op **Create**.

Als de eigenschap is gemaakt, kunt u deze bewerken door te klikken op de eigenschap. Als u de naam van de eigenschap wijzigt, worden alle beleidsregels die verwijzen naar die eigenschap automatisch bijgewerkt voor het gebruik van de nieuwe naam.

Zie voor meer informatie over het bewerken van een eigenschap met de REST API [bewerken van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Een eigenschap wilt verwijderen

Als u wilt verwijderen van een eigenschap, klikt u op **verwijderen** naast de eigenschap te verwijderen.

> [!IMPORTANT]
> Als de eigenschap wordt verwezen door een beleid, kunt u zich niet is verwijderd totdat u de eigenschap verwijderd uit alle beleidsregels die worden gebruikt.
> 
> 

Zie voor meer informatie over het verwijderen van een eigenschap met de REST API [verwijderen van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-named-values"></a>Om te zoeken en filteren met de naam waarden

De **benoemde waarden** tabblad Zoeken en filteren van mogelijkheden voor het beheren van uw benoemde waarden bevat. Voer een zoekterm in om te filteren de lijst met eigenschappen op de naam van eigenschap, de **zoeken naar de eigenschap** tekstvak. Als u wilt weergeven van alle naamwaarden, schakelt u de **zoeken naar de eigenschap** tekstvak in en druk op enter.

Als u wilt filteren de lijst met eigenschappen op tagwaarden, voert u een of meer labels in de **filteren op tags** tekstvak. Als u wilt weergeven van alle naamwaarden, schakelt u de **filteren op tags** tekstvak in en druk op enter.

## <a name="to-use-a-property"></a>Een eigenschap wilt gebruiken

Voor het gebruik van een eigenschap in een beleid, plaatst u de naam van de eigenschap binnen een paar dubbele accolades, zoals `{{ContosoHeader}}`, zoals wordt weergegeven in het volgende voorbeeld:

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

In dit voorbeeld `ContosoHeader` wordt gebruikt als de naam van een kop in een `set-header` -beleid en `ContosoHeaderValue` wordt gebruikt als de waarde van deze header. Als dit beleid wordt geëvalueerd tijdens een aanvraag of antwoord met de API Management-gateway, `{{ContosoHeader}}` en `{{ContosoHeaderValue}}` zijn vervangen door hun respectieve eigenschapswaarden.

Benoemde waarden kunnen worden gebruikt als volledige kenmerk of de waarden van het element zoals wordt weergegeven in het vorige voorbeeld, maar ze kunnen ook worden ingevoegd of gecombineerd met onderdeel van een expressie letterlijke tekst, zoals wordt weergegeven in het volgende voorbeeld: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Benoemde waarden kunnen ook beleidsexpressies bevatten. In het volgende voorbeeld wordt de `ExpressionProperty` wordt gebruikt.

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

Als dit beleid wordt geëvalueerd, `{{ExpressionProperty}}` wordt vervangen door de waarde ervan: `@(DateTime.Now.ToString())`. Omdat de waarde een beleidsexpressie voor een is, wordt de expressie wordt geëvalueerd en wordt het beleid wordt voortgezet met de uitvoering ervan.

Dit scenario kunt u testen in de portal voor ontwikkelaars door het aanroepen van een bewerking die een beleid met naamwaarden binnen het bereik heeft. In het volgende voorbeeld wordt een bewerking wordt aangeroepen met het vorige voorbeeld van de twee `set-header` beleidsregels met naamwaarden. Houd er rekening mee dat het antwoord bevat twee aangepaste kopteksten die zijn geconfigureerd met beleid van benoemde waarden.

![ontwikkelaarsportal][api-management-send-results]

Als u naar kijkt de [traceren met API Inspector](api-management-howto-api-inspector.md) voor een aanroep die de twee vorige voorbeeld beleidsregels met benoemde waarden bevat, ziet u de twee `set-header` beleid met de waarden van de eigenschap ingevoegd en de Groepsbeleid-expressie evaluatie voor de eigenschap die deel uitmaakt van de beleidsexpressie.

![Traceren met API Inspector][api-management-api-inspector-trace]

Terwijl eigenschapswaarden beleidsexpressies bevatten kunnen, mag geen eigenschapswaarden andere benoemde waarden bevatten. Als tekst met een eigenschapverwijzing wordt gebruikt voor de waarde van een eigenschap, zoals `Property value text {{MyProperty}}`, die eigenschapverwijzing wordt niet worden vervangen en worden opgenomen als onderdeel van de waarde van eigenschap.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het werken met beleidsregels
  * [Beleid in API Management](api-management-howto-policies.md)
  * [Naslaginformatie over beleidsregels](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)

[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

