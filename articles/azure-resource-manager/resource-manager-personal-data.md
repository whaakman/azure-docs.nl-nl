---
title: Persoonlijke gegevens van Azure Resource Manager | Microsoft Docs
description: Informatie over het beheren van persoonlijke gegevens die zijn gekoppeld met Azure Resource Manager-bewerkingen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: 5c176dd185a9d2a245045e9c954279506d713e5f
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Persoonlijke gegevens die zijn gekoppeld met Azure Resource Manager beheren

Om te voorkomen dat gevoelige informatie, geen persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of labels te verwijderen. Azure Resource Manager biedt bewerkingen die u kunnen beheren, persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of labels.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Persoonlijke gegevens in de geschiedenis van implementatie verwijderen

Voor implementaties behoudt de Resource Manager parameterwaarden en statusberichten in de implementatiegeschiedenis van de. Deze waarden bewaard totdat u de implementatie uit de geschiedenis verwijderen. Als u wilt zien als u de persoonlijke gegevens in deze waarden hebt opgegeven, lijst van de implementaties. Als u persoonlijke gegevens vinden, kunt u de implementaties verwijderd uit de geschiedenis.

Aan lijst **implementaties** gebruiken in de geschiedenis:

* [Lijst met door resourcegroep](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [implementatie van AZ groepslijst](/cli/azure/group/deployment#az-group-deployment-list)

Verwijderen van **implementaties** vanuit de geschiedenis gebruiken:

* [Verwijderen](/rest/api/resources/deployments/delete)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [implementatie AZ verwijderen](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Persoonlijke gegevens in de namen van resourcegroepen verwijderen

De naam van de resourcegroep blijft bestaan totdat u de resourcegroep verwijderen. Als u wilt zien als u de namen van de persoonlijke gegevens hebt opgegeven, lijst van de resourcegroepen. Als u persoonlijke gegevens vinden [verplaatsen van de resources](resource-group-move-resources.md) naar een nieuwe resourcegroep en de resourcegroep met de naam van de persoonlijke gegevens verwijderen.

Aan lijst **resourcegroepen**, gebruiken:

* [lijst](/rest/api/resources/resourcegroups/list)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [lijst van de groep AZ](/cli/azure/group#az-group-list)

Verwijderen van **resourcegroepen**, gebruiken:

* [Verwijderen](/rest/api/resources/resourcegroups/delete)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Persoonlijke gegevens in labels verwijderen

Labels-namen en waarden bewaard totdat u verwijderen of wijzigen van het label. Als u wilt zien als u de persoonlijke gegevens in de tags hebt opgegeven, lijst van de labels. Als u persoonlijke gegevens hebt gevonden, verwijdert u de labels.

Aan lijst **labels**, gebruiken:

* [lijst](/rest/api/resources/tags/list)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [lijst met AZ labels](/cli/azure/tag#az-tag-list)

Verwijderen van **labels**, gebruiken:

* [Verwijderen](/rest/api/resources/tags/delete)
* [Verwijder AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [AZ label verwijderen](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van Azure Resource Manager, de [wat is er Resource Manager?](resource-group-overview.md)