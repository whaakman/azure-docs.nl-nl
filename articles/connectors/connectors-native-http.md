---
title: Verbinding maken met een HTTP-eindpunt met Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die met een HTTP-eindpunt communiceren met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: e11c6b4d-65a5-4d2d-8e13-38150db09c0b
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: e1561e3be95847efccf487c96bd9c9a8104f161b
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106445"
---
# <a name="call-http-or-https-endpoints-with-azure-logic-apps"></a>Aanroepen van HTTP of HTTPS-eindpunten met Azure Logic Apps

Met Azure Logic Apps en de connector Hypertext Transfer Protocol (HTTP), kunt u met elke HTTP of HTTPS-eindpunt communiceren met het bouwen van logic apps-werkstromen automatiseren. Bijvoorbeeld, kunt u het service-eindpunt voor uw website controleren. Wanneer een gebeurtenis plaatsvindt op dat eindpunt, zoals uw website uitvalt, wordt de gebeurtenis wordt de werkstroom van uw logische app geactiveerd en de opgegeven acties worden uitgevoerd. 

U kunt de HTTP-trigger als de eerste stap in uw werkstroom bevindt gebruiken om te controleren of *polling* een eindpunt op een regelmatige. Op elke check-, verzendt de trigger een oproep of *aanvraag* naar het eindpunt. Reactie van het eindpunt wordt bepaald of de werkstroom van uw logische app wordt uitgevoerd. De trigger wordt doorgegeven aan de inhoud uit het antwoord op de acties in uw logische app. 

U kunt de HTTP-actie als een andere stap in uw werkstroom voor het aanroepen van het eindpunt als u wilt gebruiken. Reactie van het eindpunt bepaalt hoe uw resterende werkstroomacties worden uitgevoerd.

Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* De URL voor de doel-eindpunt dat u wilt aanroepen 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app van waar u aan te roepen van het doel-eindpunt om te beginnen met de HTTP-trigger [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Start uw logische app met een trigger voor het gebruik van de HTTP-actie.

## <a name="add-http-trigger"></a>HTTP-trigger toevoegen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw lege, logische app in Logic App Designer, als het niet al geopend.

1. Typ 'http' als filter in het zoekvak. Selecteer onder de lijst met triggers, de **HTTP** trigger. 

   ![HTTP-trigger selecteren](./media/connectors-native-http/select-http-trigger.png)

1. Geef de [HTTP-trigger parameters en waarden](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger) u wilt opnemen in de aanroep naar de doel-eindpunt. Terugkeerpatroon voor hoe vaak u wilt dat de trigger instellen om te controleren of de doel-voor.

   ![HTTP-trigger parameters invoeren](./media/connectors-native-http/http-trigger-parameters.png)

   Zie voor meer informatie over de HTTP-trigger, parameters en waarden, [Trigger en actie typen verwijzing](../logic-apps/logic-apps-workflow-actions-triggers.md##http-trigger).

1. Doorgaan met het ontwikkelen van uw logische app-werkstroom met acties die worden uitgevoerd wanneer de trigger wordt geactiveerd.

## <a name="add-http-action"></a>HTTP-actie toevoegen

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies onder de laatste stap waarin u wilt toevoegen van de HTTP-actie **nieuwe stap**. 

   In dit voorbeeld wordt de logische app gestart met de HTTP-trigger als de eerste stap.

1. Typ 'http' als filter in het zoekvak. Selecteer onder de lijst met acties, de **HTTP** actie.

   ![HTTP-actie selecteren](./media/connectors-native-http/select-http-action.png)

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Geef de [HTTP-actie parameters en waarden](../logic-apps/logic-apps-workflow-actions-triggers.md##http-action) u wilt opnemen in de aanroep naar de doel-eindpunt. 

   ![HTTP-actieparameters invoeren](./media/connectors-native-http/http-action-parameters.png)

1. Wanneer u klaar bent, zorg ervoor dat u uw logische app opslaat. Kies **Opslaan** op de werkbalk van de ontwerper. 

## <a name="authentication"></a>Verificatie

Om verificatie, kies **geavanceerde opties weergeven** in de actie of trigger. Zie voor meer informatie over beschikbare verificatietypen voor HTTP-triggers en acties [Trigger en actie typen verwijzing](../logic-apps/logic-apps-workflow-actions-triggers.md#connector-authentication).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)