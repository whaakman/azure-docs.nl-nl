---
title: Azure portal gebruiken voor het implementeren van Azure-resources | Microsoft Docs
description: Azure-portal en Azure Resource Manager gebruiken om uw resources te implementeren.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 7b28129a3afe9f78d0ef749fa0c7759082c5f758
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652447"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resources implementeren met Resource Manager-sjablonen en Azure Portal

In dit artikel ziet u hoe u de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) implementeren van uw Azure-resources. Zie voor meer informatie over het beheren van uw resources, [Azure-resources beheren met behulp van de Azure-portal](manage-resources-portal.md).

## <a name="create-resource-group"></a>Een resourcegroep maken

1. Voor het maken van een lege resourcegroep selecteert **resourcegroepen**.

   ![Resourcegroepen selecteren](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Selecteer onder de resourcegroepen, **toevoegen**.

   ![Resourcegroep toevoegen](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Wijs hieraan een naam en locatie en, indien nodig, selecteert u een abonnement. U moet een locatie voor de resourcegroep opgeven, omdat de resourcegroep metagegevens op over de resources slaat. Om wettelijke redenen, kunt u opgeven waar de metagegevens worden opgeslagen. In het algemeen is het raadzaam dat u opgeeft dat een locatie waar de meeste van uw resources worden opgeslagen. Met behulp van dezelfde locatie bevinden, kan de sjabloon vereenvoudigen.

   ![Waarden instellen](./media/resource-group-template-deploy-portal/set-group-properties.png)

   Wanneer u klaar bent met het instellen van de eigenschappen, selecteert u **maken**.

1. Uw nieuwe resourcegroep Selecteer **vernieuwen**.

   ![Resourcegroepen vernieuwen](./media/resource-group-template-deploy-portal/refresh-resource-groups.png)

## <a name="deploy-resources-from-marketplace"></a>Resources implementeren vanuit Marketplace

Nadat u een resourcegroep hebt gemaakt, kunt u resources te implementeren vanuit de Marketplace. De Marketplace biedt vooraf gedefinieerde oplossingen voor algemene scenario's.

1. Selecteer eerst een implementatie **een resource maken**.

   ![Nieuwe resource](./media/resource-group-template-deploy-portal/new-resources.png)

1. Zoek het type resource dat u wilt implementeren.

   ![Resourcetype selecteren](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Als er geen de bepaalde oplossing die u wilt implementeren, kunt u de Marketplace voor het zoeken. Bijvoorbeeld, als u een Wordpress-oplossing zoekt, begint te typen **Wordpress** en selecteer de gewenste optie.

   ![Zoeken in marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

1. Afhankelijk van het type geselecteerde bron hebt u een verzameling relevante eigenschappen om in te stellen vóór de implementatie. U moet een doelresourcegroep selecteren voor alle typen. De volgende afbeelding ziet u hoe u een web-app maakt en implementeert u deze in de resourcegroep die u hebt gemaakt.

   ![Een resourcegroep maken](./media/resource-group-template-deploy-portal/select-existing-group.png)

   U kunt ook een resourcegroep maken bij het implementeren van uw resources. Selecteer **nieuw** en geef een naam op voor de resourcegroep.

   ![Nieuwe resourcegroep maken](./media/resource-group-template-deploy-portal/select-new-group.png)

1. Uw implementatie begint. De implementatie kan enkele minuten duren. Wanneer de implementatie is voltooid, ziet u een melding.

   ![Melding weergeven](./media/resource-group-template-deploy-portal/view-notification.png)

1. Na de implementatie van uw resources, kunt u meer resources toevoegen aan de resourcegroep door te selecteren **toevoegen**.

   ![Resource toevoegen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Resources implementeren vanuit een aangepaste sjabloon

Als u wilt uitvoeren van een implementatie, maar niet een van de sjablonen op Marketplace, kunt u een aangepaste sjabloon die de infrastructuur voor uw oplossing definieert. Zie voor meer informatie over het maken van sjablonen, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).

> [!NOTE]
> De interface van de portal biedt geen ondersteuning voor die verwijst naar een [geheim van een Key Vault](resource-manager-keyvault-parameter.md). In plaats daarvan gebruik [PowerShell](resource-group-template-deploy.md) of [Azure CLI](resource-group-template-deploy-cli.md) voor het implementeren van uw sjabloon, lokaal of vanaf een externe URI.

1. Voor het implementeren van een aangepaste sjabloon via de portal, selecteert u **een resource maken**, en zoek naar de **sjabloonimplementatie** totdat u deze in de opties kunt selecteren.

   ![De sjabloonimplementatie zoeken](./media/resource-group-template-deploy-portal/search-template.png)

1. Selecteer **Maken**.

   ![Selecteer Maken](./media/resource-group-template-deploy-portal/show-template-option.png)

1. U verschillende opties voor het maken van een sjabloon te bekijken. Selecteer **Bouw uw eigen sjabloon in de editor**.

   ![Opties weergeven](./media/resource-group-template-deploy-portal/see-options.png)

1. U hebt een lege sjabloon die beschikbaar zijn voor het aanpassen van is.

   ![Sjabloon maken](./media/resource-group-template-deploy-portal/blank-template.png)

1. U kunt de JSON-syntaxis handmatig bewerken, of Selecteer een vooraf gemaakte sjabloon uit de [sjabloon snelstartgalerie](https://azure.microsoft.com/resources/templates/). Voor dit artikel hebt u echter gebruiken de **resource toevoegen** optie.

   ![Sjabloon bewerken](./media/resource-group-template-deploy-portal/select-add-resource.png)

1. Selecteer **opslagaccount** en geef een naam. Wanneer u klaar bent het opgeven van de waarden, selecteert u **OK**.

   ![Opslagaccount selecteren](./media/resource-group-template-deploy-portal/add-storage-account.png)

1. De editor voegt automatisch JSON toe voor het brontype. U ziet dat dit een parameter voor het definiëren van het type opslagaccount bevat. Selecteer **Opslaan**.

   ![Sjabloon weergeven](./media/resource-group-template-deploy-portal/show-json.png)

1. U hebt nu de optie voor het implementeren van de resources die zijn gedefinieerd in de sjabloon. Te implementeren, ga akkoord met de voorwaarden en bepalingen, en selecteer **aankoop**.

   ![Sjabloon implementeren](./media/resource-group-template-deploy-portal/provide-custom-template-values.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Resources implementeren vanuit een sjabloon die is opgeslagen in uw account

De portal kunt u een sjabloon opslaan in uw Azure-account en later opnieuw te implementeren. Zie voor meer informatie over sjablonen [maken en implementeren van uw eerste Azure Resource Manager-sjabloon](resource-manager-create-first-template.md).

1. Selecteer uw opgeslagen sjablonen vindt **meer services**.

   ![Meer services](./media/resource-group-template-deploy-portal/more-services.png)

1. Zoeken naar **sjablonen** en selecteert u die optie.

   ![Sjablonen zoeken](./media/resource-group-template-deploy-portal/find-templates.png)

1. Selecteer in de lijst met sjablonen die zijn opgeslagen in uw account die u wilt werken.

   ![Opgeslagen sjablonen](./media/resource-group-template-deploy-portal/saved-templates.png)

1. Selecteer **implementeren** om deze opgeslagen sjabloon opnieuw te implementeren.

   ![Opgeslagen sjabloon implementeren](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Volgende stappen

- Zie bekijken van auditlogboeken [bewerkingen controleren met Resource Manager](./resource-group-audit.md).
- Zie voor het oplossen van fouten bij de implementatie, [implementatiebewerkingen bekijken](./resource-manager-deployment-operations.md).
- Als u wilt een sjabloon exporteren uit een implementatie of de resourcegroep, Zie [exporteren van Azure Resource Manager-sjablonen](./manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Veilig rollout uw service in meerdere regio's Zie [Azure Deployment Manager](./deployment-manager-overview.md).
