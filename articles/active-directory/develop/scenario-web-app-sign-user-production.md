---
title: Web-app waarmee gebruikers zich aanmeldt (verplaatsen naar productie) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app die zich aanmeldt gebruikers (verplaatsen naar productie)
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
ms.openlocfilehash: d41ad2518f885bbaa02dda3b01f0c02e9fc1d217
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074708"
---
# <a name="web-app-that-signs-in-users---move-to-production"></a>Web-app die zich in gebruikers - verplaatsen naar productie

Nu dat u hoe u om een token weet voor het aanroepen van web-API's te verkrijgen, informatie over het verplaatsen naar productie.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Volgende stappen

### <a name="calling-web-apis-scenario"></a>Aanroepen web-API's-scenario

Zodra uw web-app zich aanmeldt gebruikers kunt aanroepen web-API's namens de aangemelde gebruikers. Aanroepen web-API's van de web-app is het object van het volgende scenario:

> [!div class="nextstepaction"]
> [Web-app die web-API's aanroept](scenario-web-app-call-api-overview.md)

### <a name="deep-dive---web-app-tutorial"></a>Deep dive - zelfstudie-web-app

Meer informatie over andere manieren om gebruikers aanmelden met de ASP.NET Core-zelfstudie: [ms-identity-aspnetcore-Web-App-tutorial](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial). Dit is een progressieve zelfstudie met productiecode gereed voor een web-app met inbegrip van het toevoegen van aanmelding.

<!--- Removed the diagram as it's already shown in the above link to GitHub

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
