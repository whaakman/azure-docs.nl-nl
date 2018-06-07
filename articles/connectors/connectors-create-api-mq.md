---
title: Verbinding maken met server MQ - Azure Logic Apps | Microsoft Docs
description: Verzenden en berichten met een Azure of on-premises MQ-server en Azure Logic Apps ophalen
author: valthom
manager: cfowler
ms.author: valthom
ms.date: 06/01/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 0877d0e9394d1a54eae5482001bd53da0e1a83b0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34609445"
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a>Verbinding maken met een IBM MQ server vanuit logic apps met behulp van de connector MQ 

Microsoft-Connector voor MQ verzendt en berichten die zijn opgeslagen in een MQ Server on-premises of in Azure ophaalt. Deze connector omvat een Microsoft-MQ-client die met een externe IBM MQ-server via een TCP/IP-netwerk communiceert. Dit document is een starter-handleiding voor het gebruik van de connector MQ. Wij raden dat u begint met bladeren door een enkel bericht in een wachtrij en probeert u de andere acties uitvoeren.    

De connector MQ omvat de volgende acties. Er zijn geen triggers.

-   Een enkel bericht bladeren zonder dat het bericht is verwijderd van de IBM MQ-Server
-   Berichten batchgewijs zoeken zonder het verwijderen van de berichten van de IBM MQ-Server
-   Een enkel bericht ontvangen en verwijderen van het bericht uit de IBM MQ-Server
-   Een batch met berichten ontvangen en berichten van de IBM MQ Server verwijderen
-   Een enkel bericht verzenden naar de IBM MQ-Server 

## <a name="prerequisites"></a>Vereisten

* Als u een on-premises MQ server, [installeren van de lokale data gateway](../logic-apps/logic-apps-gateway-install.md) op een server in uw netwerk. Als de Server MQ openbaar beschikbaar, of beschikbaar is in Azure, is klikt u vervolgens de gegevensgateway niet gebruikt of nodig.

    > [!NOTE]
    > De server waarop de On-Premises Data Gateway is geïnstalleerd, moet ook beschikken over .net Framework 4.6 is geïnstalleerd voor de Connector MQ functie.

* Maken van de Azure-resource voor de lokale data gateway - [instellen van de gateway gegevensverbinding](../logic-apps/logic-apps-gateway-connection.md).

* IBM WebSphere MQ versies officieel ondersteund:
   * MQ 7.5
   * MQ 8.0

## <a name="create-a-logic-app"></a>Een logische app maken

