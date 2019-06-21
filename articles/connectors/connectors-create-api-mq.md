---
title: Verbinding maken met IBM MQ-server maken-Azure Logic Apps
description: Verzenden en ophalen van berichten met een Azure of on-premises IBM MQ-server en Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: valrobb
ms.author: valthom
ms.reviewer: chrishou, LADocs
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: a2894799946d069916b27a4f5bcc7bd3244705b2
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273122"
---
# <a name="connect-to-an-ibm-mq-server-from-azure-logic-apps"></a>Verbinding maken met een IBM MQ-server van Azure Logic Apps

De IBM MQ-connector verzendt en haalt berichten die zijn opgeslagen in een IBM MQ server on-premises of in Azure. Deze connector bevat een Microsoft-MQ-client die met een externe IBM MQ-server via een TCP/IP-netwerk communiceert. Dit artikel bevat een starter-handleiding voor het gebruik van de connector MQ. U kunt beginnen door te bladeren door een enkel bericht in een wachtrij en probeer het vervolgens andere acties.

De IBM MQ-connector bevat deze acties, maar er zijn geen triggers biedt:

- Een enkel bericht bladeren zonder dat het bericht is verwijderd uit de IBM MQ-server
- Berichten batchgewijs bladeren zonder de berichten uit de IBM MQ-server verwijderen
- Een enkel bericht ontvangen en het bericht uit de IBM MQ-server verwijderen
- Een batch van berichten ontvangen en de berichten van de IBM MQ-server verwijderen
- Een enkel bericht verzenden naar de IBM MQ-server

## <a name="prerequisites"></a>Vereisten

* Als u een on-premises MQ-server, [installeren van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md) op een server in uw netwerk. De server waarop de on-premises gegevensgateway is geïnstalleerd, moet ook .NET Framework 4.6 geïnstalleerd voor de MQ-connector om te werken. U moet ook een resource maken in Azure voor de on-premises gegevensgateway. Zie voor meer informatie, [instellen van de verbinding voor de gateway](../logic-apps/logic-apps-gateway-connection.md).

  Echter, als uw MQ-server openbaar beschikbaar of beschikbaar is in Azure is, u hoeft te gebruiken van de data gateway.

* Officieel ondersteunde IBM WebSphere MQ-versies:

  * MQ 7.5
  * MQ 8.0
  * MQ 9.0

