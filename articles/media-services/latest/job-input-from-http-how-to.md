---
title: Maak een Azure Media Services-taak van een HTTPS-URL | Microsoft Docs
description: In dit onderwerp laat zien hoe de Taakinvoer van een maken via een URL voor HTTP (s).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: 5e301a671551ee65e8dc56ca6f86e273fe2f6241
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417173"
---
# <a name="create-a-job-input-from-an-https-url"></a>De Taakinvoer van een maken van een HTTPS-URL

Wanneer u taken voor het verwerken van uw video's, verzendt in Media Services v3 hebt u Media Services zien waar vind ik de invoervideo. Een van de opties is het opgeven van een HTTPS-URL als een taak invoeren (zoals weergegeven in dit voorbeeld). Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's. Zie voor een compleet voorbeeld [GitHub voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code toont hoe u een taak maken met een HTTPS-URL invoeren.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Foutcodes voor taak

Zie [foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Volgende stappen

[De Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).
