---
title: Het uitschakelen van de functies in Azure Functions
description: Meer informatie over het uitschakelen en inschakelen van functies in Azure Functions 1.x en 2.x.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: glenga
ms.openlocfilehash: ab9cf429a0af69db116fe910ab90b83d404afbb7
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093631"
---
# <a name="how-to-disable-functions-in-azure-functions"></a>Het uitschakelen van de functies in Azure Functions

In dit artikel wordt uitgelegd hoe u een functie in Azure Functions uitschakelen. Naar *uitschakelen* betekent dat een functie waarmee de runtime de automatische trigger die is gedefinieerd voor de functie negeren. De manier waarop u dit doen, is afhankelijk van de runtimeversie en de programmeertaal:

* Functions 1.x
  * Scripttalen
  * C#-klassebibliotheken
* Functions 2.x
  * Een manier voor alle talen
  * Optionele manier voor C#-klassebibliotheken

## <a name="functions-1x---scripting-languages"></a>1.x - scripttalen functies

Voor scripts talen zoals C#-script en JavaScript, gebruikt u de `disabled` eigenschap van de *function.json* bestand om te zien van de runtime niet voor het activeren van een functie. Deze eigenschap kan worden ingesteld op `true` of de naam van een app-instelling:

```json
{
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ],
    "disabled": true
}
```
of 

```json
    "bindings": [
        ...
    ],
    "disabled": "IS_DISABLED"
```

In het tweede voorbeeld wordt de functie is uitgeschakeld als er een app-instelling die de naam IS_DISABLED en is ingesteld op `true` of 1.

U kunt het bestand in de Azure portal of gebruik bewerken de **Functiestatus** overschakelen op van de functie **beheren** tabblad. De portal switch werkt door het veranderen van de *function.json* bestand.

![Functie status switch](media/disable-function/function-state-switch.png)

## <a name="functions-1x---c-class-libraries"></a>1.x - C#-klassebibliotheken functies

In een functies 1.x-klassebibliotheek, gebruikt u een `Disable` kenmerk om te voorkomen dat een functie die wordt geactiveerd. U kunt het kenmerk zonder een constructorparameter kunt gebruiken, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public static class QueueFunctions
{
    [Disable]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Het kenmerk zonder een constructorparameter is vereist dat u opnieuw te compileren en implementeren van het project als u wilt wijzigen van de functie uitgeschakeld. Een meer flexibele manier om het gebruik van het kenmerk is om op te nemen van een constructorparameter die naar een Booleaanse app-instelling verwijst, zoals wordt weergegeven in het volgende voorbeeld:

```csharp
public static class QueueFunctions
{
    [Disable("MY_TIMER_DISABLED")]
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

Deze methode kunt u inschakelen en uitschakelen van de functie door het wijzigen van de app-instelling, zonder te compileren of opnieuw te implementeren. Wijzigen van een app-instelling zorgt ervoor dat de functie-app opnieuw wordt opgestart, zodat de wijziging in de uitgeschakelde status onmiddellijk wordt herkend.

> [!IMPORTANT]
> De `Disabled` kenmerk is de enige manier om uit te schakelen van de functie van een klasse-bibliotheek. Het gegenereerde *function.json* -bestand voor de functie van een klasse-bibliotheek is niet bedoeld voor rechtstreeks worden bewerkt. Als u dit bestand bewerken, wat u doen op de `disabled` eigenschap heeft geen effect.
>
> Hetzelfde geldt voor de **functie status** overschakelen op de **beheren** tabblad, omdat dit door het wijzigen van gebeurt de *function.json* bestand.
>
> Merk ook op dat de portal op dat de functie is uitgeschakeld duiden kan als dat niet zo is.



## <a name="functions-2x---all-languages"></a>Functies 2.x - alle talen

In de functies 2.x u een functie uitschakelen met behulp van een app-instelling. Bijvoorbeeld, een functie uitschakelen met de naam `QueueTrigger`, maken van een app-instelling met de naam `AzureWebJobs.QueueTrigger.Disabled`, en stel deze in op `true`. Zodat de functie door de appinstelling in te stellen op `false`. U kunt ook de **Functiestatus** overschakelen op van de functie **beheren** tabblad. De switch werkt door te maken en verwijderen van de `AzureWebJobs.<functionname>.Disabled` app-instelling.

![Functie status switch](media/disable-function/function-state-switch.png)

## <a name="functions-2x---c-class-libraries"></a>Functies 2.x - C#-klassebibliotheken

U wordt aangeraden dat u de methode die geschikt is voor alle talen wilt gebruiken in een bibliotheek met Functions 2.x klasse. Maar als u liever, kunt u [gebruikt het kenmerk uitschakelen in functies 1.x](#functions-1x---c-class-libraries).

## <a name="next-steps"></a>Volgende stappen

Dit artikel is over het uitschakelen van automatische triggers. Zie voor meer informatie over triggers [Triggers en bindingen](functions-triggers-bindings.md).
