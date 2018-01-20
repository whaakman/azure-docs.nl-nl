---
title: Hoe gegevens profiel gegevensbronnen
description: Hoe kan ik artikel markeren het opnemen van profielen voor tabel - en op kolomniveau gegevens bij het registreren van gegevensbronnen in Azure Data Catalog en hoe gegevens profielen gebruiken om te begrijpen van gegevensbronnen.
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 80181b729ffa6d6cbc2d17fe8a5ae8ee4e3d41ab
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="data-profile-data-sources"></a>Gegevensbronnen met gegevensprofielen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van detectie voor zakelijke gegevensbronnen fungeert. Met andere woorden, **Azure Data Catalog** alles draait om medewerkers detecteren, begrijpen en gebruiken van gegevensbronnen helpen en organisaties Haal meer uit hun bestaande gegevens te helpen. Wanneer een gegevensbron is geregistreerd met **Azure Data Catalog**, de metagegevens wordt gekopieerd en geïndexeerd door de service, maar het artikel er eindigt niet.

De **gegevens profilering** functie van **Azure Data Catalog** onderzoekt de gegevens van ondersteunde gegevensbronnen in de catalogus en verzamelt statistische gegevens en informatie over die gegevens. Het is gemakkelijk om op te nemen van een profiel van de activa van uw gegevens. Wanneer u een gegevensasset registreert, kies **gegevens profiel opnemen** in het registratiehulpprogramma van gegevensbronnen.

## <a name="what-is-data-profiling"></a>Wat is profileren van gegevens
Profileren van gegevens onderzoekt de gegevens in de gegevensbron wordt geregistreerd en verzamelt statistische gegevens en informatie over die gegevens. Tijdens de detectie van gegevensbron kunt deze statistische gegevens u de geschiktheid van de gegevens voor het oplossen van het zakelijke probleem te bepalen.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

De volgende gegevensbronnen ondersteunen gegevens samenstellen van profiel:

* SQL Server (inclusief Azure SQL DB en Azure SQL Data Warehouse)-tabellen en weergaven
* Oracle-tabellen en weergaven
* Teradata-tabellen en weergaven
* Hive-tabellen

Inclusief gegevens profielen wanneer gebruikers voor het registreren van gegevensassets helpt beantwoorden vragen over gegevensbronnen, waaronder:

* Kan worden gebruikt voor het oplossen van mijn zakelijke probleem?
* Voldoet de gegevens aan bepaalde standaarden of patronen?
* Wat zijn enkele van de afwijkingen van de gegevensbron?
* Wat zijn mogelijk uitdagingen bij het integreren van deze gegevens in mijn toepassing?

> [!NOTE]
> U kunt ook documentatie toevoegen aan een asset te beschrijven hoe de gegevens in een toepassing kan worden geïntegreerd. Zie [gegevensbronnen documenteren](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Het opnemen van een profiel van de gegevens bij het registreren van een gegevensbron
Het is gemakkelijk om op te nemen van een profiel van de gegevensbron. Wanneer u een gegevensbron registreren in de **te registreren objecten** deelvenster van het hulpprogramma voor de registratie, de optie **gegevens profiel opnemen**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Zie voor meer informatie over het registreren van gegevensbronnen, [gegevensbronnen registreren](data-catalog-how-to-register.md) en [aan de slag met Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filteren op gegevensassets die gegevens profielen bevatten
U kunt opnemen om gegevensassets te detecteren die een profiel van de gegevens bevatten, `has:tableDataProfiles` of `has:columnsDataProfiles` als een van de zoektermen.

> [!NOTE]
> Selecteren **gegevens profiel opnemen** in de gegevensbron bevat registratiehulpprogramma tabel en profielgegevens op kolomniveau. De API van Data Catalog kunnen echter gegevensassets worden geregistreerd met slechts één set met profielgegevens opgenomen.
>
>

## <a name="viewing-data-profile-information"></a>Data-profielgegevens weergeven
Als u een geschikte gegevensbron aan een profiel hebt gevonden, kunt u de details van de gegevens kunt weergeven. Selecteer een gegevensasset profiel weer te geven gegevens, en kies **gegevens profiel** in het venster van Data Catalog-portal.

![](media/data-catalog-data-profile/data-catalog-view.png)

Een profiel van de gegevens in **Azure Data Catalog** toont tabel en de kolom profiel informatie zoals:

### <a name="object-data-profile"></a>Object gegevens profiel
* Aantal rijen
* De tabelgrootte van de
* Wanneer het object voor het laatst is bijgewerkt

### <a name="column-data-profile"></a>Kolom gegevens profiel
* Gegevenstype van kolom
* Aantal afzonderlijke waarden
* Aantal rijen met NULL-waarden
* Minimum, maximum, gemiddelde en standaarddeviatie voor kolomwaarden

## <a name="summary"></a>Samenvatting
Profileren van gegevens biedt statistieken en informatie over geregistreerde gegevensassets om te bepalen van de geschiktheid van de gegevens voor het oplossen van bedrijfsproblemen. Samen met aantekeningen maken en gegevensbronnen documenteren, kunnen profielen van de gegevens gebruikers geven een beter begrip van uw gegevens.

## <a name="see-also"></a>Zie ook
* [Gegevensbronnen registreren](data-catalog-how-to-register.md)
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md)
