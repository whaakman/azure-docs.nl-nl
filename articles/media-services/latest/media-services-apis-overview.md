---
title: Ontwikkelen met v3 API's - Azure | Microsoft Docs
description: Dit artikel worden de regels die betrekking hebben op entiteiten en API's bij het ontwikkelen met Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/02/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 3ce20b56fc2cbebbed4b525eeccc2c12d14cccc3
ms.sourcegitcommit: 9e8dfa1169a55c3c8af93a6c5f4e0dace4de48b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65556234"
---
# <a name="developing-with-media-services-v3-apis"></a>Ontwikkelen met mediaservices v3 API 's

Als een ontwikkelaar kunt u de [REST-API](https://aka.ms/ams-v3-rest-ref) van Media Services gebruiken, of clientbibliotheken waarmee u kunt communiceren met de REST-API, om eenvoudig aangepaste mediawerkstromen te maken, beheren en onderhouden. De API van [Media Services versie 3](https://aka.ms/ams-v3-rest-sdk) is gebaseerd op de OpenAPI-specificatie (voorheen bekend als een Swagger).

Dit artikel worden de regels die betrekking hebben op entiteiten en API's bij het ontwikkelen met Media Services v3.

## <a name="accessing-the-azure-media-services-api"></a>Toegang tot de Azure mediaservices API

Als u wilt worden geautoriseerd voor toegang tot Media Services-resources en de API van Media Services, moet u eerst worden geverifieerd. Media Services ondersteunt [Azure Active Directory (Azure AD)-op basis van](../../active-directory/fundamentals/active-directory-whatis.md) verificatie. Er zijn twee algemene verificatieopties:
 
* **Service-principal verificatie** : wordt gebruikt voor het verifiëren van een service (bijvoorbeeld: web-apps, functie-apps, logic apps, API en microservices). Toepassingen die gebruikmaken van deze verificatiemethode vaak zijn apps die daemon-services, services van de middelste laag of geplande taken worden uitgevoerd. Bijvoorbeeld, voor Web moeten er altijd toepassingen een middelste laag die verbinding met Media Services met een Service-Principal maakt.
* **Verificatie van de gebruiker** : wordt gebruikt voor het verifiëren van een persoon die de app wordt gebruikt om te communiceren met Media Services-resources. De interactieve toepassing moet eerst de gebruiker gevraagd om referenties van de gebruiker. Een voorbeeld is een management console-app die wordt gebruikt door gemachtigde gebruikers om te controleren coderingstaken of live streamen.

De API van Media Services is vereist dat de gebruiker of toepassing die de REST-API vraagt toegang hebben tot de resource van de Media Services en gebruik een **Inzender** of **eigenaar** rol. De API kan worden geopend met de **lezer** rol, maar alleen **ophalen** of **lijst**   bewerkingen beschikbaar zal zijn. Zie voor meer informatie, [rollen gebaseerd toegangsbeheer voor Media Services-accounts](rbac-overview.md).

In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor toegang tot de API van Media Services via Azure Resource Manager. Zie voor meer informatie over beheerde identiteiten voor Azure-resources, [wat is beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Azure AD-service-principal 

Als u een Azure AD-toepassing en service principal, wordt de toepassing heeft zich in een eigen tenant. Nadat u de toepassing maakt, geeft u de app **Inzender** of **eigenaar** rollen gebaseerde toegang tot het Media Services-account. 

Als u niet zeker weet of u gemachtigd bent om te maken van een Azure AD-toepassing, Zie [vereiste machtigingen](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

In de volgende afbeelding ziet geven de getallen de stroom van de aanvragen in chronologische volgorde:

![Middelste laag apps](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Een app voor de middelste laag vraagt een toegangstoken van Azure AD dat de volgende parameters heeft:  

   * Azure AD-tenant-eindpunt.
   * Media Services-resource-URI.
   * Resource-URI voor de REST-mediaservices.
   * Waarden van Azure AD-toepassing: de client-ID en clientgeheim.
   
   Als u de gewenste waarden, Zie [toegang tot Azure Media Services-API met de Azure CLI](access-api-cli-how-to.md)

2. Het Azure AD-toegangstoken wordt verzonden naar de middelste laag.
4. De middelste laag stuurt de aanvraag naar de Azure REST-API voor Media met de Azure AD-token.
5. De middelste laag wordt in de gegevens van Media Services.

### <a name="samples"></a>Voorbeelden

Zie de volgende voorbeelden die laten Zie hoe u verbinding maakt met Azure AD-service-principal:

* [Verbinding maken met REST](media-rest-apis-with-postman.md)  
* [Verbinding maken met Java](configure-connect-java-howto.md)
* [Verbinding maken met .NET](configure-connect-dotnet-howto.md)
* [Verbinding maken met Node.js](configure-connect-nodejs-howto.md)
* [Verbinding maken met Python](configure-connect-python-howto.md)

## <a name="naming-conventions"></a>Naamconventies

Namen van Azure Media Services v3-resources (bijvoorbeeld activa, taken, transformaties) zijn onderhevig aan de naamgevingsbeperkingen van Azure Resource Manager. In overeenstemming met Azure Resource Manager zijn de resourcenamen altijd uniek. U kunt dus alle unieke id-strings (bijvoorbeeld GUID's) gebruiken voor uw resourcenamen. 

Namen van Media Services-resources mogen niet de volgende tekens bevatten: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', enkele aanhalingstekens of besturingstekens. Alle andere tekens zijn toegestaan. De maximale lengte van een resourcenaam is 260 tekens. 

Zie [Naamvereisten](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) en [Naamconventies](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) voor meer informatie over Azure Resource Manager-naamgeving.

## <a name="long-running-operations"></a>Langlopende bewerkingen

De bewerkingen die zijn gemarkeerd met `x-ms-long-running-operation` in de Azure Media Services [swagger-bestanden](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) zijn lang bewerkingen. 

Zie voor meer informatie over hoe u kunt Azure-asynchrone bewerkingen bijhouden [asynchrone bewerkingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation).

Media Services heeft de volgende langlopende bewerkingen:

* LiveEvent maken
* LiveEvent bijwerken
* LiveEvent verwijderen
* LiveEvent starten
* LiveEvent stoppen
* LiveEvent opnieuw instellen
* LiveOutput maken
* LiveOutput verwijderen
* Streamingendpoint zo maken
* Update StreamingEndpoint
* Streamingendpoint zo verwijderen
* Start StreamingEndpoint
* Stop StreamingEndpoint
* Schaal streamingendpoint zo


## <a name="sdks"></a>SDK's

> [!NOTE]
> De Azure Media Services v3-SDK's zijn niet noodzakelijkerwijs thread-veilig. Bij het ontwikkelen van een toepassing met meerdere threads, moet u uw eigen logica voor het synchroniseren van thread voor het beveiligen van de client of het gebruik van een nieuw object AzureMediaServicesClient per thread toevoegen. Ook moet u voorzichtig met multithreading problemen geïntroduceerd door optionele objecten die worden geleverd door uw code aan de client (zoals een instantie van HttpClient in .NET).

|SDK|Referentie|
|---|---|
|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET-ref](https://aka.ms/ams-v3-dotnet-ref)|
|[Java SDK](https://aka.ms/ams-v3-java-sdk)|[Java-ref](https://aka.ms/ams-v3-java-ref)|
|[Python SDK](https://aka.ms/ams-v3-python-sdk)|[Python-ref](https://aka.ms/ams-v3-python-ref)|
|[Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk) |[Node.js-ref](https://aka.ms/ams-v3-nodejs-ref)| 
|[Go SDK](https://aka.ms/ams-v3-go-sdk) |[Go-ref](https://aka.ms/ams-v3-go-ref)|
|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

### <a name="see-also"></a>Zie ook

- [EventGrid .NET-SDK met Media Service-gebeurtenissen](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [Definities van Media Services-gebeurtenissen](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)

## <a name="azure-media-services-explorer"></a>Azure Media Services Explorer

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) is een hulpprogramma beschikbaar voor Windows-klanten die kennis willen maken met Media Services. AMSE is een Winforms/C#-toepassing voor het uploaden, downloaden, coderen en streamen van VOD en live-inhoud met Media Services. Het AMSE-hulpprogramma is voor clients die Media Services willen testen zonder code te schrijven. De AMSE-code wordt geleverd als een bron voor klanten die willen ontwikkelen met Media Services.

AMSE is een Open-Source-project, en dus wordt er ondersteuning geboden door de community (problemen kunnen worden gemeld aan https://github.com/Azure/Azure-Media-Services-Explorer/issues). Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [veelgestelde vragen over de gedragscode](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met opencode@microsoft.com als u aanvullende vragen of opmerkingen hebt.

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Filters, bestellen, wisselbestand van Media Services-entiteiten

Zie [filteren, bestellen en van Azure Media Services-entiteiten](entities-overview.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

* [Verbinding maken met mediaservices met behulp van Java](configure-connect-java-howto.md)
* [Verbinding maken met mediaservices met .NET](configure-connect-dotnet-howto.md)
* [Verbinding maken met mediaservices met behulp van Node.js](configure-connect-nodejs-howto.md)
* [Verbinding maken met mediaservices met Python](configure-connect-python-howto.md)
