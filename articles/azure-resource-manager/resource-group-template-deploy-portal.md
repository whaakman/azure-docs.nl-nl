---
title: Azure portal gebruiken voor het implementeren van Azure-resources | Microsoft Docs
description: Azure-portal en Azure Resource Manager gebruiken om uw resources te implementeren.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: tomfitz
ms.openlocfilehash: a171d9b4f054c942eebb08e7e11dd1164545f408
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460348"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Resources implementeren met Resource Manager-sjablonen en Azure Portal

Meer informatie over het gebruik van de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) implementeren van uw Azure-resources. Zie voor meer informatie over het beheren van uw resources, [Azure-resources beheren met behulp van de Azure-portal](manage-resources-portal.md).

Azure-resources implementeren met behulp van de Azure-portal meestal bestaat uit twee stappen:

- Maak een resourcegroep.
- Resources implementeren op de resourcegroep.

Bovendien kunt u ook een Azure Resource Manager-sjabloon voor het maken van Azure-resources implementeren.

In dit artikel ziet u beide methoden.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

1. Voor het maken van een nieuwe resourcegroep selecteert **resourcegroepen** uit de [Azure-portal](https://portal.azure.com).

   ![Resourcegroepen selecteren](./media/resource-group-template-deploy-portal/select-resource-groups.png)

1. Selecteer onder de resourcegroepen, **toevoegen**.

   ![resourcegroep toevoegen](./media/resource-group-template-deploy-portal/add-resource-group.png)

1. Selecteer of Voer de waarden van de volgende eigenschappen:

    - **Abonnement**: Selecteer een Azure-abonnement.
    - **Resourcegroep**: De resourcegroep een naam geven.
    - **Regio**: Geef een Azure-locatie. Dit is waar de resourcegroep slaat metagegevens op over de resources. Om wettelijke redenen, kunt u opgeven waar de metagegevens worden opgeslagen. In het algemeen is het raadzaam dat u opgeeft dat een locatie waar de meeste van uw resources worden opgeslagen. Met behulp van dezelfde locatie bevinden, kan de sjabloon vereenvoudigen.

   ![Waarden instellen](./media/resource-group-template-deploy-portal/set-group-properties.png)

1. Selecteer **Controleren + maken**.
1. Bekijk de waarden en selecteer vervolgens **maken**.
1. Selecteer **vernieuwen** voordat u de nieuwe resourcegroep in de lijst kunt zien.

## <a name="deploy-resources-to-a-resource-group"></a>Resources implementeren op een resourcegroep

Nadat u een resourcegroep hebt gemaakt, kunt u resources aan de groep implementeren vanuit de Marketplace. De Marketplace biedt vooraf gedefinieerde oplossingen voor algemene scenario's.

1. Selecteer eerst een implementatie **een resource maken** uit de [Azure-portal](https://portal.azure.com).

   ![Nieuwe resource](./media/resource-group-template-deploy-portal/new-resources.png)

1. Zoek het type resource dat u wilt implementeren. De resources zijn ingedeeld in categorieën. Als er geen de bepaalde oplossing die u wilt implementeren, kunt u de Marketplace voor het zoeken. De volgende schermafbeelding ziet u dat de Ubuntu-Server is geselecteerd.

   ![Resourcetype selecteren](./media/resource-group-template-deploy-portal/select-resource-type.png)

1. Afhankelijk van het type geselecteerde bron hebt u een verzameling relevante eigenschappen om in te stellen vóór de implementatie. U moet een doelresourcegroep selecteren voor alle typen. De volgende afbeelding ziet u hoe u een Linux-machine maken en deze implementeren in de resourcegroep die u hebt gemaakt.

   ![Een resourcegroep maken](./media/resource-group-template-deploy-portal/select-existing-group.png)

   U kunt ook een resourcegroep maken bij het implementeren van uw resources. Selecteer **nieuw** en geef een naam op voor de resourcegroep.

1. Uw implementatie begint. De implementatie kan enkele minuten duren. Sommige resources nemen meer tijd dan andere bronnen. Wanneer de implementatie is voltooid, ziet u een melding. Selecteer **naar de resource gaan** te openen

   ![Melding weergeven](./media/resource-group-template-deploy-portal/view-notification.png)

1. Na de implementatie van uw resources, kunt u meer resources toevoegen aan de resourcegroep door te selecteren **toevoegen**.

   ![Resource toevoegen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Resources implementeren vanuit een aangepaste sjabloon

Als u wilt uitvoeren van een implementatie, maar niet een van de sjablonen op Marketplace, kunt u een aangepaste sjabloon die de infrastructuur voor uw oplossing definieert. Zie voor meer informatie over het maken van sjablonen, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).

> [!NOTE]
> De interface van de portal biedt geen ondersteuning voor die verwijst naar een [geheim van een Key Vault](resource-manager-keyvault-parameter.md). In plaats daarvan gebruik [PowerShell](resource-group-template-deploy.md) of [Azure CLI](resource-group-template-deploy-cli.md) voor het implementeren van uw sjabloon, lokaal of vanaf een externe URI.

1. Voor het implementeren van een aangepaste sjabloon via de portal, selecteert u **een resource maken**, zoeken naar **sjabloon**. en selecteer vervolgens **sjabloonimplementatie**.

   ![De sjabloonimplementatie zoeken](./media/resource-group-template-deploy-portal/search-template.png)

1. Selecteer **Maken**.
1. U verschillende opties voor het maken van een sjabloon te bekijken:

    - **Bouw uw eigen sjabloon in de editor**: maken van een sjabloon met behulp van de sjabloon-editor.  De editor is in staat om een schema van de sjabloon resource toevoegen.
    - **Algemene sjablonen**: Er zijn vier algemene templatess voor het maken van een virtuele Linux-machine, Windows virtuele machine, een webtoepassing en een Azure SQL database.
    - **Een snelstartsjabloon van GitHub laden**: gebruik een bestaande [snelstartsjablonen](https://azure.microsoft.com/resources/templates/).

   ![Opties weergeven](./media/resource-group-template-deploy-portal/see-options.png)

    Deze zelfstudie bevat de instructies voor het laden van een quickstart-sjabloon.

1. Onder **een snelstartsjabloon van GitHub laden**typt of selecteert u **101-storage-account-create**.

    U hebt hiervoor twee opties:

    - **Selecteer de sjabloon**: implementeer de sjabloon.
    - **Sjabloon bewerken**: quickstart-sjabloon bewerken voordat u deze implementeert.

1. Selecteer **template bewerken** verkennen van de sjabloon-editor. De sjabloon wordt geladen in de editor. Er zijn twee parameters: **Opslagaccounttype** en **locatie**.

   ![Sjabloon maken](./media/resource-group-template-deploy-portal/show-json.png)

1. Breng een kleine wijziging aan de sjabloon. Bijvoorbeeld, werken de **storageAccountName** variabele:

    ```json
    "storageAccountName": "[concat('azstore', uniquestring(resourceGroup().id))]"
    ```

1. Selecteer **Opslaan**. U ziet nu de interface van de implementatie sjabloon. U ziet de twee parameters die u hebt gedefinieerd in de sjabloon.
1. Typ of Selecteer de waarden van eigenschappen:

    - **Abonnement**: Selecteer een Azure-abonnement.
    - **Resourcegroep**: Selecteer **nieuw** en geef een naam.
    - **Locatie**: Selecteer een Azure-locatie.
    - **Opslagaccounttype**: Gebruik de standaardwaarde.
    - **Locatie**: Gebruik de standaardwaarde.
    - **Ik ga akkoord met de bovenstaande voorwaarden**: (selecteren)

1. Selecteer **Aankoop**.

## <a name="next-steps"></a>Volgende stappen

- Zie bekijken van auditlogboeken [bewerkingen controleren met Resource Manager](./resource-group-audit.md).
- Zie voor het oplossen van fouten bij de implementatie, [implementatiebewerkingen bekijken](./resource-manager-deployment-operations.md).
- Als u wilt een sjabloon exporteren uit een implementatie of de resourcegroep, Zie [exporteren van Azure Resource Manager-sjablonen](./manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Zie voor het veilig implementatie van uw service in meerdere regio's, [Azure Deployment Manager](./deployment-manager-overview.md).
