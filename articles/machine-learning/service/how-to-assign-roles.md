---
title: Rollen in een Azure Machine Learning-werkruimte beheren
titleSuffix: Azure Machine Learning service
description: Leer hoe u de toegang tot de werkruimte van een Azure Machine Learning-service met behulp van op rollen gebaseerd toegangsbeheer (RBAC).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: larryfr
author: Blackmist
ms.date: 2/20/2019
ms.custom: seodec18
ms.openlocfilehash: b40edf705ba61713f4b695dd55a6a20028936c82
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57993821"
---
# <a name="manage-access-to-an-azure-machine-learning-workspace"></a>Toegang tot een Azure Machine Learning-werkruimte beheren

In dit artikel leert u hoe u voor het beheren van toegang tot een Azure Machine Learning-werkruimte. [Op rollen gebaseerd toegangsbeheer (RBAC)](/azure/role-based-access-control/overview) wordt gebruikt om toegang tot Azure-resources te beheren. Gebruikers in uw Azure Active Directory zijn specifieke rollen die toegang tot resources geven toegewezen. Azure biedt zowel ingebouwde rollen en de mogelijkheid om aangepaste rollen te maken.

## <a name="default-roles"></a>Standaardrollen

Een Azure Machine Learning-werkruimte is een Azure-resource. Net als andere Azure-resources, wanneer een nieuwe Azure Machine Learning-werkruimte wordt gemaakt, deze wordt geleverd met drie standaardrollen. U kunt gebruikers toevoegen aan de werkruimte en deze toewijzen aan een van deze ingebouwde rollen.

| Rol | Toegangsniveau |
| --- | --- |
| **Lezer** | Alleen-lezen-acties in de werkruimte. Lezers kunnen lijst en activa weergeven in een werkruimte, maar kan niet maken of bijwerken van deze elementen. |
| **Inzender** | Weergeven, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. Bijvoorbeeld, kunnen bijdragers maken van een experiment, maken of koppelen van een rekencluster indienen van een uitvoering en een webservice implementeren. |
| **Eigenaar** | Volledige toegang tot de werkruimte, inclusief de mogelijkheid om te bekijken, maken, bewerken of verwijderen (indien van toepassing) activa in een werkruimte. U kunt bovendien roltoewijzingen wijzigen. |

> [!IMPORTANT]
> Roltoegang kan worden gericht op meerdere niveaus in Azure. Bijvoorbeeld iemand met de eigenaar van de toegang tot een werkgroep mogelijk niet de eigenaar van de toegang tot de resourcegroep met de werkgroep. Zie voor meer informatie, [hoe RBAC werkt](/azure/role-based-access-control/overview#how-rbac-works).

Zie voor meer informatie over specifieke ingebouwde rollen, [ingebouwde rollen voor Azure](/azure/role-based-access-control/built-in-roles).

## <a name="manage-workspace-access"></a>Werkruimtetoegang beheren

Als u een eigenaar van een werkruimte bent, kunt u toevoegen en verwijderen van rollen voor de werkruimte. U kunt ook rollen toewijzen aan gebruikers. Gebruik de volgende koppelingen om te ontdekken hoe u om toegang te beheren:
- [Gebruikersinterface van Azure portal](/azure/role-based-access-control/role-assignments-portal)
- [PowerShell](/azure/role-based-access-control/role-assignments-powershell)
- [Azure-CLI](/azure/role-based-access-control/role-assignments-cli)
- [REST API](/azure/role-based-access-control/role-assignments-rest)
- [Azure Resource Manager-sjablonen](/azure/role-based-access-control/role-assignments-template)

Als u hebt geïnstalleerd het [Azure Machine Learning CLI](reference-azure-machine-learning-cli.md), u kunt ook een CLI-opdracht gebruiken aan de rollen toewijzen aan gebruikers.

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

Maak eerst een functie van de definitie van JSON-bestand dat Hiermee geeft u de machtiging en het bereik voor de rol voor het maken van een aangepaste rol. Het volgende voorbeeld definieert een aangepaste rol met de naam 'Gegevenswetenschapper"binnen het bereik op het niveau van een specifieke werkruimte:

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

Na de implementatie wordt deze rol beschikbaar gesteld in de opgegeven werkruimte. U kunt nu toevoegen en toewijzen van deze rol in Azure portal. Of u kunt deze rol toewijzen aan een gebruiker met behulp van de `az ml workspace share` CLI-opdracht:

```azurecli-interactive
az ml workspace share -n my_workspace -g my_resource_group --role "Data Scientist" --user jdoe@contoson.com
```


Zie voor meer informatie, [aangepaste rollen voor Azure-resources](/azure/role-based-access-control/custom-roles).

## <a name="next-steps"></a>Volgende stappen

- [Enterprise security-overzicht](concept-enterprise-security.md)
- [Experimenten en inferentietaken binnen een virtueel netwerk veilig uitvoeren](how-to-enable-virtual-network.md)
- [Zelfstudie: Modellen trainen](tutorial-train-models-with-aml.md)
