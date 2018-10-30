---
title: Maken en beheren van geautomatiseerde werkstromen met Visual Studio Code - Azure Logic Apps | Microsoft Docs
description: Snelstartgids voor het maken en beheren van logische apps met JSON in Visual Studio Code (VS-Code)
services: logic-apps
ms.service: logic-apps
ms.workload: azure-vs
author: ecfan
ms.author: estfan
ms.topic: article
ms.reviewer: klam, deli, LADocs
ms.suite: integration
ms.date: 10/05/2018
ms.openlocfilehash: 0fec590523fa130af2e5670a92914c056df289d1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50229614"
---
# <a name="quickstart-create-and-manage-automated-logic-app-workflows---visual-studio-code"></a>QuickStart: Maken en beheren van geautomatiseerde logic app workflows - Visual Studio Code

Met [Azure Logic Apps](../logic-apps/logic-apps-overview.md) en Visual Studio Code, die u kunt maken en beheren van logische apps die u helpen bij automatiseren van taken, werkstromen en processen voor het integreren van apps, gegevens, systemen en services voor organisaties en ondernemingen. Deze quickstart laat zien hoe u kunt maken en definities voor logische Apps-werkstroom bewerken door te werken met de werkstroom-definitieschema in JavaScript Object Notation (JSON) via een ervaring op basis van code. U kunt ook werken op bestaande logische apps die al zijn geïmplementeerd op <a href="https://docs.microsoft.com/azure/guides/developer/azure-developer-guide" target="_blank">Azure</a> in de cloud. 

Maar u kunt dezelfde taken in uitvoeren de <a href="https://portal.azure.com" target="_blank">Azure-portal</a> en in Visual Studio, u kunt sneller aan de slag in Visual Studio Code als u al bekend met definities voor logische Apps bent en wilt werken rechtstreeks in de code. U kunt bijvoorbeeld uitschakelen, inschakelen, verwijderen en vernieuwen van de gemaakte logische apps. U kunt ook werken op logic apps en integratieaccounts elk ontwikkelplatform waarop Visual Studio Code wordt uitgevoerd, zoals Linux, Windows en Mac.

In dit artikel kunt u dezelfde logische app zoals in de [Quick Start voor het maken van een logische app in Azure portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), die meer is gericht op de basisconcepten. In Visual Studio Code, is de logische app ziet eruit zoals in dit voorbeeld:

![Voltooide logische app](./media/create-logic-apps-visual-studio-code/overview.png)

Voordat u begint, zorg er dan voor dat u hebt deze items:

* Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>.

* Basiskennis over [definities voor logische Apps-werkstroom](../logic-apps/logic-apps-workflow-definition-language.md) en de structuur, waarbij JavaScript Object Notation (JSON) 

  Als u geen ervaring met Logic Apps, probeert u de Quick Start maakt u kennis met [over het maken van uw eerste logische app in Azure portal](../logic-apps/quickstart-create-first-logic-app-workflow.md), die meer is gericht op de basisconcepten. 

* Toegang tot de webpagina voor aanmelding bij Azure en uw Azure-abonnement

