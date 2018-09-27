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
ms.date: 09/24/2018
ms.author: juliako
ms.openlocfilehash: 8b8bfb578b9a7190e93da721531041bb93a9a03c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224729"
---
# <a name="create-a-job-input-from-an-https-url"></a>De Taakinvoer van een maken van een HTTPS-URL

Wanneer u taken voor het verwerken van uw video's, verzendt in Media Services v3 hebt u Media Services zien waar vind ik de invoervideo. Een van de opties is het opgeven van een HTTP (s)-URL als een taak invoeren (zoals weergegeven in dit voorbeeld). Houd er rekening mee dat op dit moment AMS v3 ondersteunt geen gesegmenteerde overdrachtscodering via HTTPS-URL's. Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code toont hoe u een taak maken met een HTTPS-URL invoeren.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Volgende stappen

[De Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).
