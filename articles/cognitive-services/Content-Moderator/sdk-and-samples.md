---
title: Controle-SDK's en voorbeelden van inhoud voor de beheerder van Azure-inhoud | Microsoft Docs
description: Ophalen van SDK's en voorbeelden voor inhoud beheerder
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: 40b8fc0f63383e837f0813f876f20806e6e8c6eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344526"
---
# <a name="content-moderator-sdks-and-samples"></a>Inhoud beheerder SDK's en voorbeelden

## <a name="sdks-for-python-java-nodejs-and-net"></a>SDK's voor Python, Java, Node.js en .NET

- [Inhoud beheerder SDK voor Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Inhoud beheerder SDK voor Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Inhoud beheerder SDK voor Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [Inhoud beheerder SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>Voorbeelden van de .NET SDK

De volgende lijst bevat koppelingen naar de codevoorbeelden die is gebouwd met behulp van de beheerder van Azure Content SDK voor .NET.

- **Hulpbibliotheek**: [maken van een client inhoud beheerder voor gebruik in andere voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Zie [Quick Start](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Toezicht 
- **Afbeelding van toezicht**: [evalueren van een afbeelding voor de inhoud voor volwassenen en mooie, tekst en vlakken](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Zie [Quick Start](image-moderation-quickstart-dotnet.md).
- **Aangepaste installatiekopieën**: [gemiddeld met aangepaste installatiekopie lijsten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Zie [Quick Start](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Er is een maximumlimiet van **5 installatiekopie lijsten** met elke lijst **niet meer dan 10.000 afbeeldingen**.
>

- **Tekst toezicht**: [tekst voor taalgebruik en persoonsgegevens (PII) scherm](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Zie [Quick Start](text-moderation-quickstart-dotnet.md).
- **Aangepaste voorwaarden**: [gemiddeld met aangepaste term lijsten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Zie [Quick Start](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Er is een maximumlimiet van **geeft een lijst van 5 term** met elke lijst **niet meer dan 10.000 voorwaarden**.
>

- **Video toezicht**: [video voor inhoud voor volwassenen en mooie scannen en resultaten](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Zie [Quick Start](video-moderation-api.md).

### <a name="review"></a>Beoordelen
- **Afbeelding van taken**: [Start een controle-taak die gescand en wordt gemaakt van beoordelingen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Zie [Quick Start](moderation-jobs-quickstart-dotnet.md).
- **Afbeelding van beoordelingen**: [beoordelingen voor human in de lus maken](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Zie [Quick Start](moderation-reviews-quickstart-dotnet.md).
- **Video bekijkt**: [maken video bekijkt voor human in de lus](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Zie [Quick Start](video-reviews-quickstart-dotnet.md)
- **De tekst van de video bekijkt**: [maken de tekst van video bekijkt voor human in de lus](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Zie [Quick Start](video-reviews-quickstart-dotnet.md)

Zie van alle .NET-voorbeelden op de [inhoud beheerder .NET-voorbeelden op GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>Voorbeelden van de REST-API in C#

- [Beheer van afbeeldingen](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Beheer van tekst](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Beheer van video](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Beoordelingen van afbeeldingen](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [De installatiekopie van taken](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Bekijk voor scenario's van deze voorbeelden, de [op aanvraag webinar](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Zelfstudies
- [e-commerce catalogus toezicht](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Zie [zelfstudie](ecommerce-retail-catalog-moderation.md).
- [Facebook inhoud toezicht](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Zie [zelfstudie](facebook-post-moderation.md).
- [Video's en de tekst van toezicht en bekijk oplossing](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Zie [zelfstudie](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Op aanvraag webinars
- [Machine-ondersteunde inhoud toezicht op schaal met de beheerder van de inhoud](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
