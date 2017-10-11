---
title: Een functie maken die verbinding met Azure-services maakt | Microsoft Docs
description: Azure Functions gebruiken om een toepassing zonder server te maken die verbinding maakt met andere Azure-services.
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: Azure-functies, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 65964a322f0adab4f648fb350bedb77b46bf9054
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a>Azure Functions gebruiken om een toepassing te maken die verbinding maakt met andere Azure-services

In dit onderwerp leert u hoe u een functie maakt in Azure Functions die luistert naar berichten in een Azure Storage-wachtrij en die berichten naar rijen in een Azure Storage-tabel kopieert. Een door een timer geactiveerde functie wordt gebruikt voor het laden van berichten in de wachtrij. Een tweede functie leest uit de wachtrij en schrijft berichten naar de tabel. De wachtrij en de tabel worden voor u gemaakt door Azure Functions op basis van de bindingsdefinities. 

Daarnaast wordt één functie in JavaScript geschreven en de andere in C#-script. Dit laat zien hoe een functie-app functies in verschillende talen kan hebben. 

Dit scenario wordt uitgebeeld in een [video op kanaal 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).

## <a name="create-a-function-that-writes-to-the-queue"></a>Een functie die naar de wachtrij schrijft maken

Voordat u verbinding met een opslagwachtrij kunt maken, moet u een functie maken die de berichtenwachtrij laadt. Deze JavaScript-functie maakt gebruik van een timeractivering die ervoor zorgt dat er elke 10 seconden een bericht naar de wachtrij wordt geschreven. Als u nog geen Azure-account hebt, kunt u [Try Azure Functions](https://functions.azure.com/try) (Azure Functions proberen) bekijken of [een gratis Azure-account maken](https://azure.microsoft.com/free/).

1. Ga naar Azure Portal en zoek de functie-app.

2. Klik op **Nieuwe functie** > **TimerTrigger-JavaScript**. 

3. Geef de functie de naam **FunctionsBindingsDemo1**, voer een Cron-expressiewaarde in van `0/10 * * * * *` voor **Planning** en klik vervolgens op **Maken**.
   
    ![Een door een timer geactiveerde functie toevoegen](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    U hebt nu een door een timer geactiveerde functie gemaakt die elke 10 seconden wordt uitgevoerd.

5. Klik in het tabblad **Ontwikkelen** op **Logboeken** en bekijk de activiteit in het logboek. U ziet dat er elke 10 seconden een logboekvermelding wordt geschreven.
   
    ![Controleer in het logboek of de functie werkt](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Een uitvoerbinding voor de berichtenwachtrij toevoegen

1. Kies op het tabblad **Integreren** **Nieuwe uitvoer** > **Azure Queue Storage** > **Selecteren**.

    ![Een door een timer geactiveerde functie toevoegen](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Voer `myQueueItem` in voor **Naam van de berichtparameter** en `functions-bindings` voor **Wachtrijnaam**, selecteer een bestaande **Opslagaccountverbinding** of klik op **Nieuw** om een opslagaccountverbinding te maken, en klik vervolgens op **Opslaan**.  

    ![De uitvoerbinding naar de opslagwachtrij maken](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Ga terug naar het tabblad **Ontwikkelen** en voeg de volgende code toe aan de functie:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Zoek de *Als*-instructie rond regel 9 van de functie en voer de volgende code in achter deze instructie.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Met deze code maakt u een **myQueueItem** en stelt u de eigenschap **tijd** daarvan in op de huidige tijdstempel. Vervolgens wordt het nieuwe wachtrij-item toegevoegd aan de **myQueueItem**-binding van de context.

3. Klik op **Opslaan en uitvoeren**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Opslagupdates weergeven met behulp van Opslagverkenner
U kunt controleren of de functie werkt door berichten te bekijken in de wachtrij die u hebt gemaakt.  U kunt verbinding maken met uw opslagwachtrij met behulp van Cloud Explorer in Visual Studio. Via de portal kun u echter gemakkelijk verbinding maken met uw opslagaccount met behulp van Microsoft Azure Opslagverkenner.

1. Klik in het tabblad **Integreren** op de uitvoerbinding van uw wachtrij > **Documentatie**. Maak vervolgens de verbindingsreeks voor uw opslagaccount zichtbaar en kopieer de waarde. Met deze waarde kunt u verbinding maken met uw opslagaccount.

    ![Azure Opslagverkenner downloaden](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Als u dat nog niet hebt gedaan, downloadt en installeert u [Microsoft Azure Opslagverkenner](http://storageexplorer.com). 
 
3. Klik in Opslagverkenner op het pictogram voor verbinding maken met Azure Storage, plak de verbindingsreeks in het veld en voltooi de wizard.

    ![Een verbinding toevoegen in Opslagverkenner](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. Vouw onder **Local and attached** (Lokaal en gekoppeld) **Opslagaccounts** > uw opslagaccount > **Wachtrijen** > **functions-bindings** uit en controleer of berichten naar de wachtrij worden geschreven.

    ![Weergave van berichten in de wachtrij](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Als de wachtrij niet bestaat of leeg is, is er waarschijnlijk een probleem met uw functiebinding of code.

## <a name="create-a-function-that-reads-from-the-queue"></a>Een functie die van de wachtrij leest maken

Nu dat er berichten aan de wachtrij worden toegevoegd, kunt u een functie maken die van de wachtrij leest en de berichten permanent naar een Azure Storage-tabel schrijft.

1. Klik op **Nieuwe functie** > **QueueTrigger-CSharp**. 
 
2. Geef de functie de naam `FunctionsBindingsDemo2`, voer **functions-bindings** in het veld **Wachtrijnaam** in, selecteer een bestaand opslagaccount of maak een nieuwe, en klik op **Maken**.

    ![Een timerfunctie voor de uitvoerwachtrij toevoegen](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Optioneel) U kunt controleren of de nieuwe functie werkt door de nieuwe wachtrij zoals eerder uitgelegd in Opslagverkenner te bekijken. U kunt ook Cloud Explorer gebruiken in Visual Studio.  

4. (Optioneel) Vernieuw de wachtrij **functions-bindings** en kijk of er items zijn verwijderd uit de wachtrij. De verwijdering doet zich voor omdat de functie als een invoeractivering is gebonden aan de wachtrij **functions-bindings** en de functie de wachtrij leest. 
 
## <a name="add-a-table-output-binding"></a>Een uitvoerverbinding voor de tabel toevoegen

1. Klik in FunctionsBindingsDemo2 op **Integreren** > **Nieuwe uitvoer** > **Azure Table Storage** > **Selecteren**.

    ![Een binding aan een Azure Storage-tabel toevoegen](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Voer `functionbindings` in voor **Tabelnaam** en `myTable` voor **Naam van de tabelparameter**, kies een **Opslagaccountverbinding** of maak een nieuwe, en klik vervolgens op **Opslaan**.

    ![De binding van de Storage-tabel configureren](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. Vervang in het tabblad **Ontwikkelen** de bestaande functiecode door het volgende:
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    De klasse **TableItem** vertegenwoordigt een rij in de opslagtabel. U voegt het item toe aan de verzameling `myTable` van **TableItem**-objecten. Stel de eigenschappen **PartitionKey** en **RowKey** in om in de tabel te kunnen invoegen.

4. Klik op **Opslaan**.  Ten slotte kunt u controleren of de functie werkt door de tabel te bekijken in Opslagverkenner of Cloud Explorer van Visual Studio.

5. (Optioneel) Vouw in uw opslagaccount in Opslagverkenner **Tabellen** > **functionsbindings** uit en controleer of er rijen worden toegevoegd aan de tabel. U kunt hetzelfde doen met Cloud Explorer in Visual Studio.

    ![Weergave van de rijen in de tabel](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Als de tabel niet bestaat of leeg is, is er waarschijnlijk een probleem met uw functiebinding of code. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende onderwerpen voor meer informatie over Azure Functions:

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het hostingabonnement Consumption, en helpt u bij het kiezen van het juiste abonnement. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]

