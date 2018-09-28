---
title: Het gebruik van gegevens-gegevensbronnen in Azure Data Catalog-profilering
description: Hoe kan ik artikel waarbij het opnemen van profielen voor tabel - en kolomniveau gegevens bij het registreren van gegevensbronnen in Azure Data Catalog en gegevens profielen gebruiken om te begrijpen van gegevensbronnen zijn gemarkeerd.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: b40bbe7d1eb89f934d6ae07688e698f1949f8e76
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410235"
---
# <a name="data-profile-data-sources"></a>Gegevensbronnen met gegevensprofielen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als registratiesysteem en detectiesysteem voor zakelijke gegevensbronnen fungeert. Met andere woorden, **Azure Data Catalog** draait helpt mensen detecteren, begrijpen en gebruiken van gegevensbronnen en waarmee organisaties meer waarde halen uit hun bestaande gegevens. Wanneer een gegevensbron is geregistreerd bij **Azure Data Catalog**, de metagegevens wordt gekopieerd en door de service geïndexeerd, maar het verhaal er niet beëindigen.

De **gegevens profilering** functie van **Azure Data Catalog** onderzoekt de gegevens van ondersteunde gegevensbronnen in uw catalogus en statistieken en informatie over die gegevens worden verzameld. Het is eenvoudig om op te nemen van een profiel van uw gegevensassets. Wanneer u zich registreert voor een gegevensasset, kiest u **Gegevensprofiel opnemen** in het hulpprogramma voor registratie.

## <a name="what-is-data-profiling"></a>Wat is Data profilering
Gegevens profilering onderzoekt de gegevens in de gegevensbron wordt geregistreerd en statistieken en informatie over die gegevens worden verzameld. Tijdens de detectie van gegevensbronnen kunt deze gegevens u de geschiktheid van de gegevens voor het oplossen van het zakelijke probleem te bepalen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

De volgende gegevensbronnen bieden ondersteuning voor het profileren van gegevens:

* SQL Server (met inbegrip van Azure SQL DB en Azure SQL Data Warehouse)-tabellen en weergaven
* Oracle-tabellen en weergaven
* Teradata-tabellen en weergaven
* Hive-tabellen

Inclusief gegevens profielen bij het registreren van gegevensassets kan gebruikers beantwoorden van vragen over gegevensbronnen, waaronder:

* Kan worden gebruikt voor het oplossen van mijn zakelijke probleem?
* De gegevens voldoen aan bepaalde standaarden of patronen?
* Wat zijn enkele van de afwijkingen van de gegevensbron?
* Wat zijn mogelijk kunnen maken van deze gegevens integreren in mijn toepassing?

> [!NOTE]
> U kunt ook documentatie toevoegen aan een asset om te beschrijven hoe de gegevens in een toepassing kan worden geïntegreerd. Zie [gegevensbronnen documenteren](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Een gegevensprofiel opnemen tijdens het registreren van een gegevensbron
Het is eenvoudig om op te nemen van een profiel van de gegevensbron. Wanneer u een gegevensbron registreren in de **te registreren objecten** deelvenster van het registratiehulpprogramma voor gegevensbronnen, kiest u **Gegevensprofiel opnemen**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Zie voor meer informatie over het registreren van gegevensbronnen, [informatie over het registreren van gegevensbronnen](data-catalog-how-to-register.md) en [aan de slag met Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filteren op de gegevensassets die gegevens profielen bevatten
Voor het detecteren van gegevensassets die een gegevensprofiel opnemen, u kunt opnemen `has:tableDataProfiles` of `has:columnsDataProfiles` als een van uw zoektermen.

> [!NOTE]
> Selecteren **Gegevensprofiel opnemen** in de gegevensbron bevat hulpprogramma voor registratie tabel en profielgegevens op kolomniveau. De API van Data Catalog kan echter gegevensassets worden geregistreerd met slechts één set profielgegevens die zijn opgenomen.
>
>

## <a name="viewing-data-profile-information"></a>Gegevens-profielgegevens weergeven
Als u een geschikte gegevensbron met een profiel hebt gevonden, kunt u de details van gegevens kunt weergeven. De als gegevensprofiel wilt bekijken, selecteert u een gegevensasset en kiest **Gegevensprofiel** in het venster van Data Catalog-portal.

![](media/data-catalog-data-profile/data-catalog-view.png)

Een gegevensprofiel in **Azure Data Catalog** toont tabel en kolom profiel informatie zoals:

### <a name="object-data-profile"></a>Gegevensprofiel object
* Aantal rijen
* Tabelgrootte
* Wanneer het object voor het laatst is bijgewerkt

### <a name="column-data-profile"></a>Kolomprofiel gegevens
* Gegevenstype van kolom
* Aantal afzonderlijke waarden
* Aantal rijen met NULL-waarden
* Minimum, maximum, gemiddelde en standaarddeviatie voor kolomwaarden

## <a name="summary"></a>Samenvatting
Gegevens profilering bevat statistische gegevens en informatie over geregistreerde gegevensassets om te bepalen van de geschiktheid van de gegevens voor het oplossen van bedrijfsproblemen. Samen met het toevoegen van aantekeningen en documenteren van gegevensbronnen, kunnen gegevens profielen gebruikers geven een beter begrip van uw gegevens.

## <a name="see-also"></a>Zie ook
* [Gegevensbronnen registreren](data-catalog-how-to-register.md)
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md)
