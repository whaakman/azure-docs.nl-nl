---
title: De API Management-ontwikkelaarsportal met behulp van sjablonen aanpassen-Azure | Microsoft Docs
description: Informatie over het aanpassen van de Azure API Management-ontwikkelaarsportal met behulp van sjablonen.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: a195675b-f7d0-4fc9-90bf-860e6f17ccf7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 00d5e3df78e85d19a519786dad1a1b176ad7fa08
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733056"
---
# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Over het aanpassen van de Azure API Management-ontwikkelaarsportal met behulp van sjablonen

Er zijn die manieren waarop u de ontwikkelaarsportal in Azure API Management kunt aanpassen:

* [De inhoud van statische pagina's en pagina-indelingselementen bewerken][modify-content-layout]
* [De stijlen bijwerken die worden gebruikt voor pagina-elementen in de ontwikkelaarsportal][customize-styles]
* [De sjablonen die worden gebruikt voor pagina's die worden gegenereerd door de portal bewerken] [ portal-templates] (uitgelegd in deze handleiding)

Sjablonen worden gebruikt voor het aanpassen van de inhoud van het systeem gegenereerde ontwikkelaarsportalpagina's (bijvoorbeeld API-documenten, producten, gebruikersverificatie, enz.). Met behulp van [DotLiquid](http://dotliquidmarkup.org/) syntaxis en een opgegeven set met resources van de gelokaliseerde tekenreeks, pictogrammen en paginabesturingselementen, hebt u geweldige flexibiliteit voor het configureren van de inhoud van de pagina's wens naar.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="developer-portal-templates-overview"></a>Overzicht van de Developer-portal sjablonen

Bewerken van sjablonen wordt gedaan via de **ontwikkelaarsportal** terwijl u aangemeld als beheerder. Wilt u er eerst open Azure Portal en klik op **ontwikkelaarsportal** vanaf de servicewerkbalk van uw API Management-exemplaar.

Voor toegang tot de sjablonen voor ontwikkelaarsportals, klikt u op het pictogram aanpassen aan de linkerkant om het menu aanpassing weergeven en klikt u op **sjablonen**.

![Sjablonen voor ontwikkelaarsportals][api-management-customize-menu]

De lijst met sjablonen worden verschillende categorieën met sjablonen die betrekking hebben op de verschillende pagina's in de portal voor ontwikkelaars. Elke sjabloon is anders, maar de stappen voor het bewerken en publiceren van de wijzigingen zijn hetzelfde. Als u wilt een sjabloon bewerken, klikt u op de naam van de sjabloon.

![Sjablonen voor ontwikkelaarsportals][api-management-templates-menu]

Een sjabloon op te klikken gaat u naar de developer portal-pagina die kan worden aangepast door deze sjabloon. In dit voorbeeld wordt de **lijst met producten** sjabloon wordt weergegeven. De **lijst met producten** sjabloon bepaalt het gebied van het scherm aangegeven door de rode rechthoek.

![De sjabloon lijst met producten][api-management-developer-portal-templates-overview]

Sommige sjablonen, zoals de **gebruikersprofiel** sjablonen, verschillende onderdelen van dezelfde pagina aanpassen.

![Gebruiker-profielsjablonen][api-management-user-profile-templates]

De editor voor elke ontwikkelaar sjabloon heeft twee secties weergegeven aan de onderkant van de pagina. Aan de linkerkant geeft het deelvenster bewerken voor de sjabloon, en de rechterkant wordt weergegeven het gegevensmodel voor de sjabloon.

De sjabloon bewerken deelvenster bevat de opmaak die Hiermee bepaalt u het uiterlijk en gedrag van de bijbehorende pagina in de portal voor ontwikkelaars. De opmaak van de sjabloon maakt gebruik van de [DotLiquid](http://dotliquidmarkup.org/) syntaxis. Is een populaire editor voor DotLiquid [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Alle wijzigingen aan de sjabloon tijdens het bewerken in realtime worden weergegeven in de browser, maar zijn niet zichtbaar voor uw klanten totdat u [opslaan](#to-save-a-template) en [publiceren](#to-publish-a-template) de sjabloon.

![Sjabloon markup][api-management-template]

De **sjabloongegevens** deelvenster biedt een handleiding voor het gegevensmodel voor de entiteiten die beschikbaar voor gebruik in een bepaalde sjabloon zijn. Het biedt deze handleiding door de live-gegevens die op dit moment worden weergegeven in de portal voor ontwikkelaars weer te geven. U kunt de deelvensters van de sjabloon uitbreiden door te klikken op de rechthoek in de rechterbovenhoek van de **sjabloongegevens** deelvenster.

![Sjabloon-gegevensmodel][api-management-template-data]

In het vorige voorbeeld, er zijn twee producten weergegeven in de portal voor ontwikkelaars die zijn opgehaald uit de gegevens die worden weergegeven de **sjabloongegevens** deelvenster, zoals wordt weergegeven in het volgende voorbeeld:

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

De opmaak van de **lijst met producten** sjabloon verwerkt de gegevens voor de gewenste uitvoer door te doorlopen van de verzameling van producten voor het weergeven van informatie en een koppeling naar elk afzonderlijk product. Houd er rekening mee de `<search-control>` en `<page-control>` elementen in de opmaak. Deze bepalen van het zoeken en besturingselementen op de pagina voor het wisselbestand wordt weergegeven. `ProductsStrings|PageTitleProducts` is een verwijzing naar de gelokaliseerde tekenreeks met de `h2` koptekst voor de pagina. Zie voor een lijst van de tekenreeksresources, paginabesturingselementen en pictogrammen beschikbaar voor gebruik in sjablonen voor ontwikkelaarsportals [naslaginformatie over API Management developer portal sjablonen](api-management-developer-portal-templates-reference.md).

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

## <a name="to-save-a-template"></a>Een sjabloon opslaan
Klik op Opslaan om een sjabloon hebt opgeslagen, in de sjablooneditor.

![Sjabloon opslaan][api-management-save-template]

Opgeslagen wijzigingen zijn niet in de portal voor ontwikkelaars live totdat ze worden gepubliceerd.

## <a name="to-publish-a-template"></a>Een sjabloon publiceren
Opgeslagen sjablonen kunnen worden gepubliceerd, afzonderlijk of alles bij elkaar. Voor het publiceren van een afzonderlijke sjabloon, klik op publiceren in de sjablooneditor.

![Sjabloon publiceren][api-management-publish-template]

Klik op **Ja** om te bevestigen en de sjabloon maken live op de portal voor ontwikkelaars.

![Controleer of publiceren][api-management-publish-template-confirm]

Als u wilt alle momenteel niet-gepubliceerde sjabloonversies publiceren, klikt u op **publiceren** in de lijst met sjablonen. Niet-gepubliceerde sjablonen worden aangeduid met een asterisk naam van de sjabloon te volgen. In dit voorbeeld wordt de **lijst met producten** en **Product** sjablonen worden gepubliceerd.

![Sjablonen publiceren][api-management-publish-templates]

Klik op **aanpassingen publiceren** om te bevestigen.

![Controleer of publiceren][api-management-publish-customizations]

Onlangs gepubliceerde sjablonen zijn direct in de portal voor ontwikkelaars van kracht.

## <a name="to-revert-a-template-to-the-previous-version"></a>Om terug te zetten van een sjabloon naar de vorige versie
Als u een sjabloon in de vorige gepubliceerde versie, klik op herstellen in de sjablooneditor.

![Sjabloon terugzetten][api-management-revert-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-revert-template-confirm]

De eerder gepubliceerde versie van een sjabloon is meteen live in de portal voor ontwikkelaars, nadat de herstelbewerking voltooid is.

## <a name="to-restore-a-template-to-the-default-version"></a>Een sjabloon terugzetten naar de standaardversie
Herstellen van sjablonen naar de standaardversie is een proces in twee stappen. Eerst de sjablonen moeten worden hersteld en vervolgens de herstelde versie moeten worden gepubliceerd.

Voor het herstellen van een sjabloon voor de standaardversie klikt u op herstellen in de sjablooneditor.

![Sjabloon terugzetten][api-management-reset-template]

Klik op **Ja** om te bevestigen.

![Bevestigen][api-management-reset-template-confirm]

Als u alle sjablonen op de standaardversies herstellen, klikt u op **herstellen standaardsjablonen** op de lijst met sjablonen.

![Herstellen van sjablonen][api-management-restore-templates]

De herstelde sjablonen moeten vervolgens worden gepubliceerd afzonderlijk of in één keer met de volgende stappen in [voor het publiceren van een sjabloon](#to-publish-a-template).

## <a name="next-steps"></a>Volgende stappen
Zie voor informatie over sjablonen voor ontwikkelaarsportals, tekenreeksresources, pictogrammen en paginabesturingselementen, [naslaginformatie over API Management developer portal sjablonen](api-management-developer-portal-templates-reference.md).

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
