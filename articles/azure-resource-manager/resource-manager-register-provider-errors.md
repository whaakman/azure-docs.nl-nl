---
title: Azure-resource provider-registratiefouten | Microsoft Docs
description: Beschrijft hoe Azure-resource provider registratiefouten op te lossen.
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
ms.date: 03/09/2018
ms.author: tomfitz
ms.openlocfilehash: b90009c1cd08a1004e58c4b9f25cd6350712fbcd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="resolve-errors-for-resource-provider-registration"></a>Los de fouten voor registratie resourceprovider

In dit artikel beschrijft de fouten die optreden kunnen wanneer u een resourceprovider die u niet eerder hebt gebruikt in uw abonnement.

## <a name="symptom"></a>Symptoom

Bij het implementeren van de resource wordt de volgende foutcode en het bericht:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Of een vergelijkbaar bericht waarin wordt vermeld:

```
Code: MissingSubscriptionRegistration
Message: The subscription is not registered to use namespace {resource-provider-namespace}
```

Het foutbericht geeft suggesties voor de ondersteunde locaties en API-versies. U kunt uw sjabloon wijzigen in een van de voorgestelde waarden. De meeste providers zijn geregistreerde automatisch door de Azure-portal of de opdrachtregelinterface die u gebruikt, maar niet alle. Als u niet een bepaalde bron-provider voorafgaand aan hebt gebruikt, moet u wellicht dat de provider te registreren.

## <a name="cause"></a>Oorzaak

U ontvangt deze fouten voor een van drie redenen:

1. De resourceprovider is niet geregistreerd voor uw abonnement
1. API-versie niet ondersteund voor het brontype
1. Locatie niet ondersteund voor het brontype

## <a name="solution-1---powershell"></a>Oplossing 1 - PowerShell

Gebruik voor PowerShell **Get-AzureRmResourceProvider** om uw registratiestatus te bekijken.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

Gebruik voor het registreren van een provider **registreren AzureRmResourceProvider** en geef de naam van de resourceprovider die u wilt registreren.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

Als u de ondersteunde locaties voor een bepaald type resource, gebruikt u:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Als u de ondersteunde API-versies voor een bepaald type resource, gebruikt u:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

## <a name="solution-2---azure-cli"></a>Oplossing 2: Azure CLI

U kunt controleren of de provider is geregistreerd, gebruiken de `az provider list` opdracht.

```azurecli-interactive
az provider list
```

Voor het registreren van een resourceprovider, gebruiken de `az provider register` opdracht in en geef de *naamruimte* te registreren.

```azurecli-interactive
az provider register --namespace Microsoft.Cdn
```

Overzicht van de ondersteunde locaties en API-versies voor een brontype gebruiken:

```azurecli-interactive
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="solution-3---azure-portal"></a>Oplossing 3 - Azure-portal

U kunt de registratiestatus zien en de naamruimte via de portal een resourceprovider registreren.

1. Selecteer voor uw abonnement, **resourceproviders**.

   ![resourceproviders selecteren](./media/resource-manager-register-provider-errors/select-resource-provider.png)

1. Bekijk de lijst met resourceproviders en selecteer indien nodig de **registreren** koppeling naar de registerbronprovider is van het type dat u probeert te implementeren.

   ![resourceproviders vermelden](./media/resource-manager-register-provider-errors/list-resource-providers.png)
