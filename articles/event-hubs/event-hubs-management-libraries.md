---
title: Azure Event Hubs managementbibliotheken | Microsoft Docs
description: Event Hubs-naamruimten en entiteiten beheren vanaf .NET
services: event-hubs
cloud: na
documentationcenter: na
author: sethmanheim
manager: timlt
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/19/2017
ms.author: sethm
ms.openlocfilehash: 2ae2f8f2006507284338fb4fa62e4942476cf2bc
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="event-hubs-management-libraries"></a>Event Hubs management-bibliotheken

De management-bibliotheken van Event Hubs kunnen richten op dynamische wijze Event Hubs-naamruimten en entiteiten. Deze dynamische aard kunnen complexe implementaties en berichtverzending, zodat u via een programma welke entiteiten te richten bepalen kunt. Deze bibliotheken zijn momenteel beschikbaar voor .NET.

## <a name="supported-functionality"></a>Ondersteunde functionaliteit

* Namespace maken, bijwerken, verwijderen
* Event Hubs maken, bijwerken, verwijderen
* Consumergroep maken, bijwerken, verwijderen

## <a name="prerequisites"></a>Vereisten

Om te beginnen met behulp van de Event Hubs-management-bibliotheken, moet u verifiëren met Azure Active Directory (AAD). AAD vereist dat u verificatie uitvoeren als een service-principal die toegang tot uw Azure-resources biedt. Zie voor informatie over het maken van een service principal, een van deze artikelen:  

* [De Azure portal gebruiken om Active Directory-toepassing en service-principal die toegang bronnen tot te maken](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Azure CLI gebruiken om een service-principal te maken voor toegang tot resources](../azure-resource-manager/resource-group-authenticate-service-principal-cli.md)

Deze zelfstudies beschikt u over een `AppId` (Client-ID), `TenantId`, en `ClientSecret` (verificatiesleutel), die allemaal worden gebruikt voor verificatie door de management-bibliotheken. U moet hebben **eigenaar** machtigingen voor de resourcegroep die u wilt uitvoeren.

## <a name="programming-pattern"></a>Patroon programmering

Hier volgt een gemeenschappelijke protocol het patroon voor het bewerken van een Event Hubs-resource:

1. Verkrijgen van een token van AAD met behulp van de `Microsoft.IdentityModel.Clients.ActiveDirectory` bibliotheek.
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

1. De aanroep worden uitgevoerd.
    ```csharp
    await ehClient.EventHubs.CreateOrUpdateAsync(resourceGroupName, namespaceName, EventHubName, ehParams);
    ```

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeld van de .NET-management](https://github.com/Azure-Samples/event-hubs-dotnet-management/)
* [Microsoft.Azure.Management.EventHub verwijzing](/dotnet/api/Microsoft.Azure.Management.EventHub) 
