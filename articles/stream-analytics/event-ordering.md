---
title: Configureren van beleid voor gebeurtenisvolgordes voor Azure Stream Analytics
description: In dit artikel wordt beschreven hoe u gaat over het configureren van instellingen ook rangschikken in Stream Analytics
services: stream-analytics
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 970eeb871775e24abb87c8b977e214645e514d3b
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58190518"
---
# <a name="configuring-event-ordering-policies-for-azure-stream-analytics"></a>Configureren van beleid voor gebeurtenisvolgordes voor Azure Stream Analytics

Dit artikel wordt beschreven hoe u kunt instellen en laat aankomst en out volgorde gebeurtenis beleid gebruiken in Azure Stream Analytics. Deze beleidsregels worden alleen toegepast wanneer u de [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) -component in uw query.

## <a name="event-time-and-arrival-time"></a>Tijd van de gebeurtenis- en aankomsttijd

Uw Stream Analytics-taak kunt verwerken van gebeurtenissen op basis van een *gebeurtenistijd* of *aankomsttijd*. **Tijd van de gebeurtenis/toepassing** de tijdstempel aanwezig zijn in de nettolading is (als de gebeurtenis is gegenereerd). **Ontvangsttijd** de tijdstempel is wanneer de gebeurtenis is ontvangen op de invoerbron (Event Hubs/IoT Hub/Blob storage). 

Stream Analytics verwerkt standaard gebeurtenissen op *aankomsttijd*, maar u kunt kiezen voor het verwerken van gebeurtenissen op *gebeurtenistijd* met behulp van [TIMESTAMP BY](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) -component in uw query. Laat aankomst en out volgorde beleidsregels zijn alleen van toepassing als u gebeurtenissen worden verwerkt door de tijd van de gebeurtenis. Houd rekening met vereisten voor latentie en juistheid voor uw scenario bij het configureren van deze instellingen. 

## <a name="what-is-late-arrival-policy"></a>Wat is beleid voor latere aankomst?

Gebeurtenissen worden soms gemeld verschillende oorzaken hebben. Bijvoorbeeld, heeft een gebeurtenis die 40 seconden laat binnenkomt gebeurtenistijd = 00:10:00 uur en aankomst tijd = 00:10:40. Als u het beleid voor latere aankomst ingesteld op 15 seconden, elke gebeurtenis die later zijn dan 15 seconden een verwijderd (niet verwerkt door Stream Analytics) of hun tijd van de gebeurtenis hebt aangepast binnenkomt. In het bovenstaande voorbeeld als de gebeurtenis is aangekomen 40 seconden laat (meer dan beleidsset), wordt de tijd van de gebeurtenis aangepast aan de maximale late aankomst beleid 00:10:25 (aankomsttijd - laat aankomst beleidswaarde). Laat aankomst standaardbeleid is 5 seconden.

## <a name="what-is-out-of-order-policy"></a>Wat is beleid voor out-van-order? 

Gebeurtenis kan ook niet de juiste volgorde binnenkomen. Nadat de tijd van de gebeurtenis wordt aangepast op basis van beleid voor latere aankomst, kunt u ook om automatisch te verwijderen of aanpassen van gebeurtenissen die out-van-order. Als u dit beleid op 8 seconden instelt, wordt alle gebeurtenissen die niet de juiste volgorde, maar in het venster 8 seconde binnenkomen volgorde van tijd van de gebeurtenis wordt gewijzigd. Gebeurtenissen die later binnenkomen zal worden verwijderd of aangepast aan de beleidswaarde voor maximale out volgorde. Standaardbeleid voor out-van-order is 0 seconden. 

## <a name="adjust-or-drop-events"></a>Aanpassen of verwijderen van gebeurtenissen

Als er gebeurtenissen plaatsvinden laat of out volgorde op basis van de beleidsregels die u hebt geconfigureerd, kunt u dergelijke neerzetten (niet verwerkt door Stream Analytics) of hun tijd van de gebeurtenis aangepast.

We bekijken een voorbeeld van het beleid in actie.
<br> **Laat aankomst beleid:** 15 seconden
<br> **Beleid voor out-van-volgorde:** 8 seconden

| Nee. de gebeurtenis | Tijdstip van gebeurtenis | Ontvangsttijd | System.Timestamp | Uitleg |
| --- | --- | --- | --- | --- |
| **1** | 00:10:00  | 00:10:40  | 00:10:25  | Gebeurtenis laat en buiten tolerantieniveau is aangekomen. Tijd van de gebeurtenis wordt dus aangepast aan maximale laat aankomst tolerantie.  |
| **2** | 00:10:30 | 00:10:41  | 00:10:30  | Gebeurtenis is aangekomen laat maar binnen de toegestane niveau. Tijd van de gebeurtenis wordt dus niet ophalen aangepast.  |
| **3** | 00:10:42 | 00:10:42 | 00:10:42 | Gebeurtenissen ontvangen op tijd. Geen aanpassing die nodig zijn.  |
| **4** | 00:10:38  | 00:10:43  | 00:10:38 | Gebeurtenis is aangekomen out volgorde, maar binnen de tolerantie van 8 seconden. Tijd van de gebeurtenis wordt dus niet ophalen aangepast. Voor testdoeleinden analytics, wordt deze gebeurtenis worden beschouwd als het voorgaande gebeurtenisnummer 4.  |
| **5** | 00:10:35 | 00:10:45  | 00:10:37 | Gebeurtenis is aangekomen out volgorde en buiten tolerantie van 8 seconden. Tijd van de gebeurtenis wordt dus ingesteld op maximale tolerantie voor out-van-order. |

