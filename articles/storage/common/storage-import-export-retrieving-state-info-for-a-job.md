---
title: Bij het ophalen van informatie over de status voor een Azure Import/Export-job | Microsoft Docs
description: Informatie over het verkrijgen van informatie over de status van taken voor Microsoft Azure Import/Export-service.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 22d7e5f0-94da-49b4-a1ac-dd4c14a423c2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
ms.openlocfilehash: 13169716c47cf9389c8f2651393ac744441bdd6f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="retrieving-state-information-for-an-importexport-job"></a>Bij het ophalen van informatie over de status voor een taak van Import/Export
U kunt aanroepen de [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) bewerking voor het ophalen van informatie over beide importeren en exporteren van taken. De geretourneerde informatie omvat:

-   De huidige status van de taak.

-   De geschatte percentage dat elke taak is voltooid.

-   De huidige status van elk station.

-   URI's voor blobs bevat foutenlogboeken en uitgebreide logboekregistratie-informatie (indien ingeschakeld).

De volgende secties worden de informatie die wordt geretourneerd door de `Get Job` bewerking.

## <a name="job-states"></a>Taakstatussen
De tabel en het onderstaande diagram kunt status beschrijven de statussen die een taak via tijdens de levensduur overgangen. De huidige status van de taak kan worden bepaald door het aanroepen van de `Get Job` bewerking.

![JobStates](./media/storage-import-export-retrieving-state-info-for-a-job/JobStates.png "JobStates")

De volgende tabel beschrijft elke status die een taak kan doorgeven.

|Taakstatus|Beschrijving|
|---------------|-----------------|
|`Creating`|Nadat u de taak Put-bewerking aanroept, wordt een taak gemaakt en de status is ingesteld op `Creating`. Terwijl de taak wordt de `Creating` staat, de Import/Export-service wordt ervan uitgegaan dat de schijven nog niet zijn verzonden naar het datacenter. Een taak kan blijven in de `Creating` staat zijn voor maximaal twee weken, waarna deze worden automatisch verwijderd door de service.<br /><br /> Als u de bewerking Update taakeigenschappen niet aanroepen terwijl de taak wordt de `Creating` staat, wordt de taak blijft in de `Creating` status en de time-outinterval wordt opnieuw ingesteld op twee weken.|
|`Shipping`|Nadat u uw pakket verzendt, moet u aanroepen de taakeigenschappen bijwerken bewerking update de status van de taak is `Shipping`. De status van de back-ups kan alleen worden ingesteld als de `DeliveryPackage` (postcode carrier en volgnummer) en de `ReturnAddress` eigenschappen zijn ingesteld voor de taak.<br /><br /> De taak blijft in de status van de back-ups van twee weken. Als twee weken hebt doorgegeven en de schijven niet zijn ontvangen, wordt de operators Import/Export-service worden gewaarschuwd.|
|`Received`|Nadat alle stations zijn ontvangen in het datacenter, wordt de taakstatus worden ingesteld op de status Received hebben.|
|`Transferring`|Nadat de stations zijn ontvangen in het datacenter en ten minste één station verwerking is gestart, de taakstatus wordt ingesteld op de `Transferring` staat. Zie de `Drive States` sectie hieronder voor meer informatie.|
|`Packaging`|Nadat alle stations verwerking is voltooid, kunt u de taak wordt geplaatst de `Packaging` status totdat de stations terug naar de klant worden verzonden.|
|`Completed`|Nadat alle stations zijn verzonden naar de klant, als de taak is voltooid zonder fouten, klikt u vervolgens de taak wordt zo ingesteld dat de `Completed` staat. De taak automatisch worden verwijderd na 90 dagen in de `Completed` staat.|
|`Closed`|Nadat alle stations zijn verzonden naar de klant, als er fouten tijdens het verwerken van de taak zijn, klikt u vervolgens de taak wordt zo ingesteld dat de `Closed` staat. De taak automatisch worden verwijderd na 90 dagen in de `Closed` staat.|

U kunt een taak alleen op bepaalde statussen annuleren. Een geannuleerde taak de gegevens kopiëren stap overslaat, maar anders volgt de dezelfde statusovergangen als een taak die niet is geannuleerd.

De volgende tabel beschrijft de fouten die voor elke taakstatus, evenals de gevolgen voor de taak optreden kunnen wanneer een fout optreedt.

