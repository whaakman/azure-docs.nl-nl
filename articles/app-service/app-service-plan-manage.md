---
title: Een App Service-abonnement in Azure beheren | Microsoft Docs
description: Ontdek hoe App service plannen verschillende taken voor het beheren van een App Service-abonnement uitvoeren.
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
ms.openlocfilehash: 61179c5bf29ed2c338b45ba909ec01237806cf26
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="manage-an-app-service-plan-in-azure"></a>Een App Service-abonnement in Azure beheren

Een [App Service-abonnement](azure-web-sites-web-hosting-plans-in-depth-overview.md) biedt de resources die een App Service-app moet worden uitgevoerd. Deze instructies handleiding laat zien hoe een App Service-abonnement te beheren.

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

> [!TIP]
> Als u een App Service-omgeving hebt, raadpleegt u [maken van een App Service-abonnement in een App-serviceomgeving](environment/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan).

U kunt een lege App Service-abonnement maken of als onderdeel van het maken van de app.

In de [Azure-portal](https://portal.azure.com), klikt u op **nieuw** > **Web en mobiel**, en selecteer vervolgens **Web-App** of een ander type App Service-app.

![Een app maken in de Azure-portal.][createWebApp]

Vervolgens kunt u een bestaand App Service-abonnement selecteren of maken van een plan voor de nieuwe app.

 ![Maak een App Service-abonnement.][createASP]

Als u wilt een App Service-abonnement maken, klikt u op **[+] maken nieuwe**, type de **App Service-abonnement** een naam en selecteer vervolgens een geschikte **locatie**. Klik op **prijscategorie**, en selecteer vervolgens een juiste prijscategorie voor de service. Selecteer **weergeven van alle** naar de weergave meer opties, zoals prijzen **vrije** en **gedeelde**. 

Nadat u de prijscategorie hebt geselecteerd, klikt u op de **Selecteer** knop.

<a name="move"></a>

## <a name="move-an-app-to-another-app-service-plan"></a>Een app verplaatsen naar een andere App Service-abonnement

U kunt een app verplaatsen naar een ander App Service-plan, zolang het bronplan en het doel-plan worden de _dezelfde resourcegroep en de geografische regio_.

Als u wilt een app verplaatsen naar een ander plan, navigeert u naar de app die u verplaatsen wilt de [Azure-portal](https://portal.azure.com).

In de **Menu**, zoekt u naar de **App Service-Plan** sectie.

Selecteer **wijziging App Service-abonnement** om het proces te starten.

**App Service-abonnement wijzigen** Hiermee opent u de **App Service-abonnement** selector. Selecteer een bestaand abonnement verplaatsen van deze app in. 

> [!IMPORTANT]
> De **Selecteer App Service-abonnement** pagina wordt gefilterd op de volgende criteria: 
> - Er bestaat in dezelfde resourcegroep 
> - Er bestaat in dezelfde geografische regio 
> - Er bestaat in de dezelfde webruimte  
> 
> Een _webruimte_ is een logische constructie in App Service die een groepering van serverbronnen definieert. Een geografische regio (zoals VS-West) bevat veel webspaces om klanten die gebruikmaken van App Service toewijzen. Op dit moment App Service-bronnen kunnen worden verplaatst tussen webspaces niet. 
> 

![Selector van App Service-plan.][change]

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Elk plan heeft zijn eigen prijscategorie. Bijvoorbeeld, het verplaatsen van een site uit een **vrije** laag naar een **standaard** servicetier, kunnen alle apps die zijn toegewezen om aan de functies en bronnen van de **standaard** laag. Een app uit vanaf een hoger plan gelaagde verplaatsen naar een lagere gelaagde plan betekent echter dat u geen toegang meer tot bepaalde functies hebt. Als uw app gebruikmaakt van een functie die niet beschikbaar in het plan doel, krijgt u een fout die laat zien welke functie wordt gebruikt die niet beschikbaar. Bijvoorbeeld, als een van uw apps SSL-certificaten gebruikt, ziet u mogelijk het volgende foutbericht: `Cannot update the site with hostname '<app_name>' because its current SSL configuration 'SNI based SSL enabled' is not allowed in the target compute mode. Allowed SSL configuration is 'Disabled'.`In dit geval moet u de prijscategorie van de doel-planning wilt opschalen **Basic** of hoger, of moet u alle SSL-verbindingen verwijderen uw app, voordat u de app naar de doel-abonnement verplaatsen kunt.

## <a name="move-an-app-to-a-different-region"></a>Een app verplaatsen naar een andere regio

De regio waarin uw app wordt uitgevoerd, is het gebied van de App Service-abonnement in. U kunt echter een App Service-abonnement regio wijzigen. Als u uitvoeren van uw app in een andere regio wilt, een alternatief is app klonen. Klonen wordt een kopie van uw app in een nieuw of bestaand App Service-abonnement in elke regio.

U vindt **kloon App** in de **ontwikkelingsprogramma's** gedeelte van het menu.

> [!IMPORTANT]
> Klonen, is enkele beperkingen die u op over kunt lezen [Azure App Service-App klonen](app-service-web-app-cloning.md).

## <a name="scale-an-app-service-plan"></a>Schalen van een App Service-abonnement

Schalen van een App Service plan de prijscategorie, Zie [een app in Azure opschalen](web-sites-scale.md).

Als u wilt uitbreiden een app-exemplaren, Zie [aantal exemplaren handmatig of automatisch schalen](../monitoring-and-diagnostics/insights-how-to-scale.md).

<a name="delete"></a>

## <a name="delete-an-app-service-plan"></a>Een App Service-abonnement verwijderen

Om te voorkomen onverwachte kosten wanneer u de laatste app in App Service-abonnement verwijdert, verwijdert App Service tevens het plan standaard. Als het plan in plaats daarvan behoudt, moet u het plan naar **vrije** servicetier zodat u niet ophalen in rekening gebracht.

> [!IMPORTANT]
> **App Service-plannen** waarvoor geen apps die zijn gekoppeld aan hen nog steeds kosten omdat ze nog steeds reserveren geconfigureerde VM-exemplaren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een app in Azure opschalen](web-sites-scale.md)

[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
