---
title: Verwerken van gebeurtenis volgorde en lateness met Azure Stream Analytics | Microsoft Docs
description: Meer informatie over de werking van Stream Analytics met out volgorde of laat gebeurtenissen in gegevensstromen.
keywords: volgorde, laat, gebeurtenissen
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: 71929b449f2a0fa55327fd3f9741208506859e85
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-stream-analytics-event-order-considerations"></a>Azure Stream Analytics gebeurtenis volgorde overwegingen

## <a name="arrival-time-and-application-time"></a>Aankomsttijd en het tijdstip van de toepassing

In een tijdelijke gegevensstroom van gebeurtenissen krijgt elke gebeurtenis een tijdstempel. Azure Stream Analytics wijst een tijdstempel toe aan elke gebeurtenis met aankomst tijd of tijd van de toepassing. De **System.Timestamp** kolom heeft de tijdstempel die is toegewezen aan de gebeurtenis. 

Aankomsttijd is toegewezen aan de invoerbron wanneer de gebeurtenis de bron bereikt. Aankomsttijd is toegankelijk via de **EventEnqueuedTime** eigenschap voor event hub invoer- en met behulp van de [BlobProperties.LastModified](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) eigenschap voor blob-invoer. 

Tijd van de toepassing wordt toegewezen wanneer de gebeurtenis wordt gegenereerd en het deel van de nettolading uitmaakt. U kunt gebeurtenissen verwerken door toepassing tijd de **tijdstempel door** -component in de query select. Als de **tijdstempel door** component is afwezig, gebeurtenissen worden verwerkt door aankomsttijd. 

Azure Stream Analytics produceert uitvoer in de volgorde van het tijdstempel en instellingen om te gaan met out volgorde gegevens biedt.


## <a name="handling-of-multiple-streams"></a>De verwerking van meerdere streams

Een Azure Stream Analytics-taak combineert gebeurtenissen van meerdere tijdlijnen in gevallen als volgt:

* Het opstellen van uitvoer van meerdere partities. Query's waarvoor geen een expliciete **partitie door PartitionId** component moet een combinatie van gebeurtenissen van alle partities.
* De samenvoeging van twee of meer verschillende invoerbronnen.
* Lid worden van invoerbronnen.

In scenario's waarbij meerdere tijdlijnen worden gecombineerd, levert Azure Stream Analytics uitvoer voor tijdstempel *t1* pas nadat alle bronnen die zijn gecombineerd ten minste gelijktijdig zijn *t1*. Stel bijvoorbeeld dat de query uit een event hub-partitie met twee partities kan lezen. Een van de partities *P1*, gebeurtenissen heeft tot keer *t1*. De andere partitie *P2*, gebeurtenissen heeft tot keer *t1 + x*. Uitvoer geproduceerd vervolgens tot keer *t1*. Maar als er een expliciete **partitie door PartitionId** component, zowel de partities voortgang onafhankelijk.

De instelling voor late ontvangst tolerantie wordt gebruikt om te gaan met het ontbreken van gegevens in een aantal partities.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Late ontvangst tolerantie en out-van-order tolerantie configureren
Invoer stromen die zich niet in volgorde zijn:
* Gesorteerd (en dus vertraagd)
* Aangepast door het systeem volgens het beleid door gebruiker opgegeven

Stream Analytics maximaal gebeurtenissen laat en out volgorde wanneer u door de tijd van de toepassing verwerkt bent wordt toegestaan. De volgende instellingen zijn beschikbaar in de **gebeurtenis ordening** optie in de Azure-portal: 

