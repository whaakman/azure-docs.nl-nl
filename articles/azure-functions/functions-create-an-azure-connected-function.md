---
title: Een Azure-functie maken die wordt gebonden aan een Azure-service | Microsoft Docs
description: Bouw een Azure-functie, een toepassing zonder server, die met andere Azure-services communiceert.
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
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/25/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9ffd199c9e3c621a808ade109ed044b6c9b689b7


---
# <a name="create-an-azure-function-which-binds-to-an-azure-service"></a>Een Azure-functie maken die wordt gebonden aan een Azure-service
[!INCLUDE [Getting Started Note](../../includes/functions-getting-started.md)]

In deze korte video leert u hoe u een Azure-functie maakt die luistert naar berichten op een Azure-wachtrij en die berichten naar een Azure-blob kopieert.

## <a name="watch-the-video"></a>Video bekijken
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-an-input-queue-trigger-function"></a>Een activeringsfunctie voor een invoerwachtrij maken
Met deze functie wordt elke tien seconden een bericht naar een wachtrij geschreven. Daarvoor moet u de functie en berichtwachtrijen maken, en de code toevoegen om berichten naar de zojuist gemaakte wachtrijen te schrijven.

1. Ga naar Azure Portal en zoek de Azure-functie-app.
2. Klik op **Nieuwe functie** > **TimerTrigger - Knooppunt**. Noem de functie **FunctionsBindingsDemo1**
3. Voer de waarde '0/10 * * * * *' in voor de planning. Deze waarde heeft de vorm van een CRON-expressie. Hiermee zorgt u ervoor dat de timer elke tien seconden wordt uitgevoerd.
4. Klik op de knop **Maken** om de nieuwe functie te maken.
   
    ![Een activeringstimerfunctie toevoegen](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)
5. Controleer of de functie werkt door activiteit in het logboek te bekijken. Misschien moet u in de rechterbovenhoek op de koppeling **Logboeken** klikken om het logboekvenster weer te geven.
   
   ![Controleren of de functie werkt door het logboek weer te geven](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

### <a name="add-a-message-queue"></a>Een berichtenwachtrij toevoegen
1. Ga naar het tabblad **Integreren**.
2. Kies **Nieuwe uitvoer** > **Azure Storage-wachtrij** > **Selecteren**.
3. Typ **myQueueItem** in het tekstvak **Naam van de berichtparameter**.
4. Selecteer een opslagaccount of klik op **Nieuw** als u een opslagaccount wilt maken omdat u er nog geen hebt.
5. Typ **functions-bindings** in het tekstvak **Wachtrijnaam**.
6. Klik op **Opslaan**.  
   
   ![Een activeringstimerfunctie toevoegen](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

### <a name="write-to-the-message-queue"></a>Naar de berichtenwachtrij schrijven
1. Ga terug naar het tabblad **Ontwikkelen** en voeg de volgende code achter de bestaande code toe:
   
    ```javascript
   
    function myQueueItem() 
      {
        return {
        msg: "some message goes here",
        time: "time goes here"
      }
    }
   
    ```
2. Wijzig de bestaande functiecode om de code op te roepen die u hebt toegevoegd in stap 1. Voeg de volgende code in rond regel 9 van de functie, achter de *Als*-instructie.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueue = toBeQed;
   
    ```
   
    Met deze code maakt u een **myQueueItem** en stelt u de eigenschap **tijd** daarvan in op de huidige tijdstempel. Vervolgens wordt het nieuwe wachtrij-item toegevoegd aan de myQueue-binding van de context.
3. Klik op **Opslaan en uitvoeren**.
4. Controleer of de code werkt door de wachtrij te bekijken in Visual Studio.
   
   * Open Visual Studio en ga naar **Weergave** > **Cloud** **Explorer**.
   * Zoek het opslagaccount en de wachtrij **functions-bindings** die u hebt gebruikt toen u de wachtrij myQueue maakte. Hier ziet u rijen met logboekgegevens. Misschien moet u zich via Visual Studio aanmelden bij Azure.  

## <a name="create-an-output-queue-trigger-function"></a>Een activeringsfunctie voor een uitvoerwachtrij maken
1. Klik op **Nieuwe functie** > **QueueTrigger - C#**. Noem de functie **FunctionsBindingsDemo2**. Merk op dat u talen in dezelfde functie-app (knooppunt en C# in dit geval) kunt combineren.
2. Typ **functions-bindings** in het veld **Wachtrijnaam**"
3. Selecteer het opslagaccount dat u wilt gebruiken of maak een nieuw opslagaccount.
4. Klik op **Maken**.
5. Controleer of de nieuwe functie werkt door het logboek van de functie en Visual Studio voor updates weer te geven. In het logboek van de functie ziet u dat de functie wordt uitgevoerd en items uit de wachtrij worden verwijderd. Omdat de functie als een invoeractivering is gebonden aan de uitvoerwachtrij **functions-bindings**, ziet u wanneer u de wachtrij **functions-bindings** vernieuwt dat de items zijn verdwenen. Ze zijn uit wachtrij verwijderd.   
   
   ![Een timerfunctie voor de uitvoerwachtrij toevoegen](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png)   

### <a name="modify-the-queue-item-type-from-json-to-object"></a>Het wachtrij-itemtype wijzigen van JSON in object
1. Vervang de code in **FunctionsBindingsDemo2** door de volgende code:    
   
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
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.Msg} | {myQueueItem.Time}");
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
   
    Met deze code voegt u twee klassen toe, **TableItem** en **QItem**. Deze gebruikt u om gegevens te lezen uit en te schrijven naar wachtrijen. Daarnaast is de functie **Run** zo gewijzigd dat deze de parameter **QItem** en **TraceWriter** accepteert in plaats van een **tekenreeks** en een **TraceWriter**. 
2. Klik op de knop **Opslaan**.
3. Controleer of de code werkt door het logboek te controleren. Merk op dat Azure-functies heb object automatisch voor u serialiseren en deserialiseren, zodat u gemakkelijker toegang hebt tot de wachtrij op een objectgeoriënteerde manier om gegevens door te geven. 

## <a name="store-messages-in-an-azure-table"></a>Berichten opslaan in een Azure-tabel
Nu de wachtrijen samenwerken, gaat u een Azure-tabel toevoegen waarin u de wachtrijgegevens permanent kunt opslaan.

1. Ga naar het tabblad **Integreren**.
2. Maak een Azure Storage-tabel voor uitvoer en noem deze **myTable**.
3. Geef **functionsbindings** op als antwoord op de vraag 'Naar welke tabel moeten de gegevens worden geschreven?'.
4. Wijzig de instelling **PartitionKey** van **{project-id}** in **{partition}**.
5. Kies een opslagaccount of maak een nieuw opslagaccount.
6. Klik op **Opslaan**.
7. Ga naar het tabblad **Ontwikkelen**.
8. Maak een klasse **TableItem** die een Azure-tabel vertegenwoordigt, en wijzig de functie Run zo dat deze het zojuist gemaakte TableItem-object accepteert. Dit werkt alleen als u de eigenschappen **PartitionKey** en **RowKey** gebruikt.
   
    ```cs
   
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
   
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.RowKey} | {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
    ```
9. Klik op **Opslaan**.
10. Controleer of de code werkt door de logboeken van de functie weer te geven in Visual Studio. Als u dit wilt controleren in Visual Studio, gebruikt u de **Cloud Explorer** om naar de Azure-tabel **functionbindings** te gaan en controleert u of deze rijen bevat.

[!INCLUDE [Getting Started Note](../../includes/functions-bindings-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