## <a name="can-these-settings-delay-output-of-my-job"></a>Kunnen deze instellingen uitvoer van Mijn taak uitstellen? 

Ja. Out-van-order beleid is standaard ingesteld op nul (00 minuten en 00 seconden). Als u de standaardwaarde wijzigt, wordt de eerste uitvoer van uw taak is vertraagd door deze waarde (of hoger). 

Als een van de partities van de invoer gebeurtenissen heeft ontvangen, moet u de uitvoer worden uitgesteld door de latere aankomst-beleidswaarde verwacht. Als u wilt weten waarom, lees de onderstaande sectie met InputPartition fout. 

## <a name="i-see-lateinputevents-messages-in-my-activity-log"></a>Ik bekijken LateInputEvents berichten op mijn activiteitenlogboek

Deze berichten worden weergegeven om u te informeren dat gebeurtenissen hebben laat is aangekomen en zijn verwijderd of op basis van uw configuratie aangepast. Als u op de juiste wijze laat aankomst beleid hebt geconfigureerd, kunt u deze berichten negeren. 

Voorbeeld van dit bericht is: <br>
<code>
{"message Time":"2019-02-04 17:11:52Z","error":null,
"message":"First Occurred: 02/04/2019 17:11:48 | Resource Name: ASAjob | Message: Source 'ASAjob' had 24 data errors of kind 'LateInputEvent' between processing times '2019-02-04T17:10:49.7250696Z' and '2019-02-04T17:11:48.7563961Z'. Input event with application timestamp '2019-02-04T17:05:51.6050000' and arrival time '2019-02-04T17:10:44.3090000' was sent later than configured tolerance.","type":"DiagnosticMessage","correlation ID":"49efa148-4asd-4fe0-869d-a40ba4d7ef3b"} 
</code>

## <a name="i-see-inputpartitionnotprogressing-in-my-activity-log"></a>Zie ik InputPartitionNotProgressing in mijn activiteitenlogboek

De invoerbron (Event Hub/IoT Hub) waarschijnlijk heeft meerdere partities. Azure Stream Analytics geeft een resultaat voor tijdstempel t1 pas nadat alle partities die zijn gecombineerd ten minste op tijdstip t1 zijn. Bijvoorbeeld, wordt ervan uitgegaan dat de query van een event hub-partitie met twee partities leest. Een van de partities, P1, heeft gebeurtenissen tot en met tijdstip t1. De andere partitie, P2, heeft gebeurtenissen tot tijdstip t1 + x. Uitvoer wordt vervolgens tot tijdstip t1 geproduceerd. Maar als er een expliciete Partition by PartitionId-component, zowel de voortgang van de partities onafhankelijk van elkaar. 

Als meerdere partities van de dezelfde invoerstroom worden gecombineerd, is de latere aankomst tolerantie de maximale hoeveelheid tijd dat elke partitie moet voor nieuwe gegevens wachten. Als er één partitie in uw Event Hub, of als IoT Hub invoer ontvangen heeft, de planning voor het betreffende partitie niet voortgang totdat het de latere aankomst tolerantie drempelwaarde is bereikt. Dit de uitvoer de drempelwaarde voor tolerantie van laat aankomst uitgesteld. In dat geval ziet u het volgende bericht:
<br><code>
{"message Time":"2/3/2019 8:54:16 PM UTC","message":"Input Partition [2] does not have additional data for more than [5] minute(s). Partition will not progress until either events arrive or late arrival threshold is met.","type":"InputPartitionNotProgressing","correlation ID":"2328d411-52c7-4100-ba01-1e860c757fc2"} 
</code><br><br>
Dit bericht om u te informeren dat ten minste één partitie in uw invoer leeg is en de uitvoer van de latere aankomst drempelwaarde wordt vertraagd. Als u wilt dit ondervangen, u kunt het beste een van beide: 1. Zorg ervoor dat alle partities van de Event Hub/IoT-Hub invoer ontvangen. 2. Gebruik Partition by PartitionID-component in uw query. 

## <a name="next-steps"></a>Volgende stappen
* [Overwegingen bij het verwerken van tijd](stream-analytics-time-handling.md)
* [Metrische gegevens beschikbaar zijn in Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics)