1. In de **Azure start board**, selecteer **+** (plusteken) **Web en mobiel**, en vervolgens **logische App**. 
2. Voer de **naam**, zoals MQTestApp, **abonnement**, **resourcegroep**, en **locatie** (gebruikt u de locatie waar de lokale Data Gateway-verbinding is geconfigureerd). Selecteer **vastmaken aan dashboard**, en selecteer **maken**.  
![Logische App maken](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Een trigger toevoegen

> [!NOTE]
> De MQ-Connector heeft geen geen triggers bestaan. Dus een andere trigger te gebruiken om te beginnen uw logische app, zoals de **terugkeerpatroon** trigger. 

1. De **Logic Apps Designer** wordt geopend, selecteer **terugkeerpatroon** in de lijst met algemene triggers.
2. Selecteer **bewerken** binnen de Trigger terugkeerpatroon. 
3. Instellen de **frequentie** naar **dag**, en stel de **Interval** naar **7**. 

## <a name="browse-a-single-message"></a>Een enkel bericht bladeren
1. Selecteer **+ een nieuwe stap**, en selecteer **een actie toevoegen**.
2. Typ in het zoekvak `mq`, en selecteer vervolgens **MQ - bladeren bericht**.  
![Bericht bladeren](media/connectors-create-api-mq/Browse_message.png)

3. Als er geen een bestaande MQ-verbinding, klikt u vervolgens de verbinding te maken:  

    1. Selecteer **verbinden via lokale gegevensgateway**, en voert u de eigenschappen van uw server MQ.  
    Voor **Server**, voer de naam van de server MQ of geef het IP-adres gevolgd door een dubbelepunt en het poortnummer. 
    2. De **gateway** dropdown geeft een lijst van de bestaande gatewayverbindingen die zijn geconfigureerd. Selecteer uw gateway.
    3. Selecteer **Maken** nadat dit is voltooid. Uw verbinding ziet er ongeveer als volgt uit:   
    ![Verbindingseigenschappen](media/connectors-create-api-mq/Connection_Properties.png)

4. U kunt in de actie-eigenschappen:  

    * Gebruik de **wachtrij** eigenschap voor toegang tot een andere wachtrijnaam heeft dan de waarde die is gedefinieerd in de verbinding
    * Gebruik de **MessageId**, **CorrelationId**, **GroupId**, en andere eigenschappen om te bladeren naar een bericht op basis van de verschillende eigenschappen voor MQ-bericht
    * Stel **IncludeInfo** naar **waar** aanvullend berichtinformatie in de uitvoer wilt opnemen. Of stel deze in op **False** niet op te nemen informatie aanvullende berichten in de uitvoer.
    * Voer een **time-out** waarde om te bepalen hoe lang moet worden gewacht voor een bericht moet worden uitgevoerd in een lege wachtrij. Als er niets wordt opgegeven, wordt het eerste bericht in de wachtrij opgehaald en er is geen tijd besteed aan het wachten op een bericht wordt weergegeven.  
    ![Berichteigenschappen bladeren](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Sla** uw wijzigingen en vervolgens **uitvoeren** uw logische app:  
![Opslaan en uitvoeren](media/connectors-create-api-mq/Save_Run.png)

6. Na enkele seconden, de stappen van de uitvoering worden weergegeven en kunt u de uitvoer bekijken. Selecteer in het groen vinkje details van elke stap. Selecteer **Zie ruwe uitvoer** voor aanvullende informatie over de uitvoergegevens.  
![Bericht uitvoer bladeren](media/connectors-create-api-mq/Browse_message_output.png)  

    Onbewerkte uitvoer:  
    ![Bericht onbewerkte uitvoer bladeren](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Wanneer de **IncludeInfo** optie is ingesteld op true, wordt de volgende uitvoer weergegeven:  
![Bladeren bericht bevatten info](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Meerdere berichten bladeren
De **berichten Bladeren** actie bevat een **BatchSize** optie om aan te geven hoeveel berichten uit de wachtrij moeten worden geretourneerd.  Als **BatchSize** heeft geen vermelding alle berichten worden geretourneerd. De resulterende uitvoer is een matrix van berichten.

1. Bij het toevoegen van de **berichten Bladeren** actie voor de eerste verbinding die is geconfigureerd, is standaard geselecteerd. Selecteer **verbinding wijzigen** een nieuwe verbinding maken of Selecteer een andere verbinding.

2. De uitvoer van bladeren berichten wordt weergegeven:  
![Berichten uitvoer bladeren](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Een enkel bericht ontvangen
De **ontvangen van berichten** actie heeft de dezelfde invoer en uitvoer als de **bladeren bericht** in te grijpen. Wanneer u **ontvangen van berichten**, het bericht is verwijderd uit de wachtrij.

## <a name="receive-multiple-messages"></a>Meerdere berichten ontvangen
De **berichten ontvangen** actie heeft de dezelfde invoer en uitvoer als de **berichten Bladeren** in te grijpen. Wanneer u **berichten ontvangen**, worden de berichten uit de wachtrij verwijderd.

Als er geen berichten in de wachtrij zijn bij het uitvoeren van een bladeren of een ontvangen, wordt de stap mislukt met de volgende uitvoer:  
![MQ Nee bericht fout](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Een bericht verzenden
1. Bij het toevoegen van de **bericht verzenden** actie voor de eerste verbinding die is geconfigureerd, is standaard geselecteerd. Selecteer **verbinding wijzigen** een nieuwe verbinding maken of Selecteer een andere verbinding. Het geldige **berichttypen** zijn **Datagram**, **antwoord**, of **aanvragen**.  
![Eigenschappen van bericht verzenden](media/connectors-create-api-mq/Send_Msg_Props.png)

2. De uitvoer van een bericht verzenden ziet er als volgt:  
![De uitvoer bericht verzenden](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/mq/).

## <a name="next-steps"></a>Volgende stappen
[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md). Bekijk de beschikbare connectors in Logic Apps op onze [API's lijst](apis-list.md).
