---
title: Verbinding maken met Salesforce vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken, maken en beheren van Salesforce-records en -taken met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.topic: article
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 292d517f2c99974f4674a4c94472a0a320320ce4
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233834"
---
# <a name="monitor-create-and-manage-salesforce-resources-by-using-azure-logic-apps"></a>Bewaken en Salesforce-resources beheren met behulp van Azure Logic Apps maken

Met Azure Logic Apps en de Salesforce-connector, kunt u geautomatiseerde taken en werkstromen voor uw Salesforce-resources, zoals records, taken en -objecten, bijvoorbeeld:

* Monitor wanneer records worden gemaakt of gewijzigd. 
* Maken, ophalen, en beheren van taken en -records, waaronder invoegen, bijwerken en verwijderen van acties.

U kunt de Salesforce-triggers die te antwoorden krijgen van Salesforce en de uitvoer beschikbaar voor andere acties. U kunt acties in uw logische apps gebruiken om uit te voeren taken met Salesforce-resources. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Een [Salesforce-account](https://salesforce.com/)

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Salesforce-account. Om te beginnen met een trigger Salesforce [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Salesforce-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-salesforce"></a>Verbinding maken met Salesforce

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'salesforce' als filter voor lege, logische apps, in het zoekvak. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps, onder de stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. Typ 'salesforce' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

1. Als u wordt gevraagd of u aanmelden bij Salesforce, nu aanmelden en toegang toestaan.

   Uw referenties toestaan dat de logische app een verbinding met Salesforce maken en toegang tot uw gegevens.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/salesforce/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)