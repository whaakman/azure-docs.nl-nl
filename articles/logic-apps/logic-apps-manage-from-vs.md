---
title: Logische apps in Visual Studio - Azure Logic Apps beheren | Microsoft Docs
description: Logische apps en andere Azure activa met Visual Studio Cloud Explorer beheren
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: a5bf24de1a7a2b6d4c1ae6416c95d83ef7506da3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Beheren van uw logische apps met Visual Studio Cloud Explorer

Hoewel de [Azure-portal](https://portal.azure.com/) biedt een goede manier om te ontwerpen en beheren van Azure Logic Apps, kunt u Visual Studio Cloud Explorer voor het beheren van veel Azure activa, met inbegrip van logische apps. Visual Studio Cloud Explorer kunt u bladeren, beheren, bewerken en download gepubliceerde logic apps. Beheertaken omvatten inschakelen, uitschakelen en historie weergeven die worden uitgevoerd. 

Voordat u kunt toegang tot en beheren van uw logische apps in Visual Studio, installeren en configureren van deze Visual Studio-hulpprogramma's voor Azure Logic Apps. 

## <a name="prerequisites"></a>Vereisten

* [Visual Studio 2015 of Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Nieuwste Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 of hoger)
* [Visual Studio Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Toegang tot het Internet wanneer u de ontwerpfunctie

## <a name="install-visual-studio-tools-for-logic-apps"></a>Installeer Visual Studio-hulpprogramma's voor Logic Apps

Nadat u de vereiste onderdelen hebt geïnstalleerd, downloaden en installeren van de Azure Logic Apps-Tools voor Visual Studio.

1. Open Visual Studio. Op de **extra** selecteert u **uitbreidingen en Updates**.
2. Vouw de **Online** categorie zodat u online in de Visual Studio-galerie zoeken kunt.
3. Blader of zoek naar **Logic Apps** totdat u **Azure Logic Apps-Tools voor Visual Studio**.
4. Als u wilt downloaden en installeren van de extensie, klikt u op **downloaden**.
5. Visual Studio starten na de installatie.

> [!NOTE]
> Ga naar de Azure Logic Apps-hulpprogramma's voor Visual Studio rechtstreeks downloaden, de [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Bladeren naar logische apps in de Cloud Explorer

1.  Cloud Explorer openen op de **weergave** menu kiezen **Cloud Explorer**.
2.  Blader naar uw logische app, resourcegroep of brontype. 

    * Als u bladert per resourcetype, selecteert u uw Azure-abonnement, vouw de **Logic Apps** uit en selecteer uw logische app. 
    * Als u door de resourcegroep bladeren, vouwt u de resourcegroep met uw logische app en selecteer uw logische app.

    Als opdrachten wilt weergeven voor uw logische app, met de rechtermuisknop op uw logische app of aan de onderkant van de Cloud Explorer kiezen uit de **acties** menu.

    ![Blader naar uw logische app](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Uw logische app met Logic Apps Designer bewerken

Vanuit de Cloud Explorer kunt u een momenteel geïmplementeerde logische app openen in de ontwerpfunctie voor dezelfde die u in de Azure portal gebruiken. 

* Als u wilt bewerken van uw logische app, in de Cloud Explorer met de rechtermuisknop op uw logische app en selecteer **geopend met Logic App Editor**. 

* Als u wilt de updates publiceren naar de cloud, kies **publiceren**. 

* Kies voor het starten van een nieuw run **uitvoeren Trigger**.

![Logic Apps Designer](./media/logic-apps-manage-from-vs/designer.png)

In de ontwerpfunctie, kunt u ook **downloaden** een logische app. Deze actie automatisch parameterizes de definitie van logic Apps en de definitie van de opgeslagen als een Azure Resource Manager-implementatiesjabloon. U kunt deze sjabloon voor de implementatie toevoegen aan uw project Azure-resourcegroep.

## <a name="browse-your-logic-app-run-history"></a>Uw logische app uitvoeringsgeschiedenis bladeren

Als u wilt weergeven van de uitvoeringsgeschiedenis voor uw logische app, met de rechtermuisknop op uw logische app en selecteer **Open uitvoeringsgeschiedenis**. Selecteer de kolomkop om de volgorde van de uitvoeringsgeschiedenis op basis van een van de eigenschappen die worden weergegeven, te.

![geschiedenis uitvoeren](media/logic-apps-manage-from-vs/runs.png)

Dubbelklik op een van de exemplaren uitvoeren om weer te geven van de uitvoeringsgeschiedenis voor een exemplaar, zodat u de uitgevoerde resultaten, inclusief de invoer en uitvoer van elke stap kunt bekijken.

![Uitvoeringsgeschiedenis resultaten, invoer en uitvoer van de stappen](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Volgende stappen

* [Een logische app maken](logic-apps-create-a-logic-app.md)
* [Ontwerpen, bouwen en implementeren van logische apps in Visual Studio](logic-apps-deploy-from-vs.md)
* [Algemene voorbeelden en scenario's weergeven](logic-apps-examples-and-scenarios.md)
* [Video: Automatiseren van bedrijfsprocessen met Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Video: Uw systemen integreren met Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
