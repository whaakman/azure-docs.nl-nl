---
title: Azure-resource provider registratiefouten | Microsoft Docs
description: Beschrijft hoe u registratie oplossen voor Azure-resource provider.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: e87ff83470ac8d375b4eee3b69f8793e3e11c0f5
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497414"
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Los fouten voor de registratie van de resourceprovider

Dit artikel beschrijft de fouten die optreden kunnen bij het gebruik van een resourceprovider die u hebt nog niet eerder in uw abonnement gebruikt.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="symptom"></a>Symptoom

Als u resources implementeert, krijgt u mogelijk de volgende code en het bericht:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Of een soortgelijk bericht waarin wordt vermeld:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Het foutbericht geeft suggesties voor de ondersteunde locaties en API-versies. U kunt de sjabloon wijzigen in een van de voorgestelde waarden. De meeste providers zijn geregistreerde automatisch door de Azure-portal of de opdrachtregelinterface die u gebruikt, maar niet alle. Als u een bepaalde resourceprovider voordat u dit nog niet hebt gebruikt, moet u wellicht dat de provider registreren.

## <a name="cause"></a>Oorzaak

U ontvangt deze fouten voor een van drie redenen:

* De resourceprovider is niet geregistreerd voor uw abonnement
* API-versie niet ondersteund voor het resourcetype
* Locatie niet ondersteund voor het resourcetype

## <a name="solution-1---powershell"></a>Oplossing 1: PowerShell

Gebruik voor PowerShell **Get-AzResourceProvider** om te zien van de registratiestatus van uw.

```powershell
Get-AzResourceProvider -ListAvailable
```

Gebruik voor het registreren van een provider **registreren AzResourceProvider** en geef de naam van de resource-provider die u wilt registreren.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Cdn
```

Voor de ondersteunde locaties voor een bepaald type resource, gebruikt u:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Voor de ondersteunde API-versies voor een bepaald type resource, gebruikt u:

```powershell
((Get-AzResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Oplossing 2 - Azure CLI

Als u wilt zien of de provider is geregistreerd, gebruikt u de `az provider list` opdracht.

```azurecli-interactive
az provider list
```

Gebruik voor het registreren van een resourceprovider de `az provider register` opdracht en geeft u de *naamruimte* om u te registreren.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Als u wilt zien van de ondersteunde locaties en API-versies voor een resourcetype, gebruikt u:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Oplossing 3 - Azure portal

U kunt de status van inschrijving en registreren van een naamruimte van de resource provider via de portal.

1. Selecteer in de portal **alle services**.

   ![Alle services selecteren](./media/resource-manager-register-provider-errors/select-all-services.png)

1. Selecteer **Abonnementen**.

   ![Abonnementen selecteren](./media/resource-manager-register-provider-errors/select-subscriptions.png)

1. Selecteer het abonnement dat u gebruiken wilt voor het registreren van de resourceprovider in de lijst met abonnementen.

   ![Abonnement selecteren om resourceprovider te registreren](./media/resource-manager-register-provider-errors/select-subscription-to-register.png)

1. Voor uw abonnement en selecteer **resourceproviders**.

   ![Resourceproviders selecteren](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Bekijk de lijst met resourceproviders en selecteer indien nodig, de **registreren** koppeling voor het registreren van de resourceprovider van het type dat u probeert te implementeren.

   ![Lijst met resourceproviders](./media/resource-manager-register-provider-errors/list-resource-providers.png)
