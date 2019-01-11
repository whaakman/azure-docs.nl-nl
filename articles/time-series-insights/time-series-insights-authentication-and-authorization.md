---
title: Over het verifiëren en autoriseren door API in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van verificatie en autorisatie voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/27/2017
ms.custom: seodec18
ms.openlocfilehash: b9f3561155038e91bc278f5d7ddc995adb53ad78
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54197994"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

In dit artikel wordt uitgelegd hoe het configureren van de verificatie en autorisatie gebruikt in een aangepaste toepassing die de Azure Time Series Insights-API aanroept.

## <a name="service-principal"></a>Service-principal

Deze sectie wordt uitgelegd hoe u een toepassing toegang krijgt tot de Time Series Insights-API namens de toepassing wilt configureren. De toepassing kan vervolgens gegevens op te vragen of het publiceren van referentiegegevens in de Time Series Insights-omgeving met de referenties van de toepassing in plaats van gebruikersreferenties.

Wanneer u een toepassing die u toegang Time Series Insights moet hebt, moet u een Azure Active Directory-toepassing instellen en toewijzen van het beleid voor gegevenstoegang in de Time Series Insights-omgeving. Deze methode heeft de voorkeur boven aan de app onder uw eigen referenties te voeren, omdat:

* U kunt machtigingen toewijzen aan de identiteit van de app die verschillen van uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot alleen wat de app is vereist. U kunt bijvoorbeeld toestaan dat de app alleen lezen van gegevens in een specifieke Time Series Insights-omgeving.
* U hoeft niet te wijzigen van de referenties van de app als uw verantwoordelijkheden wijzigt.
* U kunt een certificaat of een Toepassingssleutel gebruiken voor het automatiseren van verificatie wanneer u een script zonder toezicht uitvoert.

In dit artikel wordt beschreven hoe u deze stappen uitvoert via Azure portal. Dit artikel gaat over de toepassing van een één tenant waar de toepassing is bedoeld om uit te voeren in slechts één organisatie. Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

De setup-stroom bestaat uit drie stappen op hoog niveau:

1. Maak een toepassing in Azure Active Directory.
2. Toestaan dat deze toepassing te krijgen tot de Time Series Insights-omgeving.
3. Gebruik de toepassings-ID en sleutel voor een token te verkrijgen de `"https://api.timeseries.azure.com/"` doelgroep of resource. Het token kan vervolgens worden gebruikt voor het aanroepen van de Time Series Insights-API.

Hier volgen de gedetailleerde stappen:

1. Selecteer in de Azure portal, **Azure Active Directory** > **App-registraties** > **nieuwe toepassing registreren**.

   ![Nieuwe toepassing registreren in Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Geef een naam op voor de toepassing, selecteert u het type moet **Web-app / API**, selecteer een geldige URI voor **aanmeldings-URL**, en klikt u op **maken**.

   ![De toepassing maken in Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Selecteer de zojuist gemaakte toepassing en de toepassings-ID kopiëren naar uw favoriete teksteditor.

   ![Kopieer de toepassings-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Selecteer **sleutels**, voer de naam van de sleutel, selecteert u de vervaldatum en klikt u op **opslaan**.

   ![Sleutels van de toepassing selecteren](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Voer de naam van sleutel en de vervaldatum en klik op Opslaan](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. De sleutel kopiëren naar uw favoriete teksteditor.

   ![Kopieer de Toepassingssleutel](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Selecteer voor de Time Series Insights-omgeving, **beleid voor gegevenstoegang** en klikt u op **toevoegen**.

   ![Toegangsbeleid voor nieuwe gegevens toevoegen aan de Time Series Insights-omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. In de **Select User** in het dialoogvenster, plak de naam van de toepassing (uit stap 2) of toepassings-ID (uit stap 3).

   ![Een toepassing niet vinden in het dialoogvenster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Selecteer de rol (**lezer** voor het opvragen van gegevens, **Inzender** voor het opvragen van gegevens en referentiegegevens wijzigen) en klikt u op **Ok**.

   ![Lezer of Inzender kiezen in het dialoogvenster rol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Sla het beleid door te klikken op **Ok**.

10. Gebruik de toepassings-ID (uit stap 3) en de sleutel van de toepassing (uit stap 5) om het token namens de toepassing te verkrijgen. Het token kan vervolgens worden doorgegeven de `Authorization` header wanneer de Time Series Insights-API-aanroepen van de toepassing.

    Als u C#, kunt u de volgende code om het token namens de toepassing te verkrijgen. Zie voor een compleet voorbeeld [gegevens opvragen met C#](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

De toepassings-ID en sleutel gebruiken in uw toepassing om te verifiëren met Azure Time Series Insight. 

## <a name="next-steps"></a>Volgende stappen
- Zie voor een voorbeeld van code die de Time Series Insights-API aanroept, [gegevens opvragen met C#](time-series-insights-query-data-csharp.md).
- Voor de API-referentie-informatie, Zie [Query API-verwijzing](/rest/api/time-series-insights/ga-query-api).

> [!div class="nextstepaction"]
> [Een service-principal maken](../active-directory/develop/howto-create-service-principal-portal.md)
