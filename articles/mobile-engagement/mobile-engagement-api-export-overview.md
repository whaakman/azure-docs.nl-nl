---
title: Overzicht van Mobile Engagement Export-API
description: Leer de basisbeginselen van de onbewerkte gegevens die zijn gegenereerd door de apparaten van uw gebruikers gebruikmaken van deze in uw eigen's exporteren
services: mobile-engagement
documentationcenter: mobile
author: kpiteira
manager: erikre
editor: 
ms.assetid: 9380d47b-d7fa-4d4c-888f-97e6482196bb
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 04/26/2016
ms.author: kapiteir
ms.openlocfilehash: 346e0e480ff84ee849f135a7605d27df9e32f966
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="mobile-engagement-export-api-overview"></a>Overzicht van Mobile Engagement Export-API
## <a name="introduction"></a>Inleiding
In dit document leert u de basisbeginselen over het exporteren van de onbewerkte gegevens die zijn gegenereerd door de apparaten van uw gebruikers gebruikmaken van deze in uw eigen hulpmiddelen.

## <a name="pre-requisites"></a>Vereisten
De onbewerkte gegevens exporteren van Mobile Engagement vereist:

* API-verificatie-instellingen kunnen gebruikmaken van de API's (Zie [verificatie handmatige installatie](mobile-engagement-api-authentication-manual.md)),
* Ofwel de REST-API's gebruiken of de [.net SDK](mobile-engagement-dotnet-sdk-service-api.md),
* Een Azure Storage-account.

> [!NOTE]
> Ook wordt geadviseerd de uitstekende [Microsoft Azure Storage Explorer](http://storageexplorer.com/), ten minste tijdens de ontwikkelingsfase zoals deze voor interactie met Azure Storage biedt een gebruikersinterface te gebruiken.
> 
> 

## <a name="what-can-be-exported"></a>Wat kunnen worden geëxporteerd?
Mobile Engagement kunnen de gebruikers veel soorten gegevens verzamelen en daarom heeft verschillende typen van uitvoer die geschikt is voor deze verschillende gegevenstypen.
Er zijn 2 essentiële typen van uitvoer:

* Momentopname: gebruikt om gegevens te exporteren vertegenwoordigt die doorgaans een status en waarvoor de Mobile Engagement geen hebben een geschiedenis. Dit bevat bijvoorbeeld Tags (app-info)-tokens of push campagne feedback. Als gevolg hiervan dat deze typen export niet zijn gerelateerd aan een datum.
* historische: dit type uitvoer wordt gebruikt voor gegevens die zich bijvoorbeeld na verloop van tijd zoals gebeurtenissen of activiteiten.

De onderstaande tabel worden uitgebreid alle mogelijke exportbestanden beschreven:

| Type exporteren | Gegevenstype | Beschrijving |
| --- | --- | --- |
| Momentopname |Push |Exporteren van een van de feedback Push campagnes op basis van per apparaat-id/userid genereert |
| Momentopname |Label |Exporteren van een van de labels (app-info) die is gekoppeld aan elk apparaat wordt gegenereerd |
| Momentopname |Apparaat |Exporteren van een van de meeste gegevens over apparaten zoals de technicals (model, landinstellingen, tijdzone,...), de labels, gezien eerst genereert... |
| Momentopname |Token |Exporteren van een van de geldige tokens genereert |
| Historische |Activiteit |Genereert een export van de activiteiten voor elk apparaat op een opgegeven periode |
| Historische |Gebeurtenis |Genereert een export van de activiteiten voor elk apparaat op een opgegeven periode |
| Historische |Job |Genereert een exporteren van alle taken voor elk apparaat op een opgegeven periode |
| Historische |Fout |Genereert een exporteren met alle fouten voor elk apparaat op een opgegeven periode |

## <a name="how-does-it-work"></a>Hoe werkt het?
Uitvoer zijn lang actieve taken die kan leiden tot grote gegevensbestanden. Daarom moet ze niet aanroepen om terug te keren onmiddellijk een bestand te downloaden.
Om gegevens uit de Mobile Engagement exporteert, u moet maken van een **taak exporteren** via API waarin u in het algemeen opgeeft:

* Het type uitvoer (momentopname of historische)
* Het gegevenstype
* De **Azure Storage-Container** (met inbegrip van een geldige SAS met schrijftoegang) waar het resultaat van het exporteren worden geschreven.
* bijvoorbeeld voorbeeld Container URL-parameter zouden worden https://[StorageAccountName].blob.core.windows.net/[ContainerName]? [SASWritePermissionsToken]  

Hier volgt een voorbeeld van een echte wereld. https://testazmeexport.BLOB.Core.Windows.NET/test1234azme?SV=2015-12-11&SS=b&Srt=SCO&SP=rwdlac&se=2016-12-17T04:59:26Z & st = 2016-12-16T20:59:26Z & spr = https & sig = KRF3aVWjp2NEJDzjlmoplmu0M9HHlLdkBWRPAFmw90Q % 3D

Houd er rekening mee dat het enkele minuten voordat de taak duren kan moet worden gestart en vervolgens kan worden uitgevoerd vanuit een paar seconden voor kleine apps tot enkele uren voor apps met een groot aantal gebruikers- of -activiteit.

Zodra de taak is gemaakt, is het mogelijk om te controleren van de status ervan weer te geven als deze nog wordt uitgevoerd of als deze is voltooid.

Zodra de taak is voltooid, is het resulterende bestand is beschikbaar op de opgegeven storage-container.

