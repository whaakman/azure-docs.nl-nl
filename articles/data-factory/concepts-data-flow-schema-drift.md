---
title: Azure Data Factory gegevensstroom Schema Drift toewijzen
description: Bouw robuuste gegevens stromen in Azure Data Factory met Schema-afwijking
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 5a3d817f96770370615ff4ae953cb8655bd245c1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56329044"
---
# <a name="mapping-data-flow-schema-drift"></a>Toewijzing van gegevensstroom Schema afwijking

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Het concept van Schema Drift is het geval waarin uw bronnen vaak metagegevens wijzigen. Velden, kolommen, typen, enzovoort kunnen worden toegevoegd, verwijderd of gewijzigd op elk gewenst moment. Zonder verwerking voor Schema Drift wordt uw gegevensstroom kwetsbaar voor wijzigingen in de upstream-gegevensbron zijn gewijzigd. Als binnenkomende velden en kolommen wijzigt, mislukken typische ETL-patronen omdat ze vaak worden gekoppeld aan de namen van deze gegevensbronnen.

Als u wilt beveiligen tegen Schema Drift, is het belangrijk dat u hebt de faciliteiten in een programma gegevens stromen kunt u, als een technicus van gegevens tot:

* Bronnen die u hebt de veranderlijke veldnamen, gegevenstypen, waarden en grootten definiëren
* Definieer transformatieparameters die met gegevenspatronen in plaats van de vastgelegde velden en waarden werken kunnen
* Expressies die padpatronen binnenkomende velden begrijpen, in plaats van met de naam velden definiëren

In Azure Data Factory gegevensstroom, worden die faciliteiten opgehaald via deze werkstroom:

* Kies 'Schema Drift toestaan' in uw bron-transformatie

<img src="media/data-flow/schemadrift001.png" width="400">

* Wanneer u deze optie hebt geselecteerd, worden alle binnenkomende velden worden gelezen vanuit de bron van elke uitvoering van de stroom van gegevens en worden doorgegeven via de volledige stroom voor de Sink.

* Zorg ervoor dat u 'Auto-kaart' om toe te wijzen alle nieuwe velden in de Sink-transformatie zodat alle nieuwe velden opgenomen-up en bevindt zich in de bestemming:

<img src="media/data-flow/automap.png" width="400">

* Alles werkt wanneer nieuwe velden zijn geïntroduceerd in dit scenario met een eenvoudige bron Sink -> (ook wel kopiëren) toewijzen.

* Om toe te voegen transformaties in een werkstroom die verantwoordelijk is voor schema drift, kunt u jokertekens zodat deze overeenkomen met kolommen op naam, type en de waarde.

* Klik op 'Toevoegen kolom patroon' in het afgeleide kolom- of statistische transformatie als u wilt maken van een transformatie die werkt met 'Schema Drift'.

<img src="media/data-flow/columnpattern.png" width="400">

> [!NOTE]
> U moet treffen architectuurmatige beslissingen in uw gegevensstroom schema drift in uw stroom accepteren. Als u dit doet, kunt u beveiligen op basis van wijzigingen in het schema van de bronnen. Echter, verliest u vroege-binding van de kolommen en typen in de gegevensstroom. Azure Data Factory schema drift stromen behandelt als late-binding stromen, zodat wanneer u uw transformaties bouwt, de kolomnamen niet beschikbaar zijn in de schema-weergaven in de hele stroom.

<img src="media/data-flow/taxidrift1.png" width="400">

In het voorbeeld Taxi Demo gegevensstroom is er een voorbeeld Schema Drift in de gegevensstroom onder aan de bron TripFare. U ziet dat we het ontwerp van de 'kolom patroon' voor de aggregatie-velden gebruiken in de cumulatieve transformatie. In plaats van specifieke kolommen naamgeving, of op zoek naar kolommen op positie, gaan we ervan uit dat de gegevens wijzigen kunt en kan niet worden weergegeven in dezelfde volgorde wordt uitgevoerd.

In dit voorbeeld van Azure Data Factory-gegevensstroom schema drift verwerken, we hebben gebouwd en aggregatie die scant voor kolommen van het type 'dubbele', te weten dat het domein van de gegevens is dat prijzen voor elke fietstocht bevat. We kunnen vervolgens de berekening van een statistische berekening uitvoeren voor alle dubbele velden in de bron, ongeacht waar de kolom terechtkomt en ongeacht de naam van de kolom.

De syntaxis van de gegevensstroom van Azure Data Factory maakt gebruik van $ voor elke overeenkomende kolom uit de overeenkomende patroon. U kunt ook vergelijken op de namen van kolommen met complexe tekenreeks zoeken en functies van de reguliere expressie. In dit geval gaan we een nieuwe geaggregeerd veldnaam op basis van elke overeenkomst van een 'dubbele' type van kolom maken en toevoegen van de tekst ```_total``` gekoppeld aan elk van deze namen: 

```concat($$, '_total')```

Vervolgens wordt afgerond en som van de waarden voor elk van deze overeenkomende kolommen:

```round(sum ($$))```

U kunt dit allemaal met het voorbeeld Azure Data Factory-gegevensstroom "Taxi Demo" testen. Ga op de sessie voor foutopsporing met behulp van de wisselknop foutopsporing boven aan het ontwerpgebied gegevensstroom zodat u kunt uw resultaten interactief bekijken:

<img src="media/data-flow/taxidrift2.png" width="800">

