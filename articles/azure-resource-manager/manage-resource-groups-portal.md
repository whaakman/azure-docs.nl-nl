---
title: Azure Resource Manager-groepen beheren met behulp van Azure portal | Microsoft Docs
description: Azure portal gebruiken om uw Azure Resource Manager-groepen te beheren.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: cb1eb5ac27c53f4c0d48fe3644febc62f848486d
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484692"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Azure Resource Manager-resourcegroepen beheren met behulp van Azure portal

Meer informatie over het gebruik van de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resourcegroepen. Zie voor het beheren van Azure-resources, [Azure-resources beheren met behulp van de Azure-portal](./manage-resources-portal.md).

Andere artikelen over het beheren van resourcegroepen:

- [Azure-resourcegroepen beheren met behulp van Azure CLI](./manage-resources-cli.md)
- [Azure-resourcegroepen beheren met behulp van Azure PowerShell](./manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Wat is een resourcegroep

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

## <a name="list-resource-groups"></a>Een lijst met resourcegroepen maken

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

Na het instellen van uw resourcegroep is, is het raadzaam om de Resource Manager-sjabloon voor de resourcegroep weer te geven. De sjabloon exporteren biedt twee voordelen:

- Automatiseer toekomstige implementaties van de oplossing omdat de sjabloon de volledige infrastructuur bevat.
- Meer informatie over de sjabloonsyntaxis van de door te kijken op JavaScript Object Notation (JSON) die uw oplossing aangeeft.

Er zijn twee manieren om een sjabloon te exporteren:

- U kunt de daadwerkelijke sjabloon die wordt gebruikt voor de implementatie van exporteren. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u resources via de portal hebt geïmplementeerd en u de sjabloon voor het maken van deze resources wilt zien. Deze sjabloon kan ongewijzigd worden gebruikt. 
- U kunt een gegenereerde sjabloon die de huidige status van de resourcegroep exporteren. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor implementatie gebruikt. In plaats daarvan wordt een sjabloon die een 'snapshot' of 'back-up van de resourcegroep is gemaakt. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Gebruik deze optie om resources aan dezelfde resourcegroep opnieuw te implementeren. Mogelijk moet u voor het gebruik van deze sjabloon voor een andere resourcegroep, aanzienlijk te wijzigen.

### <a name="export-templates-from-deployment-history"></a>Sjablonen exporteren uit de implementatiegeschiedenis

Deze methode exporteert u de sjablonen voor bepaalde implementaties. Als u de resources zijn gewijzigd vanuit de portal of de resource toegevoegd of verwijderd in meerdere implementaties, Zie [sjablonen exporteren uit resourcegroepen](#export-templates-from-resource-groups).

1. Open de resourcegroep die u wilt exporteren.  Zie [resourcegroepen openen](#open-resource-groups).
2. Selecteer in het linkerdeelvenster **implementaties**, of Selecteer de koppeling onder **implementaties**.  Op de volgende schermafbeelding ziet **4 geslaagd** omdat er vier gescheiden implementaties met vier verschillende namen. U ziet mogelijk **1 geslaagd**.

    ![Azure resource group exporteren sjablonen](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history.png)

3. Selecteer een van de implementaties in de lijst.
4. Selecteer in het linkerdeelvenster **sjabloon**. Resource Manager haalt de volgende zes bestanden voor u op:

   - **Sjabloon**: de sjabloon die de infrastructuur voor uw oplossing definieert. Toen u het opslagaccount via de portal maakte, heeft Resource Manager een sjabloon gebruikt om het te implementeren. De sjabloon is opgeslagen voor toekomstig gebruik.
   - **Parameters**: een parameterbestand dat u kunt gebruiken om tijdens de implementatie waarden door te geven. Het bevat de waarden die u tijdens de eerste implementatie hebt opgegeven. U kunt deze waarden wijzigen wanneer u de sjabloon opnieuw gaat implementeren.
   - **CLI** -een Azure-CLI-scriptbestand dat u gebruiken kunt om de sjabloon te implementeren.
   - **PowerShell**: een Azure PowerShell-scriptbestand dat u kunt gebruiken om de sjabloon te implementeren.
   - **.NET**: een .NET-klasse die u kunt gebruiken om de sjabloon te implementeren.
   - **Ruby**: een Ruby-klasse die u kunt gebruiken om de sjabloon te implementeren.

     De portal wordt standaard de sjabloon weergegeven.

5. Selecteer **downloaden** een sjabloon exporteren naar uw lokale computer.

    ![Azure resource group exporteren sjablonen](./media/manage-resource-groups-portal/manage-resource-groups-export-templates-deployment-history-download.png)

<a name="export-templates-from-resource-groups"></a>
### <a name="export-templates-from-resource-groups"></a>Sjablonen exporteren uit resourcegroepen

Als u uw resources vanuit de portal hebt gewijzigd of toegevoegd of verwijderen van resources in meerdere implementaties, weergegeven bij het ophalen van een sjabloon uit de implementatiegeschiedenis niet in de huidige status van de resourcegroep. In deze sectie ziet u hoe u een sjabloon kunt exporteren die de huidige status van de resourcegroep voorstelt. Het is bedoeld als een momentopname van de resourcegroep die u gebruiken kunt om opnieuw aan dezelfde resourcegroep te implementeren. Als u wilt de geëxporteerde sjabloon gebruiken voor andere oplossingen, moet u deze aanzienlijk wijzigen.

1. Open de resourcegroep die u wilt exporteren.  Zie [resourcegroepen openen](#open-resource-groups).
2. Selecteer in het linkerdeelvenster **sjabloon exporteren**. Resource Manager haalt de volgende zes bestanden voor u op:

   - **Sjabloon**: de sjabloon die de infrastructuur voor uw oplossing definieert. Toen u het opslagaccount via de portal maakte, heeft Resource Manager een sjabloon gebruikt om het te implementeren. De sjabloon is opgeslagen voor toekomstig gebruik.
   - **Parameters**: een parameterbestand dat u kunt gebruiken om tijdens de implementatie waarden door te geven. Het bevat de waarden die u tijdens de eerste implementatie hebt opgegeven. U kunt deze waarden wijzigen wanneer u de sjabloon opnieuw gaat implementeren.
   - **CLI** -een Azure-CLI-scriptbestand dat u gebruiken kunt om de sjabloon te implementeren.
   - **PowerShell**: een Azure PowerShell-scriptbestand dat u kunt gebruiken om de sjabloon te implementeren.
   - **.NET**: een .NET-klasse die u kunt gebruiken om de sjabloon te implementeren.
   - **Ruby**: een Ruby-klasse die u kunt gebruiken om de sjabloon te implementeren.

     De portal wordt standaard de sjabloon weergegeven.
3. Selecteer **downloaden** een sjabloon exporteren naar uw lokale computer.

Sommige geëxporteerde sjablonen moeten sommige bewerkingen voordat ze kunnen worden gebruikt. Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).

### <a name="export-template-before-deploying"></a>Sjabloon exporteren voordat u implementeert

U kunt de portal gebruiken voor het definiëren van een resource.  Voordat u de resource implementeert, kunt u weergeven en exporteren van een sjabloon. Zie voor instructies [Quickstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal](./resource-manager-quickstart-create-templates-use-the-portal.md).

### <a name="fix-export-issues"></a>Problemen met exports oplossen

Niet alle resourcetypen ondersteunen de functie voor het exporteren van sjablonen. U ziet alleen exporteren problemen bij het exporteren van een resourcegroep in plaats van geschiedenis van uw implementatie. Als uw laatste implementatie een nauwkeurige weergave is van de huidige status van de resourcegroep, kunt u het beste de sjabloon exporteren uit de implementatiegeschiedenis in plaats van uit de resourcegroep. Exporteer alleen vanuit een resourcegroep wanneer u wijzigingen hebt aangebracht in de resourcegroep die niet zijn gedefinieerd in één sjabloon.

Exporteren-problemen oplossen door handmatig de ontbrekende resources terug in de sjabloon toevoegen. Het foutbericht bevat de resourcetypen die niet kunnen worden geëxporteerd. Zoek dat resourcetype in de Engelstalige [naslaghandleiding over sjablonen](/azure/templates/). Als u bijvoorbeeld handmatig een virtuele netwerkgateway wilt toevoegen, raadpleegt u [Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways) in de handleiding. De sjabloonverwijzing beschikt u over de JSON naar de resource toevoegen aan uw sjabloon.

Nadat u de JSON-indeling voor de resource, moet u de waarden van de resource ophalen. U ziet de waarden voor de resource met behulp van de GET-bewerking in de REST-API voor het brontype. Bijvoorbeeld, als u de waarden voor uw virtuele netwerkgateway, Zie [ophalen van de virtuele netwerkgateways -](/rest/api/network-gateway/virtualnetworkgateways/get).

## <a name="manage-access-to-resource-groups"></a>Toegang tot resourcegroepen beheren

[Op rollen gebaseerde toegangsbeheer (RBAC)](../role-based-access-control/overview.md) is de manier waarop u de toegang tot resources in Azure beheert. Zie voor meer informatie, [toegang met RBAC en de Azure-portal beheren](../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer Azure Resource Manager [overzicht van Azure Resource Manager](./resource-group-overview.md).
- Zie voor meer informatie de syntaxis van de Resource Manager-sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md).
- Zie voor meer informatie over het ontwikkelen van sjablonen, de [zelfstudies met stapsgewijze instructies](/azure/azure-resource-manager/).
- Als u de Azure Resource Manager-sjabloon schema's, Zie [sjabloonverwijzing](/azure/templates/).