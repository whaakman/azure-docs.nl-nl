---
title: Azure Media Services encoding foutcodes | Microsoft Docs
description: In dit onderwerp worden de foutcodes die kunnen worden geretourneerd in het geval een fout tijdens het coderen taak uitvoeren opgetreden is...
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 5ab537d94c56f1d5a9a0b32e3a18a416baf74a75
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995744"
---
# <a name="encoding-error-codes"></a>Foutcodes voor codering

De volgende tabel bevat de foutcodes die kunnen worden geretourneerd in het geval een fout tijdens de uitvoering van de taak codering opgetreden is.  Als u details van fouten in uw .NET-code, gebruikt de [ErrorDetails](https://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) klasse. Als u details van fouten in uw code REST, gebruikt de [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST-API.

| ErrorDetail.Code | Mogelijke oorzaken voor fout |
| --- | --- |
| Onbekend |Onbekende fout tijdens het uitvoeren van de taak |
| ErrorDownloadingInputAssetMalformedContent |Categorie van fouten die betrekking heeft op fouten bij het downloaden van invoer asset, zoals onjuiste bestandsnamen, nul bestanden met een lengte, onjuist opgemaakt enzovoort. |
| ErrorDownloadingInputAssetServiceFailure |De categorie van fouten die betrekking heeft op problemen aan de servicezijde - voorbeeld van de netwerk- of fouten tijdens het downloaden. |
| ErrorParsingConfiguration |Categorie van fouten waar de taak <see cref="MediaTask.PrivateData"/> (configuratie) is niet geldig, bijvoorbeeld de configuratie is niet een geldig systeem vooraf ingesteld of bevat ongeldige XML. |
| ErrorExecutingTaskMalformedContent |De categorie van fouten tijdens het uitvoeren van de taak waar de problemen in de invoer mediabestanden mislukken. |
| ErrorExecutingTaskUnsupportedFormat |Categorie van fouten waarbij de opgegeven bestanden kan niet worden verwerkt door de Mediaprocessor - media-indeling niet ondersteund of komt niet overeen met de configuratie. Bijvoorbeeld, probeert te maken van een alleen-audio-uitvoer van een asset die alleen video heeft |
| ErrorProcessingTask |De categorie van andere fouten die de Mediaprocessor tegenkomt tijdens de verwerking van de taak die niet gerelateerd aan de inhoud zijn. |
| ErrorUploadingOutputAsset |Categorie van fouten tijdens het uploaden van de uitvoerasset |
| ErrorCancelingTask |Categorie van fouten voor fouten bij het annuleren van de taak |
| TransientError |Categorie van fouten voor tijdelijke problemen (zoals) tijdelijke netwerkproblemen met Azure Storage) |

Om de hulp van de **mediaservices** team, open een [ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde codering taken uitvoeren door aan te passen voorinstellingen voor Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md)
* [Quota en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
