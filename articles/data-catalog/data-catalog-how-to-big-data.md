---
title: Over het werken met gegevensbronnen van 'big data' in Azure Data Catalog
description: Patronen voor het gebruik van Azure Data Catalog met 'big data'-gegevensbronnen, waaronder Azure Blob Storage, Azure Data Lake en Hadoop HDFS markeren artikel met instructies.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: d59b637d1c5ad2335492883503e738a80071a8dd
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053577"
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Over het werken met grote gegevensbronnen in Azure Data Catalog
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als registratiesysteem en detectiesysteem voor zakelijke gegevensbronnen fungeert. Het is alle informatie over mensen voor hulp bij het detecteren, begrijpen en met gegevensbronnen en helpt organisaties kunt u meer waarde halen uit hun bestaande gegevensbronnen, met inbegrip van big data.

**Azure Data Catalog** biedt ondersteuning voor de registratie van Azure BLOB-opslag-blobs en mappen, evenals Hadoop HDFS-bestanden en mappen. De semi-gestructureerde aard van deze gegevensbronnen biedt hoge mate van flexibiliteit. Echter, om de meeste waarde van het registreren met **Azure Data Catalog**, gebruikers rekening moeten houden met hoe de gegevensbronnen zijn ingedeeld.

## <a name="directories-as-logical-data-sets"></a>Mappen als logische gegevenssets
Er is een algemeen patroon voor het ordenen van grote gegevensbronnen mappen behandelen als logische gegevenssets. Op het hoogste niveau mappen worden gebruikt voor het definiëren van een verzameling, terwijl submappen Definieer partities en de bestanden die ze bevatten de gegevens zelf.

Een voorbeeld van dit patroon is mogelijk:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

In dit voorbeeld vertegenwoordigen vehicle_maintenance_events en location_tracking_events logische gegevenssets. Elk van deze mappen bevat bestanden die zijn ingedeeld op jaar en maand in submappen. Elk van deze mappen kan mogelijk honderden of duizenden bestanden bevat.

In dit patroon registreren van afzonderlijke bestanden met **Azure Data Catalog** waarschijnlijk niet logisch. In plaats daarvan de mappen die staan voor de gegevenssets die zinvol is voor de gebruikers die werken met de gegevens worden geregistreerd.

## <a name="reference-data-files"></a>Referentie-gegevensbestanden
Een aanvullende patroon is voor het opslaan van sets van verwijzingsgegevens als afzonderlijke bestanden. Deze gegevenssets kunnen worden beschouwd als de 'kleine'-zijde van big data, en zijn vaak net als bij dimensies in een model analytische gegevens. Referentie-gegevensbestanden bevat records die worden gebruikt voor de context van het grootste deel van de bestanden elders wordt opgeslagen in het big data-archief.

Een voorbeeld van dit patroon is mogelijk:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Wanneer een analist of gegevens gegevenswetenschappers van elk werkt met de gegevens in de grotere directory-structuren samen, kan de gegevens in deze verwijzen naar bestanden kunnen worden gebruikt om meer gedetailleerde informatie voor entiteiten die alleen op naam of ID worden aangeduid in de grotere gegevensset te geven.

In dit patroon is het zinvol om u te registreren van de gegevensbestanden van de afzonderlijke verwijzing met **Azure Data Catalog**. Elk bestand vertegenwoordigt een set gegevens en elkaar kan worden van aantekeningen voorzien en afzonderlijk gedetecteerd.

## <a name="alternate-patterns"></a>Alternatieve patronen
De patronen die zijn beschreven in de vorige sectie zijn slechts twee manieren die een big data-archief kan worden georganiseerd, maar elke implementatie verschilt. Ongeacht hoe uw gegevensbronnen zijn gestructureerd, bij het registreren van big data-bronnen met **Azure Data Catalog**, richt u op het registreren van de bestanden en mappen die staan voor de gegevenssets die zijn van de waarde met anderen binnen uw de organisatie. Registreren van alle bestanden en mappen kunt bruikbaar blijft de catalogus, waardoor het moeilijker voor gebruikers te weten wat ze nodig hebben.

## <a name="summary"></a>Samenvatting
Registreren van gegevensbronnen met **Azure Data Catalog** kunt u ze eenvoudiger te detecteren en begrijpen. Door te registreren en annoteren van de big data-bestanden en mappen die logische gegevenssets vertegenwoordigen, kunt u gebruikers vinden en gebruiken van de big data-bronnen die ze nodig hebben.
