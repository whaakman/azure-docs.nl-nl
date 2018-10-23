---
title: Definitie van door Azure beheerde toepassing maken | Microsoft Docs
description: In dit artikel leest u hoe u een beheerde Azure-toepassing maakt die is bedoeld voor leden van uw organisatie.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 1f80d7e63d994f0e3eb3733b99afaa1b056f4686
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48804905"
---
# <a name="publish-an-azure-managed-application-definition"></a>De definitie van een door Azure beheerde toepassing publiceren

In deze snelstart vindt u een inleiding tot het werken met beheerde toepassingen. U voegt de definitie van een beheerde toepassing toe aan een interne catalogus voor gebruikers in uw organisatie. Om de inleiding te vereenvoudigen, hebben we de bestanden voor uw beheerde toepassing al gemaakt. Deze bestanden zijn beschikbaar via GitHub. U krijgt informatie over het bouwen van die bestanden in de zelfstudie [Servicecatalogustoepassing maken](publish-service-catalog-app.md).

Wanneer u klaar bent, hebt u een resourcegroep met de naam **appDefinitionGroup** die de definitie van de beheerde toepassing bevat.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Een resourcegroep voor de definitie maken

De definitie van uw beheerde toepassing bevindt zich in een resourcegroep. De resourcegroep is een logische verzameling waarin Azure-resources worden geïmplementeerd en beheerd.

Gebruik de volgende opdracht om een resourcegroep te maken:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>De definitie van de beheerde toepassing maken

Bij het definiëren van de beheerde toepassing selecteert u een gebruiker, groep of toepassing die de resources voor de gebruiker beheert. Deze identiteit heeft machtigingen voor de beheerde resourcegroep die overeenkomen met de toegewezen rol. Meestal maakt u een Azure Active Directory-groep om resources te beheren. In dit artikel gebruikt u echter uw eigen identiteit.

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
* **lock-level**: het type vergrendeling dat is toegepast op de beheerde resourcegroep. Hiermee voorkomt u dat de klant ongewenste bewerkingen uitvoert op deze resourcegroep. ReadOnly is momenteel het enige ondersteunde vergrendelingsniveau. Als ReadOnly is opgegeven, kan de klant alleen de resources die aanwezig zijn in de beheerde resourcegroep lezen. De uitgeversidentiteiten die toegang tot de beheerde resourcegroep krijgen, zijn uitgesloten van de vergrendeling.
* **authorizations**: beschrijft de principal-id en de roldefinitie-id die worden gebruikt om machtiging te verlenen aan de beheerde resourcegroep. Wordt opgegeven in de `<principalId>:<roleDefinitionId>`-indeling. Als meer dan één waarde is vereist, geeft u deze op in de vorm `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>`. De waarden worden gescheiden door een spatie.
* **package-file-uri**: de locatie van een .zip-pakket dat de vereiste bestanden bevat. Het pakket moet de bestanden **mainTemplate.json** en **createUiDefinition.json** bevatten. **mainTemplate.json** definieert de Azure-resources die zijn gemaakt als onderdeel van de beheerde toepassing. De sjabloon is niet anders dan een reguliere Resource Manager-sjabloon. **createUiDefinition.json** genereert de gebruikersinterface voor gebruikers die de beheerde toepassing via de portal maken.

## <a name="next-steps"></a>Volgende stappen

U hebt de definitie van de beheerde toepassing gepubliceerd. Nu krijgt u informatie over het implementeren van een exemplaar van deze definitie.

> [!div class="nextstepaction"]
> [Snelstart: Servicecatalogus-app implementeren](deploy-service-catalog-quickstart.md)