---
title: Bewerkingen toevoegen aan een API in Azure API Management | Microsoft Docs
description: Informatie over het bewerkingen toevoegen aan een API in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 1158a023-1913-4e9c-93de-9164b672f9b3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 105fc51c2d1152a40a5757985da47330e0b7b8cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Bewerkingen toevoegen aan een API in Azure API Management
Voordat een API in API Management kan worden gebruikt, kunnen bewerkingen moeten worden toegevoegd. Deze handleiding laat zien hoe toevoegen en configureren van verschillende typen aan een API-bewerkingen in API Management.

## <a name="add-operation"></a>Een bewerking toevoegen
Bewerkingen zijn toegevoegd en geconfigureerd voor een API in de publicatieportal. Voor toegang tot de publicatieportal bevindt, klikt u op **publicatieportal** in de Azure-Portal voor uw API Management-service.

![Publicatieportal][api-management-management-console]

> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Selecteer de gewenste API in de publicatieportal en selecteer vervolgens de **Operations** tabblad. 

![Bewerkingen][api-management-operations]

Klik op **bewerking toevoegen** toevoegen van een nieuwe bewerking. De **nieuwe bewerking** wordt weergegeven en de **handtekening** tabblad wordt standaard geselecteerd.

![Bewerking toevoegen][api-management-add-operation]

Geef de **HTTP-term** door te kiezen uit de vervolgkeuzelijst.

![HTTP-methode][api-management-http-method]

<a name="url-template"></a>

Definieer de URL-sjabloon door in een URL-fragment die bestaan uit een of meer URL-padsegmenten en nul of meer queryreeksparameters te typen. De URL-sjabloon, toegevoegd aan de basis-URL van de API identificeert één HTTP-bewerking. Bevat een of meer benoemde variabele onderdelen die zijn geïdentificeerd door accolades. Deze variabele delen Sjabloonparameters worden genoemd en worden de waarden die worden opgehaald uit de aanvraag-URL wanneer de aanvraag wordt verwerkt door het platform van API Management dynamisch toegewezen.

> De URL-sjabloon kan jokertekenpatronen bevatten. Bijvoorbeeld, geven `/*` wordt sturen alle aanvragen voor deze HTTP-methode voor de back-end service.

![URL-sjabloon][api-management-url-template]

<a name="rewrite-url-template"></a>

Geef desgewenst de **herschrijven van URL sjabloon**. Hiermee kunt u de standaard URL-sjabloon gebruiken voor het verwerken van inkomende aanvragen op de front-end tijdens het aanroepen van de back-end via een geconverteerde URL volgens de sjabloon opnieuw. Sjabloonparameters van de URL-sjabloon moeten worden gebruikt in de sjabloon opnieuw. Het volgende voorbeeld ziet hoe inhoudstype gecodeerd zoals padsegment in de webservice van het vorige voorbeeld kan worden opgegeven als een queryparameter in de API gepubliceerd via het API Management-platform met behulp van de URL-sjablonen.

![Herschrijven van URL-sjabloon][api-management-url-template-rewrite]

Aanroepfuncties voor de bewerking wordt de notatie gebruikt `/customers?customerid=ALFKI` en deze worden toegewezen aan `/Customers('ALFKI')` wanneer de back-endservice wordt aangeroepen.

**Weergave** naam en **beschrijving** Geef een beschrijving van de bewerking en worden gebruikt voor het bieden van documentatie voor de ontwikkelaars die met deze API in de portal voor ontwikkelaars.

![Beschrijving][api-management-description]

De beschrijving van de bewerking kan worden opgegeven als tekst zonder opmaak of HTML-code in de **beschrijving** in het tekstvak.

## <a name="operation-caching"></a>Bewerking opslaan in cache
Antwoord in cache opslaan, vermindert de latentie waargenomen door de API-consumenten, verlaagt bandbreedteverbruik en vermindert de belasting van het HTTP-web service-implementatie van de API. 

Om snel en eenvoudig cachebewerkingen voor de bewerking, selecteer de **opslaan in cache** tabblad en controleer de **inschakelen** selectievakje.

![Caching][api-management-caching-tab]

**Duur** Hiermee geeft u de periode gedurende welke de bewerkingsantwoord in de cache blijft. De standaardwaarde is 3600 seconden oftewel 1 uur.

