---
title: Een servicetoepassing registreren in Azure Active Directory - API van Azure voor FHIR
description: In dit artikel wordt uitgelegd hoe u een servicetoepassing registreren in Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 19f730c1d8a0fc0f809e8e16016795e725d80b61
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824093"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Een service-client-toepassing registreren in Azure Active Directory

In dit artikel leert u hoe u een service-client-toepassing registreren in Azure Active Directory. Client-toepassingsregistraties zijn Azure Active Directory representaties van toepassingen die kunnen worden gebruikt om te verifiëren en verkrijgen van tokens. Een serviceclient is bedoeld om te worden gebruikt door een toepassing om te verkrijgen van een toegangstoken zonder interactieve verificatie van een gebruiker. Hiermee worden bepaalde machtigingen van de toepassing hebt en een toepassingsgeheim (wachtwoord) gebruikt bij het ophalen van tokens voor toegang.

Volg onderstaande stappen voor het maken van een nieuwe serviceclient.

## <a name="app-registrations-in-azure-portal"></a>App-registraties in Azure portal

1. Klik in de [Azure-portal](https://portal.azure.com) in het navigatiepaneel aan de linkerkant op **Azure Active Directory**.

2. In de **Azure Active Directory** blade op **(Preview) van de App-registraties**:

    ![Azure-portal. Nieuwe App-registratie.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Klik op **registratie van nieuwe**.

## <a name="service-client-application-details"></a>Details van Service-client-toepassing

* De serviceclient moet een weergavenaam en u kunt ook een antwoord-URL opgeven, maar het wordt normaal gesproken niet worden gebruikt.

    ![Azure-portal. Nieuwe Service Client App-registratie.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>API-machtigingen

U moet de service-client verlenen toepassingsrollen. 

1. Open de **API-machtigingen** en selecteer uw [FHIR API Resource Toepassingsregistratie](register-resource-azure-ad-client-app.md):

    ![Azure-portal. Service-Client-API-machtigingen](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Selecteer de rollen van de toepassing u van de namen die zijn gedefinieerd op de resourcetoepassing:

    ![Azure-portal. Machtigingen voor service-Client van de toepassing](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Toestemming verlenen tot de toepassing. Als u niet de vereiste machtigingen hebt, neem contact op met uw Azure Active Directory-beheerder:

    ![Azure-portal. Beheerderstoestemming voor service-Client](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Toepassingsgeheim

De serviceclient moet een geheim (wachtwoord), die u gebruikt bij het ophalen van tokens.

1. Klik op **certificaten &amp; geheimen**

2. Klik op **nieuwe clientgeheim**

    ![Azure-portal. Clientgeheim service](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Geef een duur van de geheime sleutel.

4. Zodra deze is gegenereerd, wordt deze alleen weergegeven eenmaal in de portal. Maak een notitie van deze en op te slaan een veilig.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een service-client-toepassing registreren in Azure Active Directory. Vervolgens implementeert u een FHIR-API in Azure.
 
>[!div class="nextstepaction"]
>[Open Source-FHIR-server implementeren](fhir-oss-powershell-quickstart.md)