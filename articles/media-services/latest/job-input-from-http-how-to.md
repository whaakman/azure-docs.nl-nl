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
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: bac11640f5256223c1053f03da42c763508af98f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377375"
---
# <a name="create-a-job-input-from-an-https-url"></a>De Taakinvoer van een maken van een HTTPS-URL

Wanneer u taken voor het verwerken van uw video's, verzendt in Media Services v3 hebt u Media Services zien waar vind ik de invoervideo. Een van de opties is het opgeven van een HTTP (s)-URL als een taak invoeren (zoals weergegeven in dit voorbeeld). Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's. Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code toont hoe u een taak maken met een HTTPS-URL invoeren.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Volgende stappen

[De Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).
