---
title: Verbinding maken met Slack vanuit Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die bestanden bewaken en beheren van kanalen, groepen en berichten in uw Slack-account met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 7af2db528866d687064e854e00e43e81d2601b2b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042322"
---
# <a name="monitor-and-manage-slack-with-azure-logic-apps"></a>Controleren en beheren van Slack met Azure Logic Apps

Met Azure Logic Apps en de Slack-connector, kunt u geautomatiseerde taken en werkstromen die uw Slack bestanden bewaken en beheren van uw Slack-kanalen, berichten, groepen, enzovoort, bijvoorbeeld:

* Monitor als de nieuwe bestanden gemaakt.
* Maken, weergeven, en kanalen toevoegen 
* Berichten plaatsen.
* Groepen maken en niet storen instellen.

U kunt triggers die te antwoorden krijgen van uw Slack-account en de uitvoer beschikbaar voor andere acties. U kunt acties die taken met uw Slack-account uitvoeren gebruiken. U kunt ook andere acties waarmee de uitvoer van de Slack-acties hebben. Wanneer een nieuw bestand wordt gemaakt, kunt u bijvoorbeeld e-mailbericht met de Office 365 Outlook-connector verzendt. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Uw [Slack](https://slack.com/) -account en de gebruikersreferenties

  Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw Slack-account.

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Slack-account. Om te beginnen met een trigger Slack, [maken van een lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Voor het gebruik van een Slack-actie start uw logische app met een trigger, zoals een Slack-trigger of een andere trigger, zoals de **terugkeerpatroon** trigger.

## <a name="connect-to-slack"></a>Verbinding maken met Slack

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Typ 'slack' als filter voor lege, logische apps, in het zoekvak. Selecteer de gewenste trigger onder de lijst met triggers. 

   -of-

   Voor bestaande logische apps, onder de laatste stap waarin u wilt toevoegen van een actie kiezen **nieuwe stap**. 
   Typ 'slack' als filter in het zoekvak. 
   Selecteer de actie die u wilt onder de lijst met acties.

   Als u wilt toevoegen een actie tussen fasen, de aanwijzer over de pijl tussen fasen. 
   Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.

1. Als u wordt gevraagd of u zich aanmeldt bij Slack, moet u zich aanmelden bij uw Slack-werkruimte. 

   ![Aanmelden bij de werkruimte Slack](./media/connectors-create-api-slack/slack-sign-in-workspace.png)

1. Verleent toegang voor uw logische app.

   ![Toegang verlenen aan Slack](./media/connectors-create-api-slack/slack-authorize-access.png)

1. Geef de benodigde informatie voor de geselecteerde trigger of actie. Voeg meer acties toe om door te gaan met het bouwen van uw logic app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/slack/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
