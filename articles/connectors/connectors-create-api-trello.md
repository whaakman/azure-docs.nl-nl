---
title: Verbinding maken met Trello vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bewaken en beheren van lijsten, kaarten en uw projecten Trello-kaarten met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: fe7a4377-5c24-4f72-ab1a-6d9d23e8d895
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 4ae8d3dff108f14844c31d7b9d0b0871326832a3
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046146"
---
# <a name="monitor-and-manage-trello-with-azure-logic-apps"></a>Controleren en beheren van Trello met Azure Logic Apps

Met Azure Logic Apps en de Trello-connector, kunt u geautomatiseerde taken en werkstromen die bewaken en beheren van uw Trello-lijst, kaarten, kaarten, teamleden, enzovoort, bijvoorbeeld:

* Monitor als de nieuwe kaarten worden toegevoegd aan lijsten en kaarten. 
* Maken, ophalen en beheren van kaarten, kaarten en lijsten.
* Opmerkingen en leden toevoegen aan de kaarten.
* Lijst met kaarten, bord labels, kaarten op kaarten, kaart opmerkingen, leden van de kaart, teamleden en teams waar u lid bent. 
* Get-teams.

U kunt triggers die te antwoorden krijgen van uw Trello-account en de uitvoer beschikbaar voor andere acties. U kunt acties die taken met uw account Trello uitvoeren gebruiken. U kunt ook andere acties waarmee de uitvoer van de Trello-acties hebben. Wanneer een nieuwe kaart aan lijst of bord wordt toegevoegd, kunt u bijvoorbeeld berichten met de Slack-connector verzenden. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw Trello-account en de gebruikersreferenties

  Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw account Trello.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw account Trello. Om te beginnen met een trigger Trello [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Trello-actie beginnen uw logische app met een trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-trello"></a>Verbinding maken met Trello

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'trello' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. 
   Typ 'trello' als filter in het zoekvak. 
   Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Als u wordt gevraagd of u aanmelden met Trello, verleent toegang voor uw logische app en meld u aan.

1. Geef de benodigde informatie voor uw geselecteerde trigger of actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/trello/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)