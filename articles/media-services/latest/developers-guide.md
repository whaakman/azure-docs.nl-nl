---
title: Azure Media Services v3 SDK's - Azure
description: In dit artikel vindt u een overzicht van hoe u begint aan het ontwikkelen van apps met Media Services v3-API met behulp van SDK's/hulpprogramma's.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/20/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: cda6c9cd1f9c8b9305349f0904aeb744ba373711
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58350246"
---
# <a name="start-developing-with-media-services-v3-api-using-sdkstools"></a>Beginnen met ontwikkelen met Media Services v3-API met behulp van SDK's/hulpprogramma's

Als een ontwikkelaar kunt u de [REST-API](https://aka.ms/ams-v3-rest-ref) van Media Services gebruiken, of clientbibliotheken waarmee u kunt communiceren met de REST-API, om eenvoudig aangepaste mediawerkstromen te maken, beheren en onderhouden. De API van [Media Services versie 3](https://aka.ms/ams-v3-rest-sdk) is gebaseerd op de OpenAPI-specificatie (voorheen bekend als een Swagger).

In dit onderwerp vindt u koppelingen naar de SDK's, hulpprogramma's, documentatie. Het bevat ook nuttige informatie voor andere ontwikkelomgevingen.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met het ontwikkelen met Media Services, hebt u het volgende nodig:

- Een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Meer informatie over basisconcepten](concepts-overview.md)
- [Een Azure Media Services-account maken](create-account-cli-how-to.md)

## <a name="start-developing"></a>Start met ontwikkelen

Azure Media Services ondersteunt de volgende SDK's/hulpprogramma's 

- [Azure-CLI](https://aka.ms/ams-v3-cli) 
- [.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Node.js-SDK](https://aka.ms/ams-v3-nodejs-sdk)
- [Python SDK](https://aka.ms/ams-v3-python-sdk)
- [Go SDK](https://aka.ms/ams-v3-go-sdk)
- [Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)

### <a name="azure-media-services-explorer"></a>Azure Media Services Explorer

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) is een hulpprogramma beschikbaar voor Windows-klanten die kennis willen maken met Media Services. AMSE is een Winforms/C#-toepassing voor het uploaden, downloaden, coderen en streamen van VOD en live-inhoud met Media Services. Het AMSE-hulpprogramma is voor clients die Media Services willen testen zonder code te schrijven. De AMSE-code wordt geleverd als een bron voor klanten die willen ontwikkelen met Media Services.

AMSE is een Open-Source-project, en dus wordt er ondersteuning geboden door de community (problemen kunnen worden gemeld aan https://github.com/Azure/Azure-Media-Services-Explorer/issues). Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [veelgestelde vragen over de gedragscode](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met opencode@microsoft.com als u aanvullende vragen of opmerkingen hebt.

## <a name="rest"></a>REST

Installeer een REST-client als u wilt beginnen met het ontwikkelen met Media Services REST-API's. Bijvoorbeeld **Postman**, **Visual Studio Code** met de REST-invoegtoepassing, of **Telerik Fiddler**. We gebruiken [Postman](media-rest-apis-with-postman.md) voor Media Services v3-voorbeelden.

Raadpleeg de [REST API-naslagdocumentatie](https://aka.ms/ams-v3-rest-ref) van Media Services en bekijk deze voorbeelden:

- [Zelfstudie: Extern bestand coderen op basis van URL en video streamen - REST](stream-files-tutorial-with-rest.md)
- [Bestanden uploaden naar een Media Services-account - REST](upload-files-rest-how-to.md)
- [Filters maken met Media Services - REST](filters-dynamic-manifest-rest-howto.md)
- [REST API op basis van Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates)

<!-- ## CLI -->
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

Raadpleeg de [Azure CLI-naslagdocumentatie](https://aka.ms/ams-v3-cli-ref) van Media Services en bekijk deze voorbeelden:

- [Gereserveerde media-eenheden schalen - CLI](media-reserved-units-cli-how-to.md)
- [Een Azure Media Services-account maken - CLI](./scripts/cli-create-account.md) 
- [De referenties voor het account opnieuw instellen - CLI](./scripts/cli-reset-account-credentials.md)
- [Assets maken - CLI](./scripts/cli-create-asset.md)
- [Een bestand uploaden - CLI](./scripts/cli-upload-file-asset.md)
- [Transformaties maken - CLI](./scripts/cli-create-transform.md)
- [Taken maken - CLI](./scripts/cli-create-jobs.md)
- [EventGrid maken - CLI](./scripts/cli-create-event-grid.md)
- [Een asset publiceren - CLI](./scripts/cli-publish-asset.md)
- [Filteren - CLI](filters-dynamic-manifest-cli-howto.md)

## <a name="net"></a>.NET

Raadpleeg de [.NET-naslagdocumentatie](https://aka.ms/ams-v3-dotnet-ref) van Media Services en bekijk deze voorbeelden:

- [Zelfstudie: Video's uploaden, coderen en streamen - .NET](stream-files-tutorial-with-api.md) 
- [Zelfstudie: Live streamen met Azure Media Services v3 - .NET](stream-live-tutorial-with-api.md)
- [Zelfstudie: Video's analyseren met Media Services v3 - .NET](analyze-videos-tutorial-with-api.md)
- [Een taakinvoer maken vanuit een lokaal bestand - .NET](job-input-from-local-file-how-to.md)
- [Een taakinvoer maken vanuit een HTTPS-URL - .NET](job-input-from-http-how-to.md)
- [Coderen met een aangepaste transformatie - .NET](customize-encoder-presets-how-to.md)
- [Dynamische AES-128-versleuteling en de sleutelleveringsservice gebruiken - .NET](protect-with-aes128.md)
- [Dynamische DRM-versleuteling en licentielevering gebruiken - .NET](protect-with-drm.md)
- [Een ondertekeningssleutel ophalen uit het bestaand beleid - .NET](get-content-key-policy-dotnet-howto.md)
- [Filters maken met Media Services - .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Voorbeelden van geavanceerde video on demand van Azure Functions-v2 met Media Services v3](https://aka.ms/ams3functions)

## <a name="java"></a>Java

Raadpleeg de [Java-naslagdocumentatie](https://aka.ms/ams-v3-java-ref) van Media Services.

## <a name="nodejs"></a>Node.js

Raadpleeg de [Node.js-naslagdocumentatie](https://aka.ms/ams-v3-nodejs-ref) van Media Services en bekijk [voorbeelden](https://github.com/Azure-Samples/media-services-v3-node-tutorials) waarin u ziet hoe u Media Services-API gebruikt met Node.js.

## <a name="python"></a>Python

Raadpleeg de [Python-naslagdocumentatie](https://aka.ms/ams-v3-python-ref) van Media Services.

## <a name="go"></a>Aan de slag

Raadpleeg de [Go-naslagdocumentatie](https://aka.ms/ams-v3-go-ref) van Media Services.

## <a name="next-steps"></a>Volgende stappen

- [Een account maken - CLI](create-account-cli-how-to.md)
- [Toegang tot API's - CLI](access-api-cli-how-to.md)

