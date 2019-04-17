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
ms.date: 04/15/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed10354047060825b4368e02160d4655e33bc8f6
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617393"
---
# <a name="developing-with-media-services-v3-apis"></a>Ontwikkelen met mediaservices v3 API 's

Dit artikel worden de regels die betrekking hebben op entiteiten en API's bij het ontwikkelen met Media Services v3.

## <a name="accessing-the-azure-media-services-api"></a>Toegang tot de Azure mediaservices API

Voor toegang tot de resources van Azure Media Services, kunt u de verificatie van service-principal voor de Azure Active Directory (AD).
De API van Media Services is vereist dat de gebruiker of toepassing die de REST-API vraagt toegang hebben tot de resource van de Media Services en gebruik een **Inzender** of **eigenaar** rol. De API kan worden geopend met de **lezer** rol, maar alleen **ophalen** of **lijst**   bewerkingen beschikbaar zal zijn. Zie voor meer informatie, [rollen gebaseerd toegangsbeheer voor Media Services-accounts](rbac-overview.md).

In plaats van het maken van een service-principal, kunt u overwegen beheerde identiteiten voor Azure-resources voor toegang tot de API van Media Services via Azure Resource Manager. Zie voor meer informatie over beheerde identiteiten voor Azure-resources, [wat is beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Azure AD-service-principal 

Als u een Azure AD-toepassing en service principal, wordt de toepassing heeft zich in een eigen tenant. Nadat u de toepassing maakt, geeft u de app **Inzender** of **eigenaar** rollen gebaseerde toegang tot het Media Services-account. 

Als u niet zeker weet of u gemachtigd bent om te maken van een Azure AD-toepassing, Zie [vereiste machtigingen](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

In de volgende afbeelding ziet geven de getallen de stroom van de aanvragen in chronologische volgorde:

![Middelste laag apps](../previous/media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Een app voor de middelste laag vraagt een toegangstoken van Azure AD dat de volgende parameters heeft:  

   * Azure AD-tenant-eindpunt.
   * Media Services-resource-URI.
   * Resource-URI voor de REST-mediaservices.
   * Waarden van Azure AD-toepassing: de client-ID en clientgeheim.
   
   Als u de gewenste waarden, Zie [toegang tot Azure Media Services-API met de Azure CLI](access-api-cli-how-to.md)

2. Het Azure AD-toegangstoken wordt verzonden naar de middelste laag.
4. De middelste laag stuurt de aanvraag naar de Azure REST-API voor Media met de Azure AD-token.
5. De middelste laag wordt in de gegevens van Media Services.

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

## <a name="next-steps"></a>Volgende stappen

[Begin te ontwikkelen met Media Services v3 API met behulp van SDK's / hulpprogramma 's](developers-guide.md)
