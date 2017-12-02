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
ms.openlocfilehash: 208dfa14d5d18e106d654539cd80bafdeb90cdf8
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Azure Stream Analytics gebeurtenis volgorde overweging

## <a name="understand-arrival-time-and-application-time"></a>Begrijpen aankomsttijd en het tijdstip van de toepassing.

In een tijdelijke gegevensstroom van gebeurtenissen krijgt elke gebeurtenis een tijdstempel. Tijdstempel Azure Stream Analytics toegewezen aan elke gebeurtenis met behulp van de aankomsttijd of de tijd van de toepassing. De kolom 'System.Timestamp' heeft de tijdstempel die is toegewezen aan de gebeurtenis. Aankomsttijd is toegewezen aan de invoerbron wanneer de gebeurtenis de bron bereikt. Aankomsttijd is EventEnqueuedTime voor invoer van de Event Hub en [blob laatst gewijzigd om](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) voor blob-invoer. Tijd van de toepassing wordt toegewezen wanneer de gebeurtenis wordt gegenereerd en het deel van de nettolading uitmaakt. Voor het verwerken van gebeurtenissen door de tijd van de toepassing, door de component 'Tijdstempel door' te gebruiken in de query select. Als de component 'Tijdstempel door' niet aanwezig is, worden door aankomsttijd gebeurtenissen verwerkt. Aankomsttijd toegankelijk gebruikt EventEnqueuedTime eigenschap voor event hub en BlobLastModified-eigenschap voor blob-invoer. Azure Stream Analytics produceert uitvoer in de volgorde van het tijdstempel en vindt u enkele instellingen om te gaan met de volgorde van gegevens.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Azure Stream Analytics-verwerking van meerdere streams

Azure Stream Analytics-taak combineert meerdere tijdlijnen in enkele gevallen, met inbegrip van gebeurtenissen

* Het opstellen van uitvoer van meerdere partities. Query's waarvoor geen een expliciete 'partitie door PartitionId' zou moeten gebeurtenissen van alle partities worden gecombineerd.
* De samenvoeging van twee of meer verschillende invoerbronnen.
* Lid worden van invoerbronnen.

In scenario's waarbij meerdere tijdlijnen worden gecombineerd, Azure Stream Analytics produceert uitvoer voor een tijdstempel *t1* pas nadat alle bronnen die zijn gecombineerd ten minste gelijktijdig zijn *t1*.
Bijvoorbeeld, als de query leest uit een *Event Hub* partitie die twee partities en een van de partitie heeft *P1* heeft gebeurtenissen tot keer *t1* en andere partitie  *P2* heeft gebeurtenissen tot keer *t1 + x*, uitvoer geproduceerd tot keer *t1*.
Maar als er een expliciete *'Partitie door PartitionId'* component, zowel de partities verloopt onafhankelijk.
Laat aankomen van de tolerantie-instelling wordt gebruikt om te gaan met de afwezigheid van gegevens in een aantal partities.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Late ontvangst tolerantie en volgorde tolerantie configureren
Invoer stromen die zich niet in volgorde zijn:
* Gesorteerd (en dus **vertraagd**).
* Aangepast door het systeem, volgens de door de gebruiker opgegeven beleid.

Stream Analytics maximaal wordt toegestaan laat en volgorde gebeurtenissen bij het verwerken van door **toepassing tijd**. De volgende instellingen zijn beschikbaar in de **gebeurtenis ordening** optie in Azure-portal: 

