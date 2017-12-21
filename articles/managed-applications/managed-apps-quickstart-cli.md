---
title: Een beheerde Azure-toepassing maken met Azure CLI | Microsoft Docs
description: In dit artikel leest u hoe u een beheerde Azure-toepassing maakt die is bedoeld voor leden van uw organisatie.
services: azure-resource-manager
author: tfitzmac
manager: timlt
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: 35059603096279f7d58da1c1b40dd2ab3f1b5c38
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-and-deploy-an-azure-managed-application-with-azure-cli"></a>Een beheerde Azure-toepassing maken en implementeren met Azure CLI

In dit artikel leest u over de basisbeginselen van het werken met beheerde toepassingen. U voegt de definitie van een beheerde toepassing toe aan een interne catalogus voor gebruikers in uw organisatie. Vervolgens implementeert u de beheerde toepassing in uw abonnement. Om de inleiding te vereenvoudigen, hebben we de bestanden voor uw beheerde toepassing al gemaakt. Een bestand definieert de infrastructuur voor uw oplossing. Een tweede bestand definieert de gebruikersinterface voor het implementeren van de beheerde toepassing via de portal. Deze bestanden zijn beschikbaar via GitHub. U krijgt informatie over het bouwen van die bestanden in de zelfstudie [Servicecatalogustoepassing maken](publish-service-catalog-app.md).

Wanneer u klaar bent, hebt u drie resourcegroepen met verschillende onderdelen van de beheerde toepassing.

| Resourcegroep | Contains | Beschrijving |
| -------------- | -------- | ----------- |
| appDefinitionGroup | De definitie van de beheerde toepassing. | De uitgever maakt deze resourcegroep en de definitie voor de beheerde toepassing. Iedereen met toegang tot de definitie van de beheerde toepassing kan deze implementeren. |
| applicationGroup | Het exemplaar van de beheerde toepassing. | De consument maakt deze resourcegroep en het exemplaar van de beheerde toepassing. De consument kan de beheerde toepassing via dit exemplaar bijwerken. |
| infrastructureGroup | De resources voor de beheerde toepassing. | Deze resourcegroep wordt automatisch gemaakt wanneer de beheerde toepassing wordt gemaakt. De uitgever heeft toegang tot deze resourcegroep om de toepassing te beheren. De consument heeft beperkte toegang tot de resourcegroep. |

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Een resourcegroep voor de definitie maken

De definitie van uw beheerde toepassing bevindt zich in een resourcegroep. De resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Gebruik de volgende opdracht om een resourcegroep te maken:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

Bij het definiëren van de beheerde toepassing selecteert u een gebruiker, groep of toepassing die de resources namens de gebruiker beheert. Deze identiteit heeft machtigingen voor de beheerde resourcegroep in overeenstemming met de rol die is toegewezen. Meestal maakt u een Azure Active Directory-groep voor het beheren van de resources. In dit artikel gebruikt u echter uw eigen identiteit.

Als u de object-id van uw identiteit wilt ophalen, geeft u uw user principal name op in de volgende opdracht:

```azurecli-interactive
userid=$(az ad user show --upn-or-object-id example@contoso.org --query objectId --output tsv)
```

