---
title: Azure Service Bus-beheerbibliotheken | Microsoft Docs
description: Service Bus-naamruimten en berichtentiteiten van .NET beheren.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: b7f63ac8a52da4898abb3d30b71f3ee3a3e1a4b3
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394689"
---
# <a name="service-bus-management-libraries"></a>Service Bus-beheerbibliotheken

De Azure Service Bus-beheerbibliotheken kunnen richten op dynamische wijze Service Bus-naamruimten en entiteiten. Dit maakt het complexe implementaties en berichtverzending en maakt het mogelijk om via een programma te bepalen welke entiteiten om in te richten. Deze bibliotheken zijn momenteel beschikbaar voor .NET.

## <a name="supported-functionality"></a>Ondersteunde functies

* Namespace maken, bijwerken, verwijderen
* De wachtrij is gemaakt, bijwerken, verwijderen
* Onderwerp maken, bijwerken, verwijderen
* Abonnement maken, bijwerken, verwijderen

## <a name="prerequisites"></a>Vereisten

Om te beginnen met behulp van de Service Bus-beheerbibliotheken, moet u verifiëren met de service Azure Active Directory (Azure AD). Azure AD is vereist dat u een verificatie uitvoeren als een service-principal, waarmee u toegang hebt tot uw Azure-resources. Zie een van de volgende artikelen voor informatie over het maken van een service principal:  

* [De Azure portal gebruiken voor het maken van Active Directory-toepassing en service-principal die toegang hebben tot resources](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Azure CLI gebruiken om een service-principal te maken voor toegang tot resources](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Deze zelfstudie beschikt u over een `AppId` (Client-ID), `TenantId`, en `ClientSecret` (verificatiesleutel), die allemaal worden gebruikt voor verificatie via de management-bibliotheken. U moet hebben **eigenaar** machtigingen voor de resourcegroep die u wilt uitvoeren.

## <a name="programming-pattern"></a>Patroon voor programmeren

Het patroon voor het bewerken van een Service Bus-resource met de volgende een gemeenschappelijke protocol:

1. Een token verkrijgen van Azure AD via de **Microsoft.IdentityModel.Clients.ActiveDirectory** bibliotheek:
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```
2. Maak de `ServiceBusManagementClient` object:

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```
3. Stel de `CreateOrUpdate` parameters met de opgegeven waarden:

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```
4. De aanroep uitvoeren:

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Volgende stappen

* [Voorbeeld van .NET-management](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API-verwijzing](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
