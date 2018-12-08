---
title: Beheerbibliotheken - Azure Event Hubs | Microsoft Docs
description: In dit artikel bevat informatie over de bibliotheek die u gebruiken kunt voor het beheren van Azure Event Hubs-naamruimten en entiteiten van .NET.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 431fe04461f422274697d1e91c4b56e914ce2d4e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53102709"
---
# <a name="event-hubs-management-libraries"></a>Event Hubs-beheerbibliotheken

U kunt de Azure Event Hubs-beheerbibliotheken om u te richten op dynamische wijze Event Hubs-naamruimten en entiteiten. Dit dynamische karakter kan complexe implementaties en berichtverzending, zodat u via een programma welke entiteiten om in te richten bepalen kunt. Deze bibliotheken zijn momenteel beschikbaar voor .NET.

## <a name="supported-functionality"></a>Ondersteunde functies

* Namespace maken, bijwerken, verwijderen
* Event Hubs maken, bijwerken, verwijderen
* Consumentengroep maken, bijwerken, verwijderen

## <a name="prerequisites"></a>Vereisten

Om te beginnen met behulp van de Event Hubs-beheerbibliotheken, moet u verifiëren met Azure Active Directory (AAD). AAD is vereist dat u een verificatie uitvoeren als een service-principal, waarmee u toegang hebt tot uw Azure-resources. Zie een van de volgende artikelen voor informatie over het maken van een service principal:  

* [De Azure portal gebruiken voor het maken van Active Directory-toepassing en service-principal die toegang hebben tot resources](../active-directory/develop/howto-create-service-principal-portal.md)
* [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](../active-directory/develop/howto-authenticate-service-principal-powershell.md)
* [Azure CLI gebruiken om een service-principal te maken voor toegang tot resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Deze zelfstudie beschikt u over een `AppId` (Client-ID), `TenantId`, en `ClientSecret` (verificatiesleutel), die allemaal worden gebruikt voor verificatie via de management-bibliotheken. U moet hebben **eigenaar** machtigingen voor de resourcegroep die u wilt uitvoeren.

## <a name="programming-pattern"></a>Patroon voor programmeren

Het patroon voor het bewerken van alle Event Hubs-resource met de volgende een gemeenschappelijke protocol:

1. Een token verkrijgen van het gebruik van AAD de `Microsoft.IdentityModel.Clients.ActiveDirectory` bibliotheek.
    ```csharp
    var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

    var result = await context.AcquireTokenAsync(
        "https://management.core.windows.net/",
        new ClientCredential(clientId, clientSecret)
    );
    ```

1. Maak de `EventHubManagementClient` object.
    ```csharp
    var creds = new TokenCredentials(token);
    var ehClient = new EventHubManagementClient(creds)
    {
        SubscriptionId = SettingsCache["SubscriptionId"]
    };
    ```

1. Stel de `CreateOrUpdate` parameters met de opgegeven waarden.
    ```csharp
    var ehParams = new EventHubCreateOrUpdateParameters()
    {
        Location = SettingsCache["DataCenterLocation"]
    };
    ```

1. De aanroep uitvoeren.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Volgende stappen
* [Management .NET-voorbeeld](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub verwijzing](/dotnet/api/Microsoft.Azure.Management.EventHub) 
