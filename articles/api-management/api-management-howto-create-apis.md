---
title: API's maken in Azure API Management
description: Informatie over het maken en configureren van API's in Azure API Management.
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 14c20da4-f29f-4b28-bec7-3d4c50b734da
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: ab08256fbc3caca05bf23a12016ad2acf4fc7412
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-apis-in-azure-api-management"></a>API's maken in Azure API Management
Een API Management-API vertegenwoordigt een reeks bewerkingen die kunnen worden aangeroepen door clienttoepassingen. Nieuwe API's worden gemaakt in de publicatieportal en klikt u vervolgens de gewenste bewerkingen worden toegevoegd. Nadat de bewerkingen zijn toegevoegd, wordt de API is toegevoegd aan een product en kan worden gepubliceerd. Als een API is gepubliceerd, kunt u het abonnement op en gebruikt door ontwikkelaars.

Deze handleiding bevat de eerste stap in het proces: het maken en configureren van een nieuwe API in API Management. Zie voor meer informatie over het toevoegen van bewerkingen en een product publiceren [bewerkingen toevoegen aan een API] [ How to add operations to an API] en [maken en publiceren van een product] [ How to create and publish a product].

## <a name="create-new-api"></a>Maken van een nieuwe API
API's zijn gemaakt en geconfigureerd in de publicatieportal. Voor toegang tot de publicatieportal bevindt, klikt u op **publicatieportal** in de Azure-Portal voor uw API Management-service.

![Publicatieportal][api-management-management-console]

> Als u nog geen service-exemplaar van API Management hebt gemaakt, raadpleegt u [Service-exemplaar van API Management maken][Create an API Management service instance] in de zelfstudie [Aan de slag met Azure API Management][Get started with Azure API Management].
> 
> 

Klik op **API's** van de **API Management** menu aan de linkerkant en klik vervolgens op **API toevoegen**.

![API maken][api-management-create-api]

Gebruik de **nieuwe API toevoegen** venster voor het configureren van de nieuwe API.

![Nieuwe API toevoegen][api-management-add-new-api]

De volgende velden worden gebruikt voor het configureren van de nieuwe API.

* **De naam van de web-API** biedt een unieke en beschrijvende naam voor de API. Deze wordt weergegeven in de portals ontwikkelaars en uitgever.
* **Webservice-URL** verwijst naar de HTTP-service voor het implementeren van de API. API management verzendt aanvragen naar dit adres.
* **Achtervoegsel voor web-API-URL** wordt toegevoegd aan de basis-URL voor de API management-service. De basis-URL is gemeenschappelijk voor alle API's die worden gehost door een exemplaar van API Management-service. API Management API's onderscheidt door hun achtervoegsel en daarom het achtervoegsel moet uniek zijn voor elke API voor een opgegeven uitgever.
* **Web-API-URL-schema** bepaalt welke protocollen kunnen worden gebruikt voor toegang tot de API. Standaard HTTPs is opgegeven.
* Als u wilt eventueel deze nieuwe API toevoegen aan een product, klikt u op de **producten (optioneel)** vervolgkeuzelijst en kies een product. Deze stap kan meerdere keren toevoegen van de API aan meerdere producten worden herhaald.

Nadat de gewenste waarden zijn geconfigureerd, klikt u op **opslaan**. Zodra de nieuwe API is gemaakt, wordt de overzichtspagina voor de API weergegeven in de publicatieportal.

![API-overzicht][api-management-api-summary]

## <a name="configure-api-settings"></a>Instellingen voor de API configureren
U kunt de **instellingen** tab om te controleren en bewerken van de configuratie voor een API. **De naam van de web-API**, **webservice-URL**, en **achtervoegsel URL Web-API** zijn in eerste instantie ingesteld als de API wordt gemaakt en kan worden gewijzigd hier. **Beschrijving** biedt een optionele beschrijving en **Web API-URL-schema** bepaalt welke protocollen kunnen worden gebruikt voor toegang tot de API.

![Instellingen voor de API][api-management-api-settings]

Als gatewayverificatie wilt configureren voor het implementeren van de API van de back-endservice, selecteer de **beveiliging** tabblad. De **met referenties** vervolgkeuzelijst kan worden gebruikt voor het configureren van **HTTP basic** of **clientcertificaten** verificatie. Voer de gewenste referenties voor het gebruik van HTTP-basisverificatie. Zie voor meer informatie over het gebruik van verificatie van clientcertificaten [het beveiligen van back-end-services met behulp van client certificaatverificatie in Azure API Management][How to secure back-end services using client certificate authentication in Azure API Management].

De **beveiliging** tabblad kan ook worden gebruikt voor het configureren van **gebruikersautorisatie** met behulp van OAuth 2.0. Zie voor meer informatie [hoe autoriseren met behulp van OAuth 2.0 in Azure API Management ontwikkelaarsaccounts][How to authorize developer accounts using OAuth 2.0 in Azure API Management].

![Instellingen voor basisverificatie][api-management-api-settings-credentials]

Klik op **opslaan** wijzigingen in de API-instellingen wilt opslaan.

## <a name="next-steps"> </a>Volgende stappen
Zodra een API is gemaakt en de instellingen die zijn geconfigureerd, de volgende stappen zijn de bewerkingen toevoegen aan de API, de API toevoegen aan een product en publiceren zodat deze beschikbaar voor ontwikkelaars. Zie de volgende artikelen voor meer informatie.

* [Bewerkingen toevoegen aan een API][How to add operations to an API]
* [Het maken en een product publiceren][How to create and publish a product]

[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[How to secure back-end services using client certificate authentication in Azure API Management]: api-management-howto-mutual-certificates.md
[How to authorize developer accounts using OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md
