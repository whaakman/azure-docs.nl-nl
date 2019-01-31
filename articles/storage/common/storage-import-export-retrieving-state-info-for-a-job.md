---
title: Bij het ophalen van informatie over de status voor een Azure Import/Export-taak | Microsoft Docs
description: Informatie over het verkrijgen van informatie over de status voor Microsoft Azure Import/Export-service-taken.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: e51803a8baffc904c8d68a8cf22d1ff584e065d1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456201"
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>Bij het ophalen van informatie over de status voor een taak voor importeren/exporteren
U kunt aanroepen de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking voor het ophalen van informatie over beide importeren en exporteren van taken. De geretourneerde informatie bevat:

-   De huidige status van de taak.

-   De geschatte percentage dat elke taak is voltooid.

-   De huidige status van elk station.

-   URI's voor blobs met foutenlogboeken en uitgebreide logboekregistratie informatie (indien ingeschakeld).

De volgende secties worden de gegevens die zijn geretourneerd door de `Get Job` bewerking.

## <a name="job-states"></a>Taakstatussen
In de tabel en de status van onderstaande diagram wordt beschreven welke status een taak via tijdens de levenscyclus overgangen. De huidige status van de taak kan worden bepaald door het aanroepen van de `Get Job` bewerking.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

De volgende tabel beschrijft elke status die een taak kan passeren.

|Taakstatus|Description|
|---------------|-----------------|
|`Creating`|Nadat u de taak Put-bewerking aanroept, een taak gemaakt wordt en de status is ingesteld op `Creating`. Terwijl de taak wordt de `Creating` staat, wordt de Import/Export-service wordt ervan uitgegaan dat de schijven niet zijn verzonden naar het datacenter. Een taak kan blijven de `Creating` staat voor maximaal twee weken, waarna deze worden automatisch verwijderd door de service.<br /><br /> Als u de eigenschappen van de taak-bewerking aanroept terwijl de taak wordt de `Creating` staat, de taak blijft in de `Creating` status en de time-outinterval tot twee weken opnieuw wordt ingesteld.|
|`Shipping`|Nadat u het pakket te verzenden, moet u aanroepen de update van de bewerking taakeigenschappen bijwerken van de status van de taak is `Shipping`. De status van de verzending kan alleen worden ingesteld als de `DeliveryPackage` (postcode carrier en Traceringsnummer) en de `ReturnAddress` eigenschappen zijn ingesteld voor de taak.<br /><br /> De taak blijft in de status van de verzending voor maximaal twee weken. Als twee weken hebt doorgegeven en de schijven niet zijn ontvangen, wordt de Import/Export-service-operators hoogte gebracht.|
|`Received`|Nadat alle stations zijn ontvangen in het datacenter, wordt de taakstatus van de worden ingesteld op de status van de ontvangen.|
|`Transferring`|Nadat de schijven zijn ontvangen in het datacenter en ten minste één station verwerking is gestart, de taakstatus wordt ingesteld op de `Transferring` staat. Zie de `Drive States` sectie hieronder voor meer informatie.|
|`Packaging`|Nadat alle stations verwerking is voltooid, de taak wordt geplaatst de `Packaging` status totdat de schijven terug naar de klant worden geleverd.|
|`Completed`|Nadat alle stations zijn verzonden naar de klant als de taak is voltooid zonder fouten, klikt u vervolgens de taak wordt ingesteld op de `Completed` staat. De taak automatisch worden verwijderd na 90 dagen in de `Completed` staat.|
|`Closed`|Nadat alle stations zijn verzonden naar de klant, als er fouten tijdens de verwerking van de taak zijn, klikt u vervolgens de taak wordt ingesteld op de `Closed` staat. De taak automatisch worden verwijderd na 90 dagen in de `Closed` staat.|

U kunt een taak alleen op bepaalde statussen annuleren. Een geannuleerde taak slaat de gegevens kopiëren-stap echter anders volgt de dezelfde statusovergangen als een taak die niet is geannuleerd.

De volgende tabel beschrijft de fouten die voordat de taakstatus van elke, evenals de gevolgen zijn voor de taak optreden kunnen wanneer er een fout optreedt.

