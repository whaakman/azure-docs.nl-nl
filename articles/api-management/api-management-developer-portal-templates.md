---
title: De ontwikkelaarsportal van API Management-sjablonen aanpassen-Azure | Microsoft Docs
description: Informatie over het aanpassen van de Azure API Management portal voor ontwikkelaars met behulp van sjablonen.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 40d25726d31d2018785b77d169a8811c565316bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Het aanpassen van de Azure API Management portal voor ontwikkelaars met behulp van sjablonen

Er zijn die manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:

* [De inhoud van statische pagina's en pagina-indelingselementen bewerken][modify-content-layout]
* [De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal][customize-styles]
* [Wijzigen van de sjablonen voor pagina's die worden gegenereerd door de portal gebruikt] [ portal-templates] (uitgelegd in deze handleiding)

Sjablonen worden gebruikt voor het aanpassen van de inhoud van het systeem gegenereerde developer portal-pagina's (bijvoorbeeld API docs, producten, gebruikersverificatie, enz.). Met behulp van [DotLiquid](http://dotliquidmarkup.org/) syntaxis en een opgegeven set bronnen met gelokaliseerde tekenreeksen, pictogrammen en paginabesturingselementen, hebt u aanzienlijke flexibiliteit voor het configureren van de inhoud van de pagina's wens naar.

## <a name="developer-portal-templates-overview"></a>Overzicht van Developer portal-sjablonen
Het bewerken van sjablonen wordt uitgevoerd van de **ontwikkelaarsportal** tijdens wordt aangemeld als beheerder. Wilt u er eerst open de Azure-Portal en klik op **publicatieportal** werkbalk van de service van uw exemplaar van API Management.

![Publicatieportal][api-management-management-console]

Klik daarna in de rechterbovenhoek op **Ontwikkelaarsportal**. 

![Menu Developer-portal][api-management-developer-portal-menu]

Voor toegang tot de portal developer-sjablonen, klikt u op het pictogram aanpassen aan de linkerkant om het menu aanpassing weergeven en klik op **sjablonen**.

![Developer portal-sjablonen][api-management-customize-menu]

De lijst met sjablonen worden verschillende categorieën van sjablonen die betrekking hebben op de andere pagina's in de portal voor ontwikkelaars. Elke sjabloon verschilt, maar de stappen voor deze bewerken en publiceren van de wijzigingen zijn hetzelfde. Als u wilt een sjabloon te bewerken, klikt u op de naam van de sjabloon.

![Developer portal-sjablonen][api-management-templates-menu]

Een sjabloon te klikken gaat u naar de developer portal pagina die kan worden aangepast door dat de sjabloon. In dit voorbeeld de **productlijst** sjabloon wordt weergegeven. De **productlijst** sjabloon bepaalt het gebied van het scherm aangegeven door de rode rechthoek. 

![De sjabloon lijst met producten][api-management-developer-portal-templates-overview]

Sommige sjablonen, zoals de **gebruikersprofiel** sjablonen, verschillende onderdelen van dezelfde pagina aanpassen. 

![Gebruiker-profielsjablonen][api-management-user-profile-templates]

De editor voor elke developer portal-sjabloon bevat twee secties aan de onderkant van de pagina weergegeven. De linkerkant geeft het deelvenster bewerken voor de sjabloon en de rechterkant weergegeven in het gegevensmodel voor de sjabloon. 

De sjabloon bewerken deelvenster bevat de opmaak die het uiterlijk en gedrag van de bijbehorende pagina in de portal voor ontwikkelaars bepaalt. De opmaak van de sjabloon maakt gebruik van de [DotLiquid](http://dotliquidmarkup.org/) syntaxis. Is een populair editor voor DotLiquid [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Wijzigingen in de sjabloon tijdens het bewerken van in realtime worden weergegeven in de browser, maar zijn niet zichtbaar voor uw klanten totdat u [opslaan](#to-save-a-template) en [publiceren](#to-publish-a-template) de sjabloon.

![Sjabloon aantekeningen][api-management-template]

De **sjabloongegevens** deelvenster biedt een handleiding voor het gegevensmodel voor de entiteiten die beschikbaar voor gebruik in een bepaalde sjabloon zijn. Deze handleiding biedt door de dynamische gegevens die momenteel worden weergegeven in de portal voor ontwikkelaars weer te geven. U kunt de sjabloon deelvensters uitbreiden door te klikken op de rechthoek in de rechterbovenhoek van de **sjabloongegevens** deelvenster.

![Sjabloon-gegevensmodel][api-management-template-data]

In het vorige voorbeeld zijn er twee producten weergegeven in de portal voor ontwikkelaars die zijn opgehaald van de gegevens die worden weergegeven de **sjabloongegevens** deelvenster, zoals wordt weergegeven in het volgende voorbeeld.

```json
{
    "Paging": {
        "Page": 1,
        "PageSize": 10,
        "TotalItemCount": 2,
        "ShowAll": false,
        "PageCount": 1
    },
    "Filtering": {
        "Pattern": null,
        "Placeholder": "Search products"
    },
    "Products": [
        {
            "Id": "56ec64c380ed850042060001",
            "Title": "Starter",
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
            "Terms": "",
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        },
        {
            "Id": "56ec64c380ed850042060002",
            "Title": "Unlimited",
            "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
            "Terms": null,
            "ProductState": 1,
            "AllowMultipleSubscriptions": false,
            "MultipleSubscriptionsCount": 1
        }
    ]
}
```

De opmaak van de **productlijst** sjabloon verwerkt de gegevens voor de gewenste uitvoer door de verzameling van producten weer te geven informatie en een koppeling voor elk afzonderlijk product doorloopt. Opmerking de `<search-control>` en `<page-control>` elementen in de opmaak. Deze bepalen of het zoeken en besturingselementen op de pagina van het wisselbestand wordt weergegeven. `ProductsStrings|PageTitleProducts`is een verwijzing gelokaliseerde tekenreeks met de `h2` koptekst voor de pagina. Zie voor een lijst met tekenreeksbronnen, paginabesturingselementen en pictogrammen beschikbaar voor gebruik in developer portal-sjablonen, [API Management-referentie voor ontwikkelaars portal sjablonen](api-management-developer-portal-templates-reference.md).

```html
<search-control></search-control>
<div class="row">
    <div class="col-md-9">
        <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
    </div>
</div>
<div class="row">
    <div class="col-md-12">
    {% if products.size > 0 %}
    <ul class="list-unstyled">
    {% for product in products %}
        <li>
            <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
            {{product.description}}
        </li>    
    {% endfor %}
    </ul>
    <paging-control></paging-control>
    {% else %}
    {% localized "CommonResources|NoItemsToDisplay" %}
    {% endif %}
    </div>
</div>
```

## <a name="to-save-a-template"></a>Een sjabloon wordt opgeslagen
Als u wilt een sjabloon opslaan, klikt u op opslaan in de sjablooneditor.

![Sjabloon opslaan][api-management-save-template]

Opgeslagen wijzigingen zijn niet in de portal voor ontwikkelaars live totdat ze worden gepubliceerd.

## <a name="to-publish-a-template"></a>Voor het publiceren van een sjabloon
Opgeslagen sjablonen kunnen worden gepubliceerd, afzonderlijk of Alles samenvoegen. Klik op publiceren voor het publiceren van een afzonderlijke sjabloon in de sjablooneditor.

![Sjabloon publiceren][api-management-publish-template]

Klik op **Ja** om te bevestigen en de sjabloon maken live op de portal voor ontwikkelaars.

![Bevestig publiceren][api-management-publish-template-confirm]

Voor het publiceren van alle sjabloonversies van de momenteel niet-gepubliceerde, klikt u op **publiceren** in de lijst met sjablonen. Niet-gepubliceerde sjablonen worden aangewezen door een sterretje achter de sjabloonnaam. In dit voorbeeld wordt de **productlijst** en **Product** sjablonen worden gepubliceerd.

![Sjablonen publiceren][api-management-publish-templates]

Klik op **aanpassingen publiceert** om te bevestigen.

![Bevestig publiceren][api-management-publish-customizations]

Nieuw gepubliceerde sjablonen zijn effectieve direct in de portal voor ontwikkelaars.

## <a name="to-revert-a-template-to-the-previous-version"></a>Een sjabloon met de vorige versie herstellen
Als u wilt terugkeren naar de vorige versie van de gepubliceerde sjabloon, klik op herstellen in de sjablooneditor.

![Sjabloon herstellen][api-management-revert-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-revert-template-confirm]

De eerder gepubliceerde versie van een sjabloon wordt in de portal voor ontwikkelaars live zodra de herstelbewerking voltooid is.

## <a name="to-restore-a-template-to-the-default-version"></a>Een sjabloon naar de standaardversie herstellen
Herstellen van de sjablonen naar hun standaardversie is een proces. Eerst de sjablonen moeten worden hersteld en vervolgens de herstelde versies moeten worden gepubliceerd.

Klik op herstellen in de sjablooneditor te herstellen één sjabloon voor de standaardversie.

![Sjabloon herstellen][api-management-reset-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-reset-template-confirm]

Voor het herstellen van alle sjablonen voor de versies van hun standaard, klikt u op **herstellen standaardsjablonen** op de lijst met sjablonen.

![Sjablonen herstellen][api-management-restore-templates]

De herstelde sjablonen moeten vervolgens worden gepubliceerd afzonderlijk of in één keer door de stappen in [voor het publiceren van een sjabloon](#to-publish-a-template).

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over developer portal-sjablonen, tekenreeksbronnen pictogrammen en paginabesturingselementen, [API Management-referentie voor ontwikkelaars portal sjablonen](api-management-developer-portal-templates-reference.md).

[modify-content-layout]: api-management-modify-content-layout.md
[customize-styles]: api-management-customize-styles.md
[portal-templates]: api-management-developer-portal-templates.md

[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-management-console]: ./media/api-management-developer-portal-templates/api-management-management-console.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







