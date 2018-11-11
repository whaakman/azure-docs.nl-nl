---
title: Azure-resource-providers en resourcetypen | Microsoft Docs
description: Beschrijving van de resourceproviders die ondersteuning bieden voor Resource Manager, wordt hun schema's en beschikbare API-versies en de regio's waar de resources.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: fafc16bdf00f947d4ba8ffe56d7cf2ae3e0bc489
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344940"
---
# <a name="resource-providers-and-types"></a>Resourceproviders en -typen

Bij het implementeren van resources, moet u vaak informatie ophalen over de resourceproviders en -typen. In dit artikel leert u naar:

* Alle resourceproviders bekijken in Azure
* Controleer de status van de registratie van een resourceprovider
* Een resourceprovider registreren
* Resource-typen voor een resourceprovider bekijken
* Geldige locaties voor een resourcetype weergeven
* Ongeldig API-versies voor een resourcetype weergeven

U kunt deze stappen via de portal, PowerShell of Azure CLI uitvoeren.

## <a name="powershell"></a>PowerShell

Als u wilt zien van alle resourceproviders in Azure en de status van de registratie voor uw abonnement, gebruikt u:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
```

Dit retourneert resultaten die vergelijkbaar is met:

```powershell
ProviderNamespace                RegistrationState
-------------------------------- ------------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Dit retourneert resultaten die vergelijkbaar is met:

```powershell
ProviderNamespace : Microsoft.Batch
RegistrationState : Registering
ResourceTypes     : {batchAccounts, operations, locations, locations/quotas}
Locations         : {West Europe, East US, East US 2, West US...}
```

Als u nog steeds resourcetypen van die resourceprovider in uw abonnement hebt niet kan u een resourceprovider registratie ongedaan maken.

Gebruik de volgende informatie voor een bepaalde resourceprovider:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
```

Dit retourneert resultaten die vergelijkbaar is met:

```powershell
{ProviderNamespace : Microsoft.Batch
RegistrationState : Registered
ResourceTypes     : {batchAccounts}
Locations         : {West Europe, East US, East US 2, West US...}

...
```

Als u wilt zien welke resourcetypen voor een resourceprovider, gebruikt u:

```azurepowershell-interactive
(Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
```

Dat geeft als resultaat:

```powershell
batchAccounts
operations
locations
locations/quotas
```

De API-versie komt overeen met een versie van de REST API-bewerkingen die worden uitgegeven door de resourceprovider. Een resourceprovider nieuwe functies is ingeschakeld, geeft deze een nieuwe versie van de REST-API.

Voor de beschikbare API-versies voor een resourcetype, gebruikt u:

```azurepowershell-interactive
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
```

Dat geeft als resultaat:

```powershell
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien kunnen er beperkingen met betrekking tot uw abonnement die verhinderen dat u met behulp van sommige regio's die ondersteuning bieden voor de resource.

Als u de ondersteunde locaties voor een resourcetype, gebruiken.

```azurepowershell-interactive
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

Dat geeft als resultaat:

```powershell
West Europe
East US
East US 2
West US
...
```

## <a name="azure-cli"></a>Azure-CLI

Als u wilt zien van alle resourceproviders in Azure en de status van de registratie voor uw abonnement, gebruikt u:

```azurecli
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

Dit retourneert resultaten die vergelijkbaar is met:

```azurecli
Provider                         Status
-------------------------------- ----------------
Microsoft.ClassicCompute         Registered
Microsoft.ClassicNetwork         Registered
Microsoft.ClassicStorage         Registered
Microsoft.CognitiveServices      Registered
...
```

Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

```azurecli
az provider register --namespace Microsoft.Batch
```

Retourneert een bericht van die registratie is continue.

Als u nog steeds resourcetypen van die resourceprovider in uw abonnement hebt niet kan u een resourceprovider registratie ongedaan maken.

Gebruik de volgende informatie voor een bepaalde resourceprovider:

```azurecli
az provider show --namespace Microsoft.Batch
```

Dit retourneert resultaten die vergelijkbaar is met:

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

Als u wilt zien welke resourcetypen voor een resourceprovider, gebruikt u:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[*].resourceType" --out table
```

Dat geeft als resultaat:

```azurecli
Result
---------------
batchAccounts
operations
locations
locations/quotas
```

De API-versie komt overeen met een versie van de REST API-bewerkingen die worden uitgegeven door de resourceprovider. Een resourceprovider nieuwe functies is ingeschakeld, geeft deze een nieuwe versie van de REST-API.

Voor de beschikbare API-versies voor een resourcetype, gebruikt u:

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].apiVersions | [0]" --out table
```

Dat geeft als resultaat:

```azurecli
Result
---------------
2017-05-01
2017-01-01
2015-12-01
2015-09-01
2015-07-01
```

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien kunnen er beperkingen met betrekking tot uw abonnement die verhinderen dat u met behulp van sommige regio's die ondersteuning bieden voor de resource.

Als u de ondersteunde locaties voor een resourcetype, gebruiken.

```azurecli
az provider show --namespace Microsoft.Batch --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" --out table
```

Dat geeft als resultaat:

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

Als u wilt zien van alle resourceproviders in Azure en de status van de registratie voor uw abonnement, selecteer **abonnementen**.

![abonnementen selecteren](./media/resource-manager-supported-services/select-subscriptions.png)

Kies het abonnement om weer te geven.

![abonnement opgeven](./media/resource-manager-supported-services/subscription.png)

Selecteer **resourceproviders** en de lijst met beschikbare resourceproviders bekijken.

![Resourceproviders weergeven](./media/resource-manager-supported-services/show-resource-providers.png)

Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Als u wilt een resourceprovider registreren, moet u gemachtigd om uit te voeren de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Selecteer voor het registreren van een resourceprovider, **registreren**.

![De resourceprovider registreren](./media/resource-manager-supported-services/register-provider.png)

Als u nog steeds resourcetypen van die resourceprovider in uw abonnement hebt niet kan u een resourceprovider registratie ongedaan maken.

Gegevens voor een bepaalde resourceprovider wilt bekijken, selecteert u **alle services**.

![Alle services selecteren](./media/resource-manager-supported-services/more-services.png)

Zoeken naar **Resource Explorer** en selecteert u deze uit de beschikbare opties.

![Selecteer resource explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

Selecteer **Providers**.

![Providers selecteren](./media/resource-manager-supported-services/select-providers.png)

Selecteer de resourceprovider en het resourcetype dat u wilt weergeven.

![Resourcetype selecteren](./media/resource-manager-supported-services/select-resource-type.png)

Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien kunnen er beperkingen met betrekking tot uw abonnement die verhinderen dat u met behulp van sommige regio's die ondersteuning bieden voor de resource. De resource explorer geeft geldige locaties voor het brontype.

![Locaties weergeven](./media/resource-manager-supported-services/show-locations.png)

De API-versie komt overeen met een versie van de REST API-bewerkingen die worden uitgegeven door de resourceprovider. Een resourceprovider nieuwe functies is ingeschakeld, geeft deze een nieuwe versie van de REST-API. De resource explorer geeft geldige API-versies voor het brontype.

![API-versies weergeven](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor meer informatie over het implementeren van resources, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).
* Als u de bewerkingen voor een resourceprovider, Zie [REST API van Azure](/rest/api/).