* Download en installeer deze hulpprogramma's als u ze nog niet hebt: 

  * <a href="https://code.visualstudio.com/" target="_blank">Visual Studio Code versie 1.25.1 of hoger</a>, wat gratis is

  * Visual Studio Code-extensie voor Azure Logic Apps

    U kunt downloaden en installeer deze uitbreiding van de [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps) of rechtstreeks uit in Visual Studio Code. 
    Zorg ervoor dat u Visual Studio Code opnieuw laden na de installatie. 

    !["Visual Studio Code-extensie voor Azure Logic Apps" zoeken](./media/create-logic-apps-visual-studio-code/find-install-logic-apps-extension.png)

    Als u wilt controleren of de extensie correct, de Azure geïnstalleerd weergegeven pictogram in de werkbalk van Visual Studio Code. 

    ![Extensie geïnstalleerd](./media/create-logic-apps-visual-studio-code/installed-extension.png)

    Zie voor meer informatie, <a href="https://code.visualstudio.com/docs/editor/extension-gallery" target="_blank">extensie Marketplace</a>. U kunt ook bekijken en bijdragen aan open-source-versie van deze extensie indienen door naar de pagina de [Azure Logic Apps-extensie voor Visual Studio Code op GitHub](https://github.com/Microsoft/vscode-azurelogicapps). 

<a name="sign-in-azure"></a>

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

1. Open Visual Studio Code. Selecteer het Azure-pictogram op de werkbalk Visual Studio Code. 

   ![Selecteer Azure pictogram](./media/create-logic-apps-visual-studio-code/open-extension.png)

1. In het venster Azure onder **Logic Apps**, selecteer **aanmelden bij Azure**. 

   ![Selecteer 'Aanmelden bij Azure'](./media/create-logic-apps-visual-studio-code/sign-in-azure.png)

   U wordt gevraagd nu aanmelden met behulp van de opgegeven verificatiecode op te geven. 

1. Kopieer de verificatiecode op te geven en kies vervolgens **kopiëren & openen**, waarmee u een nieuw browservenster opent.

   ![Prompt voor aanmelden](./media/create-logic-apps-visual-studio-code/sign-in-prompt.png)

1. Voer de verificatiecode op te geven. Wanneer u hierom wordt gevraagd, kiest u **doorgaan**.

   ![Code invoeren](./media/create-logic-apps-visual-studio-code/authentication-code.png)

1. Selecteer uw Azure-account. Nadat u zich hebt aangemeld, kunt u sluit de browser en Ga terug naar Visual Studio Code.

   De deelvensters van Logic Apps en Integratieaccounts, weergegeven in het venster Azure nu de Azure-abonnementen in uw account. 

   ![Abonnement selecteren](./media/create-logic-apps-visual-studio-code/select-azure-subscription.png)

   Als u de abonnementen niet ziet u verwacht dat, naast **Logic Apps** label, kiest u **abonnementen selecteren** (filterpictogram). Zoek en selecteer de abonnementen die u wilt.

1. Als u wilt weergeven van alle bestaande logic apps of integratieaccounts in uw Azure-abonnement, vouw uw abonnement.

   ![Logic apps en integratieaccounts weergeven](./media/create-logic-apps-visual-studio-code/existing-logic-apps.png)

<a name="create-logic-app"></a>

## <a name="create-logic-app"></a>Logische app maken

1. Als u dit nog niet hebt aangemeld bij uw Azure-abonnement uit in Visual Studio Code, volg de stappen in dit artikel voor [nu aanmelden](#sign-in-azure).

1. Selecteer in het contextmenu van uw abonnement, **maken**.

   ![Selecteer 'Maken'](./media/create-logic-apps-visual-studio-code/create-logic-app.png)

1. Selecteer een bestaande resourcegroep in de lijst waarin Azure-resourcegroepen in uw abonnement, of **maken van een nieuwe resourcegroep**. 

   In dit voorbeeld wordt een nieuwe resourcegroep gemaakt:

   ![Nieuwe resourcegroep maken](./media/create-logic-apps-visual-studio-code/select-or-create-azure-resource-group.png)

1. Geef een naam op voor uw Azure-resourcegroep en druk op ENTER.

   ![Noem uw resourcegroep](./media/create-logic-apps-visual-studio-code/enter-name-resource-group.png)

1. Selecteer de locatie van het datacenter voor het opslaan van metagegevens van uw logische app.

   ![Locatie selecteren](./media/create-logic-apps-visual-studio-code/select-location.png)

1. Geef een naam voor uw logische app en druk op ENTER.

   ![Naam van uw logische app](./media/create-logic-apps-visual-studio-code/enter-name-logic-app.png)

   De nieuwe logische app wordt nu weergegeven in het venster Azure onder uw Azure-abonnement. U kunt nu beginnen met het maken van definitie van de werkstroom van uw logische app.

1. Selecteer in het snelmenu van uw logische app, **openen in Editor**. 

   ![Open logische app in de editor](./media/create-logic-apps-visual-studio-code/open-new-logic-app.png)

   Visual Studio Code opent u een sjabloon voor logische app werkstroom-definitie (. logicapp.json-bestand) zodat u kunt beginnen met het maken van de werkstroom van uw logische app.

   ![Nieuwe logische app werkstroomdefinitie](./media/create-logic-apps-visual-studio-code/blank-logic-app-workflow-definition.png)

1. Begin de definitie van de werkstroom van uw logische app bouwen in het sjabloonbestand logic app workflow definitie. Zie voor technische documentatie, de [Definitietaal van werkstroom-schema voor Azure Logic Apps](../logic-apps/logic-apps-workflow-definition-language.md).

   Hier volgt een voorbeeld van de logische-definitie. Normaal gesproken op alfabetische volgorde weergegeven van de JSON-elementen in elke sectie, maar in dit voorbeeld bevat deze elementen min of meer in de volgorde die de logische app stappen worden weergegeven op de ontwerpfunctie.

   ```json
   {
      "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "$connections": {
            "defaultValue": {},
            "type": "Object"
         }
      },
      "triggers": {
         "When_a_feed_item_is_published": {
            "recurrence": {
               "frequency": "Minute",
               "interval": 1
            },
            "splitOn": "@triggerBody()?['value']",
            "type": "ApiConnection",
            "inputs": {
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['rss']['connectionId']"
                  }
               },
               "method": "get",
               "path": "/OnNewFeed",
               "queries": {
                  "feedUrl": "http://feeds.reuters.com/reuters/topNews"
               }
            }
         }
      },
      "actions": {
         "Send_an_email": {
            "runAfter": {},
            "type": "ApiConnection",
            "inputs": {
               "body": {
                  "Body": "Title: @{triggerBody()?['title']}\n\nDate published: @{triggerBody()?['publishDate']}\n\nLink: @{triggerBody()?['primaryLink']}",
                  "Subject": "New RSS item: @{triggerBody()?['title']}",
                  "To": "Sophie.Owen@contoso.com"
               },
               "host": {
                  "connection": {
                     "name": "@parameters('$connections')['outlook']['connectionId']"
                  }
               },
               "method": "post",
               "path": "/Mail"
            }
         }
      },
      "outputs": {}
   }   
   ```

1. Wanneer u klaar bent, sla uw logische app-definitiebestand. Wanneer u wordt gevraagd te bevestigen van de definitie van uw logische app uploaden naar uw Azure-abonnement, kiest u Visual Studio Code **uploaden**.

   ![De nieuwe logische app uploaden](./media/create-logic-apps-visual-studio-code/upload-new-logic-app.png)

   Nadat Visual Studio Code kunt u uw logische app naar Azure gepubliceerd, kunt u uw app vinden nu live en worden uitgevoerd in Azure portal. 

   ![Logische app is gepubliceerd in Azure portal](./media/create-logic-apps-visual-studio-code/published-logic-app.png)

<a name="edit-logic-app"></a>

## <a name="edit-logic-app"></a>Logische app bewerken

U kunt om te werken op een bestaande logische app die al geïmplementeerd in Azure, werkstroom-definitiebestand van de app openen in Visual Studio Code.

1. Als u dit nog niet hebt aangemeld bij uw Azure-abonnement uit in Visual Studio Code, volg de stappen in dit artikel voor [nu aanmelden](#sign-in-azure).

1. In het venster Azure onder **Logic Apps**, vouwt u uw Azure-abonnement en selecteert u de logische app die u wilt. 

1. Selecteer in het menu van uw logische app **openen in Editor**. Of naast de naam van uw logische app, kies het pictogram voor bewerken.

   ![Open editor voor bestaande logische app](./media/create-logic-apps-visual-studio-code/open-editor-existing-logic-app.png)

   Visual Studio Code opent u de. logicapp.json-bestand voor de definitie van de werkstroom van uw logische app.

   ![Geopende logic app werkstroomdefinitie](./media/create-logic-apps-visual-studio-code/edit-logic-app-workflow-definition-file.png)

1. Breng uw wijzigingen in de definitie van uw logische app.

1. Sla de wijzigingen op als u klaar bent.

1. Wanneer Visual Studio Code wordt gevraagd om bij te werken van de definitie van uw logische app in uw Azure-abonnement, kiest u **uploaden**. 

   ![Uploaden van uw bewerkingen](./media/create-logic-apps-visual-studio-code/upload-logic-app-changes.png)

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het <a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps" target="_blank">Azure Logic Apps forum</a> (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de <a href="https://aka.ms/logicapps-wish" target="_blank">website voor feedback van Logic Apps-gebruikers</a>.

