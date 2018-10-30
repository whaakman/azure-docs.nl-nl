---
title: Verbinding maken met Yammer vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken, boeken en berichten, feeds en nog veel meer in Yammer beheren met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: b5ae0827-fbb3-45ec-8f45-ad1cc2e7eccc
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: ca2d28f3438fd166fa282488206662c95777bf3b
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233324"
---
# <a name="monitor-and-manage-your-yammer-account-by-using-azure-logic-apps"></a>Controleren en beheren van uw Yammer-account met behulp van Azure Logic Apps

Met Azure Logic Apps en de Yammer-connector, kunt u geautomatiseerde taken en werkstromen die bewaken en beheren van berichten, kanalen en meer in uw Yammer-account, samen met andere acties, bijvoorbeeld:

* Monitor als nieuwe berichten worden weergegeven in de gevolgde kanalen en groepen.
* Ophalen van berichten, groepen, netwerken, de gebruikers details en meer.
* Boeken en achtige berichten.

U kunt triggers die te antwoorden krijgen van uw Yammer-account en de uitvoer beschikbaar voor andere acties. U kunt acties die taken met uw Yammer-account uitvoeren gebruiken. U kunt ook andere acties waarmee de uitvoer van de Yammer-acties hebben. Wanneer nieuwe berichten worden weergegeven in de kanalen of groepen, kunt u die berichten delen met de Slack-connector. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw Yammer-account en de gebruikersreferenties

   Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw Yammer-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Yammer-account. Om te beginnen met een trigger Yammer [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Yammer-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-yammer"></a>Verbinding maken met Yammer

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'yammer' als filter voor lege, logische apps, in het zoekvak. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps: 
   
     * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

       -of-

     * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
     Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.
     
       Typ 'yammer' als filter in het zoekvak. 
       Selecteer de actie die u wilt onder de lijst met acties.

1. Als u wordt gevraagd of u zich aanmeldt bij Yammer, meld u nu Meld u nu, zodat u toegang kunt toestaan.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/yammer/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)