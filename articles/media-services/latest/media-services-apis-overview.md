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
ms.openlocfilehash: 4c5b30ab075bbca22b6a58ccf65e55d332820937
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406553"
---
# <a name="developing-with-media-services-v3-apis"></a>Ontwikkelen met mediaservices v3 API 's

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

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Filters, bestellen, wisselbestand van Media Services-entiteiten

Zie [filteren, bestellen en van Azure Media Services-entiteiten](entities-overview.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

[Begin te ontwikkelen met Media Services v3 API met behulp van SDK's / hulpprogramma 's](developers-guide.md)
