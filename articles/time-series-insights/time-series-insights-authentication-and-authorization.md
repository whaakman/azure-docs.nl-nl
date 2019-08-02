---
title: Verifiëren en autoriseren met behulp van een API in Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe u verificatie en autorisatie configureert voor een aangepaste toepassing die de Azure Time Series Insights-API aanroept.
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
ms.openlocfilehash: bdf0fbfb2b40e932f9e3627c3bc0356eb0afb472
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677934"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Verificatie en autorisatie voor Azure Time Series Insights-API

In dit document wordt beschreven hoe u een app in Azure Active Directory kunt registreren met behulp van de nieuwe Azure Active Directory-Blade. Met apps die zijn geregistreerd in Azure Active Directory kunnen gebruikers verifiëren en worden gemachtigd om de Azure time series Insight-API te gebruiken die is gekoppeld aan een Time Series Insights omgeving.

## <a name="service-principal"></a>Service-principal

In de volgende secties wordt beschreven hoe u een toepassing kunt configureren voor toegang tot de Time Series Insights-API namens een app. De toepassing kan vervolgens met behulp van de referenties van de eigen toepassing een query uitvoeren op of publiceren naar referentie gegevens in de Time Series Insights omgeving via Azure Active Directory.

## <a name="summary-and-best-practices"></a>Samen vatting en aanbevolen procedures

De registratie stroom van de Azure Active Directory-app bestaat uit drie belang rijke stappen.

1. [Een toepassing registreren](#azure-active-directory-app-registration) in azure Active Directory.
1. De toepassing machtigen om [gegevens toegang te geven tot de time series Insights omgeving](#granting-data-access).
1. Gebruik de **toepassings-id** en het **client geheim** om `https://api.timeseries.azure.com/` een token te verkrijgen in uw [client-app](#client-app-initialization). Het token kan vervolgens worden gebruikt om de Time Series Insights-API aan te roepen.

In het volgende voor **stap 3**kunt u met behulp van de referenties van uw toepassing en uw gebruikers gegevens:

* Wijs machtigingen toe aan de app-identiteit die verschilt van uw eigen machtigingen. Deze machtigingen zijn doorgaans beperkt tot wat de app nodig heeft. U kunt bijvoorbeeld toestaan dat de app alleen gegevens uit een bepaalde Time Series Insights omgeving kan lezen.
* De beveiliging van de app isoleren van de verificatie referenties van de gebruiker met behulp van een **client geheim** of beveiligings certificaat. Als gevolg hiervan zijn de referenties van de toepassing niet afhankelijk van de referenties van een specifieke gebruiker. Als de rol van de gebruiker wordt gewijzigd, is voor de toepassing niet noodzakelijkerwijs nieuwe referenties of verdere configuratie vereist. Als de gebruiker het wacht woord wijzigt, zijn geen nieuwe referenties of sleutels vereist voor toegang tot de toepassing.
* Voer een script zonder toezicht uit met behulp van een **client geheim** of beveiligings certificaat in plaats van een specifieke gebruikers referenties (vereist dat ze aanwezig zijn).
* Gebruik een beveiligings certificaat in plaats van een wacht woord om de toegang tot uw Azure Time Series Insights-API te beveiligen.

> [!IMPORTANT]
> Volg het principe van de **schei ding van de problemen** (beschreven in dit scenario hierboven) bij het configureren van uw Azure time series Insights-beveiligings beleid.

> [!NOTE]
> * Het artikel richt zich op een toepassing met één Tenant waarbij de toepassing in slechts één organisatie is bedoeld om te worden uitgevoerd.
> * Normaal gesp roken gebruikt u toepassingen met één Tenant voor line-of-business-toepassingen die worden uitgevoerd in uw organisatie.

## <a name="detailed-setup"></a>Gedetailleerde installatie

### <a name="azure-active-directory-app-registration"></a>App-registratie Azure Active Directory

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Toegang tot gegevens verlenen

1. Selecteer voor de Time Series Insights omgeving **Data Access policies** en selecteer **toevoegen**.

   [![Nieuw beleid voor gegevens toegang toevoegen aan de Time Series Insights omgeving](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. Plak in het dialoog venster **gebruiker selecteren** de naam van de **toepassing** of de **toepassings-id** in het gedeelte Azure Active Directory app-registratie.

   [![Een toepassing zoeken in het dialoog venster gebruiker selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Selecteer de rol. Selecteer **lezer** om gegevens op te vragen of **Inzender** om gegevens op te vragen en referentie gegevens te wijzigen. Selecteer **OK**.

   [![Lezer of Inzender kiezen in het dialoog venster gebruikersrol selecteren](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Sla het beleid op door **OK**te selecteren.

   > [!TIP]
   > Meer informatie over het [verlenen van toegang tot gegevens](./time-series-insights-data-access.md) in uw time series Insights-omgeving in azure Active Directory.

### <a name="client-app-initialization"></a>Initialisatie van client-app

1. Gebruik de **toepassings-id** en het **client geheim** (toepassings sleutel) uit de sectie app-registratie van Azure Active Directory om het token namens de toepassing te verkrijgen.

    In C#kan de volgende code het token namens de toepassing verkrijgen. Zie [query data using C# ](time-series-insights-query-data-csharp.md)(Engelstalig) voor een volledig voor beeld.

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

1. Het token kan vervolgens worden door gegeven in `Authorization` de header wanneer de toepassing de time series Insights-API aanroept.

## <a name="next-steps"></a>Volgende stappen

- Zie [query data using C# ](./time-series-insights-query-data-csharp.md)(Engelstalig) voor voorbeeld code voor het aanroepen van de Ga Time Series Insights-API.

- Zie [query voorbeeld gegevens gebruiken C# ](./time-series-insights-update-query-data-csharp.md)voor preview-time series Insights-API-code voorbeelden.

- Zie [query-API-verwijzing](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)voor informatie over API-verwijzingen.

- Meer informatie over het [maken van een Service-Principal](../active-directory/develop/howto-create-service-principal-portal.md).