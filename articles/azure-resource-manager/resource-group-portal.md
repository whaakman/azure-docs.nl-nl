---
title: Azure-portal gebruiken voor het beheren van Azure-resources | Microsoft Docs
description: Azure portal en Azure Resource Manager gebruiken voor het beheren van uw resources. Toont het werken met dashboards voor het bewaken van resources.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2016
ms.author: tomfitz
ms.openlocfilehash: 27213482c3ef6b35e1e3f887c9a336b946850802
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-azure-resources-through-portal"></a>Azure-resources via de portal beheren

Dit artikel laat zien hoe u de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resources. Zie voor meer informatie over het implementeren van resources via de portal, [implementeren van resources met Resource Manager-sjablonen en Azure-portal](resource-group-template-deploy-portal.md).

## <a name="manage-resource-groups"></a>Resourcegroepen beheren

Een resourcegroep is een container met verwante resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. In het algemeen resources die dezelfde levenscyclus naar dezelfde resourcegroep delen, zodat u kunt eenvoudig implementeren, bijwerken en ze als een groep verwijderen toevoegen. 

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

1. Selecteer om te zien alle resourcegroepen in uw abonnement, **resourcegroepen**.
   
    ![resourcegroepen bladeren](./media/resource-group-portal/browse-groups.png)
2. U maakt een lege resourcegroep selecteren **toevoegen**.
   
    ![toevoegen van resourcegroep](./media/resource-group-portal/add-resource-group.png)
3. Geef een naam en locatie voor de nieuwe resourcegroep. Selecteer **Maken**.
   
    ![resourcegroep maken](./media/resource-group-portal/create-empty-group.png)
4. U wilt selecteren **vernieuwen** om te zien van de resourcegroep hebt gemaakt.
   
    ![vernieuwen van resourcegroep](./media/resource-group-portal/refresh-resource-groups.png)
5. Selecteer voor het aanpassen van de weergegeven informatie voor uw resourcegroepen **kolommen**.
   
    ![kolommen aanpassen](./media/resource-group-portal/select-columns.png)
6. Selecteer de kolommen wilt toevoegen en selecteer vervolgens **Update**.
   
    ![kolommen toevoegen](./media/resource-group-portal/add-columns.png)
7. Zie voor meer informatie over het implementeren van resources op uw nieuwe resourcegroep, [implementeren van resources met Resource Manager-sjablonen en Azure-portal](resource-group-template-deploy-portal.md).
8. U kunt de resourcegroep voor snelle toegang tot een resourcegroep vastmaken aan uw dashboard.
   
    ![pincode resourcegroep](./media/resource-group-portal/pin-group.png)
9. Het dashboard toont de resourcegroep en de bijbehorende bronnen. U kunt ofwel de resourcegroepen of een van de daarbij behorende bronnen om te navigeren naar het item selecteren.
   
    ![pincode resourcegroep](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Tag resources
U kunt tags toepassen resourcegroepen en resources voor uw assets logische manier te organiseren. Zie voor meer informatie over het werken met labels [met labels om uw Azure-resources te organiseren](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Resources controleren
Wanneer u een resource selecteert, geeft het portal standaard grafieken en tabellen voor bewaking van dat resourcetype.

1. Selecteer een resource die en u ziet de **bewaking** sectie. Dit omvat grafieken die relevant voor het brontype zijn. De volgende afbeelding ziet u de bewakingsgegevens voor een opslagaccount.
   
    ![bewaking weergeven](./media/resource-group-portal/show-monitoring.png)
2. U kunt een sectie vastmaken aan uw dashboard met het weglatingsteken (...) boven de sectie selecteren. U kunt ook de grootte van de sectie aanpassen of verwijder deze volledig. De volgende afbeelding laat zien hoe vastmaken, aanpassen of verwijderen van de CPU en geheugen-sectie.
   
    ![sectie van de pincode](./media/resource-group-portal/pin-cpu-section.png)
3. Na het vastmaken van de sectie aan het dashboard, ziet u de samenvatting op het dashboard. En onmiddellijk te selecteren, gaat u naar meer informatie over de gegevens.
   
    ![weergave-dashboard](./media/resource-group-portal/view-startboard.png)
4. Om volledig aanpassen aan de gegevens die u via de portal bewaken, navigeer naar uw standaarddashboard en selecteer **nieuwe dashboard**.
   
    ![dashboard](./media/resource-group-portal/dashboard.png)
5. Geef een naam op voor uw nieuwe dashboard en sleep tegels op het dashboard. De tegels worden gefilterd door verschillende opties.
   
    ![dashboard](./media/resource-group-portal/create-dashboard.png)
   
     Zie voor meer informatie over het werken met dashboards, [maken en delen van dashboards in de Azure portal](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Resources beheren
Als u een resource in de portal weergeeft, ziet u de opties voor het beheren van die resource.

![Resources beheren](./media/resource-group-portal/manage-resources.png)

U kunt bewerkingen zoals het starten en stoppen van een virtuele machine of opnieuw configureren van de eigenschappen van de virtuele machine uitvoeren van deze opties.

## <a name="move-resources"></a>Resources verplaatsen
Als u resources verplaatsen naar een andere resourcegroep of een ander abonnement wilt, Zie [resources verplaatsen naar de nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

## <a name="lock-resources"></a>Resources vergrendelen
U kunt een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of wijzigen van kritieke bronnen vergrendelen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Uw abonnement en kosten weergeven
U kunt informatie weergeven over uw abonnement en de samengevoegde kosten voor al uw resources. Selecteer **abonnementen** en het abonnement dat u wilt zien. U wellicht slechts één abonnement te selecteren.

![abonnement](./media/resource-group-portal/select-subscription.png)

U ziet de snelheid branden.

![frequentie branden](./media/resource-group-portal/burn-rate.png)

En een uitsplitsing van de kosten per resourcetype.

![de kosten van resource](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Sjabloon exporteren
Na het instellen van de resourcegroep, is het raadzaam om de Resource Manager-sjabloon voor de resourcegroep weer te geven. De sjabloon exporteren biedt twee voordelen:

1. Omdat de sjabloon de volledige infrastructuur bevat, kunt u eenvoudig toekomstige implementaties van de oplossing automatiseren.
2. U kunt meer vertrouwd raken met de sjabloonsyntaxis van de door te kijken in de notatie JSON (JavaScript Object) dat uw oplossing vertegenwoordigt.

Zie voor stapsgewijze instructies [Azure Resource Manager-sjabloon exporteren uit bestaande resources](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Resourcegroep of resources verwijderen
Verwijderen van een resourcegroep, worden alle resources daarin verwijderd. U kunt ook afzonderlijke resources binnen een resourcegroep verwijderen. Wees voorzichtig bij het verwijderen van een resourcegroep. Die resourcegroep kan resources die afhankelijk zijn van resources in andere resourcegroepen bevatten.

![Groep verwijderen](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Volgende stappen
* Voor activiteitenlogboeken, Zie [bewerkingen met Resource Manager controleren](resource-group-audit.md).
* Zie voor meer informatie over een implementatie, [implementatiebewerkingen weergeven](resource-manager-deployment-operations.md).
* Zie voor het implementeren van resources via de portal [implementeren van resources met Resource Manager-sjablonen en Azure-portal](resource-group-template-deploy-portal.md).
* Zie voor het beheren van toegang tot bronnen [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../active-directory/role-based-access-control-configure.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

