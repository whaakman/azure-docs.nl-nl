---
title: Een functie voor gebeurtenisverwerking maken | Microsoft Docs
description: Gebruik Azure-functies om een C#-functie te maken die wordt uitgevoerd op basis van een gebeurtenistimer.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 84bd0373-65e2-4022-bcca-2b9cd9e696f5
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 44e397c7521ba8f0ba11893c364f51177561bee4
ms.openlocfilehash: df3d303ee10fcc982552ea9756eb59198c87b650


---
# <a name="create-an-event-processing-azure-function"></a>Een Azure-functie voor gebeurtenisverwerking maken
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand ervaring waarmee u geplande of geactiveerde code-eenheden kunt maken voor implementatie in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u een nieuwe functie in C# maakt die wordt uitgevoerd op basis van een gebeurtenistimer om berichten toe te voegen aan een opslagwachtrij. 

## <a name="prerequisites"></a>Vereisten
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Als u nog geen Azure-account hebt, kunt u [Try Functions](https://functions.azure.com/try) (Functions proberen) bekijken of [een gratis Azure-account maken](https://azure.microsoft.com/free/). 

## <a name="create-a-timer-triggered-function-from-the-template"></a>Op basis van de sjabloon een functie maken die door een timer wordt geactiveerd
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Voordat u een functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/). 

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account.
2. Als u een bestaande functie-app hebt die u kunt gebruiken, selecteert u deze in **Uw functie-apps** en klikt u op **Openen**. Als u een nieuwe functie-app wilt maken, typt u een unieke **Naam** voor uw nieuwe functie-app of accepteert u de gegenereerde naam, selecteert u de gewenste **Regio** en klikt u op **Maken + aan de slag**. 
3. Klik in uw functie-app op **+ Nieuwe functie** > **TimerTrigger - C#** > **Maken**. Hierdoor wordt een functie met een standaardnaam gemaakt die volgens de standaardplanning eenmaal per minuut wordt uitgevoerd. 
   
    ![Een nieuwe functie maken die door een timer wordt geactiveerd](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)
4. Klik in de nieuwe functie op het tabblad **Integreren** > **Nieuwe uitvoer** > **Azure-opslagwachtrij** > **Selecteren**.
   
    ![Een nieuwe functie maken die door een timer wordt geactiveerd](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)
5. Selecteer in **Uitvoer Azure Storage-wachtrij** een bestaande **Storage-accountverbinding** of maak een nieuwe en klik vervolgens op **Opslaan**. 
   
    ![Een nieuwe functie maken die door een timer wordt geactiveerd](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)
6. Ga weer naar het tabblad **Ontwikkelen** en vervang het bestaande C#-script in het venster **Code** door de volgende code:
    ```cs   
    using System;

    public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
    {
        // Add a new scheduled message to the queue.
        outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";

        // Also write the message to the logs.
        log.Info(outputQueueItem);
    }
    ```
   
    Deze code voegt een nieuw bericht toe aan de wachtrij met de huidige datum en tijd van de uitvoering van de functie.
7. Klik op **Opslaan** en kijk hoe in het venster **Logboeken** de volgende functie wordt uitgevoerd.
8. (Optioneel) Navigeer naar het opslagaccount en controleer of berichten aan de wachtrij worden toegevoegd.
9. Ga terug naar het tabblad **Integreren** en stel het planningsveld in op `0 0 * * * *`. De functie wordt nu één keer per uur uitgevoerd. 

Dit is een zeer vereenvoudigd voorbeeld van een timertrigger en een binding aan de uitvoer van een opslagwachtrij. Zie [Azure Functions timer trigger](functions-bindings-timer.md) (Azure Functions-timertrigger) en [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md) (Azure Functions-triggers en -bindingen voor Azure Storage) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende onderwerpen voor meer informatie over Azure Functions.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het hostingabonnement Consumption, en helpt u bij het kiezen van het juiste abonnement.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Dec16_HO1-->


