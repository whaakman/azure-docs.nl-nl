---
title: Problemen oplossen voor een verkeerd ingedeelde invoervelden in Azure Stream Analytics | Microsoft Docs
description: Hoe weet ik welke gebeurtenis in mijn invoer gegevens probleem wordt veroorzaakt door in een Stream Analytics-taak
keywords: ''
documentationcenter: ''
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: ''
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/19/2018
ms.author: sngun
ms.openlocfilehash: 2b4f82bae20c3cb398848a89715bfdc1316e1ba1
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2018
ms.locfileid: "29766396"
---
# <a name="troubleshoot-for-malformed-input-events"></a>Ongeldige invoer gebeurtenissen oplossen

Wanneer de invoerstroom van de Stream Analytics-taak onjuiste berichten bevat, wordt de serialisatie problemen veroorzaakt. Onjuiste berichten bevatten onjuiste serialisatie zoals haakje openen ontbreekt in een JSON-object of een onjuiste stempel tijdnotatie. Wanneer een Stream Analytics-taak heeft een onjuist gevormd bericht ontvangen, komt het bericht en waarschuwt de gebruiker met een waarschuwing. Een waarschuwingssymbool wordt weergegeven op de **invoer** tegel van de Stream Analytics-taak:

![Invoer-tegel](media/stream-analytics-malformed-events/inputs_tile.png)

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing

1. Navigeer naar de invoer tegel en klik hier voor weergave van waarschuwingen.
2. De details van de invoer-tegel wordt een set van waarschuwingen met informatie over het probleem. Hieronder volgt een voorbeeld van de waarschuwing weergegeven, ziet u de waarschuwing de partitie, Offset en volgnummers wanneer er een ongeldige JSON-gegevens. 

   ![Waarschuwingsbericht weergegeven met verschuiving](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Als u de JSON-gegevens die door een onjuiste indeling heeft, het volgende codefragment worden uitgevoerd. Dit codeblok leest de partitie-Id, offset en de gegevens worden afgedrukt. U krijgt het volledige voorbeeld van de [GitHub-opslagplaats voor voorbeelden](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Zodra u de gegevens gelezen, kunt u deze kunt analyseren en verbeter de serialisatie-indeling.

```csharp
static void PrintMessages(string partitionId, long offset, int numberOfEvents)
        {
            EventHubReceiver receiver;
            
            try
            {
                foreach (var e in receiver.Receive(numberOfEvents, TimeSpan.FromMinutes(1)))
                {
                    Console.WriteLine(Encoding.UTF8.GetString(e.GetBytes()));
                    Console.WriteLine("----");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.ToString());
                return;
            }

            receiver.Close();
        }
```
## <a name="next-steps"></a>Volgende stappen

* [Naslaggids voor Azure Stream Analytics Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [REST API-naslaggids voor Azure Stream Analytics Management](https://msdn.microsoft.com/library/azure/dn835031.aspx)