![Stream Analytics gebeurtenisverwerking](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

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

* Late ontvangst tolerantie = 10 minuten<br/>
* Buiten de volgorde tolerantie = 3 minuten<br/>
* Verwerking door de tijd van de toepassing<br/>
* Evenementen:
   * Gebeurtenis 1 _toepassing tijd_ = 00:00:00 _aankomsttijd_ = 00:10:01 _System.Timestamp_ 00:00:01 aangepast omdat = (_aankomsttijd_  -  _Toepassing tijd_) is hoger dan de late ontvangst tolerantie.
   * Gebeurtenis 2 _toepassing tijd_ = 00:00:01 _aankomsttijd_ = 00:10:01 _System.Timestamp_ 00:00:01 niet aangepast omdat de tijd van de toepassing ligt binnen de late ontvangst = venster.
   * Gebeurtenis 3 _toepassing tijd_ = 00:10:00 _aankomsttijd_ = 00:10:02 _System.Timestamp_ 00:10:00 niet aangepast omdat de tijd van de toepassing is in het venster van de aankomst laat = .
   * Gebeurtenis 4 _toepassing tijd_ = 00:09:00 _aankomsttijd_ = 00:10:03 _System.Timestamp_ 00:09:00, met oorspronkelijke tijdstempel geaccepteerd, omdat de tijd van de toepassing binnen de out = volgorde tolerantie.
   * Gebeurtenis 5 _toepassing tijd_ = 00:06:00 _aankomsttijd_ = 00:10:04 _System.Timestamp_ 00:07:00 aangepast omdat het tijdstip van de toepassing is ouder dan de volgorde = tolerantie.

## <a name="practical-considerations"></a>Praktische overwegingen
Zoals eerder vermeld, *late ontvangst tolerantie* is het maximale verschil tussen de tijd van de toepassing en aankomsttijd.
Ook wanneer verwerking door toepassing tijd, gebeurtenissen die hoger zijn dan de geconfigureerde *late ontvangst tolerantie* worden aangepast voordat de *volgorde tolerantie* instelling wordt toegepast. Met ingang van volgorde is dus het minimum voor late ontvangst tolerantie en de volgorde tolerantie.

Gebeurtenissen in andere volgorde binnen een stroom oorzaken met inbegrip van
* Tijdverschil tussen de afzenders.
* Variabele latentie tussen de afzender en de gebeurtenisbron invoer.

Late ontvangst gebeurt om redenen, met inbegrip van
* Afzenders batch- en de gebeurtenissen voor een interval later verzenden na het interval.
* Latentie tussen de gebeurtenis door afzender verzenden en ontvangen van de gebeurtenis bij de invoerbron.

Tijdens het configureren van *late ontvangst tolerantie* en *volgorde tolerantie* voor een specifieke taak, juistheid, latentievereisten en hoger factoren overwegen.

Hier volgen enkele voorbeelden

### <a name="example-1"></a>Voorbeeld 1: 
Query 'Partitie door PartitionId'-component bevat en binnen een enkele partitie, gebeurtenissen worden verzonden met behulp van methoden synchrone verzenden. Synchrone verzenden methoden blokkeren totdat de gebeurtenissen worden verzonden.
In dit geval volgorde is nul omdat gebeurtenissen worden in volgorde met expliciete bevestiging voordat het verzenden van de volgende gebeurtenis verzonden. Late ontvangst is een maximale vertraging tussen het genereren van de gebeurtenis en het verzenden van de gebeurtenis + maximale latentie tussen de afzender en de invoerbron

### <a name="example-2"></a>Voorbeeld 2:
Query 'Partitie door PartitionId' component heeft en binnen een enkele partitie, gebeurtenissen worden verzonden via de methode asynchrone verzending. Verzenden van asynchrone methoden kunnen meerdere verzendt op hetzelfde moment, wat leiden gebeurtenissen in andere volgorde tot kan worden geïnitieerd.
In dit geval zijn de volgorde en late ontvangst ten minste maximale vertraging tussen het genereren van de gebeurtenis en het verzenden van de gebeurtenis + maximale latentie tussen de afzender en de invoerbron.

### <a name="example-3"></a>Voorbeeld 3:
Query heeft geen 'Partitie door PartitionId' en er zijn ten minste twee partities.
Configuratie is hetzelfde als voorbeeld 2. Gebrek aan gegevens in een van de partities kan echter de uitvoer uitstellen door een extra * late ontvangst tolerantie ' venster.

## <a name="to-summarize"></a>Samenvatten
* Late ontvangst tolerantie en volgorde venster moeten worden geconfigureerd op basis van de juistheid, latentie is vereist en moeten ook rekening houden hoe de gebeurtenissen worden verzonden.
* Het is raadzaam dat volgorde tolerantie kleiner dan late ontvangst tolerantie is.
* Bij het combineren van meerdere tijdlijnen, kan de uitvoer door een extra tolerantieperiode voor late gebrek aan gegevens in een van de bronnen of partities worden vertraagd.

## <a name="get-help"></a>Help opvragen
Voor meer informatie en ondersteuning kunt u proberen onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Volgende stappen
* [Inleiding tot Stream Analytics](stream-analytics-introduction.md)
* [Aan de slag met Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-taken schalen](stream-analytics-scale-jobs.md)
* [Naslaggids voor stream Analytics query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Stream Analytics management REST API-referentiemateriaal](https://msdn.microsoft.com/library/azure/dn835031.aspx)
