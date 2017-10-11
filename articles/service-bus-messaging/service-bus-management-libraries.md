---
title: Azure Service Bus-bibliotheken voor management | Microsoft Docs
description: Service Bus-naamruimten en berichtentiteiten in .NET beheren.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: sethm
ms.openlocfilehash: 1db00dc1f91e8976b622030450445babbe547ad8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="service-bus-management-libraries"></a>Management-Service Bus-bibliotheken

De Azure Service Bus-managementbibliotheken kunnen richten op dynamische wijze Service Bus-naamruimten en entiteiten. Dit kunt u complexe implementaties en berichtverzending en maakt het mogelijk via een programma bepalen welke entiteiten te richten. Deze bibliotheken zijn momenteel beschikbaar voor .NET.

## <a name="supported-functionality"></a>Ondersteunde functionaliteit

* Namespace maken, bijwerken, verwijderen
* Wachtrijen maken, bijwerken, verwijderen
* Onderwerp maken, bijwerken, verwijderen
* Abonnement maken, bijwerken, verwijderen

## <a name="prerequisites"></a>Vereisten

Om te beginnen met behulp van de Service Bus-bibliotheken voor beheer, moet u verifiëren met de service Azure Active Directory (AAD). AAD vereist dat u verificatie uitvoeren als een service-principal die toegang tot uw Azure-resources biedt. Zie voor informatie over het maken van een service principal, een van deze artikelen:  

* [De Azure portal gebruiken om Active Directory-toepassing en service-principal die toegang bronnen tot te maken](/azure/azure-resource-manager/resource-group-create-service-principal-portal)
* [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](/azure/azure-resource-manager/resource-group-authenticate-service-principal)
* [Azure CLI gebruiken om een service-principal te maken voor toegang tot resources](/azure/azure-resource-manager/resource-group-authenticate-service-principal-cli)

Deze zelfstudies beschikt u over een `AppId` (Client-ID), `TenantId`, en `ClientSecret` (verificatiesleutel), die allemaal worden gebruikt voor verificatie door de management-bibliotheken. U moet hebben **eigenaar** machtigingen voor de resourcegroep waarin u wilt uitvoeren.

## <a name="programming-pattern"></a>Patroon programmering

Hier volgt een gemeenschappelijke protocol het patroon voor het bewerken van een Service Bus-resource:

1. Een token verkrijgen van het gebruik van Azure Active Directory de **Microsoft.IdentityModel.Clients.ActiveDirectory** bibliotheek.
   ```csharp
   var context = new AuthenticationContext($"https://login.microsoftonline.com/{tenantId}");

   var result = await context.AcquireTokenAsync("https://management.core.windows.net/", new ClientCredential(clientId, clientSecret));
   ```

1. Maak de `ServiceBusManagementClient` object.

   ```csharp
   var creds = new TokenCredentials(token);
   var sbClient = new ServiceBusManagementClient(creds)
   {
       SubscriptionId = SettingsCache["SubscriptionId"]
   };
   ```

1. Stel de `CreateOrUpdate` parameters met de opgegeven waarden.

   ```csharp
   var queueParams = new QueueCreateOrUpdateParameters()
   {
       Location = SettingsCache["DataCenterLocation"],
       EnablePartitioning = true
   };
   ```

1. De aanroep worden uitgevoerd.

   ```csharp
   await sbClient.Queues.CreateOrUpdateAsync(resourceGroupName, namespaceName, QueueName, queueParams);
   ```

## <a name="next-steps"></a>Volgende stappen
* [Voorbeeld van .NET-management](https://github.com/Azure-Samples/service-bus-dotnet-management/)
* [Microsoft.Azure.Management.ServiceBus API-verwijzing](/dotnet/api/Microsoft.Azure.Management.ServiceBus)
