---
title: Verifiëren en autoriseren met behulp van een API in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe het configureren van verificatie en autorisatie voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/29/2019
ms.custom: seodec18
ms.openlocfilehash: 899bcffaf3a54bd541d488f99c35ec6721d751ca
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543887"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

Dit document wordt beschreven hoe u een app registreren in Azure Active Directory met behulp van de nieuwe Azure Active Directory-blade. Apps die zijn geregistreerd bij Azure Active Directory zodat gebruikers kunnen worden geverifieerd bij en worden geautoriseerd voor het gebruik de Azure Time Series Insight API die zijn gekoppeld aan een Time Series Insights-omgeving.

## <a name="service-principal"></a>Service-principal

De volgende secties wordt beschreven hoe u een toepassing toegang krijgt tot de Time Series Insights-API namens een app configureren. De toepassing kan vervolgens opvragen of referentiegegevens in de Time Series Insights-omgeving met behulp van de referenties van een eigen toepassing via Azure Active Directory te publiceren.

## <a name="summary-and-best-practices"></a>Samenvatting en aanbevolen procedures

De stroom van Azure Active Directory-app-registratie omvat drie belangrijke stappen.

1. [Een toepassing registreren](#azure-active-directory-app-registration) in Azure Active Directory.
1. De toepassing te hebben [toegang tot gegevens in de Time Series Insights-omgeving](#granting-data-access).
1. Gebruik de **toepassings-ID** en **Clientgeheim** te verkrijgen van een token van `https://api.timeseries.azure.com/` in uw [client-app](#client-app-initialization). Het token kan vervolgens worden gebruikt voor het aanroepen van de Time Series Insights-API.

Per **stap 3**, kunt u scheiden van uw toepassing en de referenties van de gebruiker:

* Machtigingen toewijzen aan de app-id die, verschilt van uw eigen machtigingen. Deze machtigingen zijn meestal beperkt tot alleen wat de app is vereist. Bijvoorbeeld, kunt u de app om gegevens te lezen alleen vanaf een specifieke Time Series Insights-omgeving.
* Isoleren van de app-beveiliging van referenties voor het maken van een gebruiker de verificatie met behulp van een **Clientgeheim** of beveiligingscertificaat. Als gevolg hiervan zijn de referenties van de toepassing niet afhankelijk van de referenties van een specifieke gebruiker. Als de rol van de gebruiker wordt gewijzigd, wordt de toepassing niet per se vereisen nieuwe referenties of verdere configuratie. Als de gebruiker het wachtwoord wijzigt, wordt alle toegang tot de toepassing geen nieuwe referenties of sleutels vereist.
* Uitvoeren van een script zonder toezicht met een **Clientgeheim** of beveiliging van het certificaat in plaats van een specifieke gebruiker referenties (waarvoor ze aanwezig zijn).
* Gebruik een beveiligingscertificaat in plaats van een wachtwoord voor het beveiligen van toegang tot uw Azure Time Series Insights-API.

> [!IMPORTANT]
> Ga als volgt het principe van **scheiding van belangen** (zoals beschreven in dit scenario hierboven) wanneer het beveiligingsbeleid van uw Azure Time Series Insights configureren.

> [!NOTE]
> * Het artikel richt zich op een toepassing één tenant is waar de toepassing is bedoeld om uit te voeren in slechts één organisatie.
> * Doorgaans gebruikt u één tenant toepassingen voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

## <a name="detailed-setup"></a>Gedetailleerde installatie

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory-app-registratie

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Gegevenstoegang verlenen

1. Selecteer voor de Time Series Insights-omgeving, **beleid voor gegevenstoegang** en selecteer **toevoegen**.

   [![Toegangsbeleid voor nieuwe gegevens toevoegen aan de Time Series Insights-omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. In de **Select User** dialoogvenster vak, plak ofwel de **toepassingsnaam** of de **toepassings-ID** uit de sectie Azure Active Directory-app-registratie.

   [![Een toepassing niet vinden in het dialoogvenster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Selecteer de rol. Selecteer **lezer** om gegevens te doorzoeken of **Inzender** gegevens op te vragen en referentiegegevens wijzigen. Selecteer **OK**.

   [![Lezer of Inzender kiezen in het dialoogvenster gebruikersrol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Sla het beleid door het selecteren van **OK**.

   > [!TIP]
   > Meer informatie over [verlenen van toegang tot gegevens](./time-series-insights-data-access.md) aan uw Time Series Insights-omgeving in Azure Active Directory.

### <a name="client-app-initialization"></a>Client-app initialiseren

1. Gebruik de **toepassings-ID** en **Clientgeheim** (Toepassingssleutel) uit de sectie Azure Active Directory-app registreren om het token namens de toepassing te verkrijgen.

    In C#, de volgende code kunt verkrijgen van het token namens de toepassing. Zie voor een compleet voorbeeld [gegevens opvragen met C#](time-series-insights-query-data-csharp.md).

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

1. Het token kan vervolgens worden doorgegeven de `Authorization` header wanneer de Time Series Insights-API-aanroepen van de toepassing.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een voorbeeld van code die de algemene beschikbaarheid Time Series Insights-API aanroept, [Query gegevens met C# ](./time-series-insights-query-data-csharp.md).

- Zie voor voorbeelden van de Preview-versie Time Series Insights-API-code, [Query-Preview-gegevens met C# ](./time-series-insights-update-query-data-csharp.md).

- Voor de API-referentie-informatie, Zie [Query API-verwijzing](/rest/api/time-series-insights/ga-query-api).

- Meer informatie over het [maken van een service-principal](../active-directory/develop/howto-create-service-principal-portal.md).