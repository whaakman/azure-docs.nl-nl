---
title: Productsjablonen in Azure API Management | Microsoft Docs
description: Informatie over het aanpassen van de inhoud van de product-pagina's in de Azure API Management-portal voor ontwikkelaars.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 49f9254c-4c5f-4ed4-9c8d-798f44e805ee
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 14090e21fb7c6ca07fe63220ffd1d44d483ac869
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61088176"
---
# <a name="product-templates-in-azure-api-management"></a>Productsjablonen in Azure API Management

Met Azure API Management biedt u de mogelijkheid om aan te passen van de inhoud van ontwikkelaarsportalpagina's met behulp van een set van sjablonen die hun inhoud hebt geconfigureerd. Met behulp van [DotLiquid](http://dotliquidmarkup.org/) syntaxis en de editor van uw keuze, zoals [DotLiquid voor ontwerpers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), en een opgegeven set gelokaliseerde [tekenreeks resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), en [pagina besturingselementen](api-management-page-controls.md), hebt u geweldige flexibiliteit voor het configureren van de inhoud van de pagina's naar eigen inzicht met behulp van deze sjablonen.  
  
 De sjablonen in deze sectie kunnen u de inhoud van de product-pagina's in de ontwikkelaarsportal aanpassen.  
  
-   [Lijst met producten](#ProductList)  
  
-   [Product](#Product)  
  
> [!NOTE]
>  Standaard-voorbeeldsjablonen zijn opgenomen in de volgende documentatie, maar kunnen worden gewijzigd vanwege de continue verbeteringen. U kunt de live standaardsjablonen weergeven in de portal voor ontwikkelaars door te navigeren naar de gewenste afzonderlijke sjablonen. Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]
  
##  <a name="ProductList"></a> Lijst met producten  
 De **lijst met producten** sjabloon kunt u de hoofdtekst van de pagina van de lijst met producten in de ontwikkelaarsportal aanpassen.  
  
 ![Lijst met producten](./media/api-management-product-templates/APIM_ProductsListTemplatePage.png "APIM_ProductsListTemplatePage")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
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
  
### <a name="controls"></a>Besturingselementen  
 De `Product list` sjabloon mogelijk gebruikt u de volgende [pagina besturingselementen](api-management-page-controls.md).  
  
-   [paging-control](api-management-page-controls.md#paging-control)  
  
-   [search-control](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a>Gegevensmodel  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Paging|[Voor het wisselbestand](api-management-template-data-model-reference.md#Paging) entiteit.|De informatie paginering voor de verzameling producten.|  
|Filtering|[Filteren](api-management-template-data-model-reference.md#Filtering) entiteit.|De filters gebruiken om gegevens voor de pagina van de lijst met producten.|  
|Products|Verzameling van [Product](api-management-template-data-model-reference.md#Product) entiteiten.|De producten die zichtbaar is voor de huidige gebruiker.|  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
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
            "Id": "56f9445ffaf7560049060001",  
            "Title": "Starter",  
            "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "Terms": "",  
            "ProductState": 1,  
            "AllowMultipleSubscriptions": false,  
            "MultipleSubscriptionsCount": 1  
        },  
        {  
            "Id": "56f9445ffaf7560049060002",  
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
  
##  <a name="Product"></a> Product  
 De **Product** sjabloon kunt u de hoofdtekst van de productpagina in de ontwikkelaarsportal aanpassen.  
  
 ![Developer portal productpagina](./media/api-management-product-templates/APIM_ProductPage.png "APIM_ProductPage")  
  
### <a name="default-template"></a>Standaardsjabloon  
  
```xml  
<h2>{{Product.Title}}</h2>  
<p>{{Product.Description}}</p>  
  
{% assign replaceString0 = '{0}' %}  
  
{% if Limits and Limits.size > 0 %}  
<h3>{% localized "ProductDetailsStrings|WebProductsUsageLimitsHeader"%}</h3>  
<ul>  
  {% for limit in Limits %}  
  <li>{{limit.DisplayName}}</li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if apis.size > 0 %}  
<p>  
  <b>  
    {% if apis.size == 1 %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockSingleApisCount" %}{% endcapture %}  
    {% else %}  
    {% capture apisCountText %}{% localized "ProductDetailsStrings|TextblockMultipleApisCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture apisCount %}{{apis.size}}{% endcapture %}  
    {{ apisCountText | replace : replaceString0, apisCount }}  
  </b>  
</p>  
  
<ul>  
  {% for api in Apis %}  
  <li>  
    <a href="/docs/services/{{api.Id}}">{{api.Name}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
  
{% if subscriptions.size > 0 %}  
<p>  
  <b>  
    {% if subscriptions.size == 1 %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockSingleSubscriptionsCount" %}{% endcapture %}  
    {% else %}  
    {% capture subscriptionsCountText %}{% localized "ProductDetailsStrings|TextblockMultipleSubscriptionsCount" %}{% endcapture %}  
    {% endif %}  
  
    {% capture subscriptionsCount %}{{subscriptions.size}}{% endcapture %}  
    {{ subscriptionsCountText | replace : replaceString0, subscriptionsCount }}  
  </b>  
</p>  
  
<ul>  
  {% for subscription in subscriptions %}  
  <li>  
    <a href="/developer#{{subscription.Id}}">{{subscription.DisplayName}}</a>  
  </li>  
  {% endfor %}  
</ul>  
{% endif %}  
{% if CannotAddBecauseSubscriptionNumberLimitReached %}  
<b>{% localized "ProductDetailsStrings|TextblockSubscriptionLimitReached" %}</b>  
{% elsif CannotAddBecauseMultipleSubscriptionsNotAllowed == false %}  
<subscribe-button></subscribe-button>  
{% endif %}  
```  
  
### <a name="controls"></a>Besturingselementen  
 De `Product list` sjabloon mogelijk gebruikt u de volgende [pagina besturingselementen](api-management-page-controls.md).  
  
-   [abonnement-knop](api-management-page-controls.md#subscribe-button)  
  
### <a name="data-model"></a>Gegevensmodel  
  
|Eigenschap|Type|Beschrijving|  
|--------------|----------|-----------------|  
|Product|[Product](api-management-template-data-model-reference.md#Product)|Het opgegeven product.|  
|IsDeveloperSubscribed|booleaans|Of de huidige gebruiker is geabonneerd op dit product.|  
|SubscriptionState|getal|De status van het abonnement. Mogelijke statussen zijn:<br /><br /> -   `0 - suspended` : het abonnement is geblokkeerd en API's van het product kan niet worden aangeroepen door de abonnee.<br />-   `1 - active` : het abonnement actief is.<br />-   `2 - expired` : het abonnement is bereikt de vervaldatum en is gedeactiveerd.<br />-   `3 - submitted` – de Abonnementaanvraag is gemaakt door de ontwikkelaar, maar is nog niet is goedgekeurd of afgewezen.<br />-   `4 - rejected` – de Abonnementaanvraag is afgewezen door een beheerder.<br />-   `5 - cancelled` : het abonnement is geannuleerd door de ontwikkelaar of beheerder.|  
|Limits|matrix|Deze eigenschap is afgeschaft en mag niet worden gebruikt.|  
|DelegatedSubscriptionEnabled|booleaans|Of [delegering](https://azure.microsoft.com/documentation/articles/api-management-howto-setup-delegation/) is ingeschakeld voor dit abonnement.|  
|DelegatedSubscriptionUrl|tekenreeks|Als delegering is ingeschakeld, de URL van de gedelegeerde abonnement.|  
|IsAgreed|booleaans|Als het product termen, zijn ongeacht of de huidige gebruiker heeft ingestemd met de voorwaarden.|  
|Subscriptions|Verzameling van [samenvatting van abonnement](api-management-template-data-model-reference.md#SubscriptionSummary) entiteiten.|De abonnementen aan het product.|  
|Apis|Verzameling van [API](api-management-template-data-model-reference.md#API) entiteiten.|De API's in dit product.|  
|CannotAddBecauseSubscriptionNumberLimitReached|booleaans|Of de huidige gebruiker is die in aanmerking komen om u te abonneren op dit product met betrekking tot de voor uw abonnement.|  
|CannotAddBecauseMultipleSubscriptionsNotAllowed|booleaans|Of de huidige gebruiker is die in aanmerking komen om u te abonneren op dit product met betrekking tot meerdere abonnementen zijn of niet wordt toegestaan.|  
  
### <a name="sample-template-data"></a>Voorbeeldgegevens voor de sjabloon  
  
```json  
{  
    "Product": {  
        "Id": "56f9445ffaf7560049060001",  
        "Title": "Starter",  
        "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
        "Terms": "",  
        "ProductState": 1,  
        "AllowMultipleSubscriptions": false,  
        "MultipleSubscriptionsCount": 1  
    },  
    "IsDeveloperSubscribed": true,  
    "SubscriptionState": 1,  
    "Limits": [],  
    "DelegatedSubscriptionEnabled": false,  
    "DelegatedSubscriptionUrl": null,  
    "IsAgreed": false,  
    "Subscriptions": [  
        {  
            "Id": "56f9445ffaf7560049070001",  
            "DisplayName": "Starter  (default)"  
        }  
    ],  
    "Apis": [  
        {  
            "id": "56f9445ffaf7560049040001",  
            "name": "Echo API",  
            "description": null,  
            "serviceUrl": "http://echoapi.cloudapp.net/api",  
            "path": "echo",  
            "protocols": [  
                2  
            ],  
            "authenticationSettings": null,  
            "subscriptionKeyParameterNames": null  
        }  
    ],  
    "CannotAddBecauseSubscriptionNumberLimitReached": false,  
    "CannotAddBecauseMultipleSubscriptionsNotAllowed": true  
}  
```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het werken met sjablonen [over het aanpassen van de API Management-ontwikkelaarsportal met behulp van sjablonen](api-management-developer-portal-templates.md).