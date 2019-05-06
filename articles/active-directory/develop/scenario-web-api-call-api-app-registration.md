---
title: Web-API die downstream web-aanroepen van API's (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een web-API die downstream web-aanroepen van API's (app-registratie)
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
ms.openlocfilehash: fb03869cdea2150b6e922e2d6d81e577c3be02da
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075383"
---
# <a name="web-api-that-calls-web-apis---app-registration"></a>Web-API die aanroepen van web-API's - app-registratie

Een web-API die downstream web-API's aanroept heeft de registratie van de dezelfde als een beveiligde web-API. Daarom moet u de instructies in [beveiligd Web API - app-registratie](scenario-protected-web-api-app-registration.md).

Echter, omdat de web-app nu aanroepen van web-API's, wordt een vertrouwelijke client-toepassing. Daarom is er extra registratie-informatie die is vereist: de app moet geheimen (clientreferenties) delen met de Microsoft identity-platform.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API-machtigingen

Web-Apps aanroepen van API's namens de gebruiker voor wie het bearer-token is ontvangen. Ze moeten overgedragen machtigingen aanvragen. Zie voor meer informatie, [machtigingen voor toegang tot web-API's toevoegen](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-web-api-call-api-app-configuration.md)
