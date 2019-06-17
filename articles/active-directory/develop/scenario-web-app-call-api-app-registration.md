---
title: Web-app dat aanroepen van web-API's (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een web-app dat aanroepen van web-API's (app-registratie)
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
ms.openlocfilehash: 5becdc287f7cad28aa6c7c07ebc107586e9a2b2a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075188"
---
# <a name="web-app-that-calls-web-apis---app-registration"></a>Web-app dat aanroepen van web-API's - app-registratie

Een Web-app aanroepen van web API's heeft de registratie van de dezelfde als een Web-App-aanmelden-gebruikers. U moet daarom Volg de instructies in [Web-app die zich aanmeldt-gebruikers - app-registratie](scenario-web-app-sign-user-app-registration.md)

Maar omdat de Web-App nu aanroepen van web-API's, wordt een vertrouwelijke client-toepassing. Daarom is er een deel van de extra registratie verplicht: nodig is om te delen van geheimen (clientreferenties) met het Microsoft identity-platform.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API-machtigingen

Web-Apps aanroepen van API's namens de aangemelde gebruiker. Ze moeten overgedragen machtigingen aanvragen. Zie voor meer informatie [machtigingen voor toegang tot web-API's toevoegen](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-web-app-call-api-app-configuration.md)
