---
title: Azure-resources beheren met behulp van Azure portal | Microsoft Docs
description: Gebruik de Azure portal en Azure Resource Manager om uw resources te beheren.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 20bf38b87ce29f8506a5611ecd25cf38f6d4ed61
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56825239"
---
# <a name="manage-azure-resources-by-using-the-azure-portal"></a>Azure-resources beheren met behulp van Azure portal

Meer informatie over het gebruik van de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resources. Zie voor het beheren van resourcegroepen, [beheren Azure-resourcegroepen met behulp van de Azure-portal](./manage-resource-groups-portal.md).

Andere artikelen over het beheren van resources:

- [Azure-resources beheren met behulp van Azure CLI](./manage-resources-cli.md)
- [Azure-resources beheren met behulp van Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="deploy-resources-to-a-resource-group"></a>Resources implementeren op een resourcegroep

Nadat u een Resource Manager-sjabloon hebt gemaakt, kunt u de Azure-portal kunt gebruiken voor het implementeren van uw Azure-resources. Zie voor het maken van een sjabloon, [Quick Start: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). Zie voor het implementeren van een sjabloon met behulp van de portal, [resources implementeren met Resource Manager-sjablonen en Azure portal](resource-group-template-deploy-portal.md).

## <a name="open-resources"></a>Geopende resources

Azure-resources zijn ingedeeld door de Azure-services en resourcegroepen. De volgende procedures ziet u hoe u een opslagaccount met de naam openen **mystorage0207**. De virtuele machine bevindt zich in een resourcegroep met de naam **mystorage0207rg**.

Openen van een resource door het servicetype:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het linkerdeelvenster de Azure-service. In dit geval **opslagaccounts**.  Als u de service die wordt vermeld niet ziet, selecteert u **alle services**, en selecteer vervolgens het servicetype.

    ![azure-resource openen in de portal](./media/manage-resources-portal/manage-azure-resources-portal-open-service.png)

3. Selecteer de resource die u wilt openen.

    ![azure-resource openen in de portal](./media/manage-resources-portal/manage-azure-resources-portal-open-resource.png)

    Lijkt op een storage-account:

    ![azure-resource openen in de portal](./media/manage-resources-portal/manage-azure-resources-portal-open-resource-storage.png)

Hiermee opent u een resource door de resourcegroep:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het linkerdeelvenster **resourcegroepen** om de resource binnen de groep weer te geven.
3. Selecteer de resource die u wilt openen. 

## <a name="manage-resources"></a>Resources beheren

Wanneer u een resource in de portal bekijkt, ziet u de opties voor het beheren van die resource.

![Azure-resources beheren](./media/manage-resources-portal/manage-azure-resources-portal-manage-resource.png)

De schermafbeelding ziet u de beheeropties voor een Azure-machine. U kunt bewerkingen zoals het starten, opnieuw te starten en stoppen van een virtuele machine uitvoeren.

## <a name="delete-resources"></a>Resources verwijderen

1. Open de resource in de portal. Zie voor de stappen [openen van bronnen](#open-resources).
2. Selecteer **Verwijderen**. De volgende schermafbeelding ziet u de beheeropties voor een virtuele machine.

    ![azure-resource verwijderen](./media/manage-resources-portal/manage-azure-resources-portal-delete-resource.png)
3. Typ de naam van de resource toe aan de verwijdering te bevestigen en selecteer vervolgens **verwijderen**.

Zie voor meer informatie over hoe Azure Resource Manager orders voor het verwijderen van resources, [verwijderen voor groep van Azure Resource Manager-resource](./resource-group-delete.md).

## <a name="move-resources"></a>Resources verplaatsen

1. Open de resource in de portal. Zie voor de stappen [openen van bronnen](#open-resources).
2. Selecteer **verplaatsen**. De volgende schermafbeelding ziet u de beheeropties voor een opslagaccount.

    ![azure-resource verplaatsen](./media/manage-resources-portal/manage-azure-resources-portal-move-resource.png)
3. Selecteer **verplaatsen naar een andere resourcegroep** of **Moeve naar een ander abonnement** afhankelijk van uw behoeften.

Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](resource-group-move-resources.md).

## <a name="lock-resources"></a>Resources vergrendelen

Vergrendeling wordt voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources, zoals Azure-abonnement, resourcegroep of resource. 

1. Open de resource in de portal. Zie voor de stappen [openen van bronnen](#open-resources).
2. Selecteer **Hiermee vergrendelt u**. De volgende schermafbeelding ziet u de beheeropties voor een opslagaccount.

    ![LOCK-azure-resource](./media/manage-resources-portal/manage-azure-resources-portal-lock-resource.png)
3. Selecteer **toevoegen**, en vervolgens geeft u de eigenschappen van de vergrendeling.

Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Resources taggen

Helpt bij het organiseren van uw resourcegroep en resources logisch-tagging. 

1. Open de resource in de portal. Zie voor de stappen [openen van bronnen](#open-resources).
2. Selecteer **Tags**. De volgende schermafbeelding ziet u de beheeropties voor een opslagaccount.

    ![tag azure-resource](./media/manage-resources-portal/manage-azure-resources-portal-tag-resource.png)
3. Geef de eigenschappen van de tag, en selecteer vervolgens **opslaan**.

Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](./resource-group-using-tags.md#portal).

## <a name="monitor-resources"></a>Resources controleren

Wanneer u een resource openen, geeft het portal standaardgrafieken en tabellen voor het bewaken van dat resourcetype. De volgende schermafbeelding ziet u de grafieken voor een virtuele machine:

![Monitor voor azure-resource](./media/manage-resources-portal/manage-azure-resources-portal-monitor-resource.png)

U kunt het speldpictogram in de rechterbovenhoek van de grafieken om de grafiek naar het dashboard vast te maken. Zie voor meer informatie over het werken met dashboards, [maken en delen van dashboards in de Azure-portal](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-access-to-resources"></a>Toegang tot resources beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).