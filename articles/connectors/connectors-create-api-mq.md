---
title: Verbinding maken met MQ-server maken-Azure Logic Apps | Microsoft Docs
description: Verzenden en ophalen van berichten met een Azure of on-premises MQ-server en Azure Logic Apps
author: valthom
manager: jeconnoc
ms.author: valthom
ms.date: 06/01/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 59ef41b6d7bfcf8831eaa7d8d7e6af4ea279e415
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57342238"
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a>Verbinding maken met een IBM MQ-server van logische apps met behulp van de connector MQ

Microsoft-Connector voor MQ verzendt en haalt berichten die zijn opgeslagen in een MQ Server on-premises of in Azure. Deze connector bevat een Microsoft-MQ-client die met een externe IBM MQ-server via een TCP/IP-netwerk communiceert. Dit document is een starter-handleiding voor het gebruik van de connector MQ. Wij raden dat u begint met een enkel bericht in een wachtrij te bladeren en vervolgens de andere acties te proberen.

De connector MQ bevat de volgende acties. Er zijn geen triggers.

- Een enkel bericht bladeren zonder dat het bericht is verwijderd uit de IBM MQ-Server
- Berichten batchgewijs bladeren zonder de berichten uit de IBM MQ-Server verwijderen
- Een enkel bericht ontvangen en het bericht uit de IBM MQ-Server verwijderen
- Een batch van berichten ontvangen en de berichten van de IBM MQ-Server verwijderen
- Een enkel bericht verzenden naar de IBM MQ-Server

## <a name="prerequisites"></a>Vereisten

* Als u gebruikmaakt van een on-premises MQ server [installeren van de on-premises gegevensgateway](../logic-apps/logic-apps-gateway-install.md) op een server in uw netwerk. Als de MQ-Server openbaar beschikbaar, of als beschikbaar in Azure is, is klikt u vervolgens de data gateway niet gebruikt of nodig zijn.

    > [!NOTE]
    > De server waarop de On-Premises gegevensgateway is geïnstalleerd moet ook .NET Framework 4.6 geïnstalleerd voor de Connector MQ functie hebben.

* Maken van de Azure-resource voor de on-premises gegevensgateway - [instellen van de verbinding voor de gateway](../logic-apps/logic-apps-gateway-connection.md).

* Officieel ondersteunde IBM WebSphere MQ-versies:
    * MQ 7.5
    * MQ 8.0

## <a name="create-a-logic-app"></a>Een logische app maken

