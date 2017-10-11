---
title: Een Azure gebruiken voor beheerde toepassing | Microsoft Docs
description: Beschrijft hoe een klant maakt u een Azure beheerde toepassing uit gepubliceerde bestanden.
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a>Een interne beheerde toepassing gebruiken

U kunt Azure verbruiken [beheerde toepassingen](managed-application-overview.md) die bestemd zijn voor leden van uw organisatie. Bijvoorbeeld, kunt u beheerde toepassingen van uw IT-afdeling die de organisatie-normen te waarborgen. Deze beheerde toepassingen zijn beschikbaar via de Servicecatalogus, niet de Azure Marketplace.

Voordat u doorgaat met dit artikel, moet u een beheerde toepassing beschikbaar is in de Servicecatalogus hebben voor uw abonnement. Als iemand zich in uw organisatie geen beheerde toepassingen gemaakt nog, raadpleegt u [publiceren van een beheerde toepassingsservices voor intern verbruik](managed-application-publishing.md).

Op dit moment kunt u Azure CLI of Azure portal gebruiken voor een beheerde toepassing.

## <a name="create-the-managed-application-by-using-the-portal"></a>De beheerde toepassing maken met behulp van de portal

Volg deze stappen voor het implementeren van een beheerde toepassing via de portal:

1. Ga naar de Azure-portal. Zoeken naar **Servicecatalogus beheerde toepassing**.

   ![Servicecatalogus voor beheerde toepassing](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. Selecteer de beheerde toepassing die u wilt maken in de lijst met beschikbare oplossingen. Selecteer **Maken**.

   ![Selectie van de beheerde toepassing](./media/managed-application-consumption/select-offer.png)

1. Geef waarden op voor de parameters die nodig zijn voor het leveren van de resources. Selecteer **West-Centraal VS** voor de locatie. Selecteer **OK**.

   ![Parameters voor de beheerde toepassing](./media/managed-application-consumption/input-parameters.png)

1. De sjabloon valideert de waarden die u hebt opgegeven. Als de validatie slaagt, selecteert u **OK** implementatie te starten.

   ![Validatie van de beheerde toepassing](./media/managed-application-consumption/validation.png)

Nadat de implementatie is voltooid, worden de juiste resources die zijn gedefinieerd in de sjabloon ingericht in de beheerde resourcegroep die u hebt opgegeven.

## <a name="create-the-managed-application-by-using-azure-cli"></a>De beheerde toepassing maken met behulp van Azure CLI

Er zijn twee manieren een beheerde toepassing maken met behulp van Azure CLI:

* Gebruik de opdracht voor het maken van beheerde toepassingen.
* Gebruik de opdracht van de implementatie reguliere sjabloon.

### <a name="use-the-template-deployment-command"></a>Gebruik de opdracht van de implementatie van sjabloon

Implementeer het bestand applianceMainTemplate.json die de leverancier gemaakt.

Vervolgens maakt u twee resourcegroepen. De eerste resourcegroep is waar de bron van de beheerde toepassing wordt gemaakt: Microsoft.Solutions/appliances. De tweede resourcegroep bevat alle resources die zijn gedefinieerd in mainTemplate.json. Deze resourcegroep wordt beheerd door de ISV.

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> Gebruik `westcentralus` als de locatie van de resourcegroep.
>

Gebruik de volgende opdracht voor het implementeren van applianceMainTemplate.json in mainResourceGroup:

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

Nadat de voorgaande sjabloon wordt uitgevoerd, wordt u gevraagd de waarden van de parameters die zijn gedefinieerd in de sjabloon. Naast de parameters die nodig zijn voor het inrichten van resources in een sjabloon, moet u twee belangrijke parameterwaarden:

- **managedResourceGroupId**: de ID van de resourcegroep met de resources die zijn gedefinieerd in applianceMainTemplate.json. De ID van het formulier is `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`. In het vorige voorbeeld is de ID van `managedResourceGroup`.
- **applianceDefinitionId**: de ID van de bron van de definitie van beheerde toepassingsservices. Deze waarde wordt verstrekt door de ISV.

> [!NOTE]
> De uitgever moet toegang verlenen aan de resourcegroep met de definitie van de beheerde toepassing. De definitie-resource is gemaakt in het abonnement op publisher. Daarom moet een gebruiker, groep of toepassing in de tenant klant leestoegang tot deze resource.

Nadat de implementatie voltooid wordt, ziet u dat de beheerde toepassing in mainResourceGroup wordt gemaakt. De resource storageAccount wordt gemaakt in managedResourceGroup.

### <a name="use-the-create-command"></a>Gebruik de opdracht maken

U kunt de `az managedapp create` opdracht voor het maken van een beheerde toepassing van de definitie van de beheerde toepassing.

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* **apparaat-definitie-Id**: de bron-ID van de definitie van de beheerde toepassing in de vorige stap hebt gemaakt. Als u deze ID, voer de volgende opdracht:

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  Deze opdracht retourneert de definitie van de beheerde toepassing. U moet de waarde van de eigenschap ID.

* **beheerde-rg-id**: de naam van de resourcegroep met de resources die zijn gedefinieerd in applianceMainTemplate.json. Deze resourcegroep is de groep beheerde bron. Het wordt beheerd door de uitgever. Als deze nog niet bestaat, wordt deze voor u gemaakt.
* **resourcegroep**: de resourcegroep waar de bron van de beheerde toepassing wordt gemaakt. De resource Microsoft.Solutions/appliance woont in deze resourcegroep.
* **parameters**: de parameters die nodig zijn voor de resources die zijn gedefinieerd in applianceMainTemplate.json.

## <a name="known-issues"></a>Bekende problemen

Deze preview-versie bevat de volgende problemen:

* Een interne serverfout 500 wordt weergegeven tijdens het maken van de beheerde toepassing. Als u dit probleem, is het waarschijnlijk onregelmatige. De bewerking opnieuw proberen.
* Een nieuwe resourcegroep is nodig voor de beheerde resourcegroep. Als u een bestaande resourcegroep gebruikt, mislukt de implementatie.
* De resourcegroep met de bron Microsoft.Solutions/appliances moet worden gemaakt in de **westcentralus** locatie.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot beheerde toepassingen, [beheerde toepassingsoverzicht](managed-application-overview.md).
* Zie voor meer informatie over het publiceren van een Servicecatalogus beheerde toepassing [maken en publiceren van een Servicecatalogus beheerde toepassing](managed-application-publishing.md).
* Zie voor meer informatie over het uitgeven van beheerde toepassingen naar Azure Marketplace [Azure beheerde toepassingen in de Marketplace](managed-application-author-marketplace.md).
* Zie voor meer informatie over het gebruiken van een beheerde toepassing in de Marketplace [Azure gebruiken beheerde toepassingen in de Marketplace](managed-application-consume-marketplace.md).
