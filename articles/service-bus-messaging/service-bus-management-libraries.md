---
title: Azure Service Bus-beheerbibliotheken | Microsoft Docs
description: Service Bus-naamruimten en berichtentiteiten van .NET beheren.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2019
ms.author: aschhab
ms.openlocfilehash: 3836eb87516eed546ae6bb69f53bf64e5df00906
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693210"
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

## <a name="complete-code-to-create-a-queue"></a>Code voor het maken van een wachtrij voltooien
Dit is de volledige code voor het maken van een Service Bus-wachtrij: 

```csharp
using System;
using System.Threading.Tasks;

using Microsoft.Azure.Management.ServiceBus;
using Microsoft.Azure.Management.ServiceBus.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using Microsoft.Rest;

namespace SBusADApp
{
    class Program
    {
        static void Main(string[] args)
        {
            CreateQueue().GetAwaiter().GetResult();
        }

        private static async Task CreateQueue()
        {
            try
            {
                var subscriptionID = "<SUBSCRIPTION ID>";
                var resourceGroupName = "<RESOURCE GROUP NAME>";
                var namespaceName = "<SERVICE BUS NAMESPACE NAME>";
                var queueName = "<NAME OF QUEUE YOU WANT TO CREATE>";

                var token = await GetToken();

                var creds = new TokenCredentials(token);
                var sbClient = new ServiceBusManagementClient(creds)
                {
                    SubscriptionId = subscriptionID,
                };

                var queueParams = new SBQueue();

                Console.WriteLine("Creating queue...");
                await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, queueName, queueParams);
                Console.WriteLine("Created queue successfully.");
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not create a queue...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

        private static async Task<string> GetToken()
        {
            try
            {
                var tenantId = "<AZURE AD TENANT ID>";
                var clientId = "<APPLICATION/CLIENT ID>";
                var clientSecret = "<CLIENT SECRET>";

                var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

                var result = await context.AcquireTokenAsync(
                    "https://management.core.windows.net/",
                    new ClientCredential(clientId, clientSecret)
                );

                // If the token isn't a valid string, throw an error.
                if (string.IsNullOrEmpty(result.AccessToken))
                {
                    throw new Exception("Token result is empty!");
                }

                return result.AccessToken;
            }
            catch (Exception e)
            {
                Console.WriteLine("Could not get a token...");
                Console.WriteLine(e.Message);
                throw e;
            }
        }

    }
}
```

> [!IMPORTANT]
> Zie voor een compleet voorbeeld de [.NET management voorbeeld op GitHub]((https://github.com/Azure-Samples/service-bus-dotnet-management/)). 

## <a name="next-steps"></a>Volgende stappen
[Microsoft.Azure.Management.ServiceBus API-verwijzing](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
