---
title: Azure Resource Manager-groepen beheren met behulp van Azure portal | Microsoft Docs
description: Azure portal gebruiken om uw Azure Resource Manager-groepen te beheren.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: bc3c1a05c64edea260bd177dd7eaefc003db5310
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296292"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Azure Resource Manager-resourcegroepen beheren met behulp van Azure portal

Meer informatie over het gebruik van de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resourcegroepen. Zie voor het beheren van Azure-resources, [Azure-resources beheren met behulp van de Azure-portal](./manage-resources-portal.md).

Andere artikelen over het beheren van resourcegroepen:

- [Azure-resourcegroepen beheren met behulp van Azure CLI](./manage-resources-cli.md)
- [Azure-resourcegroepen beheren met behulp van Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Wat is er een resourcegroep

Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Over het algemeen resources die aan dezelfde resourcegroep dezelfde levenscyclus delen, zodat u kunt eenvoudig implementeren, bijwerken en als een groep verwijderen toevoegen.

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

De resourcegroep slaat metagegevens op over de resources. Wanneer u een locatie voor de resourcegroep opgeeft, bent u op te geven waar die metagegevens worden opgeslagen.

## <a name="create-resource-groups"></a>Maken van resourcegroepen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **resourcegroepen**

    ![resourcegroep toevoegen](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. Selecteer **Toevoegen**.
4. Voer de volgende waarden in:

   - **Abonnement**: Selecteer uw Azure-abonnement. 
   - **Resourcegroep**: Voer de naam van een nieuwe resourcegroep. 
   - **Regio**: Selecteer een Azure-locatie, zoals **VS-midden**.

     ![Resourcegroep maken](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. Selecteer **beoordelen en maken**
6. Selecteer **Maken**. Het duurt een paar seconden om een resourcegroep te maken.
7. Selecteer **vernieuwen** in het menu bovenaan de lijst voor de resource-groep vernieuwen, en selecteer vervolgens de zojuist gemaakte resource-groep om deze te openen. Of selecteer **melding**(het belpictogram) van de boven- en selecteer vervolgens **gaat u naar de resourcegroep** openen van de zojuist gemaakte resource-groep

    ![Ga naar de resourcegroep](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Lijst met resourcegroepen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Als u de resourcegroepen, selecteer **resourcegroepen**

    ![resourcegroepen bladeren](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Selecteer voor het aanpassen van de informatie die wordt weergegeven voor de resourcegroepen, **kolommen bewerken**. De volgende schermafbeelding ziet u de kolommen toevoegen dat u kan toevoegen aan de weergave:

## <a name="open-resource-groups"></a>Open resourcegroepen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Resourcegroepen**.
3. Selecteer de resourcegroep die u wilt openen.

## <a name="delete-resource-groups"></a>Resourcegroepen verwijderen

1. Open de resourcegroep die u wilt verwijderen.  Zie [resourcegroepen openen](#open-resource-groups).
2. Selecteer **Resourcegroep verwijderen**.

    ![azure-resourcegroep verwijderen](./media/manage-resource-groups-portal/delete-group.png)

Zie voor meer informatie over hoe Azure Resource Manager orders voor het verwijderen van resources, [verwijderen voor groep van Azure Resource Manager-resource](./resource-group-delete.md).

## <a name="deploy-resources-to-a-resource-group"></a>Resources implementeren op een resourcegroep

Nadat u een Resource Manager-sjabloon hebt gemaakt, kunt u de Azure-portal kunt gebruiken voor het implementeren van uw Azure-resources. Zie voor het maken van een sjabloon, [Quick Start: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md). Zie voor het implementeren van een sjabloon met behulp van de portal, [resources implementeren met Resource Manager-sjablonen en Azure portal](resource-group-template-deploy-portal.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Verplaatsen naar een andere resourcegroep of abonnement

U kunt de resources in de groep verplaatsen naar een andere resourcegroep. Zie voor meer informatie [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](resource-group-move-resources.md).

## <a name="lock-resource-groups"></a>LOCK-resourcegroepen

Vergrendeling wordt voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources, zoals Azure-abonnement, resourcegroep of resource. 

1. Open de resourcegroep die u wilt verwijderen.  Zie [resourcegroepen openen](#open-resource-groups).
2. Selecteer in het linkerdeelvenster **Hiermee vergrendelt u**.
3. Selecteer een vergrendeling toevoegen aan de resourcegroep, **toevoegen**.
4. Voer **vergrendelingsnaam**, **type vergrendelen**, en **opmerkingen bij de**. Het lock-typen zijn onder andere **alleen-lezen**, en **verwijderen**.

    ![azure-resourcegroep vergrendelen](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Zie voor meer informatie, [Vergrendel resources om te voorkomen dat onverwachte wijzigingen](./resource-group-lock-resources.md).

## <a name="tag-resource-groups"></a>Tag-resourcegroepen

U kunt tags toepassen op resourcegroepen en resources voor uw activa logische manier te organiseren. Zie voor meer informatie, [met tags voor het ordenen van uw Azure-resources](./resource-group-using-tags.md#portal).

## <a name="export-resource-groups-to-templates"></a>Resourcegroepen naar sjablonen exporteren

Zie voor meer informatie over het exporteren van sjablonen [één of meerdere resources exporteren naar de sjabloon - Portal](export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Toegang tot resourcegroepen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).