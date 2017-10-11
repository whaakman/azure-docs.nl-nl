---
title: Werken met gegevensbronnen 'big data' | Microsoft Docs
description: Hoe kan ik artikel patronen voor het gebruik van Azure Data Catalog met 'big data' gegevensbronnen, waaronder Azure Blob Storage, Azure Data Lake en Hadoop HDFS is gemarkeerd.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 001d80ce42f0e87276e59d70dffb75eb561d96cd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Werken met big gegevensbronnen in Azure Data Catalog
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van detectie voor zakelijke gegevensbronnen fungeert. Het is alle over mensen helpen detecteren, begrijpen en gebruiken van gegevensbronnen en helpt organisaties meer waarde ophalen van hun bestaande gegevensbronnen, met inbegrip van big data.

**Azure Data Catalog** biedt ondersteuning voor de registratie van Azure Blog van Storage-blobs en mappen alsook Hadoop HDFS-bestanden en mappen. De semi-gestructureerde aard van deze gegevensbronnen biedt hoge mate van flexibiliteit. Echter, naar de meest waarde niet ophalen uit het registreren van deze met **Azure Data Catalog**, gebruikers moeten rekening houden met de rangschikking van de gegevensbronnen.

## <a name="directories-as-logical-data-sets"></a>Mappen als logische gegevenssets
Een algemene patroon voor het ordenen van grote gegevensbronnen is mappen behandelen als logische gegevenssets. Op het hoogste niveau mappen worden gebruikt voor het definiëren van een gegevensset terwijl submappen Definieer partities en de bestanden die ze bevatten de gegevens zelf worden opgeslagen.

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

In dit voorbeeld vertegenwoordigen vehicle_maintenance_events en location_tracking_events logische gegevenssets. Elk van deze mappen bevat gegevensbestanden die zijn ingedeeld op jaar en maand in submappen. Elk van deze mappen zich mogelijk honderden of duizenden bestanden.

In dit patroon registratie van afzonderlijke bestanden met **Azure Data Catalog** waarschijnlijk niet logisch. In plaats daarvan de mappen met daarin de gegevenssets die relevant zijn voor de gebruikers die werken met de gegevens worden geregistreerd.

## <a name="reference-data-files"></a>Verwijzing naar gegevensbestanden
Er is een aanvullende patroon voor het opslaan van sets van verwijzingsgegevens als afzonderlijke bestanden. Deze gegevenssets kunnen worden beschouwd als de 'kleine'-zijde van big data en zijn vaak net als bij dimensies in een model analytische gegevens. Referentie-gegevensbestanden bevat records die worden gebruikt voor context voor het merendeel van de bestanden elders in de big data-archief opgeslagen.

Een voorbeeld van dit patroon is mogelijk:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Wanneer een analist of gegevens wetenschappelijk samen met de gegevens in de grotere directory-structuren werkt, kan de gegevens in deze verwijzing bestanden worden gebruikt om meer gedetailleerde informatie voor entiteiten die alleen door de naam of ID worden aangeduid in de grotere gegevensset te geven.

In dit patroon is het verstandig om te registreren van de gegevensbestanden van de afzonderlijke verwijzing met **Azure Data Catalog**. Elk bestand vertegenwoordigt een set gegevens en elkaar kan worden aangetekend en afzonderlijk gedetecteerd.

## <a name="alternate-patterns"></a>Alternatieve patronen
De patronen die zijn beschreven in de vorige sectie zijn twee manieren die een big data store kan worden georganiseerd, maar elke implementatie verschilt. Ongeacht hoe uw gegevensbronnen zijn gestructureerd, bij het registreren van gegevensbronnen met big **Azure Data Catalog**, ligt de nadruk op het registreren van de bestanden en mappen die vertegenwoordigen de gegevenssets van waarde naar anderen binnen uw de organisatie. Registratie van alle bestanden en mappen kan bruikbaar blijft de catalogus, waardoor het moeilijker voor gebruikers kunnen vinden die ze nodig hebben.

## <a name="summary"></a>Samenvatting
Registreren van gegevensbronnen met **Azure Data Catalog** zorgt ervoor dat ze eenvoudiger te detecteren en begrijpen. Door te registreren en aantekeningen maken van de big data-bestanden en mappen die logische gegevenssets vertegenwoordigen, kunt u gebruikers zoeken en gebruiken van de grote gegevensbronnen die ze nodig hebben.