|Taakstatus|Gebeurtenis|Oplossing / volgende stappen|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Een of meer schijven voor een taak is aangekomen, maar de taak is niet in de `Shipping` status of er is geen record van de taak in de service.|Het operationele team van Import/Export-service probeert contact opnemen met de klant om te maken of bijwerken van de taak met de benodigde informatie om de taak.<br /><br /> Als het operationele team geen contact opnemen met de klant binnen twee weken is, probeert het operationele team te retourneren van de stations.<br /><br /> In het geval dat de schijven kunnen niet worden geretourneerd en de klant kan niet worden bereikt, wordt de stations veilige manier worden vernietigd in 90 dagen.<br /><br /> Houd er rekening mee dat een taak kan niet worden verwerkt totdat de status is bijgewerkt naar `Shipping`.|
|`Shipping`|Het pakket voor een taak is niet meer dan twee weken ontvangen.|Het operationele team ontvangt de klant van het pakket ontbreekt. Op basis van het antwoord van de klant, wordt het operationele team een uitbreiden van het interval moet worden gewacht voor het pakket te komen of annuleer de taak.<br /><br /> In het geval dat de klant is niet bereikbaar, of niet binnen 30 dagen reageert, het operationele team actie voor het verplaatsen van de taak wordt nu de `Shipping` staat rechtstreeks naar de `Closed` staat.|
|`Completed/Closed`|De schijven nooit bereikt adres van de afzender of zijn beschadigd tijdens de verzending (geldt alleen voor een exporttaak bekijken).|Als de stations adres van de afzender niet bereiken, moet eerst de klant de Get Job-bewerking aanroepen of Controleer de status van de taak in de portal om ervoor te zorgen dat de schijven zijn verzonden. Als de stations zijn verzonden, klikt u vervolgens de klant moet contact opnemen met de verzending-provider om te proberen en zoek de schijven.<br /><br /> Als de stations zijn beschadigd tijdens de verzending, kan de klant aan te vragen van een andere taak voor het exporteren of downloaden van de ontbrekende blobs wilt.|
|`Transferring/Packaging`|Taak heeft een onjuiste of ontbrekende retouradres.|Het operationele team contact op met de contactpersoon voor de taak om op te halen van het juiste adres.<br /><br /> In het geval dat de klant kan niet worden bereikt, wordt de stations veilige manier worden vernietigd in 90 dagen.|
|`Creating / Shipping/ Transferring`|Een station dat niet in de lijst met schijven voorkomt moeten worden geïmporteerd is in het pakket voor verzending opgenomen.|De extra schijven wordt niet verwerkt en naar de klant wordt geretourneerd wanneer de taak is voltooid.|

## <a name="drive-states"></a>Status van station
In de tabel en in het onderstaande diagram beschrijven de levenscyclus van een afzonderlijke station als het overgangen via een taak importeren of exporteren. U kunt de huidige status van station ophalen door het aanroepen van de `Get Job` bewerking op te halen en de `State` element van de `DriveList` eigenschap.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

De volgende tabel beschrijft elke status die een station kan passeren.

|Status van station|Description|
|-----------------|-----------------|
|`Specified`|Voor een importtaak wanneer de taak is gemaakt met de bewerking taak plaatst, wordt de initiële status voor een station is de `Specified` staat. Voor een exporttaak bekijken, omdat er geen station is opgegeven wanneer de taak is gemaakt, wordt de status van de eerste schijf is de `Received` staat.|
|`Received`|Het station verandert in de `Received` status wanneer de Import/Export-service-operator de stations die zijn ontvangen van het transportbedrijf voor een importtaak heeft verwerkt. Voor een exporttaak bekijken, de status van de eerste schijf is de `Received` staat.|
|`NeverReceived`|Het station wordt verplaatst naar de `NeverReceived` status wanneer het pakket voor een taak wordt ontvangen, maar het pakket niet het station bevat. Een station kan ook verplaatsen naar deze status als dit twee weken is, omdat de service heeft de verzendinformatie ontvangen, maar het pakket nog niet in het datacenter ontvangen is.|
|`Transferring`|Een station wordt verplaatst naar de `Transferring` status wanneer de service begint om over te dragen van gegevens uit het station naar Windows Azure Storage.|
|`Completed`|Een station wordt verplaatst naar de `Completed` status wanneer de service is de gegevens zonder fouten is overgedragen.|
|`CompletedMoreInfo`|Een station wordt verplaatst naar de `CompletedMoreInfo` status wanneer de service enkele problemen is opgetreden tijdens het kopiëren van gegevens van of naar het station. De informatie kan bestaan uit fouten, waarschuwingen of informatieve berichten over het overschrijven van blobs.|
|`ShippedBack`|Het station wordt verplaatst naar de `ShippedBack` status wanneer op de achterkant van data center is verzonden naar het retouradres.|

De volgende tabel beschrijft de statussen van de fout station en de acties die voor elke status.

|Status van station|Gebeurtenis|Oplossing / volgende stap|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|Een station dat is gemarkeerd als `NeverReceived` (omdat deze niet is ontvangen als onderdeel van de verzending van de taak) binnenkomt in een andere verzending.|Het operationele team wordt verplaatst u het station op de `Received` staat.|
|`N/A`|Een schijf die geen deel uitmaakt van een taak wordt ontvangen in het datacenter als onderdeel van een andere taak.|Het station wordt gemarkeerd als een extra schijf en naar de klant wordt geretourneerd wanneer de taak die is gekoppeld aan het oorspronkelijke pakket is voltooid.|

## <a name="faulted-states"></a>Mislukte Staten
Wanneer een taak of het station niet normaal gesproken navigatie door de verwachte levensduur, de taak of het station worden verplaatst naar een `Faulted` staat. Het operationele team wordt op dat moment contact op met de klant door e-mailadres of telefoonnummer. Als het probleem opgelost is, de mislukte taak of het station moet worden ondernomen van de `Faulted` systeemstatus- en verplaatst naar de juiste status.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