![Stream Analytics gebeurtenisverwerking](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

### <a name="late-arrival-tolerance"></a>Late ontvangst tolerantie
Late ontvangst tolerantie geldt alleen wanneer u door de tijd van de toepassing verwerkt bent. Anders wordt wordt de instelling genegeerd.

Late ontvangst tolerantie is het maximale verschil tussen het van de aankomsttijd en tijd van de toepassing. Als een gebeurtenis hoger is dan de late ontvangst tolerantie binnenkomt (bijvoorbeeld toepassing tijd *app_t* < aankomsttijd *arr_t* -late ontvangst beleid tolerantie *late_t*), wordt de de gebeurtenis wordt aangepast aan de maximale lengte van de late ontvangst tolerantie (*arr_t* - *late_t*). De late ontvangst window is de maximale vertraging tussen het genereren van gebeurtenis en ontvangst van de gebeurtenis op de invoerbron. 

Wanneer meerdere partities van de dezelfde invoerstroom of meerdere invoer stromen worden gecombineerd, is late ontvangst tolerantie de maximale hoeveelheid tijd dat elke partitie moet voor nieuwe gegevens wachten. 

Op basis van tolerantie voor late ontvangst aanpassing eerst gebeurt. Op basis van out volgorde tolerantie aanpassing gebeuren. De **System.Timestamp** kolom heeft het laatste tijdstempel is toegewezen aan de gebeurtenis.

### <a name="out-of-order-tolerance"></a>Volgorde buiten tolerantie
Gebeurtenissen die een andere volgorde ontvangen, maar binnen de set out volgorde tolerantieperiode door tijdstempel wordt gewijzigd. Gebeurtenissen die hoger is dan de tolerantieperiode binnenkomen zijn:
* **Aangepast**: aangepast, zodat deze lijkt te zijn ontvangen op de laatste toegestane tijd. 
* **Verwijderd**: verwijderd.

Wanneer de Stream Analytics volgorde gebeurtenissen die in het venster tolerantie out volgorde worden ontvangen, is de uitvoer van de query door het venster out volgorde tolerantie vertraging.

### <a name="example"></a>Voorbeeld

* Late ontvangst tolerantie = 10 minuten<br/>
* Out-van-order tolerantie = 3 minuten<br/>
* Verwerking door de tijd van de toepassing<br/>
* Evenementen:
   1. **Tijd van de toepassing** = 00:00:00 **aankomsttijd** = 00:10:01 **System.Timestamp** 00:00:01 aangepast omdat = (**aankomsttijd - tijd van de toepassing**) is meer dan de late ontvangst tolerantie.
   2. **Tijd van de toepassing** = 00:00:01 **aankomsttijd** = 00:10:01 **System.Timestamp** = 00:00:01 niet aangepast omdat toepassing tijd aan het eind aankomst-venster.
   3. **Tijd van de toepassing** = 00:10:00 **aankomsttijd** = 00:10:02 **System.Timestamp** = 00:10:00 niet aangepast omdat toepassing tijd aan het eind aankomst-venster.
   4. **Tijd van de toepassing** = 00:09:00 **aankomsttijd** = 00:10:03 **System.Timestamp** 00:09:00, met de oorspronkelijke tijdstempel geaccepteerd omdat de tijd van de toepassing ligt binnen de out-van-volgorde = tolerantie.
   5. **Tijd van de toepassing** = 00:06:00 **aankomsttijd** = 00:10:04 **System.Timestamp** = 00:07:00, omdat de tijd van de toepassing is ouder dan de tolerantie out volgorde aangepast.

### <a name="practical-considerations"></a>Praktische overwegingen
Zoals eerder vermeld, is late ontvangst tolerantie het maximale verschil tussen de tijd van de toepassing en aankomsttijd. Wanneer u door de tijd van de toepassing verwerkt bent, worden de gebeurtenissen die hoger dan de opgegeven tolerantie voor late ontvangst zijn worden aangepast voordat de out-van-order tolerantie-instelling wordt toegepast. Met ingang van volgorde is dus het minimum voor late ontvangst tolerantie en out-van-order tolerantie.

Redenen voor out-van-order gebeurtenissen binnen een stroom zijn:
* Tijdsverschil tussen de afzenders.
* Variabele latentie tussen de afzender en de invoer gebeurtenisbron.

Redenen voor late ontvangst zijn:
* Afzenders batchverwerking en het verzenden van de gebeurtenissen voor een interval later na de interval.
* Latentie tussen de gebeurtenis door afzender verzenden en ontvangen van de gebeurtenis bij de invoerbron.

Bij het configureren van late ontvangst tolerantie en out volgorde tolerantie voor een specifieke taak, overweeg juistheid latentievereisten en de voorgaande factoren.

Hier volgen enkele voorbeelden.

#### <a name="example-1"></a>Voorbeeld 1 
De query heeft een **partitie door PartitionId** component. Binnen een enkele partitie, worden gebeurtenissen verzonden via de methoden synchrone verzenden. Synchrone verzenden methoden blokkeren totdat de gebeurtenissen worden verzonden.

In dit geval volgorde is nul omdat gebeurtenissen in volgorde met expliciete bevestiging verzonden worden voordat de volgende gebeurtenis wordt verzonden. Late ontvangst is de maximale vertraging tussen het genereren van de gebeurtenis en het verzenden van de gebeurtenis, plus de maximale latentie tussen de afzender en de invoerbron.

#### <a name="example-2"></a>Voorbeeld 2
De query heeft een **partitie door PartitionId** component. Binnen een enkele partitie, worden gebeurtenissen verzonden via het verzenden van asynchrone methoden. Verzenden van asynchrone methoden kunnen meerdere verzendt op hetzelfde moment, wat leiden out volgorde gebeurtenissen tot kan initiëren.

In dit geval volgorde en late ontvangst zijn ten minste de maximale vertraging tussen het genereren van de gebeurtenis en het verzenden van de gebeurtenis, plus de maximale latentie tussen de afzender en de invoerbron.

#### <a name="example-3"></a>Voorbeeld 3
De query heeft geen een **partitie door PartitionId** component, en er zijn ten minste twee partities.

Configuratie is hetzelfde als voorbeeld 2. Gebrek aan gegevens in een van de partities kan echter de uitvoer door een extra tolerantieperiode voor late uitstellen.

## <a name="handling-event-producers-with-differing-timelines"></a>Afhandeling van gebeurtenis producenten met verschillende tijdlijnen
Een stream met één invoer gebeurtenis bevat vaak gebeurtenissen die afkomstig van meerdere producenten gebeurtenis, zoals afzonderlijke apparaten zijn. Deze gebeurtenissen mogelijk een verkeerde volgorde de eerder besproken oorzaken binnenkomen. In deze scenario's, hoewel de verslechterde tussen producenten gebeurtenis mogelijk groot is, is de verslechterde binnen de gebeurtenissen van een enkele producent klein (of zelfs niet-bestaande).

Azure Stream Analytics biedt algemene mechanismen voor het omgaan met out volgorde gebeurtenissen. Deze mechanismen leiden tot een verwerkingsvertragingen (tijdens het wachten op voor de straggling gebeurtenissen te bereiken van het systeem), verwijderd of aangepast gebeurtenissen of beide.

Nog in veel scenario's, wordt de gewenste query verwerkt gebeurtenissen van verschillende gebeurtenis producenten onafhankelijk. Het aggregeren bijvoorbeeld gebeurtenissen per venster per apparaat. In dergelijke gevallen hoeft niet uit te stellen van de uitvoer die met één gebeurtenis producent correspondeert tijdens het wachten op de andere gebeurtenis producenten te lopen. Met andere woorden, hoeft niet te bekommeren om de tijd tussen producenten scheeftrekken. U kunt deze negeren.

Natuurlijk betekent dit dat de uitvoergebeurtenissen zelf volgorde ten opzichte van de tijdstempels zijn. De downstream consument moet kunnen maken met dit gedrag. Maar alle gebeurtenissen in de uitvoer juist is.

Azure Stream Analytics deze functionaliteit wordt geïmplementeerd met behulp van de [TIMESTAMP-door via](https://msdn.microsoft.com/library/azure/mt573293.aspx) component.

## <a name="summary"></a>Samenvatting
* Late ontvangst tolerantie en het venster out volgorde is gebaseerd op juistheid en latentievereisten configureren. Denk ook na over hoe de gebeurtenissen worden verzonden.
* Het is raadzaam dat out volgorde tolerantie kleiner dan late ontvangst tolerantie is.
* Wanneer u meerdere tijdlijnen samenvoegen wilt, kan de uitvoer door een extra tolerantieperiode voor late gebrek aan gegevens in een van de bronnen of partities worden vertraagd.

## <a name="get-help"></a>Help opvragen
Voor meer informatie en ondersteuning kunt u proberen de [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