1. In de **Azure start board**, selecteer **+** (plusteken), **Web en mobiel**, en vervolgens **logische App**.
2. Voer de **naam**, zoals MQTestApp, **abonnement**, **resourcegroep**, en **locatie** (Gebruik de locatie waar de on-premises Gegevensverbinding van de Gateway is geconfigureerd). Selecteer **vastmaken aan dashboard**, en selecteer **maken**.  
![Logische App maken](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Een trigger toevoegen

> [!NOTE]
> De MQ-Connector heeft geen geen triggers. Dus een andere trigger te gebruiken om te beginnen uw logische app, zoals de **terugkeerpatroon** trigger.

1. De **ontwerper van logische Apps** wordt geopend, selecteer **terugkeerpatroon** in de lijst met veelgebruikte triggers.
2. Selecteer **bewerken** binnen de Trigger met terugkeerpatroon.
3. Stelt de **frequentie** te **dag**, en stel de **Interval** naar **7**.

## <a name="browse-a-single-message"></a>Een enkel bericht bladeren
1. Selecteer **+ nieuwe stap**, en selecteer **een actie toevoegen**.
2. Typ in het zoekvak `mq`, en selecteer vervolgens **MQ - bericht Bladeren**.  
![Bericht bladeren](media/connectors-create-api-mq/Browse_message.png)

3. Als er geen een bestaande MQ-verbinding, klikt u vervolgens de verbinding te maken:  

    1. Selecteer **verbinding maken via een on-premises gegevensgateway**, en voert u de eigenschappen van de MQ-server.  
    Voor **Server**, voer de naam van de MQ-server of het IP-adres gevolgd door een dubbele punt en het poortnummer invoeren.
    2. De **gateway** vervolgkeuzelijst geeft een lijst van de bestaande gatewayverbindingen die zijn geconfigureerd. Selecteer uw gateway.
    3. Selecteer **Maken** nadat dit is voltooid. Uw verbinding ziet er ongeveer als volgt uit:  
    ![Verbindingseigenschappen](media/connectors-create-api-mq/Connection_Properties.png)

4. In de eigenschappen van de actie, kunt u het volgende doen:  

    * Gebruik de **wachtrij** eigenschap voor toegang tot de naam van een andere wachtrij dan wat is gedefinieerd in de verbinding
    * Gebruik de **MessageId**, **CorrelationId**, **groeps-id**, en andere eigenschappen om te bladeren naar een bericht op basis van de verschillende eigenschappen van de MQ-bericht
    * Instellen **IncludeInfo** naar **waar** extra berichtinformatie in de uitvoer wilt opnemen. Of stel deze in op **False** niet extra bericht om gegevens te nemen in de uitvoer.
    * Voer een **time-out** waarde om te bepalen hoe lang moet worden gewacht op een bericht in een lege wachtrij aankomt. Als er niets is opgegeven, wordt het eerste bericht in de wachtrij wordt opgehaald en er is geen tijd besteed aan het wachten op een bericht wordt weergegeven.  
    ![Eigenschappen van berichten bladeren](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Sla** uw wijzigingen, en vervolgens **uitvoeren** uw logische app:  
![Opslaan en uitvoeren](media/connectors-create-api-mq/Save_Run.png)

6. Na enkele seconden, de stappen van de uitvoering worden weergegeven en kunt u de uitvoer bekijken. Selecteer het groene vinkje om de details van elke stap te bekijken. Selecteer **onbewerkte uitvoer Zie** voor meer informatie over de uitvoergegevens.  
![Bericht uitvoer bladeren](media/connectors-create-api-mq/Browse_message_output.png)  

    Onbewerkte uitvoer:  
    ![Bericht onbewerkte uitvoer bladeren](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Wanneer de **IncludeInfo** optie is ingesteld op true, wordt de volgende uitvoer weergegeven:  
![Bladeren-bericht opnemen info](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Meerdere berichten bladeren
De **berichten Bladeren** actie bevat een **BatchSize** optie om aan te geven hoeveel berichten uit de wachtrij moeten worden geretourneerd.  Als **BatchSize** heeft geen vermelding alle berichten worden geretourneerd. De resulterende uitvoer is een matrix van berichten.

1. Bij het toevoegen van de **berichten Bladeren** actie, de eerste verbinding die is geconfigureerd, is standaard geselecteerd. Selecteer **verbinding wijzigen** een nieuwe verbinding maken, of Selecteer een andere verbinding.

2. De uitvoer van bladeren berichten wordt weergegeven:  
![Berichten uitvoer bladeren](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Een enkel bericht ontvangen
De **ontvangen van berichten** actie heeft de dezelfde invoer en uitvoer als de **bladeren bericht** actie. Bij het gebruik van **ontvangen van berichten**, het bericht is verwijderd uit de wachtrij.

## <a name="receive-multiple-messages"></a>Meerdere berichten ontvangen
De **berichten ontvangen** actie heeft de dezelfde invoer en uitvoer als de **berichten Bladeren** actie. Bij het gebruik van **berichten ontvangen**, worden de berichten uit de wachtrij verwijderd.

Als er staan geen berichten in de wachtrij bij het uitvoeren van een bladeren of een ontvangen, wordt de stap mislukt met de volgende uitvoer:  
![MQ geen bericht fout](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Een bericht verzenden
1. Bij het toevoegen van de **bericht verzenden** actie, de eerste verbinding die is geconfigureerd, is standaard geselecteerd. Selecteer **verbinding wijzigen** een nieuwe verbinding maken, of Selecteer een andere verbinding. De geldige **berichttypen** zijn **Datagram**, **antwoord**, of **aanvragen**.  
![Msg steunbalken verzenden](media/connectors-create-api-mq/Send_Msg_Props.png)

2. De uitvoer van een bericht verzenden ziet er als volgt uit:  
![Msg uitvoer ook verzenden](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook eventuele beperkingen in de [connectorgegevens](/connectors/mq/).

## <a name="next-steps"></a>Volgende stappen
[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md). Verken de andere beschikbare connectors in Logic Apps op onze [lijst van API's](apis-list.md).
