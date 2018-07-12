---
title: Persoonlijke gegevens van Azure Resource Manager | Microsoft Docs
description: Informatie over het beheren van persoonlijke gegevens die zijn gekoppeld aan Azure Resource Manager-bewerkingen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: 71928be07080ed14fdcb93f33ea64d2572955b53
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38466021"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Persoonlijke gegevens die zijn gekoppeld met Azure Resource Manager beheren

Om te voorkomen dat gevoelige informatie, geen persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of tags te verwijderen. Azure Resource Manager biedt bewerkingen waarmee u persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of tags beheren.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Verwijderen van persoonsgegevens in de implementatiegeschiedenis

Voor implementaties, Resource Manager en blijven behouden parameterwaarden statusberichten in de geschiedenis van de implementatie. Deze waarden blijft actief totdat u de implementatie uit de geschiedenis verwijderen. Als u wilt zien als u de persoonlijke gegevens in deze waarden hebt opgegeven, een lijst de implementaties. Als u persoonlijke gegevens hebt gevonden, verwijdert u de implementaties van de geschiedenis.

Lijst **implementaties** gebruiken in de geschiedenis:

* [Lijst met door resourcegroep](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [AZ group deployment lijst](/cli/azure/group/deployment#az-group-deployment-list)

Verwijderen van **implementaties** uit de geschiedenis gebruiken:

* [Verwijderen](/rest/api/resources/deployments/delete)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [implementatie van de AZ-groep verwijderen](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Persoonlijke gegevens in de namen van resourcegroepen verwijderen

De naam van de resourcegroep clusterverbinding blijven behouden totdat u de resourcegroep verwijderen. Lijst om te zien of u de persoonlijke gegevens in de namen zijn opgegeven, de resourcegroepen. Als u persoonlijke gegevens [de resources verplaatsen](resource-group-move-resources.md) naar een nieuwe resourcegroep en verwijder de resourcegroep met de persoonlijke gegevens in de naam.

Lijst **resourcegroepen**, gebruiken:

* [Lijst met](/rest/api/resources/resourcegroups/list)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [de lijst van de AZ-groep](/cli/azure/group#az-group-list)

Verwijderen van **resourcegroepen**, gebruiken:

* [Verwijderen](/rest/api/resources/resourcegroups/delete)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Persoonlijke gegevens in labels verwijderen

Namen van de labels en waarden bewaard totdat u verwijderen of wijzigen van de tag. Als u wilt zien als u persoonlijke gegevens in de tags hebt opgegeven, een lijst de labels. Als u persoonlijke gegevens hebt gevonden, verwijdert u de labels.

Lijst **tags**, gebruiken:

* [Lijst met](/rest/api/resources/tags/list)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [AZ tag list](/cli/azure/tag#az-tag-list)

Verwijderen van **tags**, gebruiken:

* [Verwijderen](/rest/api/resources/tags/delete)
* [Remove-AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [AZ tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van Azure Resource Manager, de [wat is Resource Manager?](resource-group-overview.md)