---
title: Gebruikers en rollen in een Azure Machine Learning-werkruimte beheren
titleSuffix: Azure Machine Learning service
description: Informatie over het beheren van gebruikers en rollen in de werkruimte van een Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: hning86
ms.author: haining
ms.date: 2/20/2019
ms.custom: seodec18
ms.openlocfilehash: 623aaff3cba86e8e523a86e4adcb0a359c339c45
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57336453"
---
# <a name="manage-users-and-roles-in-an-azure-machine-learning-workspace"></a>Gebruikers en rollen in een Azure Machine Learning-werkruimte beheren

In dit artikel leert u hoe u gebruikers toevoegen aan een Azure Machine Learning-werkruimte. U leert ook hoe u gebruikers toewijzen aan verschillende rollen en aangepaste rollen maken.

## <a name="built-in-roles"></a>Ingebouwde rollen
Een Azure Machine Learning-werkruimte is een Azure-resource. Net als andere Azure-resources, wanneer een nieuwe Azure Machine Learning-werkruimte wordt gemaakt, deze wordt geleverd met drie standaardrollen. U kunt gebruikers toevoegen aan de werkruimte en deze toewijzen aan een van deze ingebouwde rollen.

- **Lezer**
    
    Deze rol kan alleen-lezen-acties in de werkruimte. Lezers kunnen lijst en activa weergeven in een werkruimte, maar kan niet maken of bijwerken van deze elementen.

- **Inzender**
    
    Deze rol kan gebruikers bekijken, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. Bijvoorbeeld, kunnen bijdragers maken van een experiment, maken of koppelen van een rekencluster indienen van een uitvoering en een webservice implementeren.

- **Eigenaar**
    
    Deze rol biedt gebruikers volledige toegang tot de werkruimte, inclusief de mogelijkheid om te bekijken, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. U kunt bovendien toevoegen of verwijderen van gebruikers en wijzigen van roltoewijzingen.

## <a name="add-or-remove-users"></a>Gebruikers toevoegen of verwijderen
Als u een eigenaar van een werkruimte bent, kunt u gebruikers toevoegen en verwijderen uit de werkruimte door een van de volgende methoden te kiezen:
- [Gebruikersinterface van Azure portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure-CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager-sjablonen](/azure/role-based-access-control/role-assignments-template)

Als u hebt geïnstalleerd het [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), u kunt ook een CLI-opdracht gebruiken om toe te voegen gebruikers aan de werkruimte.

```azurecli-interactive 
az ml workspace share -n <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

De `user` veld is het e-mailadres van een bestaande gebruiker in het exemplaar van Azure Active Directory waar het abonnement van de bovenliggende werkruimte zich bevinden. Hier volgt een voorbeeld van hoe u deze opdracht:

```azurecli-interactive 
az ml workspace share -n my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Een aangepast rol maken
Als de ingebouwde rollen onvoldoende zijn, kunt u aangepaste rollen maken. Aangepaste rollen mogelijk lezen, schrijven, verwijderen en compute-machtigingen voor resources in deze werkruimte. U kunt de rol beschikbaar maken op het niveau van een specifieke werkruimte, het niveau van een specifieke resourcegroep of een bepaald abonnement. 

> [!NOTE]
> U moet een eigenaar van de resource op dat niveau te maken van aangepaste rollen binnen die resource.

Maak eerst een rol definitie JSON-bestand dat Hiermee geeft u de machtiging en het bereik dat u wilt gebruiken voor de rol voor het maken van een aangepaste rol. Bijvoorbeeld, als volgt het definitiebestand voor een rol voor een aangepaste rol met de naam 'Gegevenswetenschapper"binnen het bereik op het niveau van een specifieke werkruimte:

`data_scientist_role.json` :
```json
{
    "Name": "Data Scientist",
    "IsCustom": true,
    "Description": "Can run experiment but can't create or delete compute.",
    "Actions": ["*"],
    "NotActions": [
        "Microsoft.MachineLearningServices/workspaces/*/delete",
        "Microsoft.MachineLearningServices/workspaces/computes/*/write",
        "Microsoft.MachineLearningServices/workspaces/computes/*/delete", 
        "Microsoft.Authorization/*/write"
    ],
    "AssignableScopes": [
        "/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.MachineLearningServices/workspaces/<workspace_name>"
    ]
}
```
U kunt wijzigen de `AssignableScopes` veld om in te stellen van het bereik van deze aangepaste rol op het abonnementsniveau, niveau van de resourcegroep of het niveau van een specifieke werkruimte.

Deze aangepaste rol kunt doen alles in de werkruimte, met uitzondering van de volgende acties:
- Het kan niet maken of bijwerken van een compute-resource.
- Het kan een compute-resource niet verwijderen.
- Het kan toevoegen, verwijderen of wijzigen van roltoewijzingen.
- Dit kan de werkruimte niet verwijderen.

Gebruik de volgende Azure CLI-opdracht voor het implementeren van deze aangepaste rol:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Na de implementatie wordt deze rol beschikbaar gesteld in de opgegeven werkruimte. U kunt nu toevoegen en toewijzen van deze rol in Azure portal. Of u een gebruiker met deze rol kunt toevoegen met behulp van de `az ml workspace share` CLI-opdracht:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```


Zie voor meer informatie over aangepaste rollen in Azure, [dit document](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Volgende stappen

Voer de volledige zelfstudie voor informatie over het gebruik van een werkruimte te bouwen, te trainen en implementeer modellen met de Azure Machine Learning-service.

> [!div class="nextstepaction"]
> [Zelfstudie: Modellen trainen](tutorial-train-models-with-aml.md)
