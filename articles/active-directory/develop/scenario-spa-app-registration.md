---
title: Toepassing met één pagina (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een toepassing met één pagina (App-registratie)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b1faf4669dca2aaaf3f873e66f859473ccd99f10
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074828"
---
# <a name="single-page-application---app-registration"></a>Single-page toepassing - app-registratie

Deze pagina leest u de details van de app-registratie voor een toepassing met één pagina (SPA).

Volg de stappen voor het [een nieuwe toepassing registreren bij Microsoft identity-platform](quickstart-register-app.md), en selecteert u de ondersteunde accounts voor uw toepassing. De beveiligd-WACHTWOORDVERIFICATIE-scenario kunt ondersteuning voor verificatie met een account in uw organisatie of elke organisatie en persoonlijke Microsoft-accounts.

Hierna leert u de specifieke aspecten van de registratie van toepassingen die betrekking hebben op één pagina toepassingen.

## <a name="register-a-redirect-uri"></a>Registreren van een omleidings-URI

De impliciete stroom verzendt de tokens in een omleiding naar de single-page-toepassing die wordt uitgevoerd in een webbrowser. Het is daarom belangrijk noodzaak voor het registreren van een omleidings-URI waar uw toepassing de tokens kan ontvangen. Zorg ervoor dat de omleiding die URI exact overeen met de URI voor uw toepassing komt.

In de [Azure-portal](https://go.microsoft.com/fwlink/?linkid=2083908), navigeer naar uw geregistreerde toepassing op de **verificatie** pagina van de toepassing, selecteert de **Web** platform en voer de waarde van de Omleidings-URI voor uw toepassing in de **omleidings-URI** veld.

## <a name="enable-the-implicit-flow"></a>De impliciete stroom inschakelen

Op dezelfde **verificatie** pagina onder **geavanceerde instellingen**, moet u ook inschakelen de **impliciete**. Als uw toepassing alleen aanmelden van gebruikers en de ID-tokens ophalen presteert, is voldoende om in te schakelen **ID-tokens** selectievakje.

Als uw toepassing ook toegangstokens moet voor het aanroepen van API's ophalen, controleert u of om in te schakelen de **toegangstokens** ook selectievakje. Zie voor meer informatie, [ID-tokens](./id-tokens.md) en [toegangstokens](./access-tokens.md).

## <a name="api-permissions"></a>API-machtigingen

Toepassingen met één pagina kunnen aanroepen van API's namens de aangemelde gebruiker. Ze moeten overgedragen machtigingen aanvragen. Zie voor meer informatie, [machtigingen voor toegang tot web-API's toevoegen](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [De configuratie van App-code](scenario-spa-app-configuration.md)
