---
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 04/19/2019
ms.author: tomfitz
ms.openlocfilehash: 8bd16378e9c82a011309c12cf241b59d03405a77
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176401"
---
| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Resources per [resourcegroep](../articles/azure-resource-manager/resource-group-overview.md#resource-groups), per resourcetype |800 |Verschilt per resourcetype |
| Implementaties per resourcegroep in de implementatiegeschiedenis |800<sup>1</sup> |800 |
| Resources per implementatie |800 |800 |
| Beheervergrendelingen per unieke bereik |20 |20 |
| Aantal tags per resource of resourcegroep |15 |15 |
| Tag sleutellengte |512 |512 |
| De lengte van de tag-waarde |256 |256 |

<sup>1</sup>als u de limiet van 800 implementaties per resourcegroep bereikt, verwijdert u implementaties uit de geschiedenis die niet meer nodig zijn. Een vermelding verwijderen uit de implementatiegeschiedenis heeft geen invloed op de geïmplementeerde resources. U kunt items verwijderen uit de geschiedenis met [az group deployment verwijderen](/cli/azure/group/deployment) voor Azure CLI of [Remove-AzResourceGroupDeployment](/powershell/module/az.resources/remove-azresourcegroupdeployment) in PowerShell.  Voor een PowerShell-script dat automatiseert verwijderen van implementaties in een continue integratie en continue levering (CI/CD)-scenario, Zie [remove-deployments.ps1](https://gist.github.com/bmoore-msft/ed33fb940dafb09380174b7fca57651f).

#### <a name="template-limits"></a>Limieten voor sjabloon

| Value | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Parameters |256 |256 |
| Variabelen |256 |256 |
| Bronnen (zoals aantal kopieën) |800 |800 |
| Uitvoer |64 |64 |
| Sjabloonexpressie |24.576 tekens |24.576 tekens |
| Resources in de geëxporteerde sjablonen |200 |200 | 
| Sjabloongrootte |1 MB |1 MB |
| De grootte van parameter |64 kB |64 kB |

U kunt sommige limieten sjabloon met behulp van een geneste sjabloon overschrijdt. Zie voor meer informatie, [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](../articles/azure-resource-manager/resource-group-linked-templates.md). Als u wilt verkleinen het aantal parameters, variabelen of uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie, [objecten als parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).
