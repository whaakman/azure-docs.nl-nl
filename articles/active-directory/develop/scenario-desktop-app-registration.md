---
title: Desktop-app dat aanroepen van web-API's (app-registratie) - Microsoft identity-platform
description: Informatie over het bouwen van een bureaublad-app dat aanroepen van web-API's (app-registratie)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5da934709274d90668d94dfea3a9c223e191d032
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076058"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Desktop-app dat aanroepen van web-API's - app-registratie

In dit artikel bevat de app-registratie van specifieke kenmerken voor een bureaubladtoepassing.

## <a name="supported-accounts-types"></a>Typen ondersteunde accounts

De accounttypen worden ondersteund in bureaubladtoepassing, is afhankelijk van de ervaring die u licht wilt-up, en daarom op de stromen u wilt gebruiken.

### <a name="audience-for-interactive-token-acquisition"></a>Voor interactieve ophalen van tokens

Als uw bureaubladtoepassing interactieve verificatie gebruikt, kunt u zich in de gebruikers uit een [accounttype](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)

### <a name="audience-for-desktop-app-silent-flows"></a>De doelgroep voor desktop-app op de achtergrond stromen

- Als u van plan bent om geïntegreerde Windows-verificatie of gebruikersnaam en wachtwoord te gebruiken, moet uw toepassing aan te melden bij gebruikers in uw eigen tenant (LOB-ontwikkelaars), of in Azure Active directory-organisaties (ISV-scenario). Deze verificatiestromen worden niet ondersteund voor persoonlijke Microsoft-accounts
- Als u de stroom van het apparaat gebruikt wilt, u kunt zich niet in de gebruikers met hun persoonlijke Microsoft-account nog
- Als u gebruikers met sociale identiteiten doorgeven van een B2C-instantie en het beleid voor aanmelden, kunt u alleen de verificatie van de interactieve en gebruikersnaam-wachtwoord.

## <a name="redirect-uris"></a>Omleidings-URI's

Opnieuw afhankelijk de omleidings-URI's in desktop toepassing van de stroom die u wilt gebruiken.

- Als u de interactieve verificatie, moet u gebruiken `https://login.microsoftonline.com/common/oauth2/nativeclient`. U kunt deze configuratie bereiken door te klikken op de bijbehorende URL in de **verificatie** sectie voor uw toepassing
  
  > [!IMPORTANT]
  > Vandaag MSAL.NET maakt gebruik van een andere omleidings-URI in die wordt uitgevoerd op Windows-bureaubladtoepassingen standaard (`urn:ietf:wg:oauth:2.0:oob`). In de toekomst we willen u kunt deze standaardwaarde wijzigen en daarom raden wij aan dat u `https://login.microsoftonline.com/common/oauth2/nativeclient`

- Als uw app alleen van geïntegreerde Windows-verificatie, gebruikersnaam en wachtwoord of apparaat Code Flow gebruikmaakt, moet u niet een omleidings-URI voor uw toepassing registreren. Inderdaad, deze stromen doen een retour naar het v2.0-eindpunt van Microsoft identity-platform en uw toepassing wordt niet terug worden aangeroepen op een specifieke URI. Om u te onderscheiden van een vertrouwelijke clientstroom toepassing, dat niet beschikt over omleidings-URI's beide (de referentie stroom gebruikt in toepassingen daemon), moet u express dat uw toepassing een openbare client-toepassing is. Deze configuratie wordt bereikt door te gaan naar de **verificatie** sectie voor uw toepassing en in de **geavanceerde instellingen** subsectie, kiest u **Ja**, op de vraag **Toepassing behandelen als een openbare client** (in de **standaard clienttype** lid)

  ![Openbare client toestaan](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API-machtigingen

Bureaubladtoepassingen aanroepen van API's namens de aangemelde gebruiker. Ze moeten overgedragen machtigingen aanvragen. Deze machtigingen van de toepassing geen aanvragen (die alleen worden verwerkt in [daemon toepassingen](scenario-daemon-overview.md))

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Desktop-app - app-configuratie](scenario-desktop-app-configuration.md)
