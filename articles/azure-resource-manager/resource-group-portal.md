---
title: Azure-portal gebruiken voor het beheren van Azure-resources | Microsoft Docs
description: Azure-portal en Azure Resource Manager gebruiken om uw resources te beheren. Laat zien hoe u werkt met dashboards voor het bewaken van resources.
services: azure-resource-manager,azure-portal
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/15/2016
ms.author: tomfitz
ms.openlocfilehash: 7398e01a46b5d296f26905e2063acdb98383f567
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38600358"
---
# <a name="manage-azure-resources-through-portal"></a>Azure-resources via de portal beheren

In dit artikel ziet u hoe u de [Azure-portal](https://portal.azure.com) met [Azure Resource Manager](resource-group-overview.md) voor het beheren van uw Azure-resources. Zie voor meer informatie over het implementeren van resources via de portal, [resources implementeren met Resource Manager-sjablonen en Azure portal](resource-group-template-deploy-portal.md).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="manage-resource-groups"></a>Resourcegroepen beheren

Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Over het algemeen resources die aan dezelfde resourcegroep dezelfde levenscyclus delen, zodat u kunt eenvoudig implementeren, bijwerken en als een groep verwijderen toevoegen. 

De resourcegroep slaat metagegevens op over de resources. Dat is de reden waarom u moet aangeven waar die metagegevens moeten worden opgeslagen als u een locatie voor de resourcegroep opgeeft. In verband met nalevingsvereisten moet u er mogelijk voor zorgen dat uw gegevens worden opgeslagen in een bepaalde regio.

1. Alle resourcegroepen in uw abonnement Selecteer **resourcegroepen**.
   
    ![resourcegroepen bladeren](./media/resource-group-portal/browse-groups.png)
2. Voor het maken van een lege resourcegroep selecteert **toevoegen**.
   
    ![resourcegroep toevoegen](./media/resource-group-portal/add-resource-group.png)
3. Geef een naam en locatie voor de nieuwe resourcegroep. Selecteer **Maken**.
   
    ![Resourcegroep maken](./media/resource-group-portal/create-empty-group.png)
4. Mogelijk moet u selecteren **vernieuwen** om te zien van de resourcegroep hebt gemaakt.
   
    ![vernieuwen van resourcegroep](./media/resource-group-portal/refresh-resource-groups.png)
5. Selecteer voor het aanpassen van de informatie die wordt weergegeven voor uw resourcegroepen, **kolommen**.
   
    ![kolommen aanpassen](./media/resource-group-portal/select-columns.png)
6. Selecteer de kolommen wilt toevoegen en selecteer vervolgens **Update**.
   
    ![kolommen toevoegen](./media/resource-group-portal/add-columns.png)
7. Zie voor meer informatie over het implementeren van resources aan de nieuwe resourcegroep, [resources implementeren met Resource Manager-sjablonen en Azure portal](resource-group-template-deploy-portal.md).
8. U kunt de resourcegroep aan uw dashboard vastmaken voor snelle toegang tot een resourcegroep.
   
    ![pincode-resourcegroep](./media/resource-group-portal/pin-group.png)
9. Het dashboard wordt weergegeven voor de resourcegroep en de daarbij behorende bronnen. U kunt de resourcegroepen of een van de daarbij behorende bronnen om te navigeren naar het item selecteren.
   
    ![pincode-resourcegroep](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a>Resources taggen
U kunt tags toepassen op resourcegroepen en resources voor uw activa logische manier te organiseren. Zie voor meer informatie over het werken met labels [met tags voor het ordenen van uw Azure-resources](resource-group-using-tags.md).

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a>Resources controleren
Wanneer u een resource selecteert, geeft het portal standaardgrafieken en tabellen voor het bewaken van dat resourcetype.

1. Selecteer een resource en u ziet dat de **bewaking** sectie. Dit omvat grafieken die relevant voor het resourcetype zijn. De volgende afbeelding ziet u de gegevens voor een opslagaccount te controleren.
   
    ![controle weergeven](./media/resource-group-portal/show-monitoring.png)
2. U kunt een sectie vastmaken aan uw dashboard door het beletselteken (...) boven de sectie te selecteren. U kunt ook de grootte van de sectie aanpassen of geheel verwijderen. De volgende afbeelding ziet hoe u kunt vastmaken, aanpassen of verwijderen van de sectie CPU en geheugen.
   
    ![pincode-sectie](./media/resource-group-portal/pin-cpu-section.png)
3. Nadat u hebt de sectie aan het dashboard vast te maken, ziet u de samenvatting op het dashboard. En onmiddellijk te selecteren, gaat u naar meer informatie over de gegevens.
   
    ![dashboard weergeven](./media/resource-group-portal/view-startboard.png)
4. Als u wilt de gegevens die u via de portal bewaken volledig aanpassen, gaat u naar uw standaarddashboard en selecteer **nieuw dashboard**.
   
    ![dashboard](./media/resource-group-portal/dashboard.png)
5. Geef een naam op voor uw nieuwe dashboard en sleep tegels op het dashboard. De tegels worden gefilterd op de verschillende opties.
   
    ![dashboard](./media/resource-group-portal/create-dashboard.png)
   
     Zie voor meer informatie over het werken met dashboards, [maken en delen van dashboards in de Azure-portal](../azure-portal/azure-portal-dashboards.md).

## <a name="manage-resources"></a>Resources beheren
Wanneer u een resource in de portal bekijkt, ziet u de opties voor het beheren van die resource.

![resources beheren](./media/resource-group-portal/manage-resources.png)

U kunt bewerkingen zoals het starten en stoppen van een virtuele machine of de eigenschappen van de virtuele machine opnieuw uitvoeren van deze opties.

## <a name="move-resources"></a>Resources verplaatsen
Als u nodig hebt om resources te verplaatsen naar een andere resourcegroep of een ander abonnement, Zie [resources verplaatsen naar een nieuwe resourcegroep of abonnement](resource-group-move-resources.md).

## <a name="lock-resources"></a>Resources vergrendelen
U kunt een abonnement, resourcegroep of resource om te voorkomen dat andere gebruikers in uw organisatie per ongeluk verwijderen of aanpassen van kritieke resources vergrendelen. Zie voor meer informatie [Resources vergrendelen met Azure Resource Manager](resource-group-lock-resources.md).

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a>Uw abonnement en kosten bekijken
U kunt informatie bekijken over uw abonnement en de samengevoegde kosten voor al uw resources. Selecteer **abonnementen** en het abonnement dat u wilt zien. U mogelijk slechts één abonnement te selecteren.

![abonnement](./media/resource-group-portal/select-subscription.png)

U ziet het branden-tarief.

![Burn rate](./media/resource-group-portal/burn-rate.png)

En een uitsplitsing van de kosten per resourcetype.

![resourcekosten](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a>Sjabloon exporteren
Na het instellen van de resourcegroep, is het raadzaam om de Resource Manager-sjabloon voor de resourcegroep weer te geven. De sjabloon exporteren biedt twee voordelen:

1. Omdat de sjabloon de volledige infrastructuur bevat, kunt u eenvoudig toekomstige implementaties van de oplossing automatiseren.
2. U kunt vertrouwd raken met de sjabloonsyntaxis van de door te kijken op JavaScript Object Notation (JSON) die uw oplossing aangeeft.

Zie voor stapsgewijze instructies [Azure Resource Manager-sjabloon exporteren uit bestaande resources](resource-manager-export-template.md).

## <a name="delete-resource-group-or-resources"></a>Resourcegroep of resources verwijderen
Een resourcegroep verwijdert, worden alle resources die hierin zijn opgenomen. U kunt ook afzonderlijke resources binnen een resourcegroep verwijderen. Wees voorzichtig bij het verwijderen van een resourcegroep. Deze resourcegroep kan resources die afhankelijk zijn resources in andere resourcegroepen bevatten.

![groep verwijderen](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a>Volgende stappen
* Activiteitenlogboeken weergeven, Zie [bewerkingen controleren met Resource Manager](resource-group-audit.md).
* Voor meer informatie over een implementatie, raadpleegt u [implementatiebewerkingen bekijken](resource-manager-deployment-operations.md).
* Zie voor het implementeren van resources via de portal, [resources implementeren met Resource Manager-sjablonen en Azure portal](resource-group-template-deploy-portal.md).
* Zie voor het beheren van toegang tot resources, [roltoewijzingen gebruiken voor het beheren van toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md).
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](/azure/architecture/cloud-adoption-guide/subscription-governance).

