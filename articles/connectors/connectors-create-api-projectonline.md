---
title: Verbinding maken met Project Online via Azure Logic Apps | Microsoft Docs
description: Werkstromen automatiseren die worden bewaakt en Project Online-projecten, taken en resources beheren met behulp van Azure Logic Apps maken
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.assetid: 40ce621e-4925-4653-93bb-71ab9abcbdf1
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: 663363d05c1875d22a0ecc0478abcf7e0ec89c99
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230366"
---
# <a name="manage-project-online-projects-tasks-and-resources-by-using-azure-logic-apps"></a>Project Online-projecten, taken en resources beheren met behulp van Azure Logic Apps

Met Azure Logic Apps en de Project Online-connector, kunt u geautomatiseerde taken en werkstromen voor uw projecten, taken en resources kunt maken in Project Online via Office 365. Uw werkstromen kunnen uitvoeren deze acties en andere resources, bijvoorbeeld:

* Monitor als nieuwe projecten, taken of resources worden gemaakt. Monitor of wanneer nieuwe projecten worden gepubliceerd.
* Maak nieuwe projecten, taken of resources.
* Lijst met bestaande projecten of taken.
* Uitchecken, inchecken of projecten publiceren.

Project Online helpt u van plan bent, prioriteit en beheren van projecten en investeringen van vrijwel overal op bijna elk apparaat door te geven van de krachtige mogelijkheden voor projectbeheer. U kunt de Project Online triggers die te antwoorden krijgen van Project Online en beschikbaar voor andere acties maken voor de uitvoer. U kunt acties in uw logische apps gebruiken voor het uitvoeren van verschillende taken in Project Online. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Project Online, beschikbaar via een [Office 365-account](https://www.office.com/), 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Project Online-gegevens. Om te beginnen met een Project Online trigger [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van Project Online acties, start uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-project-online"></a>Verbinding maken met Project Online

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Voor lege, logische apps, in het zoekvak, typ 'Project Online' als filter. 
   Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps, onder de stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. In het zoekvak, typ 'Project Online' als filter. Selecteer de actie die u wilt onder de lijst met acties.

1. Als u wordt gevraagd of u zich aanmeldt met Project Online, nu aanmelden.

   Uw referenties toestaan dat de logische app een verbinding maken met Project Online en toegang tot uw gegevens.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/projectonline/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)