|Taakstatus|Gebeurtenis|Resolutie / de volgende stappen|
|---------------|-----------|------------------------------|
|`Creating or Undefined`|Een of meer stations voor een taak is aangekomen, maar de taak is niet in de `Shipping` status of er is geen record van de taak in de service.|Het operationele team van Import/Export-service probeert contact opnemen met de klant maken of bijwerken van de taak met de benodigde informatie om de taak vooruit.<br /><br /> Als het operationele team kan geen verbinding met de klant binnen twee weken is, wordt het operationele team probeert te retourneren van de stations.<br /><br /> In het geval dat de stations kunnen niet worden geretourneerd en de klant kan niet worden bereikt, wordt de stations veilige manier worden vernietigd 90 dagen.<br /><br /> Houd er rekening mee dat een taak kan niet worden verwerkt totdat de status is bijgewerkt met `Shipping`.|
|`Shipping`|Het pakket voor een taak niet meer dan twee weken aangekomen.|Het operationele team stuurt de klant van het pakket ontbreekt. Op basis van reactie van de klant, wordt het operationele team ofwel uitbreiden van het interval moet worden gewacht op het pakket moet worden uitgevoerd of annuleer de taak.<br /><br /> In het geval dat de klant kan geen verbinding worden gemaakt of niet binnen 30 dagen reageert, initieert de teamleden actie voor het verplaatsen van de taak in de `Shipping` status rechtstreeks naar de `Closed` staat.|
|`Completed/Closed`|De stations nooit bereikt adres van de afzender of zijn beschadigd tijdens de verzending (geldt alleen voor een exporttaak).|Als de stations adres van de afzender niet bereiken, moet eerst de klant de Get Job-bewerking aanroepen of Controleer de status van de taak in de portal om ervoor te zorgen dat de stations zijn verzonden. Als de stations zijn verzonden, klikt u vervolgens de klant moet contact op met de back-upfunctie voor provider om te proberen te vinden van de stations.<br /><br /> Als de stations tijdens de verzending beschadigd zijn, wil de klant aan te vragen van een andere taak voor het exporteren of de ontbrekende blobs downloaden.|
|`Transferring/Packaging`|Taak is een onjuist of het adres van afzender ontbreekt.|Het operationele team wordt naar de contactpersoon voor de taak voor het juiste adres bereiken.<br /><br /> In het geval dat de klant kan niet worden bereikt, wordt de stations veilige manier worden vernietigd 90 dagen.|
|`Creating / Shipping/ Transferring`|Een station dat niet wordt weergegeven in de lijst met stations moeten worden geïmporteerd is in het back-upfunctie-pakket opgenomen.|De extra stations wordt niet verwerkt en aan de klant wordt geretourneerd wanneer de taak is voltooid.|

## <a name="drive-states"></a>Station statussen
De tabel en het onderstaande diagram beschrijven we de levenscyclus van een afzonderlijke schijf het overgezet door middel van een taak worden geïmporteerd of geëxporteerd. U kunt de huidige status van de schijf ophalen door het aanroepen van de `Get Job` bewerking en bekijken de `State` element van de `DriveList` eigenschap.

![DriveStates](./media/storage-import-export-retrieving-state-info-for-a-job/DriveStates.png "DriveStates")

De volgende tabel beschrijft elke status die een station kan doorgeven.

|Status van station|Beschrijving|
|-----------------|-----------------|
|`Specified`|Voor een import-taak wanneer de taak is gemaakt met de taak Put-bewerking wordt de initiële status voor een station is de `Specified` staat. Voor een exporttaak omdat er geen station wordt opgegeven wanneer de taak is gemaakt, wordt de status van de initiële station is de `Received` staat.|
|`Received`|Het station wordt overgezet naar de `Received` status wanneer de operator Import/Export-service de stations die zijn ontvangen van het bedrijf back-upfunctie voor een import-taak is verwerkt. Voor een exporttaak de status van de initiële station is de `Received` staat.|
|`NeverReceived`|Het station wordt verplaatst naar de `NeverReceived` status wanneer het pakket voor een taak binnenkomt, maar het pakket niet het station bevat. Een station kunt ook verplaatsen naar deze status als deze twee weken is sinds de service heeft ontvangen van de back-ups van gegevens, maar het pakket nog niet in het datacenter ontvangen is.|
|`Transferring`|Een station wordt verplaatst naar de `Transferring` status wanneer begint de service voor gegevensoverdracht van het station naar Windows Azure Storage.|
|`Completed`|Een station wordt verplaatst naar de `Completed` status wanneer de service heeft de gegevens zonder fouten is overgedragen.|
|`CompletedMoreInfo`|Een station wordt verplaatst naar de `CompletedMoreInfo` status wanneer de service enkele problemen is opgetreden tijdens het kopiëren van gegevens van of naar het station. De informatie kan bestaan fouten, waarschuwingen of informatieve berichten over het overschrijven van blobs.|
|`ShippedBack`|Het station wordt verplaatst naar de `ShippedBack` status wanneer deze is verzonden vanaf de achterkant van data center adres van de afzender.|

De volgende tabel beschrijft de statussen van de fout station en de acties die voor elke status.

|Status van station|Gebeurtenis|Resolutie / de volgende stap|
|-----------------|-----------|-----------------------------|
|`NeverReceived`|Een station dat is gemarkeerd als `NeverReceived` (omdat deze niet is ontvangen als onderdeel van de verzending van de taak) in een andere verzending binnenkomt.|Het operationele team verplaatst het station om de `Received` staat.|
|`N/A`|Een station dat geen deel uitmaakt van elke taak komt in het datacenter als onderdeel van een andere taak.|Het station wordt gemarkeerd als een extra schijf en naar de klant wordt geretourneerd wanneer de taak die is gekoppeld aan het oorspronkelijke pakket is voltooid.|

## <a name="faulted-states"></a>Fout statussen
Als een taak of het station niet normaal voortgang van de verwachte levensduur, de taak of het station worden verplaatst naar een `Faulted` status. Het operationele team wordt op dat moment contact op met de klant door e-mailadres of telefoonnummer. Als het probleem opgelost is, de mislukte taak of het station worden uitgevoerd buiten de `Faulted` systeemstatus- en verplaatst naar de juiste status.

## <a name="next-steps"></a>Volgende stappen

* [Met behulp van de Import/Export-service REST-API](storage-import-export-using-the-rest-api.md)