Vervolgens hebt u de roldefinitie-id nodig van de ingebouwde RBAC-rol waarmee u toegang wilt verlenen aan de gebruiker. In de volgende opdracht ziet u hoe u de roldefinitie-id ophaalt voor de rol van eigenaar:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Maak nu de definitieresource van de beheerde toepassing. De beheerde toepassing bevat alleen een opslagaccount.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/samples/201-managed-storage-account/managedstorage.zip"
```

Wanneer de opdracht is voltooid, hebt u de definitie van een beheerde toepassing in de resourcegroep. 

Enkele parameters die in het voorgaande voorbeeld worden gebruikt, zijn:

* **resource-group**: de naam van de resourcegroep waarin de definitie van de beheerde toepassing wordt gemaakt.
* **lock-level**: het type vergrendeling dat is toegepast op de beheerde resourcegroep. Dit voorkomt dat de klant ongewenste bewerkingen uitvoert op deze resourcegroep. ReadOnly is momenteel het enige ondersteunde vergrendelingsniveau. Als ReadOnly is opgegeven, kan de klant alleen de resources die aanwezig zijn in de beheerde resourcegroep lezen.
* **authorizations**: beschrijft de principal-id en de roldefinitie-id die worden gebruikt om machtiging te verlenen aan de beheerde resourcegroep. Wordt opgegeven in de `<principalId>:<roleDefinitionId>`-indeling. Er kunnen ook meerdere waarden worden opgegeven voor deze eigenschap. Als er meerdere waarden nodig zijn, moeten ze worden opgegeven in het formulier `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. Meerdere waarden worden gescheiden door een spatie.
* **package-file-uri**: de locatie van een .zip-pakket dat de vereiste bestanden bevat. Het pakket moet ten minste de bestanden **mainTemplate.json** en **createUiDefinition.json** bevatten. **mainTemplate.json** definieert de Azure-resources die zijn ingericht als onderdeel van de beheerde toepassing. De sjabloon is niet anders dan een reguliere Resource Manager-sjabloon. **createUiDefinition.json** genereert de gebruikersinterface voor gebruikers die de beheerde toepassing via de portal maken.

## <a name="create-resource-group-for-managed-application"></a>De resourcegroep voor de beheerde toepassing maken

U bent nu klaar om de beheerde toepassing te implementeren. 

U moet een resourcegroep maken om de geïmplementeerde beheerde toepassing in uit te voeren. Gebruik **westcentralus** als de locatie.

```azurecli-interactive
az group create --name applicationGroup --location westcentralus
```

## <a name="deploy-the-managed-application"></a>De beheerde toepassing implementeren

U implementeert de toepassing met de volgende opdrachten:

```azurecli-interactive
appid=$(az managedapp definition show --name ManagedStorage --resource-group appDefinitionGroup --query id --output tsv)
subid=$(az account show --query id --output tsv)
managedGroupId=/subscriptions/$subid/resourceGroups/infrastructureGroup

az managedapp create \
  --name storageApp \
  --location "westcentralus" \
  --kind "Servicecatalog" \
  --resource-group applicationGroup \
  --managedapp-definition-id $appid \
  --managed-rg-id $managedGroupId \
  --parameters "{\"storageAccountNamePrefix\": {\"value\": \"storage\"}, \"storageAccountType\": {\"value\": \"Standard_LRS\"}}"
```

Enkele parameters die in het voorgaande voorbeeld worden gebruikt, zijn:

* **managedapp-definition-id**: de id van de definitie die u eerder in dit artikel hebt gemaakt.
* **managed-rg-id**: de id van de resourcegroep voor de resources die zijn gekoppeld aan de beheerde toepassing. De opdracht maakt deze resourcegroep. Deze **mag niet bestaan voordat u de opdracht uitvoert**. Deze resourcegroep wordt beheerd door de uitgever. 
* **resource-group**: de resourcegroep waarin de beheerde toepassing wordt gemaakt.
* **parameters**: de parameters die nodig zijn voor de resources die zijn gekoppeld aan de beheerde toepassing.

Nadat de implementatie is voltooid, ziet u dat de beheerde toepassing in applicationGroup is gemaakt. De resource storageAccount wordt gemaakt in infrastructureGroup.

## <a name="next-steps"></a>Volgende stappen

* Zie [Overzicht van beheerde toepassingen](overview.md) voor een inleiding tot beheerde toepassingen.
* Zie [Voorbeelden van beheerde toepassingen](https://github.com/Azure/azure-managedapp-samples/tree/master/samples) voor voorbeelden van de bestanden.
* Zie [Aan de slag met CreateUiDefinition](create-uidefinition-overview.md) voor meer informatie over het maken van een UI-definitiebestand voor een beheerde toepassing.
