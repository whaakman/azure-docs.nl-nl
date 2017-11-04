---
title: Verwerken van gebeurtenis volgorde en lateness met Azure Stream Analytics | Microsoft Docs
description: Meer informatie over de werking van Stream Analytics met out volgorde of laat gebeurtenissen in gegevensstromen.
keywords: volgorde, laat, gebeurtenissen
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: cf57bd12a62b3de8ac49b26ce7cdc40aec0b6738
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Azure Stream Analytics volgorde gebeurtenisverwerking

In een tijdelijke gegevensstroom van gebeurtenissen krijgt elke gebeurtenis een tijdstempel. Tijdstempel Azure Stream Analytics toegewezen aan elke gebeurtenis met behulp van de aankomsttijd of de tijd van de toepassing. De kolom 'System.Timestamp' heeft de tijdstempel die is toegewezen aan de gebeurtenis. Aankomsttijd is toegewezen aan de invoerbron wanneer de gebeurtenis de bron bereikt. Aankomsttijd is EventEnqueuedTime voor invoer van de Event Hub en [blob laatst gewijzigd om](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) voor blob-invoer. Tijd van de toepassing wordt toegewezen wanneer de gebeurtenis wordt gegenereerd en het deel van de nettolading uitmaakt. Voor het verwerken van gebeurtenissen door de tijd van de toepassing, door de component 'Tijdstempel door' te gebruiken in de query select. Als de component 'Tijdstempel door' niet aanwezig is, worden door aankomsttijd gebeurtenissen verwerkt. Aankomsttijd toegankelijk gebruikt EventEnqueuedTime eigenschap voor event hub en BlobLastModified-eigenschap voor blob-invoer. Azure Stream Analytics produceert uitvoer in de volgorde van het tijdstempel en vindt u enkele instellingen om te gaan met de volgorde van gegevens.

![Stream Analytics gebeurtenisverwerking](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

Invoer stromen die zich niet in volgorde zijn:
* Gesorteerd (en dus **vertraagd**).
* Aangepast door het systeem, volgens de door de gebruiker opgegeven beleid.

Stream Analytics maximaal laat en volgorde gebeurtenissen bij het verwerken van de tijd van de toepassing wordt toegestaan.

**Late ontvangst tolerantie**

* Deze instelling is alleen toepasbaar wanneer verwerkt door de tijd van de toepassing, wordt anders genegeerd.
* Dit is het maximale verschil tussen het aankomen van de tijd en tijd van de toepassing. Als de toepassing valt vóór (aankomsttijd - venster voor Late ontvangst), is deze ingesteld op (aankomsttijd - laat aankomst venster)
* Wanneer meerdere partities van de dezelfde invoerstroom of meerdere invoer stromen worden gecombineerd, is late ontvangst tolerantie de maximale hoeveelheid tijd die elke partitie wordt gewacht op nieuwe gegevens. 

Kort samengevat kunt is late ontvangst venster de maximale vertraging tussen het genereren van gebeurtenis en ontvangst van de gebeurtenis op de invoerbron.
Op basis van tolerantie voor Late ontvangst aanpassing eerst wordt uitgevoerd en volgorde wordt uitgevoerd naast. De **System.Timestamp** kolom heeft het tijdstempel voor de uiteindelijke toegewezen aan de gebeurtenis.

**Volgorde tolerantie**

* Gebeurtenissen die volgorde maar binnen de set 'volgorde tolerantieperiode binnenkomen' zijn **opnieuw geordend door tijdstempel**. 
* Gebeurtenissen die hoger is dan de tolerantie binnenkomen zijn **op verwijderd of aangepast**.
    * **Aangepast**: aangepast, zodat deze lijkt te zijn ontvangen op de laatste toegestane tijd. 
    * **Verwijderd**: verwijderd.

Om de volgorde van gebeurtenissen ontvangen binnen 'volgorde tolerantieperiode', te uitvoer van de query is **vertraging tolerantieperiode voor onjuiste volgorde**.

**Voorbeeld**

Late ontvangst tolerantie = 10 minuten<br/>
Buiten de volgorde tolerantie = 3 minuten<br/>
Verwerking door de tijd van de toepassing<br/>

Evenementen:

Gebeurtenis 1 _toepassing tijd_ = 00:00:00 _aankomsttijd_ = 00:10:01 _System.Timestamp_ 00:00:01 aangepast omdat = (_aankomsttijd_  -  _Toepassing tijd_) is hoger dan de late ontvangst tolerantie.

Gebeurtenis 2 _toepassing tijd_ = 00:00:01 _aankomsttijd_ = 00:10:01 _System.Timestamp_ 00:00:01 niet aangepast omdat de tijd van de toepassing ligt binnen de late ontvangst = venster.

Gebeurtenis 3 _toepassing tijd_ = 00:10:00 _aankomsttijd_ = 00:10:02 _System.Timestamp_ 00:10:00 niet aangepast omdat de tijd van de toepassing is in het venster van de aankomst laat = .

Gebeurtenis 4 _toepassing tijd_ = 00:09:00 _aankomsttijd_ = 00:10:03 _System.Timestamp_ 00:09:00, met oorspronkelijke tijdstempel geaccepteerd, omdat de tijd van de toepassing binnen de out = volgorde tolerantie.

Gebeurtenis 5 _toepassing tijd_ = 00:06:00 _aankomsttijd_ = 00:10:04 _System.Timestamp_ 00:07:00 aangepast omdat het tijdstip van de toepassing is ouder dan de volgorde = tolerantie.



## <a name="get-help"></a>Help opvragen
Voor meer informatie en ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
