---
title: Verbinding maken met RSS-feeds van Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken en beheren van RSS-feeds met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.suite: integration
ms.topic: article
ms.assetid: a10a6277-ed29-4e68-a881-ccdad6fd0ad8
tags: connectors
ms.date: 08/24/2018
ms.openlocfilehash: c8d1122572764dda1fc550a06ae254109e3bf033
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42885795"
---
# <a name="manage-rss-feeds-by-using-azure-logic-apps"></a>RSS-kanalen beheren met behulp van Azure Logic Apps

Met Azure Logic Apps en de RSS-connector, kunt u geautomatiseerde taken en werkstromen voor een RSS-feed, bijvoorbeeld:

* Monitor als de RSS-feeditems worden gepubliceerd.
* Lijst met alle RSS-feeditems.

RSS (Rich Site Summary), ook wel heel eenvoudig Syndication, is een populaire indeling voor web-syndicatie en wordt gebruikt voor het publiceren van regelmatig bijgewerkte inhoud, zoals blogberichten en nieuws. Veel uitgevers van inhoud bieden dat een RSS-feed, zodat gebruikers zich op deze inhoud abonneren kunnen. 

U kunt een RSS-trigger die wordt antwoorden uit een RSS-feed opgehaald en maakt de uitvoer beschikbaar voor andere acties. U kunt een RSS-actie in uw logische apps gebruiken voor het uitvoeren van een taak met de RSS-feed. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereiste onderdelen

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* De URL voor een RSS-feed

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot een RSS-feed. Om te beginnen met een RSS-trigger, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een RSS-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-an-rss-feed"></a>Verbinding maken met een RSS-feed

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

   * Typ 'rss' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

     -of-

   * Voor bestaande logische apps, onder de stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. Typ 'rss' als filter in het zoekvak. Selecteer de actie die u wilt onder de lijst met acties.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/rss/).

## <a name="get-support"></a>Ondersteuning vragen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)