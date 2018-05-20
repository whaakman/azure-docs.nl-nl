---
title: De invoer van een Azure Media Services-taak maken op basis van een URL http (s) | Microsoft Docs
description: Dit onderwerp leest het maken van een taak voor invoer van de URL van een HTTP (s).
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: d429665de64dacc5818d1d26c2a9029531cd39b3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="create-a-job-input-from-an-https-url"></a>Maken van een taak voor invoer van de URL van een HTTP (s)

In Media Services-v3 bij het verzenden van taken voor het verwerken van uw video's, hebt u Media Services uitgelegd waar vind ik het invoervideo. Een van de opties is een HTTP (s)-URL opgeven als een taak invoer (zoals weergegeven in dit voorbeeld). Zie voor een compleet voorbeeld [github voorbeeld](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>.NET-voorbeeld

De volgende code laat zien hoe een taak te maken met een HTTPS-URL invoeren.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Volgende stappen

[De invoer van een taak maken op basis van een lokaal bestand](job-input-from-local-file-how-to.md).
