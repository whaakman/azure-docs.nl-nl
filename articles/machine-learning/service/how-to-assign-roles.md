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
ms.openlocfilehash: f6c74da2e9189c5dddb88cb80f04422f49cfaee2
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56594250"
---
# <a name="manage-users-and-roles-in-an-azure-machine-learning-workspace"></a>Gebruikers en rollen in een Azure Machine Learning-werkruimte beheren

In dit artikel leert u hoe u gebruikers toevoegen aan een Azure Machine Learning-werkruimte en deze met verschillende rollen toewijzen. U leert ook hoe u aangepaste rollen maken.

## <a name="built-in-roles"></a>Ingebouwde rollen
Azure Machine Learning-werkruimte is een Azure-resource. En net als een Azure-resource, wanneer een nieuwe Azure Machine Learning-werkruimte wordt gemaakt, wordt geleverd met drie standaardrollen. U kunt gebruikers toevoegen aan de werkruimte en deze toewijzen aan een van deze ingebouwde rollen.

- **Lezer**
    
    Deze rol kan alleen-lezen-acties in de werkruimte. U kunt een lijst met deze rol en weergave-elementen in een werkruimte, zoals experimenten, wordt uitgevoerd, berekenen, modellen, webservices, pijplijnen enzovoort gepubliceerd. Maar u niet maken of bijwerken van deze elementen zijn toegestaan.

- **Inzender**
    
    Deze rol kan gebruikers bekijken, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. Bijvoorbeeld: met deze functie kunt u een experiment maken, maken of koppelen van een rekencluster indienen een register uitvoeren, een model en een webservice implementeert.

- **Eigenaar**
    
    Deze rol biedt u volledige toegang tot de werkruimte, zodat u kunt bekijken, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. Ook kunt u ook toevoegen of verwijderen van gebruikers en hun roltoewijzingen wijzigen.

## <a name="add-or-remove-users"></a>Gebruikers toevoegen of verwijderen
Als u eigenaar bent van een werkruimte, kunt u gebruikers toevoegen aan of gebruikers verwijderen uit de werkruimte door het kiezen van een van de volgende methoden om deze actie uitvoeren:
- [Gebruikersinterface van Azure portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure-CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Management-sjablonen](/azure/role-based-access-control/role-assignments-template).

U kunt ook als u hebt geïnstalleerd [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), u kunt ook een handige CLI-opdracht gebruiken aan gebruiker toevoegen aan de werkruimte.

```azurecli-interactive 
az ml workspace share -n <workspace_name> -g <resource_group_name> --role <role_name> --user <user_corp_email_address>
```

Houd er rekening mee dat de `user` veld is het e-mailadres van een bestaande gebruiker in de dezelfde Azure Active Directory waar het bovenliggende item Azure-abonnement van de werkruimte zich bevinden. Hieronder volgt een voorbeeld van hoe u deze opdracht:

```azurecli-interactive 
az ml workspace share -n my_workspace -g my_resource_group --role Contributor --user jdoe@contoson.com
```

## <a name="create-custom-role"></a>Een aangepast rol maken
Als de ingebouwde rollen onvoldoende is voor uw behoeften zijn, kunt u ook aangepaste rollen maken. De aangepaste rollen hebben mogelijk lezen, schrijven of verwijderen van machtigingen voor de werkruimte en de rekenresource in deze werkruimte. En u kunt de rol beschikbaar maken op het niveau van een specifieke werkruimte, het niveau van een specifieke resourcegroep of een bepaald abonnement. 

> [!NOTE]
> U moet een eigenaar van de resource op dat niveau om te kunnen maken van aangepaste rollen binnen die resource.

Maak eerst een rol van de definitie van JSON-bestand op te geven de machtiging en het bereik dat u wilt gebruiken voor de rol voor het maken van een aangepaste rol. Hieronder ziet u bijvoorbeeld een rol-definitiebestand voor een aangepaste rol met de naam 'Gegevenswetenschapper"binnen het bereik op het niveau van een bepaalde werkruimte.

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

Deze rol kunt u alles in de werkruimte, met uitzondering van de volgende acties:
- Het kan niet maken of bijwerken van een compute-resource.
- Het kan een compute-resource niet verwijderen.
- Het kan toevoegen, gebruiker verwijderen of wijzigen van roltoewijzingen van een gebruiker.
- Dit kan de werkruimte niet verwijderen.

Gebruik de volgende Azure CLI-opdracht voor het implementeren van deze aangepaste rol:

```azurecli-interactive 
az role definition create --role-definition data_scientist_role.json
```

Zodra geïmplementeerd, is deze functie is beschikbaar in de opgegeven werkruimte. U kunt nu gebruikers toevoegen en deze rol aan ze toewijzen in Azure portal. Of u kunt een gebruiker met deze rol met toevoegen de `az ml workspace share` CLI-opdracht:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```

U kunt ook wijzigen de `AssignableScopes` veld om in te stellen van het bereik van deze aangepaste rol op het abonnementsniveau, niveau van de resourcegroep, of op het niveau van een specifieke werkruimte (zoals weergegeven).

Voor meer informatie van hoe aangepaste rollen in Azure werken, raadpleegt u [dit document](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Volgende stappen

Voer de volledige zelfstudie voor informatie over het gebruik van een werkruimte te bouwen, te trainen en implementeer modellen met Azure Machine Learning-service.

> [!div class="nextstepaction"]
> [Zelfstudie: Modellen trainen](tutorial-train-models-with-aml.md)
