---
title: Een App Service-abonnement in Azure beheren | Microsoft Docs
description: Informatie over het uitvoeren van verschillende taken voor het beheren van een App Service-abonnement.
keywords: App service, azure app service, schaal, app service-abonnement wijzigen, maken, management beheren
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 4859d0d5-3e3c-40cc-96eb-f318b2c51a3d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cephalin
ms.openlocfilehash: 1dfe8a903e19ff524a1c4a0228e6aefcbe9ff183
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="manage-an-app-service-plan-in-azure"></a>Een App Service-abonnement in Azure beheren

Een [Azure App Service-abonnement](azure-web-sites-web-hosting-plans-in-depth-overview.md) biedt de resources die een App Service-app moet worden uitgevoerd. Deze handleiding laat zien hoe een App Service-abonnement te beheren.

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

> [!TIP]
> Als u een App Service-omgeving hebt, raadpleegt u [maken van een App Service-abonnement in een App-serviceomgeving](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

U kunt een lege App Service-abonnement maken of kunt u een schema maken als onderdeel van het maken van de app.

1. In de [Azure-portal](https://portal.azure.com), selecteer **nieuw** > **Web en mobiel**, en selecteer vervolgens **Web-App** of een ander soort App Service-app.

2. Selecteer een bestaand App Service-abonnement of maak een plan voor de nieuwe app.

   ![Een app maken in de Azure-portal.][createWebApp]

   Een plan maken:

   a. Selecteer **[+] maken van nieuwe**.

      ![Maak een App Service-abonnement.][createASP] 

   b. Voor **App Service-abonnement**, voer de naam van het plan.

   c. Voor **locatie**, selecteer de juiste locatie.

   d. Voor **prijscategorie**, selecteer een geschikte prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer opties, zoals prijzen **vrije** en **gedeelde**. Nadat u de prijscategorie hebt geselecteerd, klikt u op de **Selecteer** knop.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Een app verplaatsen naar een andere App Service-abonnement

U kunt een app verplaatsen naar een ander App Service-plan, zolang het bronplan en het plan doel zijn de _dezelfde resourcegroep en de geografische regio_.

1. In de [Azure-portal](https://portal.azure.com), blader naar de app die u wilt verplaatsen.

2. In het menu, zoekt u naar de **App Service-Plan** sectie.

3. Selecteer **wijziging App Service-abonnement** openen de **App Service-abonnement** selector.

   ![Selector van App Service-plan.][change] 

4. In de **App Service-abonnement** selector, selecteer een bestaand plan bent te verplaatsen van deze app in.   

> [!IMPORTANT]
> De **Selecteer App Service-abonnement** pagina wordt gefilterd op de volgende criteria: 
> - Er bestaat in dezelfde resourcegroep 
> - Er bestaat in dezelfde geografische regio 
> - Er bestaat in de dezelfde webruimte  
> 
> Een _webruimte_ is een logische constructie in App Service die een groepering van serverbronnen definieert. Een geografische regio (zoals VS-West) bevat veel webspaces om klanten die gebruikmaken van App Service toewijzen. U kunt de App Service-resources op dit moment niet verplaatsen tussen webspaces. 
> 

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Elk plan heeft zijn eigen prijscategorie. Bijvoorbeeld, het verplaatsen van een site uit een **vrije** laag naar een **standaard** laag kan alle apps die zijn toegewezen om aan de functies en bronnen van de **standaard** laag. Een app uit een plan voor hogere lagen verplaatsen naar een lagere lagen plan betekent echter dat u geen toegang meer tot bepaalde functies hebt. Als uw app gebruikmaakt van een functie die niet beschikbaar in het plan doel, krijgt u een fout die laat zien welke functie wordt gebruikt die niet beschikbaar. 

Bijvoorbeeld, als een van uw apps SSL-certificaten gebruikt, ziet u mogelijk dit foutbericht:

`Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`

In dit geval voordat u de app naar de doel-abonnement verplaatsen kunt, moet u een:
- De prijscategorie van de doel-planning wilt opschalen **Basic** of hoger.
- Verwijder alle SSL-verbindingen met uw app.

## <a name="move-an-app-to-a-different-region"></a>Een app verplaatsen naar een andere regio

De regio waarin uw app wordt uitgevoerd, is het gebied van de App Service-abonnement in. U kunt echter een App Service-abonnement regio wijzigen. Als u uitvoeren van uw app in een andere regio wilt, een alternatief is app klonen. Klonen wordt een kopie van uw app in een nieuw of bestaand App Service-abonnement in elke regio.

U vindt **kloon App** in de **ontwikkelingsprogramma's** gedeelte van het menu.

> [!IMPORTANT]
> Klonen, is enkele beperkingen. U kunt meer informatie over deze in [Azure App Service-App klonen](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Schalen van een App Service-abonnement

Schalen van een App Service plan de prijscategorie, Zie [een app in Azure opschalen](web-sites-scale.md).

Als u wilt uitbreiden een app-exemplaren, Zie [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Een App Service-abonnement verwijderen

Om te voorkomen onverwachte kosten wanneer u de laatste app in App Service-abonnement verwijdert, verwijdert App Service tevens het plan standaard. Als u in plaats daarvan het abonnement behouden kiest, moet u het plan naar **vrije** servicetier zodat u bent niet in rekening gebracht.

> [!IMPORTANT]
> App Service-abonnementen die u geen apps die zijn gekoppeld aan deze hebt nog steeds worden kosten in rekening omdat ze blijven reserveren geconfigureerde VM-exemplaren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een app in Azure opschalen](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
