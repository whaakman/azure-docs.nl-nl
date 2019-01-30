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
ms.openlocfilehash: 01e2b1608d55d7852db947181482d34d6d8010d2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215042"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Persoonlijke gegevens die zijn gekoppeld met Azure Resource Manager beheren

Om te voorkomen dat gevoelige informatie, geen persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of tags te verwijderen. Azure Resource Manager biedt bewerkingen waarmee u persoonlijke gegevens die u hebt verstrekt in implementaties, resourcegroepen of tags beheren.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Verwijderen van persoonsgegevens in de implementatiegeschiedenis

Voor implementaties, Resource Manager en blijven behouden parameterwaarden statusberichten in de geschiedenis van de implementatie. Deze waarden blijft actief totdat u de implementatie uit de geschiedenis verwijderen. Als u wilt zien als u de persoonlijke gegevens in deze waarden hebt opgegeven, een lijst de implementaties. Als u persoonlijke gegevens hebt gevonden, verwijdert u de implementaties van de geschiedenis.

Lijst **implementaties** gebruiken in de geschiedenis:

* [Lijst met door resourcegroep](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzResourceGroupDeployment](/powershell/module/az.resources/Get-AzResourceGroupDeployment)
* [AZ group deployment lijst](/cli/azure/group/deployment#az-group-deployment-list)

Verwijderen van **implementaties** uit de geschiedenis gebruiken:

* [Verwijderen](/rest/api/resources/deployments/delete)
* [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/Remove-AzResourceGroupDeployment)
* [implementatie van de AZ-groep verwijderen](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Persoonlijke gegevens in de namen van resourcegroepen verwijderen

De naam van de resourcegroep clusterverbinding blijven behouden totdat u de resourcegroep verwijderen. Lijst om te zien of u de persoonlijke gegevens in de namen zijn opgegeven, de resourcegroepen. Als u persoonlijke gegevens [de resources verplaatsen](resource-group-move-resources.md) naar een nieuwe resourcegroep en verwijder de resourcegroep met de persoonlijke gegevens in de naam.

Lijst **resourcegroepen**, gebruiken:

* [Lijst met](/rest/api/resources/resourcegroups/list)
* [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup)
* [de lijst van de AZ-groep](/cli/azure/group#az-group-list)

Verwijderen van **resourcegroepen**, gebruiken:

* [Verwijderen](/rest/api/resources/resourcegroups/delete)
* [Remove-AzResourceGroup](/powershell/module/az.resources/Remove-AzResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Persoonlijke gegevens in labels verwijderen

Namen van de labels en waarden bewaard totdat u verwijderen of wijzigen van de tag. Als u wilt zien als u persoonlijke gegevens in de tags hebt opgegeven, een lijst de labels. Als u persoonlijke gegevens hebt gevonden, verwijdert u de labels.

Lijst **tags**, gebruiken:

* [Lijst met](/rest/api/resources/tags/list)
* [Get-AzTag](/powershell/module/az.resources/Get-AzTag)
* [AZ tag list](/cli/azure/tag#az-tag-list)

Verwijderen van **tags**, gebruiken:

* [Verwijderen](/rest/api/resources/tags/delete)
* [Remove-AzTag](/powershell/module/az.resources/Remove-AzTag)
* [AZ tag delete](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een overzicht van Azure Resource Manager, de [wat is Resource Manager?](resource-group-overview.md)