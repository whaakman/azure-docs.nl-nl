---
title: Een vertrouwelijke client-toepassing registreren in Azure Active Directory - API van Azure voor FHIR
description: In dit artikel wordt uitgelegd hoe u een vertrouwelijke client-toepassing registreren in Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 064cf4bb0b6e9454a4049cc6c32b51b09e5b2a53
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824032"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Een vertrouwelijke client-App in Azure Active Directory registreren

In dit artikel leert u hoe u een vertrouwelijke client-toepassing registreren in Azure Active Directory. De registratie van een client-toepassing is een Azure Active Directory-representatie van een toepassing die kan worden gebruikt om te verifiëren namens een gebruiker en de aanvraag voor toegang tot [resource toepassingen](register-resource-azure-ad-client-app.md). Een vertrouwelijke client-toepassing is een toepassing die vertrouwd worden kan voor een geheim dat geheim presenteren bij het aanvragen van tokens voor toegang. Voorbeelden van vertrouwelijke toepassingen zijn server-side '-toepassingen.

Voor het registreren van een nieuwe vertrouwelijke toepassing in de portal, de volgende stappen uit te voeren.

## <a name="app-registrations-in-azure-portal"></a>App-registraties in Azure portal

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Azure Active Directory**.

2. In de **Azure Active Directory** blade op **(Preview) van de App-registraties**:

    ![Azure-portal. Nieuwe App-registratie.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klik op de **registratie van nieuwe**.

## <a name="register-a-new-application"></a>Een nieuwe toepassing registreren

1. De toepassing een naam geven.

2. Geef een antwoord-URL. Deze informatie kunnen later worden gewijzigd, maar als u de antwoord-URL van uw toepassing weet, voert u deze nu.

    ![Nieuwe vertrouwelijke Client App-registratie.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)

## <a name="api-permissions"></a>API-machtigingen

Vervolgens voegt u API-machtigingen toe:

1. Open de **API-machtigingen** en selecteer uw [FHIR API Resource Toepassingsregistratie](register-resource-azure-ad-client-app.md):

    ![Vertrouwelijke client. API-machtigingen](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

2. Klik op **een machtiging toevoegen**

3. Selecteer uw API-resource:

    ![Vertrouwelijke client. Mijn API's](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-MyApis.png)

4. Bereiken (machtigingen) die de vertrouwelijke toepassing zou het mogelijk om te vragen namens een gebruiker selecteren:

    ![Vertrouwelijke client. Gedelegeerde machtigingen](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-DelegatedPermissions.png)

## <a name="application-secret"></a>Toepassingsgeheim

1. Maak een toepassingsgeheim (clientgeheim):

    ![Vertrouwelijke client. Toepassingsgeheim](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Geef een beschrijving en de duur van de geheime sleutel.

3. Wanneer gegenereerd, wordt deze slechts eenmaal in de portal worden weergegeven. Maak een notitie van deze en bewaar deze zorgvuldig.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe een vertrouwelijke client-toepassing registreren in Azure Active Directory. Vervolgens implementeert u een FHIR-API in Azure.
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-oss-powershell-quickstart.md)