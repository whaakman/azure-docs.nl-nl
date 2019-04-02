---
title: Een beleidstoewijzing maken met een Resource Manager-sjabloon
description: In dit artikel begeleidt u bij de stappen voor het Resource Manager-sjabloon gebruiken om een beleidstoewijzing voor het identificeren van niet-compatibele resources te maken.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: quickstart
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 354d5aa250449b87345cef17778befddc761fa19
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802504"
---
# <a name="create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Een beleidstoewijzing maken om te identificeren van niet-compatibele resources met behulp van Resource Manager-sjabloon

De eerste stap in het begrijpen van naleving in Azure is het identificeren van de status van uw resources.
In deze quickstart gaat u een beleidstoewijzing maken voor het identificeren van virtuele machines die geen beheerde schijven gebruiken.

Als u dit proces helemaal hebt doorlopen, kunt u virtuele machines identificeren die geen beheerde schijven gebruiken. Ze zijn *niet-compatibel* met de beleidstoewijzing.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="create-a-policy-assignment"></a>Een beleidstoewijzing maken

In deze quickstart maakt u een beleidstoewijzing maken en toewijzen van een ingebouwde beleidsdefinitie met de naam *virtuele machines die geen beheerde schijven gebruiken controleren*. Zie [Voorbeelden van beleid](./samples/index.md) voor een gedeeltelijke lijst met beschikbare ingebouwde beleidsregels.

Er zijn verschillende methoden voor het maken van de toewijzingen van beleid. In deze quickstart maakt u een [quickstart-sjabloon](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/).
Hier volgt een kopie van de sjabloon:

[!code-json[policy-assignment](~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json)]

> [!NOTE]
> Azure Policy-service is gratis.  Zie voor meer informatie, [overzicht van Azure Policy](./overview.md).

1. Selecteer de volgende afbeelding om te melden bij Azure portal en opent u de sjabloon:

   [![De beleidssjabloon implementeren in Azure](./media/assign-policy-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Selecteer of Voer de volgende waarden:

   | Name | Value |
   |------|-------|
   | Abonnement | Selecteer uw Azure-abonnement. |
   | Resourcegroep | Selecteer **nieuw**, Geef een naam en selecteer vervolgens **OK**. In de schermopname is de naam van de resourcegroep is *mypolicyquickstart<Date in MMDD>rg*. |
   | Locatie | Selecteer een regio. Bijvoorbeeld **US - centraal**. |
   | Naam van de toewijzing van beleid | Geef een naam van de toewijzing van beleid. U kunt de weergave van de definitie van beleid kunt gebruiken als u wilt. Bijvoorbeeld, **virtuele machines die geen beheerde schijven gebruiken controleren**. |
   | De naam van de RG | Geef de naam van een resource-groep waar u het beleid aan toewijzen. In deze snelstart gebruikt u de standaardwaarde **[resourceGroup () .name]**. **[resourceGroup()](../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)**  is een sjabloonfunctie die wordt opgehaald van de resourcegroep. |
   | Beleidsdefinitie-ID | Geef **/providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4c19-b460-a2d36003525a**. |
   | Ik ga akkoord met de bovenstaande voorwaarden | (Selecteren) |

1. Selecteer **Aankoop**.

Enkele aanvullende bronnen:

- Meer voorbeelden van sjablonen, Zie [Azure Quickstart-sjabloon](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- De sjabloonverwijzing, Ga naar [Azure sjabloonverwijzing](/azure/templates/microsoft.authorization/allversions).
- Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/).
- Zie voor meer abonnementsniveau implementatie [maken van resourcegroepen en resources op het abonnementsniveau](../../azure-resource-manager/deploy-to-subscription.md).

## <a name="identify-non-compliant-resources"></a>Niet-compatibele resources identificeren

Selecteer **Naleving** links op de pagina. Zoek dan de beleidstoewijzing **Virtuele machines zonder beheerde schijven controleren** die u hebt gemaakt.

![Beleid voor naleving-overzichtspagina](./media/assign-policy-template/policy-compliance.png)

Als er bestaande resources zijn die niet conform deze nieuwe toewijzing zijn, worden deze weergegeven bij **Niet-conforme resources**.

Zie voor meer informatie, [de werking van naleving](./how-to/get-compliance-data.md#how-compliance-works).

## <a name="clean-up-resources"></a>Resources opschonen

Als u de gemaakte toewijzing wilt verwijderen, volgt u deze stappen:

1. Selecteer **Naleving** (of **Toewijzingen**) aan de linkerkant van de pagina Azure Policy en zoek de beleidstoewijzing **Controleren van virtuele machines die geen beheerde schijven gebruiken** die u hebt gemaakt.

1. Met de rechtermuisknop op de **virtuele machines die geen beheerde schijven gebruiken controleren** beleidstoewijzing en selecteer **toewijzing verwijderen**.

   ![Een toewijzing verwijderen uit de overzichtspagina van naleving](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een ingebouwde beleidsdefinitie toegewezen aan een bereik en het nalevingsrapport geëvalueerd. De beleidsdefinitie controleert of alle resources in het bereik conform zijn en identificeert welke dit niet zijn.

Ga voor meer informatie over het toewijzen van beleid om te controleren of nieuwe resources conform zijn verder met de zelfstudie voor:

> [!div class="nextstepaction"]
> [Beleid maken en beheren](./tutorials/create-and-manage.md)