---
title: Azure Media Services v3 SDK's - Azure
description: Dit artikel bevat een overzicht van hoe u aan het begin te ontwikkelen met Media Services v3 API met behulp van SDK's.
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
ms.date: 04/11/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9fb4d1561a661387f759aada9e776d43a95aa5c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60732447"
---
# <a name="develop-against-media-services-v3-api-using-sdks"></a>Ontwikkelen met betrekking tot Media Services v3 API met behulp van SDK 's

Als een ontwikkelaar kunt u de [REST-API](https://aka.ms/ams-v3-rest-ref) van Media Services gebruiken, of clientbibliotheken waarmee u kunt communiceren met de REST-API, om eenvoudig aangepaste mediawerkstromen te maken, beheren en onderhouden. De API van [Media Services versie 3](https://aka.ms/ams-v3-rest-sdk) is gebaseerd op de OpenAPI-specificatie (voorheen bekend als een Swagger).

> [!NOTE]
> De Azure Media Services v3-SDK's zijn niet noodzakelijkerwijs thread-veilig. Bij het ontwikkelen van een toepassing met meerdere threads, moet u uw eigen logica voor het synchroniseren van thread voor het beveiligen van de client of het gebruik van een nieuw object AzureMediaServicesClient per thread toevoegen. Ook moet u voorzichtig met multithreading problemen geïntroduceerd door optionele objecten die worden geleverd door uw code aan de client (zoals een instantie van HttpClient in .NET).

In dit onderwerp vindt u koppelingen naar de SDK's, hulpprogramma's, handleidingen.

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met het ontwikkelen met Media Services, hebt u het volgende nodig:

- Een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- [Meer informatie over basisconcepten](concepts-overview.md)
- Beoordeling [ontwikkelen met Media Services v3 API's](media-services-apis-overview.md)
- [Een Azure Media Services-account maken - CLI](create-account-cli-how-to.md)

## <a name="start-developing-with-sdks"></a>Begin met ontwikkelen met SDK's

### <a name="net"></a>.NET

Gebruik [.NET SDK](https://aka.ms/ams-v3-dotnet-sdk) naar [verbinding met Media Services](configure-connect-dotnet-howto.md).

De Media Services verkennen [naslaginformatie over .NET](https://aka.ms/ams-v3-dotnet-ref) documentatie.

### <a name="java"></a>Java

Gebruik [Java SDK](https://aka.ms/ams-v3-java-sdk) naar [verbinding met Media Services](configure-connect-java-howto.md).

Controleer de Media Services [naslaginformatie over Java](https://aka.ms/ams-v3-java-ref) documentatie.

### <a name="nodejs"></a>Node.js

Gebruik [Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk) naar [verbinding met Media Services](configure-connect-nodejs-howto.md).

De Media Services verkennen [naslaginformatie over Node.js](https://aka.ms/ams-v3-nodejs-ref) documentatie en bekijk [voorbeelden](https://github.com/Azure-Samples/media-services-v3-node-tutorials) die laten zien hoe u het gebruik van Media Services-API met node.js.

### <a name="python"></a>Python

Gebruik [Python-SDK](https://aka.ms/ams-v3-python-sdk).

Controleer de Media Services [naslaginformatie over Python](https://aka.ms/ams-v3-python-ref) documentatie.

### <a name="go"></a>Aan de slag

Gebruik [Go SDK](https://aka.ms/ams-v3-go-sdk).

Controleer de Media Services [gaat verwijzing](https://aka.ms/ams-v3-go-ref) documentatie.

### <a name="ruby"></a>Ruby

Gebruik [Ruby-SDK](https://aka.ms/ams-v3-ruby-sdk).

## <a name="azure-media-services-explorer"></a>Azure Media Services Explorer

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) is een hulpprogramma beschikbaar voor Windows-klanten die kennis willen maken met Media Services. AMSE is een Winforms/C#-toepassing voor het uploaden, downloaden, coderen en streamen van VOD en live-inhoud met Media Services. Het AMSE-hulpprogramma is voor clients die Media Services willen testen zonder code te schrijven. De AMSE-code wordt geleverd als een bron voor klanten die willen ontwikkelen met Media Services.

AMSE is een Open-Source-project, en dus wordt er ondersteuning geboden door de community (problemen kunnen worden gemeld aan https://github.com/Azure/Azure-Media-Services-Explorer/issues). Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [veelgestelde vragen over de gedragscode](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met opencode@microsoft.com als u aanvullende vragen of opmerkingen hebt.

## <a name="next-steps"></a>Volgende stappen

[Overzicht](media-services-overview.md)
