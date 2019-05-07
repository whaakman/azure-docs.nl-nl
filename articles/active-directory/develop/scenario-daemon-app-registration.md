---
title: Daemon-app aanroepen van web API's (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een daemon-app die aanroepen van web-API's - app-registratie
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: 79a355ab226e56a3dde1df5369deda5142d47848
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076238"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Daemon-app dat aanroepen van web-API's - app-registratie

Een daemon-toepassing is dit wat u moet weten bij het registreren van de app.

## <a name="supported-account-types"></a>Ondersteunde accounttypen

Gezien het feit dat de daemon toepassingen alleen zinvol in Azure AD-tenant, bij het maken van de toepassing moet u kiezen:

- een van beide **Accounts in deze organisatie-map alleen**. Deze optie is het meest voorkomende geval is, zoals daemon-toepassingen meestal door line-of-business (LOB)-ontwikkelaars geschreven worden.
- of **Accounts in een organisatie-map**. U maakt deze keuze als u een ISV een hulpprogramma hulpprogramma bieden aan uw klanten. U hebt tenants die klantbeheerders goed te keuren.

## <a name="authentication---no-reply-uri-needed"></a>Verificatie - geen antwoord-URI die nodig zijn

In het geval waarbij uw toepassing vertrouwelijke client gebruikt **alleen** de clientreferenties-stroom, de antwoord-URL niet moet worden geregistreerd. Het is niet nodig voor de toepassing configuration/constructie. De clientreferentiestroom niet gebruiken.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Machtigingen van de API - app-machtigingen en toestemming van een beheerder

Een daemon-toepassing kunt alleen aanvragen voor de machtigingen van de toepassing naar API's (niet-gedelegeerde machtigingen). In de **API machtiging** -pagina voor de registratie van de toepassing, nadat u hebt geselecteerd **toevoegen van een machtiging** en kiest u ervoor gekozen de API-familie, **Toepassingsmachtigingen**, en selecteer vervolgens uw machtigingen

![App-machtigingen en toestemming van een beheerder](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

Daemon toepassingen nodig hebben van een tenant-beheerder die vooraf toestemming geven voor de toepassing die de web-API aanroept. Deze toestemming is opgegeven in dezelfde **API machtiging** pagina, door een tenant-beheerder selecteren **beheerderstoestemming te verlenen *onze organisatie***

Als u een ISV het bouwen van een toepassing met meerdere tenants, wilt u om te controleren of de [implementatie - aanvraag van multitenant-daemon-apps](scenario-daemon-production.md#deployment---case-of-multi-tenant-daemon-apps) alinea.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Daemon-app - configuratie van de app-code](./scenario-daemon-app-configuration.md)
