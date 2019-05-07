---
title: Web-API die aanroepen van web-API's (verplaatsen naar productie) - Microsoft identity-platform
description: Meer informatie over het bouwen van een web-API die aanroepen downstream web-API's (verplaatsen naar productie).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7de892a0851e556c5046fe4b3691f39b42a9d237
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074753"
---
# <a name="web-api-that-calls-web-apis---move-to-production"></a>Web-API die web-API's - aanroept verplaatsen naar productie

Zodra een token voor het aanroepen van web-API's die u hebt gekocht, kunt u uw app kunt verplaatsen naar productie.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="learn-more"></a>Meer informatie

Nu dat u weet dat de basisprincipes van over het aanroepen van web-API's van uw eigen web-API, u mogelijk geïnteresseerd is in deze zelfstudie, waarin de code die is gebruikt voor het bouwen van een beveiligde web-API aanroepen van web-API's.

| Voorbeeld | Platform | Description |
|--------|----------|-------------|
| [active-directory-aspnetcore-webapi-tutorial-v2](https://github.com/Azure-Samples/active-directory-dotnet-native-aspnetcore-v2/tree/master/2.%20Web%20API%20now%20calls%20Microsoft%20Graph) | 2.2 van ASP.NET Core Web-API, bureaublad (WPF) | 2.2 van ASP.NET Core Web-API aanroepen van Microsoft Graph, zelf aangeroepen vanuit een WPF-toepassing met behulp van de Microsoft identity-platform (v2.0) |
