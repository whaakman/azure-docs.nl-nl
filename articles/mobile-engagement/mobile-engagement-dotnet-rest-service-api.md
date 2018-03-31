---
title: Met behulp van de REST-API voor toegang tot Azure Mobile Engagement Service API 's
description: Hierin wordt beschreven hoe u de Mobile Engagement REST-API's gebruiken voor toegang tot Azure Mobile Engagement Service API 's
services: mobile-engagement
documentationcenter: mobile
author: wesmc7777
manager: erikre
editor: ''
ms.assetid: e8df4897-55ee-45df-b41e-ff187e3d9d12
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/05/2016
ms.author: wesmc;ricksal
ms.openlocfilehash: c3c0de39ba13ed47345b1042a9dceda075dc50b3
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Met behulp van de REST voor toegang tot Azure Mobile Engagement Service API 's
> [!IMPORTANT]
> Azure Mobile Engagement beëindigen op 3/31/2018. Deze pagina wordt kort na worden verwijderd.
> 

Azure Mobile Engagement biedt de [REST-API van Azure Mobile Engagement](https://msdn.microsoft.com/library/azure/mt683754.aspx) kunt beheren apparaten/te pushen Reach-campagnes enzovoort.

> [!NOTE]
> De Azure Mobile Engagement-service wordt in maart 2018 beëindigd en is momenteel alleen beschikbaar voor bestaande klanten. Zie [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/) voor meer informatie.

Als u niet rechtstreeks gebruik van de REST API's wilt, bieden we ook een [Swagger-bestand](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) waarmee u met hulpprogramma's kunt voor het genereren van SDK's voor uw voorkeurstaal. Wordt u aangeraden de [AutoRest](https://github.com/Azure/AutoRest) hulpprogramma voor het genereren van de SDK van onze Swagger-bestand. We hebben een .NET-SDK op een vergelijkbare manier waarmee u communiceren met deze API's met behulp van een C#-wrapper gemaakt en u hoeft te doen de token verificatie-onderhandeling en vernieuw zelf. Zie [Service API .NET SDK-voorbeeld](mobile-engagement-dotnet-sdk-service-api.md) voor informatie over het gebruik van de .net SDK voor API
