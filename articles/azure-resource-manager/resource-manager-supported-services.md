---
title: Azure-resource-providers en resourcetypen | Microsoft Docs
description: Beschrijving van de resourceproviders die ondersteuning bieden voor Resource Manager, wordt hun schema's en beschikbare API-versies en de regio's waar de resources.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 3c7a6fe4-371a-40da-9ebe-b574f583305b
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: tomfitz
ms.openlocfilehash: 520aeb8e47b5e94e6346e682f21f46cb0814f8f3
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445446"
---
# <a name="azure-resource-providers-and-types"></a>Azure resourceproviders en typen

Bij het implementeren van resources, moet u vaak informatie ophalen over de resourceproviders en -typen. In dit artikel leert u het volgende:

* Alle resourceproviders bekijken in Azure
* Controleer de status van de registratie van een resourceprovider
* Een resourceprovider registreren
* Resource-typen voor een resourceprovider bekijken
* Geldige locaties voor een resourcetype weergeven
* Ongeldig API-versies voor een resourcetype weergeven

U kunt deze stappen via de Azure portal, Azure PowerShell of Azure CLI kunt doen.

## <a name="azure-portal"></a>Azure Portal

Om te zien van alle resourceproviders en de registratiestatus voor uw abonnement:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle services**.

    ![abonnementen selecteren](./media/resource-manager-supported-services/select-subscriptions.png)
3. In de **alle services** Voer **abonnement**, en selecteer vervolgens **abonnementen**.
4. Selecteer het abonnement in de lijst met abonnementen om weer te geven.
5. Selecteer **resourceproviders** en de lijst met beschikbare resourceproviders bekijken.

    ![Resourceproviders weergeven](./media/resource-manager-supported-services/show-resource-providers.png)

6. Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Voor het registreren van een resourceprovider, moet u toestemming hebben de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar. Selecteer voor het registreren van een resourceprovider, **registreren**. In de vorige schermafbeelding, de **registreren** koppeling is gemarkeerd voor **Microsoft.Blueprint**.

    Als u nog steeds resourcetypen van die resourceprovider in uw abonnement hebt niet kan u een resourceprovider registratie ongedaan maken.

Om informatie te bekijken voor een bepaalde resourceprovider:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Alle services**.

    ![Alle services selecteren](./media/resource-manager-supported-services/more-services.png)

3. In de **alle services** Voer **resourceverkenner**, en selecteer vervolgens **Resource Explorer**.
4. Vouw **Providers** door de pijl-rechts te selecteren.

    ![Providers selecteren](./media/resource-manager-supported-services/select-providers.png)

5. Vouw een resourceprovider en het resourcetype dat u wilt weergeven.

    ![Resourcetype selecteren](./media/resource-manager-supported-services/select-resource-type.png)

6. Resource Manager wordt ondersteund in alle regio's, maar de bronnen die u implementeert wordt mogelijk niet ondersteund in alle regio's. Bovendien kunnen er beperkingen met betrekking tot uw abonnement die verhinderen dat u met behulp van sommige regio's die ondersteuning bieden voor de resource. De resource explorer geeft geldige locaties voor het brontype.

    ![Locaties weergeven](./media/resource-manager-supported-services/show-locations.png)

7. De API-versie komt overeen met een versie van de REST API-bewerkingen die worden uitgegeven door de resourceprovider. Een resourceprovider nieuwe functies is ingeschakeld, geeft deze een nieuwe versie van de REST-API. De resource explorer geeft geldige API-versies voor het brontype.

    ![API-versies weergeven](./media/resource-manager-supported-services/show-api-versions.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u wilt zien van alle resourceproviders in Azure en de status van de registratie voor uw abonnement, gebruikt u:

```azurepowershell-interactive
Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
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

Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Voor het registreren van een resourceprovider, moet u toestemming hebben de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
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
Get-AzResourceProvider -ProviderNamespace Microsoft.Batch
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
(Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes.ResourceTypeName
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
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).ApiVersions
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
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes | Where-Object ResourceTypeName -eq batchAccounts).Locations
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

Een resourceprovider te registreren, configureert u uw abonnement om te werken met de resourceprovider. Het bereik voor de registratie is altijd het abonnement. Standaard worden veel providers van resources automatisch geregistreerd. Mogelijk moet u echter handmatig registreren door enkele resourceproviders. Voor het registreren van een resourceprovider, moet u toestemming hebben de `/register/action` bewerking voor de resourceprovider. Deze bewerking is opgenomen in de rollen Inzender en Eigenaar.

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

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md). 
* Het sjabloonschema resource provider, Zie [sjabloonverwijzing](/azure/templates/).
* Zie voor meer informatie over het implementeren van resources, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md).
* Als u de bewerkingen voor een resourceprovider, Zie [REST API van Azure](/rest/api/).