* De logische app waaraan u wilt toevoegen van de actie MQ. Deze logische app moet moet dezelfde locatie gebruiken als uw on-premises data gateway-verbinding en al een trigger die de werkstroom wordt gestart. 

  De MQ-connector heeft geen triggers geen zodat u een trigger aan uw logische app eerst toevoegen moet. U kunt bijvoorbeeld de trigger met terugkeerpatroon. Als u geen ervaring met logische apps, probeer dit [Snelstartgids voor het maken van uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

## <a name="browse-a-single-message"></a>Een enkel bericht bladeren

1. Kies in de logische app, onder de trigger of een andere actie **nieuwe stap**. 

1. In het zoekvak, typ 'mq' en selecteer deze actie: **Bericht bladeren**

   ![Bericht bladeren](media/connectors-create-api-mq/Browse_message.png)

1. Als u een bestaande MQ-verbinding niet hebt, maakt u de verbinding:  

   1. Selecteer in de actie **verbinding maken via een on-premises gegevensgateway**.
   
   1. Geef de eigenschappen voor uw MQ-server.  

      Voor **Server**, voer de naam van de MQ-server of het IP-adres gevolgd door een dubbele punt en het poortnummer invoeren.
    
   1. Open de **gateway** lijst waarin worden alle eerder geconfigureerde gateway-verbindingen. Selecteer uw gateway.
    
   1. Wanneer u klaar bent, kiest u **Maken**. 
   
      Uw verbinding ziet eruit zoals in dit voorbeeld:

      ![Verbindingseigenschappen](media/connectors-create-api-mq/Connection_Properties.png)

1. Instellen van de actie-eigenschappen:

   * **wachtrij**: Geef een wachtrij die verschilt van de verbinding.

   * **MessageId**, **CorrelationId**, **groeps-id**, en andere eigenschappen: Bladeren naar een bericht op basis van de verschillende eigenschappen van de MQ-bericht

   * **IncludeInfo**: Geef **waar** extra berichtinformatie in de uitvoer wilt opnemen. Of geef **False** niet extra bericht om gegevens te nemen in de uitvoer.

   * **Time-out**: Voer een waarde om te bepalen hoe lang moet worden gewacht op een bericht in een lege wachtrij aankomt. Als er niets is opgegeven, wordt het eerste bericht in de wachtrij wordt opgehaald en er is geen tijd besteed aan het wachten op een bericht wordt weergegeven.

     ![Eigenschappen van berichten bladeren](media/connectors-create-api-mq/Browse_message_Props.png)

1. **Sla** uw wijzigingen, en vervolgens **uitvoeren** uw logische app.

   ![Opslaan en uitvoeren](media/connectors-create-api-mq/Save_Run.png)

   Nadat de uitvoering is voltooid, de stappen van de uitvoering worden weergegeven en kunt u de uitvoer bekijken.

1. Als u wilt controleren van de details voor elke stap, kiest u het groene vinkje. Als u wilt meer informatie over de uitvoergegevens bekijken, kiest u **onbewerkte uitvoer weergeven**.

   ![Bericht uitvoer bladeren](media/connectors-create-api-mq/Browse_message_output.png)  

   Hier volgt een voorbeeld van de onbewerkte uitvoer:

   ![Bericht onbewerkte uitvoer bladeren](media/connectors-create-api-mq/Browse_message_raw_output.png)

1. Als u instelt **IncludeInfo** op true, de volgende uitvoer wordt weergegeven:

   ![Bladeren-bericht opnemen info](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Meerdere berichten bladeren

De **berichten Bladeren** actie bevat een **BatchSize** optie om aan te geven hoeveel berichten uit de wachtrij moeten worden geretourneerd.  Als **BatchSize** heeft geen vermelding alle berichten worden geretourneerd. De resulterende uitvoer is een matrix van berichten.

1. Wanneer u het toevoegen van de **berichten Bladeren** actie, de eerste eerder geconfigureerde verbinding is standaard geselecteerd. Een nieuwe verbinding wilt maken, kies **verbinding wijzigen**. Of Selecteer een andere verbinding.

1. Nadat de logische app uitgevoerd is voltooid, als volgt een voorbeeld van uitvoer van de **berichten Bladeren** actie:

   ![Berichten uitvoer bladeren](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-single-message"></a>Enkel bericht ontvangen

De **ontvangen van berichten** actie heeft de dezelfde invoer en uitvoer als de **bladeren bericht** actie. Bij het gebruik van **ontvangen van berichten**, het bericht is verwijderd uit de wachtrij.

## <a name="receive-multiple-messages"></a>Meerdere berichten ontvangen

De **berichten ontvangen** actie heeft de dezelfde invoer en uitvoer als de **berichten Bladeren** actie. Bij het gebruik van **berichten ontvangen**, worden de berichten uit de wachtrij verwijderd.

Als er geen berichten in de wachtrij bij het uitvoeren van een bladeren of een ontvangen, wordt de stap mislukt en deze uitvoer:  

![MQ geen bericht fout](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-message"></a>Bericht verzenden

Wanneer u het toevoegen van de **berichten verzenden** actie, de eerste eerder geconfigureerde verbinding is standaard geselecteerd. Een nieuwe verbinding wilt maken, kies **verbinding wijzigen**. Of Selecteer een andere verbinding.

1. Selecteer een geldig bericht-type: **Datagram**, **antwoord**, of **aanvragen**  

   ![Msg steunbalken verzenden](media/connectors-create-api-mq/Send_Msg_Props.png)

1. Nadat de logische app is voltooid, als volgt een voorbeeld van uitvoer van de **bericht verzenden** actie:

   ![Msg uitvoer ook verzenden](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-reference"></a>Connector-verwijzing

Voor technische informatie over acties en beperkingen die worden beschreven van de connector openapi (voorheen Swagger) beschrijving van de connector controleren [-verwijzingspagina](/connectors/mq/).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over andere [Logic Apps-connectors](../connectors/apis-list.md)
