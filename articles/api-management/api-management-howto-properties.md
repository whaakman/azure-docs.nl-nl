---
title: Het gebruik van eigenschappen in Azure API Management-beleidsregels
description: Informatie over het gebruik van eigenschappen in Azure API Management-beleidsregels.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 517f434c8f7fabc1402fb938d5ff5c733b86f2fd
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Het gebruik van eigenschappen in Azure API Management-beleidsregels
API Management-beleidsregels zijn een krachtige mogelijkheid van het systeem waarmee de uitgever het gedrag van de API via configuratie wijzigen. Beleidsregels zijn een verzameling instructies die sequentieel worden uitgevoerd op de aanvraag of het antwoord van een API. Beleidsverklaringen kunnen worden samengesteld met behulp van letterlijke tekstwaarden, beleidsexpressies en eigenschappen. 

Elk exemplaar van API Management-service heeft een verzameling eigenschappen van sleutel-waardeparen die algemeen in het service-exemplaar zijn. Deze eigenschappen kunnen worden gebruikt voor het beheren van constante string-waarden voor alle configuratie-API en beleidsregels. Elke eigenschap heeft de volgende kenmerken.

| Kenmerk | Type | Beschrijving |
| --- | --- | --- |
| Naam |Tekenreeks |De naam van de eigenschap. Mogelijk bevatten alleen letters, cijfers, periode, streepjes en onderstrepingstekens bevatten. |
| Waarde |Tekenreeks |De waarde van de eigenschap. Het is niet leeg zijn of alleen witruimte bevatten. |
| Geheim |Booleaanse waarde |Bepaalt of de waarde een geheim is en moet worden versleuteld of niet. |
| Tags |Matrix van tekenreeks |Optioneel tags die de opgegeven voor het filteren van de lijst met eigenschappen kunnen worden gebruikt. |

Eigenschappen zijn geconfigureerd in de publicatieportal op de **eigenschappen** tabblad. In het volgende voorbeeld worden drie eigenschappen geconfigureerd.

![Eigenschappen][api-management-properties]

Eigenschapswaarden letterlijke tekenreeksen kunnen bevatten en [beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx). De volgende tabel ziet u de vorige drie Voorbeeld-eigenschappen en hun kenmerken. De waarde van `ExpressionProperty` is een beleidsexpressie die retourneert een tekenreeks met de huidige datum en tijd. De eigenschap `ContosoHeaderValue` is gemarkeerd als een geheim, zodat de waarde wordt niet weergegeven.

| Naam | Waarde | Geheim | Tags |
| --- | --- | --- | --- |
| ContosoHeader |trackingId |False |Contoso |
| ContosoHeaderValue |•••••••••••••••••••••• |True |Contoso |
| ExpressionProperty |@(DateTime.Now.ToString()) |False | |

## <a name="to-use-a-property"></a>Als een eigenschap wilt gebruiken
Als u een eigenschap in een beleid, plaatst u de naam van de eigenschap binnen een paar dubbele accolades zoals `{{ContosoHeader}}`, zoals wordt weergegeven in het volgende voorbeeld.

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

Houd er rekening mee dat terwijl eigenschapswaarden beleidsexpressies bevatten kunnen, eigenschapswaarden kunnen geen andere eigenschappen bevatten. Als tekst met een eigenschapverwijzing wordt gebruikt voor de waarde van een eigenschap, zoals `Property value text {{MyProperty}}`, die eigenschapverwijzing won't worden vervangen en worden opgenomen als onderdeel van de waarde van eigenschap.

## <a name="to-create-a-property"></a>Een eigenschap maken
Klik op om een eigenschap **eigenschap toevoegen** op de **eigenschappen** tabblad.

![Eigenschap toevoegen][api-management-properties-add-property-menu]

**Naam** en **waarde** vereiste waarden zijn. Als de waarde van deze eigenschap een geheim is, controleert u de **dit is een geheim** selectievakje. Geef een of meer optionele labels om te helpen bij het ordenen van uw eigenschappen en op **opslaan**.

![Eigenschap toevoegen][api-management-properties-add-property]

Wanneer een nieuwe eigenschap wordt opgeslagen, de **eigenschap zoeken** textbox is gevuld met de naam van de nieuwe eigenschap en wordt de nieuwe eigenschap weergegeven. Als u wilt weergeven van alle eigenschappen, schakelt u de **eigenschap zoeken** textbox en druk op enter.

![Eigenschappen][api-management-properties-property-saved]

Zie voor meer informatie over het maken van een eigenschap met de REST API [maakt u een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Een eigenschap bewerken
Een eigenschap bewerken, klikt u op **bewerken** naast de eigenschap te bewerken.

![De eigenschap bewerken][api-management-properties-edit]

Breng de gewenste wijzigingen klik vervolgens op **opslaan**. Als u de naam van de eigenschap wijzigt, worden alle beleidsregels die verwijzen naar die eigenschap automatisch bijgewerkt voor het gebruik van de nieuwe naam.

![De eigenschap bewerken][api-management-properties-edit-property]

Zie voor meer informatie over het bewerken van een eigenschap met de REST API [bewerken van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Een eigenschap te wijzigen
Als u wilt verwijderen van een eigenschap, klikt u op **verwijderen** naast de eigenschap te verwijderen.

![Eigenschap verwijderen][api-management-properties-delete]

Klik op **Ja, deze verwijderen** om te bevestigen.

![De verwijdering bevestigen][api-management-delete-confirm]

> [!IMPORTANT]
> Als de eigenschap wordt verwezen door andere beleidsregels, kunt u zich niet met succes te verwijderen als u de eigenschap verwijdert uit alle beleidsregels die worden gebruikt.
> 
> 

Zie voor meer informatie over het verwijderen van een eigenschap met de REST API [verwijderen van een eigenschap met de REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Om te zoeken en filteren eigenschappen
De **eigenschappen** tabblad Zoeken en filteren van mogelijkheden voor het beheren van uw eigenschappen bevat. De eigenschappenlijst filteren door de naam van eigenschap voeren een zoekterm in het **eigenschap zoeken** textbox. Als u wilt weergeven van alle eigenschappen, schakelt u de **eigenschap zoeken** textbox en druk op enter.

![Search][api-management-properties-search]

Voer voor de eigenschappenlijst filteren op labelwaarden, een of meer labels in de **filteren op labels** textbox. Als u wilt weergeven van alle eigenschappen, schakelt u de **filteren op labels** textbox en druk op enter.

![Filteren][api-management-properties-filter]

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het werken met beleid
  * [Beleidsregels in API Management](api-management-howto-policies.md)
  * [Naslaginformatie over beleidsregels](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [Beleidsexpressies](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Bekijk een video-overzicht
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

