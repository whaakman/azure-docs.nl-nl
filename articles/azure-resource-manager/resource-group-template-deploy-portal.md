---
title: Azure portal gebruiken voor het implementeren van Azure-resources | Microsoft Docs
description: Azure portal en Azure Resource Manager gebruiken voor het implementeren van uw resources.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2017
ms.author: tomfitz
ms.openlocfilehash: ea91fdd58dd3b5c118fe390afe1eb355e3c26570
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resources implementeren met Resource Manager-sjablonen en Azure Portal

Dit onderwerp wordt beschreven hoe u de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het implementeren van uw Azure-resources. Zie voor meer informatie over het beheren van uw resources, [beheren Azure-resources via de portal](resource-group-portal.md).

## <a name="create-resource-group"></a>Een resourcegroep maken

1. U maakt een lege resourcegroep selecteren **resourcegroepen**.

   ![Resourcegroepen selecteren](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Selecteer onder de resourcegroepen **toevoegen**.

   ![toevoegen van resourcegroep](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Wijs hieraan een naam en locatie en, indien nodig, selecteert u een abonnement. U moet een locatie opgeven voor de resourcegroep omdat de resourcegroep metagegevens over de bronnen worden opgeslagen. Om wettelijke redenen kan u wilt opgeven waar de metagegevens worden opgeslagen. In het algemeen is het raadzaam dat u een locatie waar de meeste van uw resources opgeven. Met behulp van dezelfde locatie, kan de sjabloon vereenvoudigen.

   ![setwaarden](./media/resource-group-template-deploy-portal/set-group-properties.png)

   Wanneer u klaar bent met het instellen van de eigenschappen, selecteert u **maken**.

1. Overzicht van uw nieuwe resourcegroep selecteren **vernieuwen**.

   ![Vernieuwen van resourcegroepen](./media/resource-group-template-deploy-portal/refresh-resource-groups.png)

## <a name="deploy-resources-from-marketplace"></a>Resources van Marketplace implementeren

Nadat u een resourcegroep hebt gemaakt, kunt u kunt resources van de Marketplace implementeren. De Marketplace biedt vooraf gedefinieerde oplossingen voor algemene scenario's.

1. Selecteer voor het starten van een implementatie **nieuw**.

   ![Nieuwe bron](./media/resource-group-template-deploy-portal/new-resources.png)

1. Vinden van het type resource dat u wilt implementeren.

   ![Brontype selecteren](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Als u de specifieke oplossing die u wilt implementeren niet ziet, kunt u de Marketplace voor het zoeken. Bijvoorbeeld, als u een Wordpress-oplossing zoekt, begint te typen **Wordpress** en selecteer de gewenste optie.

   ![zoeken marketplace](./media/resource-group-template-deploy-portal/search-resource.png)

1. Afhankelijk van het type geselecteerde bron hebt u een verzameling van de relevante eigenschappen in te stellen vóór de implementatie. Voor alle typen, moet u een resourcegroep bestemming. De volgende afbeelding laat zien hoe een web-app maken en deze implementeren in de resourcegroep die u hebt gemaakt.

   ![Een resourcegroep maken](./media/resource-group-template-deploy-portal/select-existing-group.png)

   U kunt ook een resourcegroep maken bij het implementeren van uw resources. Selecteer **nieuw** en geef de resourcegroep een naam.

   ![Nieuwe resourcegroep maken](./media/resource-group-template-deploy-portal/select-new-group.png)

1. Uw implementatie begint. De implementatie kan enkele minuten duren. Als de implementatie is voltooid, ziet u een melding.

   ![melding weergeven](./media/resource-group-template-deploy-portal/view-notification.png)

1. Na implementatie van uw resources, kunt u meer resources toevoegen aan de resourcegroep door te selecteren **toevoegen**.

   ![Resource toevoegen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Resources met aangepaste sjabloon implementeren

Als u wilt uitvoeren van een implementatie, maar geen gebruik van de sjablonen in de Marketplace, kunt u een aangepaste sjabloon die de infrastructuur voor uw oplossing definieert. Zie voor meer informatie over het maken van sjablonen, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).

1. Voor het implementeren van een aangepaste sjabloon via de portal, selecteer **nieuw**, en zoek naar **sjabloonimplementatie** totdat u deze in de opties kunt selecteren.

   ![de sjabloonimplementatie zoeken](./media/resource-group-template-deploy-portal/search-template.png)

1. Selecteer **Maken**.

   ![Selecteer maken](./media/resource-group-template-deploy-portal/show-template-option.png)

1. Er zijn verschillende opties voor het maken van een sjabloon. Selecteer **bouwen van uw eigen sjabloon in de editor**.

   ![Opties weergeven](./media/resource-group-template-deploy-portal/see-options.png)

1. U hebt een lege sjabloon die beschikbaar is voor het aanpassen.

   ![Sjabloon maken](./media/resource-group-template-deploy-portal/blank-template.png)

1. U kunt de syntaxis van de JSON handmatig bewerken of Selecteer een vooraf gemaakte sjabloon van de [sjabloon snelstartgalerie](https://azure.microsoft.com/resources/templates/). Voor dit artikel hebt u echter gebruiken de **resource toevoegen** optie.

   ![sjabloon bewerken](./media/resource-group-template-deploy-portal/select-add-resource.png)

1. Selecteer **opslagaccount** en geef een naam. Als u klaar bent waarden selecteren **OK**.

   ![Opslagaccount selecteren](./media/resource-group-template-deploy-portal/add-storage-account.png)

1. De editor wordt automatisch toegevoegd voor JSON voor het brontype. U ziet dat deze een parameter voor het definiëren van het type van het opslagaccount bevat. Selecteer **Opslaan**.

   ![sjabloon weergeven](./media/resource-group-template-deploy-portal/show-json.png)

1. U hebt nu de optie voor het implementeren van de resources die zijn gedefinieerd in de sjabloon. Te implementeren, ga akkoord met de voorwaarden en selecteer **aankoop**.

   ![Sjabloon implementeren](./media/resource-group-template-deploy-portal/provide-custom-template-values.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Resources van een sjabloon die is opgeslagen in uw account implementeren

De portal kunt u een sjabloon opslaan in uw Azure-account en het later opnieuw te implementeren. Voor meer informatie over het werken met deze sjablonen, opgeslagen [aan de slag met persoonlijke sjablonen in de Azure portal](../marketplace-consumer/mytemplates-getstarted.md).

1. Selecteer uw opgeslagen sjablonen vindt **meer services**.

   ![Meer services](./media/resource-group-template-deploy-portal/more-services.png)

1. Zoeken naar **sjablonen** en selecteert u die optie.

   ![Sjablonen zoeken](./media/resource-group-template-deploy-portal/find-templates.png)

1. Selecteer in de lijst met sjablonen die zijn opgeslagen in uw account, degene die u werken wilt op.

   ![opgeslagen sjablonen](./media/resource-group-template-deploy-portal/saved-templates.png)

1. Selecteer **implementeren** deze opgeslagen sjabloon opnieuw implementeren.

   ![opgeslagen sjabloon implementeren](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Volgende stappen
* Controlelogboeken Zie [bewerkingen met Resource Manager controleren](resource-group-audit.md).
* Zie voor het oplossen van implementatiefouten [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).
* Zie voor het ophalen van een sjabloon van een implementatie of de resourcegroep, [Azure Resource Manager-sjabloon exporteren uit bestaande resources](resource-manager-export-template.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).
