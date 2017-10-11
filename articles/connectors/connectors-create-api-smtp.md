---
title: SMTP-connector in Azure Logic Apps | Microsoft Docs
description: Logic apps maken met Azure App service. Verbinding maken met SMTP om een e-mail te verzenden.
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a>Aan de slag met de SMTP-connector
Verbinding maken met SMTP om een e-mail te verzenden.

Gebruik [elke connector](apis-list.md), moet u eerst een logische app maken. U kunt aan de slag door [maken van een logische app nu](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Verbinding maken met SMTP
Om uw logische app toegang alle services tot, moet u eerst maken een *verbinding* naar de service. Een [verbinding](connectors-overview.md) biedt connectiviteit tussen een logische app en een andere service. Bijvoorbeeld, als u wilt verbinding maken met SMTP, moet u eerst een SMTP *verbinding*. Voer de referenties die u gebruikt om toegang tot de service die u verbinding met maakt een verbinding wilt maken. Voer dus de referenties in het voorbeeld SMTP uw verbindingsnaam, het SMTP-serveradres en de aanmeldingsgegevens voor gebruiker voor het maken van de verbinding met de SMTP.  

### <a name="create-a-connection-to-smtp"></a>Maak een verbinding met de SMTP
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Gebruik een SMTP-trigger
Een trigger is een gebeurtenis die kan worden gebruikt om de werkstroom die is gedefinieerd in een logische app te starten. [Meer informatie over triggers](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

In dit voorbeeld omdat SMTP niet over een trigger van een eigen beschikt, gebruiken we de **Salesforce - wanneer een object wordt gemaakt** trigger. Deze trigger wordt geactiveerd wanneer een nieuw object in Salesforce maakt. In ons voorbeeld stellen we het zo dat elke keer wanneer een nieuwe lead wordt gemaakt in Salesforce, een *e-mail verzenden* actie plaatsvindt via de SMTP-connector met een melding van de nieuwe lead wordt gemaakt.

1. Voer *salesforce* in het zoekvak op de ontwerpfunctie van logic apps selecteert u vervolgens de **Salesforce - wanneer een object wordt gemaakt** trigger.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. De **wanneer een object wordt gemaakt** besturingselement wordt weergegeven.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Selecteer de **objecttype** Selecteer *leiden* uit de lijst met objecten. In deze stap geeft u aan dat u een trigger die uw logische app ontvangt een melding maakt wanneer er een nieuwe lead wordt gemaakt in Salesforce.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. De trigger is gemaakt.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Gebruik een SMTP-actie
Een actie is een bewerking uitgevoerd door de werkstroom die is gedefinieerd in een logische app. [Meer informatie over acties](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Nu dat de trigger is toegevoegd, volg deze stappen uit om een SMTP-actie die wordt uitgevoerd wanneer een nieuwe lead in Salesforce wordt gemaakt.

1. Selecteer **+ een nieuwe stap** om toe te voegen van de actie die u uitvoeren wilt wanneer een nieuwe lead wordt gemaakt.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Selecteer **een actie toevoegen**. Deze wordt geopend het zoekvak waarin u naar elke actie u zoeken kunt wilt uitvoeren.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Voer *smtp* om te zoeken naar acties met betrekking tot SMTP.  
4. Selecteer **SMTP - e-mailbericht verzenden** als de actie moet worden uitgevoerd wanneer de nieuwe lead wordt gemaakt. Hiermee opent u de actie in het Configuratiescherm. U moet uw SMTP-om verbinding te maken in de ontwerpfunctie blok als u dit eerder niet hebt gedaan.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Voer uw gewenste e-informatie in de **SMTP - e-mailbericht verzenden** blok.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Sla uw werk op om de werkstroom activeren.  

## <a name="connector-specific-details"></a>Connector-specifieke details

Alle triggers en acties die zijn gedefinieerd in de swagger bekijken en ziet u ook de beperkingen in de [connector details](/connectors/smtpconnector/).

## <a name="more-connectors"></a>Meer connectors
Ga terug naar de [API's lijst](apis-list.md).