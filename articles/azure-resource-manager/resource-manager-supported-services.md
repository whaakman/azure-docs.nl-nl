---
title: Azure-resourceproviders en brontypen | Microsoft Docs
description: Beschrijving van de resourceproviders die ondersteuning bieden voor Resource Manager, wordt hun schema's en beschikbare API-versies en de regio's waar de resources kunnen hosten.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: tomfitz
ms.openlocfilehash: 6a9128f45d4199404019cee594842d59c7f1aaf3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="resource-providers-and-types"></a>Resourceproviders en typen

Bij het implementeren van resources, moet u vaak informatie ophalen over de resourceproviders en typen. In dit artikel leert hoe u u kunt:

* Alle resourceproviders bekijken in Azure
* Controleer de status van de registratie van een resourceprovider
* Een resourceprovider registreren
* Weergave brontypen die voor een resourceprovider
* Geldige locaties van de weergave voor een brontype
* Geldige API-versies voor een resourcetype weergeven

U kunt deze stappen via de portal, PowerShell of Azure CLI kunt uitvoeren.

## <a name="powershell"></a>PowerShell

Als alle resourceproviders in Azure en de registratiestatus voor uw abonnement wilt weergeven, gebruikt u het:

```powershell
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Die resultaten weer die vergelijkbaar is met:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Een resourceprovider registreren, configureert u uw abonnement werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Veel resourceproviders worden standaard automatisch geregistreerd. Mogelijk moet u echter handmatig registreren sommige resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de Inzender en de eigenaar van rollen.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Die resultaten weer die vergelijkbaar is met:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

U kunt de registratie ongedaan een resourceprovider kan niet maken wanneer u nog steeds brontypen van die resourceprovider in uw abonnement hebt.

Als u informatie voor een bepaalde bron-provider, gebruiken:

```powershell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Die resultaten weer die vergelijkbaar is met:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Als u wilt zien welke resourcetypen voor een resourceprovider, gebruiken:

```powershell
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Die wordt geretourneerd:

```powershell
batchAccounts
operations
locations
locations/quotas
```

De API-versie overeenkomt met een versie van de REST-API-bewerkingen die worden uitgegeven door de resourceprovider. Als een resourceprovider kunt nieuwe functies, geeft deze een nieuwe versie van de REST-API. 

Als u de beschikbare API-versies voor een resourcetype, gebruikt u:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Die wordt geretourneerd:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien mogelijk zijn er beperkingen met betrekking tot uw abonnement die voorkomen dat u sommige regio's die ondersteuning bieden voor de resource gebruiken. 

Als u de ondersteunde locaties voor een resourcetype, gebruiken.

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Die wordt geretourneerd:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure CLI
Als alle resourceproviders in Azure en de registratiestatus voor uw abonnement wilt weergeven, gebruikt u het:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Die resultaten weer die vergelijkbaar is met:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Een resourceprovider registreren, configureert u uw abonnement werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Veel resourceproviders worden standaard automatisch geregistreerd. Mogelijk moet u echter handmatig registreren sommige resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de Inzender en de eigenaar van rollen.

```azurecli
az provider register --namespace Microsoft.Batch
```

Retourneert het bericht van deze inschrijving is continu.

U kunt de registratie ongedaan een resourceprovider kan niet maken wanneer u nog steeds brontypen van die resourceprovider in uw abonnement hebt.

Als u informatie voor een bepaalde bron-provider, gebruiken:

```azurecli
az provider show --namespace Microsoft.Batch
```

Die resultaten weer die vergelijkbaar is met:

```azurecli
{
    "id": "/subscriptions/####-####/providers/Microsoft.Batch",
    "namespace": "Microsoft.Batch",
    "registrationsState": "Registering",
    "resourceTypes:" [
        ...
    ]
}
```

Als u wilt zien welke resourcetypen voor een resourceprovider, gebruiken:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Die wordt geretourneerd:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

De API-versie overeenkomt met een versie van de REST-API-bewerkingen die worden uitgegeven door de resourceprovider. Als een resourceprovider kunt nieuwe functies, geeft deze een nieuwe versie van de REST-API. 

Als u de beschikbare API-versies voor een resourcetype, gebruikt u:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Die wordt geretourneerd:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien mogelijk zijn er beperkingen met betrekking tot uw abonnement die voorkomen dat u sommige regio's die ondersteuning bieden voor de resource gebruiken. 

Als u de ondersteunde locaties voor een resourcetype, gebruiken.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Die wordt geretourneerd:

```azurecli
Result
---------------
West Europe
East US
East US 2
West US
...
```

## <a name="portal"></a>Portal

Selecteer om te zien alle resourceproviders in Azure en de registratiestatus voor uw abonnement, **abonnementen**.

![Selecteer abonnementen](./media/resource-manager-supported-services/select-subscriptions.png)

Kies het abonnement om weer te geven.

![abonnement opgeven](./media/resource-manager-supported-services/subscription.png)

Selecteer **resourceproviders** en bekijk de lijst met beschikbare resourceproviders bekijken.

![resourceproviders weergeven](./media/resource-manager-supported-services/show-resource-providers.png)

Een resourceprovider registreren, configureert u uw abonnement werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Veel resourceproviders worden standaard automatisch geregistreerd. Mogelijk moet u echter handmatig registreren sommige resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de Inzender en de eigenaar van rollen. Selecteer voor het registreren van een resourceprovider **registreren**.

![de registerbronprovider](./media/resource-manager-supported-services/register-provider.png)

U kunt de registratie ongedaan een resourceprovider kan niet maken wanneer u nog steeds brontypen van die resourceprovider in uw abonnement hebt.

Als u informatie voor een bepaalde bron-provider, selecteer **meer services**.

![meer services selecteren](./media/resource-manager-supported-services/more-services.png)

Zoeken naar **Resource Explorer** en selecteer het uit de beschikbare opties.

![Selecteer resource explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

Selecteer **Providers**.

![Providers selecteren](./media/resource-manager-supported-services/select-providers.png)

Selecteer de resourceprovider en het brontype dat u wilt weergeven.

![Brontype selecteren](./media/resource-manager-supported-services/select-resource-type.png)

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien mogelijk zijn er beperkingen met betrekking tot uw abonnement die voorkomen dat u sommige regio's die ondersteuning bieden voor de resource gebruiken. De resource explorer geeft geldige locaties voor het brontype.

![Locaties weergeven](./media/resource-manager-supported-services/show-locations.png)

De API-versie overeenkomt met een versie van de REST-API-bewerkingen die worden uitgegeven door de resourceprovider. Als een resourceprovider kunt nieuwe functies, geeft deze een nieuwe versie van de REST-API. De resource explorer geeft geldige API-versies voor het brontype.

![API-versies weergeven](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van resources, [Implementeer een toepassing met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).
* De bewerkingen voor een resourceprovider Zie [REST API van Azure](/rest/api/).

