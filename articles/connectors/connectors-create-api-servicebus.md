---
title: Instellen voor Azure Logic Apps met Azure Service Bus-berichtenservice | Microsoft Docs
description: Berichten verzenden en ontvangen met uw logische apps met behulp van Azure Service Bus
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 02/06/2018
ms.author: ladocs
ms.openlocfilehash: e81580db17610adc6be534c9801881f9b68b14fd
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="send-and-receive-messages-with-the-azure-service-bus-connector"></a>Verzend en ontvang berichten met de Azure Service Bus-connector

Als u wilt verzenden en ontvangen berichten met uw logische app, verbinding maken met [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). U kunt uitvoeren van acties zoals verzenden naar een wachtrij verzenden naar een onderwerp, ontvangen van een wachtrij en ontvangen van een abonnement. Meer informatie over [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) en [hoe werkt prijzen voor Logic Apps activeert](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Vereisten

Voordat u de Service Bus-connector gebruiken kunt, moet u deze items, zodat ze met elkaar zichtbaar zijn in dezelfde Azure-abonnement moeten bestaan hebben:

* Een [Service Bus-naamruimte en Berichtentiteit, zoals een wachtrij](../service-bus-messaging/service-bus-create-namespace-portal.md)
* Een [logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-service-bus"></a>Verbinding maken met Azure Servicebus

Om uw logische app toegang alle services tot, u moet maken een [ *verbinding* ](./connectors-overview.md) tussen uw logische app en de service, als u dat nog niet gedaan hebt. Deze verbinding toestaat uw logische app te krijgen tot gegevens. Voor uw logische app toegang tot uw Service Bus-account, Controleer uw machtigingen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com "Azure Portal"). 

2. Ga naar uw Service Bus *naamruimte*, niet een bepaalde 'Berichtentiteit'. Op de pagina naamruimte onder **instellingen**, kies **gedeeld toegangsbeleid**. Onder **Claims**, Controleer of u hebt **beheren** machtigingen voor die naamruimte.

   ![Machtigingen voor uw Service Bus-naamruimte beheren](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Als u later handmatig invoeren de verbindingsgegevens wilt, moet u de verbindingsreeks ophalen voor uw Service Bus-naamruimte. Kies **RootManageSharedAccessKey**. Naast de primaire sleutel verbindingsreeks, kies de knop kopiëren. De verbindingstekenreeks opslaan voor later gebruik.

   ![Kopieer de verbindingsreeks voor Service Bus-naamruimte](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Om te controleren of de verbindingsreeks gekoppeld aan uw Service Bus-naamruimte of aan een specifieke entiteit is, Controleer de verbindingsreeks voor de `EntityPath` parameter. Als u deze parameter vinden, de verbindingsreeks is bestemd voor een specifieke entiteit en is niet de juiste tekenreeks voor gebruik met uw logische app.

## <a name="trigger-workflow-when-your-service-bus-gets-new-messages"></a>Werkstroom activeert wanneer nieuwe berichten ophalen die uw Service Bus

Een [ *trigger* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een gebeurtenis die een werkstroom in uw logische app start. Volg deze stappen voor het toevoegen van de trigger die door deze berichten gedetecteerd voor het starten van een werkstroom wanneer nieuwe berichten worden verzonden naar uw Service Bus.

1. In de [Azure-portal](https://portal.azure.com "Azure-portal"), gaat u naar uw bestaande logische app of een lege logische app maken.

2. Voer in Logic Apps Designer 'servicebus' in het zoekvak als filter. Selecteer de **Service Bus** connector. 

   ![Selecteer de Service Bus-connector](./media/connectors-create-api-azure-service-bus/select-service-bus-connector.png) 

3. Selecteer de trigger die u wilt gebruiken. Selecteer bijvoorbeeld voor een logische app uitvoeren wanneer een nieuw item naar een Service Bus-wachtrij verzonden, deze trigger: **Service Bus - wanneer een bericht wordt ontvangen in een wachtrij (automatisch aanvullen)**

   ![Service Bus-trigger selecteren](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   1. Als u nog een verbinding met uw Service Bus-naamruimte hebt, wordt u gevraagd om deze verbinding nu te maken. Geef een naam op voor uw verbinding en selecteer de Service Bus-naamruimte die u wilt gebruiken.

      ![Service Bus-verbinding maken](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Of voer handmatig de verbindingsreeks de optie kiezen **Voer handmatig de verbindingsinformatie**. 
      Meer informatie over [het zoeken van de verbindingsreeks](#permissions-connection-string).

   2. Het Service Bus-beleid wilt gebruiken en kiest u nu selecteren **maken**.

      ![Service Bus-verbinding, deel 2 maken](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

4. Selecteer de Service Bus-wachtrij gebruiken en het interval en frequentie voor het controleren van de wachtrij ingesteld.

   ![Selecteer de Service Bus-wachtrij en polling-interval instellen](./media/connectors-create-api-azure-service-bus/select-service-bus-queue.png)

5. Sla uw logische app op. Kies **Opslaan** op de werkbalk van de ontwerper.

Nu wanneer uw logische app controleert de geselecteerde wachtrij en een nieuw bericht gevonden, kan de trigger de acties in uw logische app voor het bericht gevonden.

## <a name="send-messages-from-your-logic-app-to-your-service-bus"></a>Berichten verzenden van uw logische app naar uw Service Bus

Een [*actie*](../logic-apps/logic-apps-overview.md#logic-app-concepts) is een taak die wordt uitgevoerd tijdens de werkstroom voor de logische app. Nadat u een trigger hebt toegevoegd aan uw logische app, kunt u een actie toevoegen om bewerkingen uit te voeren op gegevens die met deze trigger worden gegenereerd. Volg deze stappen om een bericht verzendt naar uw Service Bus messaging entiteit van uw logische app.

1. In Logic Apps Designer onder de trigger kiezen **+ een nieuwe stap** > **een actie toevoegen**.

2. Voer in het zoekvak 'servicebus' als filter. Selecteer deze connector: **Service Bus**

   ![Selecteer de Service Bus-connector](./media/connectors-create-api-azure-service-bus/select-service-bus-connector-for-action.png) 

3. Deze actie selecteert: **Service Bus - bericht verzenden**

   ![Selecteer 'Servicebus - bericht verzenden'](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png)

4. Selecteer de Berichtentiteit is de naam wachtrij of onderwerp over de locatie waar het bericht te verzenden. Voer vervolgens de inhoud van het bericht en andere details.

   ![Selecteer Berichtentiteit en details van bericht opgeven](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)    

5. Sla uw logische app op. 

U hebt nu een actie die u berichten van uw logische app verzendt ingesteld. 

## <a name="connector-specific-details"></a>Connector-specifieke details

Bekijk voor meer informatie over triggers en acties die zijn gedefinieerd door de Swagger-bestand en beperkingen, de [connector details](/connectors/servicebus/).

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [andere connectors voor Azure Logic apps](../connectors/apis-list.md)