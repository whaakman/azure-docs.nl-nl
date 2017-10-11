---
title: Azure Media Services-foutcodes codering | Microsoft Docs
description: In dit onderwerp worden de foutcodes die kunnen worden geretourneerd voor het geval een fout tijdens het codering taak uitvoeren opgetreden is...
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: f4fd2212d19f89148dde08c75c5a48cdd322d029
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="encoding-error-codes"></a>Foutcodes voor codering

De volgende tabel bevat de foutcodes die kunnen worden geretourneerd voor het geval een fout tijdens de uitvoering van de taak voor codering opgetreden is.  Als u details van fouten in uw .NET-code, gebruikt de [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) klasse. Als u details van fouten in de REST-code, gebruikt de [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST-API.

| ErrorDetail.Code | Mogelijke oorzaken voor fout |
| --- | --- |
| Onbekend |Onbekende fout tijdens het uitvoeren van de taak |
| ErrorDownloadingInputAssetMalformedContent |Categorie van fouten die betrekking heeft op fouten bij het downloaden van de invoer asset zoals namen van de beschadigde bestanden, nul lengte bestanden, onjuist opgemaakt enzovoort. |
| ErrorDownloadingInputAssetServiceFailure |De categorie van fouten die betrekking heeft op problemen aan de kant van de service - voorbeeld netwerk of opslag fouten tijdens het downloaden. |
| ErrorParsingConfiguration |Categorie van fouten waar de taak <see cref="MediaTask.PrivateData"/> (configuratie) is niet geldig, bijvoorbeeld de configuratie is niet een geldig systeem voorinstelling of bevat ongeldige XML. |
| ErrorExecutingTaskMalformedContent |De categorie van fouten tijdens het uitvoeren van de taak waar problemen binnen de invoer mediabestanden fout veroorzaken. |
| ErrorExecutingTaskUnsupportedFormat |Categorie van fouten waar de bestanden die worden geleverd door de Mediaprocessor kan niet worden verwerkt - media-indeling niet ondersteund of komt niet overeen met de configuratie. Bijvoorbeeld, probeert te produceren van een alleen audio-uitvoer van een activum met alleen video |
| ErrorProcessingTask |De categorie van andere fouten die de Mediaprocessor tegenkomt tijdens het verwerken van de taak die niet aan de inhoud. |
| ErrorUploadingOutputAsset |Categorie van fouten tijdens het uploaden van de uitvoerasset |
| ErrorCancelingTask |Categorie van fouten voor fouten bij het annuleren van de taak |
| TransientError |Categorie van fouten voor tijdelijke problemen (bv. tijdelijke netwerkproblemen met Azure Storage) |

Voor hulp van de **Media Services** team, open een [ondersteunen ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde codering taken uitvoeren met Media Encoder Standard standaardinstellingen aanpassen](media-services-custom-mes-presets-with-dotnet.md)
* [Quota's en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
