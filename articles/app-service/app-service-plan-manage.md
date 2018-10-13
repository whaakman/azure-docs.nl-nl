---
title: Een App Service-plan in Azure beheren | Microsoft Docs
description: Leer hoe u verschillende taken voor het beheren van een App Service-plan uit te voeren.
keywords: appservice, azure appservice, schaal, app service-plan, wijzigen, maken, beheren, beheer
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: f426982163a5e49264bc4f222f6869d9cbb40c89
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166063"
---
# <a name="manage-an-app-service-plan-in-azure"></a>Een App Service-plan in Azure beheren

Een [Azure App Service-plan](azure-web-sites-web-hosting-plans-in-depth-overview.md) levert de resources die een App Service-app nodig heeft om uit te voeren. Deze handleiding wordt beschreven hoe u voor het beheren van een App Service-plan.

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

> [!TIP]
> Als u een App Service-omgeving hebt, raadpleegt u [een App Service-plan maken in een App Service Environment](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

U kunt een lege App Service-plan maken of u kunt een plan maken als onderdeel van het maken van apps.

1. In de [Azure-portal](https://portal.azure.com), selecteer **nieuw** > **Web en mobiel**, en selecteer vervolgens **Web-App** of een ander soort App Service-app.

1. Selecteer een bestaand App Service-plan of maak een plan voor de nieuwe app.

   ![Een app maken in Azure portal.][createWebApp]

   Maak een plan:

   a. Selecteer **[+] Maak een nieuwe**.

      ![Een App Service-plan maken.][createASP] 

   b. Voor **App Service-plan**, voer de naam van het abonnement.

   c. Voor **locatie**, selecteer de juiste locatie.

   d. Voor **prijscategorie**, een juiste prijscategorie selecteren voor de service. Selecteer **weergeven van alle** naar de weergave meer prijsopties, zoals **gratis** en **gedeelde**. Nadat u de prijscategorie hebt geselecteerd, klikt u op de **Selecteer** knop.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Een app verplaatsen naar een andere App Service-plan

Kunt u een app verplaatsen naar een andere App Service-plan, zolang het bronplan en het doelabonnement zijn de _dezelfde resourcegroep bevinden en geografische regio_.

> [!NOTE]
> Azure heeft elke nieuwe App Service-plan geïmplementeerd in een implementatie-eenheid, intern een webruimte genoemd. Elke regio kan veel webruimten hebben, maar uw app kan alleen worden verplaatst tussen abonnementen die zijn gemaakt in de dezelfde webruimte. Een App Service Environment is een geïsoleerde webruimte, zodat apps kunnen worden verplaatst tussen abonnementen in hetzelfde App Service-omgeving, maar niet tussen abonnementen in verschillende App Service-omgevingen.
>
> U kunt de webruimte die u wilt dat bij het maken van een plan niet opgeven, maar het is mogelijk om ervoor te zorgen dat een plan is gemaakt in de dezelfde webruimte als een bestaand plan. In het kort alle abonnementen die zijn gemaakt met dezelfde resourcegroep en regio combinatie worden geïmplementeerd in de dezelfde webruimte. Bijvoorbeeld, als u een plan hebt gemaakt in resourcegroep A en B-regio, wordt klikt u vervolgens een plan dat u later in de resourcegroep A en B regio maken geïmplementeerd in de dezelfde webruimte. Houd er rekening mee dat abonnementen niet webruimten verplaatsen nadat ze zijn gemaakt, zodat u een plan kan niet naar 'de dezelfde webruimte verplaatsen' als een ander abonnement door deze te verplaatsen naar een andere resourcegroep.
> 

1. In de [Azure-portal](https://portal.azure.com), blader naar de app die u wilt verplaatsen.

1. Zoek in het menu van de **App Service-Plan** sectie.

1. Selecteer **wijziging App Service-plan** openen de **App Service-plan** selector.

   ![Kiezer voor App Service-plan.][change] 

1. In de **App Service-plan** selector, selecteer een bestaande wilt verplaatsen van deze app in.   

De **Selecteer App Service-plan** pagina toont alleen de plannen die zich in dezelfde resourcegroep bevinden en geografische regio bevinden als de huidige app App Service-plan.

Elk abonnement heeft een eigen prijscategorie. Bijvoorbeeld, het verplaatsen van een site in een **gratis** laag naar een **Standard** laag kunt u alle apps die zijn toegewezen voor het gebruik van de functies en resources van de **Standard** laag. Een app vanuit een plan voor hogere lagen verplaatsen naar een lagere lagen plan betekent echter dat u niet langer toegang tot bepaalde functies hebben. Als uw app gebruikmaakt van een functie die is niet beschikbaar in het doelabonnement, krijgt u een fout die laat zien welke functie worden gebruikt die niet beschikbaar is. 

Bijvoorbeeld, als een van uw apps SSL-certificaten gebruikt, ziet u mogelijk dit foutbericht wordt weergegeven:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

In dit geval voordat u de app naar het doelabonnement verplaatsen kunt, moet u een:
- De prijscategorie van het doel wilt opschalen **Basic** of hoger.
- Verwijder alle SSL-verbindingen met uw app.

## <a name="move-an-app-to-a-different-region"></a>Een app verplaatsen naar een andere regio

De regio waarin uw app wordt uitgevoerd, is de regio van het App Service-plan dat deel uitmaakt. U kunt een App Service-plan regio echter niet meer wijzigen. Als u wilt voor het uitvoeren van uw app in een andere regio, is een alternatieve-app klonen. Klonen, maakt een kopie van uw app in een nieuw of bestaand App Service-plan in elke regio.

U vindt **App klonen** in de **ontwikkeltools** gedeelte van het menu.

> [!IMPORTANT]
> Klonen kent enkele beperkingen. U kunt meer informatie over deze in [Azure App Service-App klonen](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Schalen van een App Service-plan

Een App Service kan worden uitgebreid plan de prijscategorie, Zie [opschalen van een app in Azure](web-sites-scale.md).

Als u wilt schalen van een app-exemplaren, Zie [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Een App Service-plan verwijderen

Onverwachte om kosten te voorkomen, wanneer u de laatste app in een App Service-plan verwijderen, verwijderd App Service ook het plan standaard. Als u ervoor in plaats daarvan het abonnement behouden kiest, moet u de planning wilt wijzigen **gratis** laag, zodat u bent niet in rekening gebracht.

> [!IMPORTANT]
> App Service-plannen die u geen apps die zijn gekoppeld aan deze hebt nog steeds kosten in rekening gebracht omdat ze blijven de geconfigureerde VM-exemplaren reserveren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een app in Azure omhoog schalen](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
