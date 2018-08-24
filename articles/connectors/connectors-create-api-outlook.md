---
title: Verbinding maken met Outlook.com - Azure Logic Apps | Microsoft Docs
description: E-mailadres, agenda's en contactpersonen met Outlook.com REST-API's en Azure Logic Apps beheren
services: logic-apps
ms.service: logic-apps
author: ecfan
ms.author: estfan
manager: jeconnoc
ms.assetid: 87113c85-d158-4dd5-9ed5-5748130003d6
ms.topic: article
ms.date: 08/18/2016
ms.reviewer: klam, LADocs
ms.suite: integration
ms.openlocfilehash: 8030ab9d317c1deefaf441008b9022c4a26bb17c
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42746159"
---
# <a name="manage-email-calendars-and-contacts-in-outlookcom-with-azure-logic-apps"></a>E-mailadres, agenda's en contactpersonen in Outlook.com met Azure Logic Apps beheren

Dit artikel wordt beschreven hoe u kunt maken en beheren van uw Outlook.com-account in een logische app met de Box-connector. Op die manier kunt u logische apps die bijvoorbeeld automatiseren van taken en werkstromen voor uw Outlook.com-account maken:

* E-mail verzenden. 
* Vergaderingen plannen.
* Contactpersonen toevoegen. 

Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Vereisten

* Een [Outlook.com-account](https://outlook.live.com/owa/)

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* De logische app waar u toegang tot uw Outlook.com-account. Als uw logische app met een Outlook-trigger wilt, moet u een [lege, logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-outlookcom"></a>Verbinding maken met Outlook.com

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Connect to Outlook.com](../../includes/connectors-create-api-outlook.md)]

## <a name="connector-reference"></a>Connector-verwijzing

Zie voor technische details, zoals triggers en acties limieten, zoals is beschreven in de Swagger-bestand van de connector, de [van de connector-verwijzingspagina](/connectors/outlook/). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)