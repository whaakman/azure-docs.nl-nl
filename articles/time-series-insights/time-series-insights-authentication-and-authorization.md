---
title: "Het verifiëren en autoriseren door Azure Time Series Insights-API"
description: In dit artikel wordt beschreven hoe configureren voor verificatie en autorisatie voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
services: time-series-insights
ms.service: time-series-insights
author: dmdenmsft
ms.author: dmden
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: dd78e1e726029aaceef5aff0e0eed84acac646cf
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

Dit artikel wordt uitgelegd hoe u configureert de authenticatie en autorisatie gebruikt in een aangepaste toepassing die de Azure Time Series Insights-API aanroept.

## <a name="service-principal"></a>Service-principal

Deze sectie wordt uitgelegd hoe een toepassing voor toegang tot de inzicht API van tijd reeks namens de toepassing wilt configureren. De toepassing kan vervolgens een query over gegevens of referentiegegevens publiceren in de omgeving Time Series Insights met referenties van de toepassing in plaats van gebruikersreferenties.

Wanneer u een toepassing die u toegang Time Series Insights moet hebt, moet u een Azure Active Directory-toepassing instellen en toewijzen van het toegangsbeleid van de gegevens in de Time Series Insights-omgeving. Deze aanpak is voorkeur boven het uitvoeren van de app met uw eigen referenties, omdat:

* U kunt machtigingen toewijzen aan de identiteit van de app die afwijken van uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot alleen wat de app is vereist. U kunt bijvoorbeeld toestaan dat de app alleen lezen van gegevens in een bepaalde tijd reeks Insights-omgeving.
* U hoeft niet te wijzigen van de referenties van de app als uw verantwoordelijkheden wijzigt.
* U kunt een certificaat of een Toepassingssleutel automatiseren verificatie wanneer u een script zonder toezicht uitvoert.

Dit artikel laat zien hoe u deze stappen uitvoert via de Azure portal. Het is gericht op een één-tenant-toepassing waarin de toepassing is bedoeld om uit te voeren in slechts één organisatie. Doorgaans gebruikt u toepassingen voor één tenant voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

De setup-stroom bestaat uit drie hoofdstappen:

1. Een toepassing maakt in Azure Active Directory.
2. De autorisatie voor deze toepassing te krijgen tot de Time Series Insights-omgeving.
3. Gebruik de toepassings-ID en de sleutel te verkrijgen van een token dat de `"https://api.timeseries.azure.com/"` doelgroep of resource. Het token kan vervolgens worden gebruikt de Time Series Insights-API aan te roepen.

Hier volgen gedetailleerde stappen:

1. Selecteer in de Azure-portal **Azure Active Directory** > **App registraties** > **registratie van de nieuwe toepassing**.

   ![Registratie van de nieuwe toepassing in Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Geef een naam op voor de toepassing, selecteer het type moet **Web-app / API**, selecteer een geldige URI voor **aanmeldings-URL**, en klik op **maken**.

   ![De toepassing in Azure Active Directory maken](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Selecteer de zojuist gemaakte toepassing en kopieer de toepassings-ID naar uw favoriete teksteditor.

   ![Kopieer de toepassings-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Selecteer **sleutels**, voer de naam van de sleutel, selecteert u de vervaldatum en klikt u op **opslaan**.

   ![Sleutels van de toepassing selecteren](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Voer de sleutelnaam en de vervaldatum en klik op Opslaan](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Kopieer de sleutel naar uw favoriete teksteditor.

   ![Kopieer de Toepassingssleutel](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Selecteer voor de omgeving Time Series Insights **Gegevenstoegangsbeleid** en klik op **toevoegen**.

   ![Nieuwe gegevens toegangsbeleid toevoegen aan de Time Series Insights-omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. In de **gebruiker selecteren** in het dialoogvenster, plak de naam van de toepassing (uit stap 2) of toepassings-ID (uit stap 3).

   ![Een toepassing niet vinden in het dialoogvenster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Selecteer de rol (**lezer** voor het opvragen van gegevens, **Inzender** voor het opvragen van gegevens en het wijzigen van referentiegegevens) en klik op **Ok**.

   ![Lezer of bijdrager kiezen in het dialoogvenster rol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Het beleid opslaan door te klikken op **Ok**.

10. Gebruik de toepassings-ID (uit stap 3) en de sleutel van de toepassing (uit stap 5) te verkrijgen van het token voor de toepassing is. Het token kan vervolgens worden doorgegeven de `Authorization` header wanneer de toepassing de Time Series Insights-API aanroept.

    Als u van C# gebruikmaakt, kunt u de volgende code te verkrijgen van het token voor de toepassing is. Zie voor een compleet codevoorbeeld [opvragen van gegevens met C#](time-series-insights-query-data-csharp.md).

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
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

De toepassings-ID en de sleutel in uw toepassing voor verificatie met Azure Time Series inzicht gebruiken. 

## <a name="next-steps"></a>Volgende stappen
- Zie voor een voorbeeld van code die de Time Series Insights-API aanroept, [opvragen van gegevens met C#](time-series-insights-query-data-csharp.md).
- Zie voor API-naslaginformatie, [Query API-referentiemateriaal](/rest/api/time-series-insights/time-series-insights-reference-queryapi).

> [!div class="nextstepaction"]
> [Een service-principal maken](../azure-resource-manager/resource-group-create-service-principal-portal.md)
