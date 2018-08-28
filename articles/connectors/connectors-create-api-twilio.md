---
title: Verbinding maken met Twilio via Azure Logic Apps | Microsoft Docs
description: Automatiseren van taken en werkstromen die globale SMS-, MMS- en IP-berichten via uw Twilio-account beheren met behulp van Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: db7677042737ea1377af54cc02ee1c82c05435c8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43047571"
---
# <a name="manage-messages-in-twilio-with-azure-logic-apps"></a>Berichten in Twilio met Azure Logic Apps beheren

Met Azure Logic Apps en de Twilio-connector, kunt u geautomatiseerde taken en werkstromen die worden ontvangen, verzenden en berichten in Twilio, waaronder berichten van globale SMS, MMS- en IP-lijst maken. U kunt deze acties gebruiken voor het uitvoeren van taken met uw Twilio-account. U kunt ook andere acties waarmee de uitvoer van de Twilio-acties hebben. Bijvoorbeeld, wanneer er een nieuw bericht binnenkomt, kunt u verzenden het bericht met de Slack-connector-inhoud. Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, <a href="https://azure.microsoft.com/free/" target="_blank">registreer u dan nu voor een gratis Azure-account</a>. 

* Van [Twilio](https://www.twilio.com/): 

  * Uw Twilio-account-ID en [verificatietoken](https://support.twilio.com/hc/en-us/articles/223136027-Auth-Tokens-and-How-to-Change-Them), die u kunt vinden op uw Twilio-dashboard

    Uw referenties toestaan dat de logische app een verbinding maken en toegang tot uw Twilio-account van uw logische app. 
    Als u een Twilio-proefaccount, kunt u alleen voor SMS-bericht verzenden *geverifieerd* telefoonnummers.

  * Een geverifieerde Twilio-telefoonnummer dat kan SMS-bericht verzenden

  * Een geverifieerde Twilio-telefoonnummer dat SMS-berichten kunt ontvangen

* Basiskennis over [over het maken van logische apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* De logische app waar u toegang tot uw Twilio-account. Voor het gebruik van een Twilio-actie beginnen uw logische app met een andere trigger, bijvoorbeeld, de **terugkeerpatroon** trigger.

## <a name="connect-to-twilio"></a>Verbinding maken met Twilio

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open uw logische app in Logic App Designer, als het niet al geopend.

1. Kies een pad: 

     * Kies onder de laatste stap waar u een actie toevoegen, **nieuwe stap**. 

       -of-

     * Tussen de stappen waar u een actie toevoegen, de aanwijzer over de pijl tussen fasen. 
     Kies het plusteken (**+**) die wordt weergegeven, en selecteer vervolgens **een actie toevoegen**.
     
       Typ 'twilio' als filter in het zoekvak. 
       Selecteer de actie die u wilt onder de lijst met acties.

1. Geeft u de benodigde informatie voor uw verbinding en kies vervolgens **maken**:

   * De naam moet worden gebruikt voor de verbinding
   * Uw Twilio-account-ID 
   * Uw Twilio-toegangstoken (verificatie)

1. Geef de benodigde informatie voor de geselecteerde actie en doorgaan met het ontwikkelen van uw logische app-werkstroom.

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over triggers en acties limieten die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/twilio/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)