Cache-sleutels worden gebruikt om onderscheid maken tussen antwoorden zodat het overeenkomt met elk ander Cachesleutel antwoord een eigen afzonderlijke waarde in de cache krijgt. U kunt desgewenst specifieke queryreeksparameters en/of HTTP-headers worden gebruikt in de cache sleutelwaarden in computing de **variëren op queryreeksparameters** en **variëren op headers** tekstvakken respectievelijk. Wanneer er geen is opgegeven, volledige aanvraag-URL en de volgende waarden van de HTTP-header worden gebruikt in de cache sleutelgeneratie: **accepteren** en **Accept-Charset**.

> Zie voor meer informatie over opslaan in cache en cachebeleidsregels [hoe in de cache van de bewerking resulteert in Azure API Management][How to cache operation results in Azure API Management].
> 
> 

## <a name="request-parameters"></a>Aanvraagparameters
Bewerkingsparameters worden op het tabblad Parameters beheerd. Parameters die zijn opgegeven de **URL sjabloon** op de **handtekening** tabblad worden automatisch toegevoegd en kan alleen door het bewerken van het URL-sjabloon worden gewijzigd. Extra parameters kunnen handmatig worden ingevoerd.

Klik op als u een nieuwe queryparameter **queryparameter toevoegen** en voer de volgende informatie:

* **Naam** -parameternaam.
* **Beschrijving** -een korte beschrijving van de parameter (optioneel).
* **Type** -type voor parameter omlaag in de vervolgkeuzelijst is geselecteerd.
* **Waarden** -waarden die kunnen worden toegewezen aan deze parameter. Een van de waarden kan worden als standaardwaarde gemarkeerd (optioneel).
* **Vereist** -de parameter verplicht te stellen door het selectievakje. 

![Parameters van de aanvraag][api-management-request-parameters]

## <a name="request-body"></a>Aanvraagtekst
Als de bewerking kunt (bijvoorbeeld PUT, POST) en een hoofdtekst kunt u desgewenst een voorbeeld van dit voor alle van de ondersteunde weergave-indelingen (bijvoorbeeld json, XML) vereist. 

> De aanvraagtekst alleen voor documentatie wordt gebruikt en niet wordt gevalideerd.
> 
> 

Als u wilt een aanvraagtekst invoeren, overschakelen naar de **hoofdtekst** tabblad.

Klik op **weergave toevoegen**, gewenste inhoudstype-naam (bijvoorbeeld application/json) begint te typen, selecteert u deze in de vervolgkeuzelijst en plak het gewenste aanvraag hoofdtekst voorbeeld in de geselecteerde indeling in het tekstvak. 

![Aanvraagtekst][api-management-request-body]

Extra's worden representaties, u kunt ook opgeven in een optionele beschrijving in de **beschrijving** in het tekstvak.

## <a name="responses"></a>Antwoorden
Het is raadzaam vindt u voorbeelden van reacties voor alle statuscodes die de bewerking kan opleveren. Elke statuscode mogelijk meer dan één antwoord instantie bijvoorbeeld één voor elk van de ondersteunde typen inhoud. 

Klik op als u een antwoord **toevoegen** en typ de gewenste statuscode. In dit voorbeeld de statuscode is **200 OK**. Zodra de code wordt weergegeven in de vervolgkeuzelijst, selecteert u deze en de antwoordcode wordt gemaakt en toegevoegd aan de bewerking opnieuw.

![Reactiecode][api-management-response-code]

Klik op **weergave toevoegen**, de naam van het gewenste type inhoud (bijvoorbeeld application/json) begint te typen en vervolgens omlaag in de vervolgkeuzelijst selecteren.

![Inhoudstype voor hoofdtekst][api-management-response-body-content-type]

Plak in het voorbeeld van de hoofdtekst van antwoord in de geselecteerde indeling in het tekstvak. 

![Antwoordtekst][api-management-response-body]

Indien gewenst, voer een optionele beschrijving in de **beschrijving** in het tekstvak.

Nadat de bewerking is geconfigureerd, klikt u op **opslaan**.

## <a name="next-steps"> </a>Volgende stappen
Nadat de bewerkingen zijn toegevoegd aan een API, wordt de volgende stap is naar de API koppelen aan een product en deze publiceren zodat ontwikkelaars bewerkingen kunnen aanroepen.

* [Het maken en een product publiceren][How to create and publish a product]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to cache operation results in Azure API Management]: api-management-howto-cache.md
