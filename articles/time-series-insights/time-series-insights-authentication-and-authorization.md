---
title: Over het verifiëren en autoriseren door API in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van verificatie en autorisatie voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 9b6cd993e9f6c6dbf173c161de638c6c4a8b18d3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237058"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

In dit artikel wordt uitgelegd hoe het configureren van de verificatie en autorisatie gebruikt in een aangepaste toepassing die de Azure Time Series Insights-API aanroept.

> [!TIP]
> Meer informatie over [verlenen van toegang tot gegevens](./time-series-insights-data-access.md) aan uw Time Series Insights-omgeving in Azure Active Directory.

## <a name="service-principal"></a>Service-principal

Deze en volgende secties wordt beschreven hoe een toepassing toegang krijgt tot de Time Series Insights-API namens de toepassing wilt configureren. De toepassing kunt en vervolgens query of het publiceren van referentiegegevens in de Time Series Insights-omgeving met de referenties van de toepassing in plaats van gebruikersreferenties.

## <a name="best-practices"></a>Aanbevolen procedures

Wanneer u een toepassing die u toegang Time Series Insights moet hebt:

1. Een Azure Active Directory-app instellen.
1. [Toewijzen van beleid voor gegevenstoegang](./time-series-insights-data-access.md) in de Time Series Insights-omgeving.

Met behulp van de toepassing in plaats van uw gebruikersreferenties is wenselijk sinds:

* U kunt machtigingen toewijzen aan de app-id die, verschilt van uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot alleen wat de app is vereist. U kunt bijvoorbeeld toestaan dat de app alleen lezen van gegevens in een specifieke Time Series Insights-omgeving.
* U hoeft te wijzigen van de app-referenties, als uw verantwoordelijkheden wijzigt.
* U kunt een certificaat of een Toepassingssleutel gebruiken voor het automatiseren van verificatie wanneer u een script zonder toezicht uitvoert.

De volgende secties laten zien hoe u deze stappen uitvoert via Azure portal. Het artikel richt zich op een toepassing één tenant is waar de toepassing is bedoeld om uit te voeren in slechts één organisatie. Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

## <a name="set-up-summary"></a>Instellen van samenvatting

De setup-stroom bestaat uit drie stappen:

1. Maak een toepassing in Azure Active Directory.
1. Toestaan dat deze toepassing te krijgen tot de Time Series Insights-omgeving.
1. De toepassings-ID en sleutel gebruiken om een token uit te verkrijgen `https://api.timeseries.azure.com/`. Het token kan vervolgens worden gebruikt voor het aanroepen van de Time Series Insights-API.

## <a name="detailed-setup"></a>Gedetailleerde installatie

1. Selecteer in de Azure portal, **Azure Active Directory** > **App-registraties** > **nieuwe toepassing registreren**.

   [![Nieuwe toepassing registreren in Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)](media/authentication-and-authorization/active-directory-new-application-registration.png#lightbox)

1. Geef een naam op voor de toepassing, selecteert u het type moet **Web-app / API**, selecteer een geldige URI voor **aanmeldings-URL**, en klikt u op **maken**.

   [![De toepassing maken in Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)](media/authentication-and-authorization/active-directory-create-web-api-application.png#lightbox)

1. Selecteer de zojuist gemaakte toepassing en de toepassings-ID kopiëren naar uw favoriete teksteditor.

   [![Kopieer de toepassings-ID](media/authentication-and-authorization/active-directory-copy-application-id.png)](media/authentication-and-authorization/active-directory-copy-application-id.png#lightbox)

1. Selecteer **sleutels**, voer de naam van de sleutel, selecteert u de vervaldatum en klikt u op **opslaan**.

   [![Sleutels van de toepassing selecteren](media/authentication-and-authorization/active-directory-application-keys.png)](media/authentication-and-authorization/active-directory-application-keys.png#lightbox)

   [![Voer de naam van sleutel en de vervaldatum en klik op Opslaan](media/authentication-and-authorization/active-directory-application-keys-save.png)](media/authentication-and-authorization/active-directory-application-keys-save.png#lightbox)

1. De sleutel kopiëren naar uw favoriete teksteditor.

   [![Kopieer de Toepassingssleutel](media/authentication-and-authorization/active-directory-copy-application-key.png)](media/authentication-and-authorization/active-directory-copy-application-key.png#lightbox)

1. Selecteer voor de Time Series Insights-omgeving, **beleid voor gegevenstoegang** en klikt u op **toevoegen**.

   [![Toegangsbeleid voor nieuwe gegevens toevoegen aan de Time Series Insights-omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. In de **Select User** dialoogvenster vak, plak de toepassingsnaam van de (van **stap 2**) of toepassings-ID (van **stap 3**).

   [![Een toepassing niet vinden in het dialoogvenster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Selecteer de rol (**lezer** voor het opvragen van gegevens, **Inzender** voor het opvragen van gegevens en referentiegegevens wijzigen) en klikt u op **OK**.

   [![Lezer of Inzender kiezen in het dialoogvenster rol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Sla het beleid door te klikken op **OK**.

1. Gebruik de toepassings-ID (van **stap 3**) en sleutel van de toepassing (van **stap 5**) om het token namens de toepassing te verkrijgen. Het token kan vervolgens worden doorgegeven de `Authorization` header wanneer de Time Series Insights-API-aanroepen van de toepassing.

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
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

Gebruik de **toepassings-ID** en **sleutel** in uw toepassing om te verifiëren met Azure Time Series Insight.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een voorbeeld van code die de Time Series Insights-API aanroept, [gegevens opvragen met C#](time-series-insights-query-data-csharp.md).

- Voor de API-referentie-informatie, Zie [Query API-verwijzing](/rest/api/time-series-insights/ga-query-api).

- Meer informatie over het [maken van een service-principal](../active-directory/develop/howto-create-service-principal-portal.md).
