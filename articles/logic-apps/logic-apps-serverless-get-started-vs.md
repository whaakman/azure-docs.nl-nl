---
title: Een zonder server-App in Visual Studio | Microsoft Docs
description: Aan de slag met uw eerste zonder server-app met deze handleiding over het maken, implementeren en beheren van de app in Visual Studio.
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 3672beda8a502e5fe2c8182076a8edef7ee9ebf6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Een zonder server-App in Visual Studio met Logic Apps en functies

Zonder server en -mogelijkheden in Azure kunnen voor snelle ontwikkeling en implementatie van cloud-toepassingen.  Dit document is gericht op het aan de slag in Visual Studio voor het bouwen van een toepassing zonder server.  Een overzicht van zonder server in Azure [vindt u in dit artikel](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Alles voorbereiden

Hier volgen de vereisten die nodig zijn voor het bouwen van een toepassing zonder server vanuit Visual Studio:

* [Visual Studio 2017](https://www.visualstudio.com/vs/) of Visual Studio 2015 - Community, Professional of Enterprise
* [Logic Apps-Tools voor Visual Studio](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [Nieuwste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Azure Functions kernonderdelen](https://www.npmjs.com/package/azure-functions-core-tools) fungeert lokaal fouten opsporen
* Toegang tot het Internet wanneer u de ontwerpfunctie van de logische App

## <a name="getting-started-with-a-deployment-template"></a>Aan de slag met een sjabloon voor de implementatie

Het beheer van resources in Azure worden uitgevoerd binnen een resourcegroep.  Een resourcegroep is een logische groepering van resources.  Resourcegroepen toestaan implementatie en beheer van een verzameling resources.  Voor een toepassing zonder server in Azure bevat onze resourcegroep Azure Logic Apps en Azure Functions.  Via het resourcegroepproject vanuit Visual Studio kunnen we ontwikkelen, beheren en implementeren van de gehele toepassing als een enkel actief.

### <a name="create-a-resource-group-project-in-visual-studio"></a>Maak een resourcegroep-project in Visual Studio

1. Klik in Visual Studio om toe te voegen een **nieuw Project**
1. In de **Cloud** categorie, selecteert u een Azure **resourcegroep** project  
 * Als u de categorie of opgenomen project niet ziet, zorg ervoor dat u hebt de Azure SDK voor Visual Studio is geïnstalleerd
1. Geef het project een naam en locatie en selecteer **Ok** Visual Studio maken wordt u gevraagd om een sjabloon te selecteren.  U kunt selecteren om te starten van leeg, te beginnen met een logische App of een andere bron.  In dit geval we gebruiken, een Snelstartsjabloon met de Azure ons gestart met een app zonder server ophalen.
1. Selecteer op de sjablonen weergeven van **Azure Quickstart** ![Azure-snelstartsjablonen selecteren][1]
1. Selecteer de sjabloon zonder server Snelstartgids: **101-logic-app-and-function-app** en klik op **Ok**

De Quick Start-sjabloon maakt een sjabloon voor de implementatie in uw resourcegroepproject.  Deze sjabloon bevat een eenvoudige logische App, die een Azure-functies aanroept en retourneert het resultaat.  Als u opent de `azuredeploy.json` bestand in Solution Explorer, ziet u de resources voor de app zonder server.

## <a name="deploying-the-serverless-application"></a>De toepassing zonder server implementeren

Voordat u de visuele ontwerpfunctie logische App in Visual Studio openen kunt, moeten er een vooraf geïmplementeerde Azure-resourcegroep.  Hierdoor kan de ontwerpfunctie maken en gebruiken van verbindingen met de resources en services in de logische app.  Om te beginnen, moet u de gemaakt oplossing implementeert.

1. Met de rechtermuisknop op het project in Visual Studio, selecteer **implementeren**, en maak een **nieuw** implementatie ![nieuwe resource implementatie selecteren][2]
1. Selecteer een geldige Azure-abonnement en resourcegroep
1. Selecteer deze optie **implementeren** de oplossing
1. Voer in de naam voor de logische App en de Azure-functie-App.  Naam van de Azure-functie hoeft te zijn van globaal unieke

De oplossing zonder server worden geïmplementeerd in de opgegeven resourcegroep.  Als u naar kijkt de **uitvoer** in Visual Studio ziet u de status van de implementatie.

## <a name="editing-the-logic-app-in-visual-studio"></a>De logische app in Visual Studio bewerken

Zodra de oplossing is geïmplementeerd in elke willekeurige resourcegroep, kan de visuele ontwerpfunctie kan worden gebruikt om te bewerken en wijzigingen aanbrengen in de logische app.

1. Met de rechtermuisknop op de `azuredeploy.json` bestand in Solution Explorer en selecteer **met Logic Apps Designer openen**
1. Selecteer de **resourcegroep** en **locatie** de oplossing is geïmplementeerd naar en selecteer **OK**

De visuele ontwerpfunctie logische App worden nu weergegeven met Visual Studio.  U kunt stappen toevoegen, wijzigen van de werkstroom en sla de wijzigingen blijven.  U kunt ook logische apps maken vanuit Visual Studio.  Als u met de rechtermuisknop op de **Resources** in de navigator sjabloon kunt u kiezen om toe te voegen een **logische App** aan het project.  Lege logische apps laden in de visuele ontwerpfunctie zonder een implementeren in een resourcegroep.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Uitvoeringsgeschiedenis voor een geïmplementeerde logische app beheren en weergeven

U kunt ook beheren en weergeven van de uitvoeringsgeschiedenis voor logic apps die zijn geïmplementeerd in Azure.  Als u opent de **Cloud Explorer** hulpprogramma in Visual Studio kunt u met de rechtermuisknop op een logische App en wilt bewerken, uitschakelen, eigenschappen weergeven of uitvoeringsgeschiedenis weergeven.  Klik op bewerken kunt u een gepubliceerde logische app in een resourcegroep met Visual Studio-project downloaden.  Dit betekent dat zelfs als u het bouwen van uw logische app in de Azure portal eerst, u kunt nog steeds importeren en het beheren van Visual Studio.

## <a name="developing-an-azure-function-in-visual-studio"></a>Een Azure-functie in Visual Studio ontwikkelen

De implementatiesjabloon implementeert geen Azure-functies die zijn opgenomen in de oplossing voor de git-opslagplaats die is opgegeven in de `azuredeploy.json` variabelen.  Als een functie-project binnen de oplossing te ontwerpen, controleert u het in broncodebeheer (GitHub, Visual Studio Team Services, enz.) en update de `repo` variabele, de sjabloon implementeert de Azure-functie.

### <a name="creating-an-azure-function-project"></a>Een Azure-functie-project maken

Als u JavaScript, Python, F #, Bash, Batch of PowerShell gebruikt, volgt u de [stappen in de CLI functies](../azure-functions/functions-run-local.md) een project maken.  Als een functie in C# ontwikkelt, kunt u een [C#-klassenbibliotheek](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) in de huidige oplossing voor de Azure-functie.

## <a name="next-steps"></a>Volgende stappen

* [Informatie over het bouwen van een zonder server sociale dashboard](logic-apps-scenario-social-serverless.md)
* [Een logische app beheren vanuit Visual Studio Cloud Explorer](logic-apps-manage-from-vs.md)
* [Definitietaal van Logic App werkstroom](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
