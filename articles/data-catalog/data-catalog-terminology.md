---
title: Terminologie voor Azure Data Catalog | Microsoft Docs
description: Dit artikel bevat een inleiding tot de concepten en termen die worden gebruikt in de documentatie van Azure Data Catalog.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 557b529f45c7fbc286b7e1893d4b4688e088ed91
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-terminology"></a>Terminologie voor Azure Data Catalog
## <a name="catalog"></a>Catalogus
De Azure Data Catalog is een cloud-gebaseerde metagegevens-opslagplaats in welke gegevens activa gegevensbronnen en gegevens kunnen worden geregistreerd. De catalogus fungeert als een centrale opslaglocatie voor de structurele metagegevens die zijn geëxtraheerd uit gegevensbronnen en beschrijvende metagegevens die zijn toegevoegd door gebruikers.

## <a name="data-source"></a>Gegevensbron
Een gegevensbron is een systeem of de container die gegevensassets beheert. Voorbeelden zijn SQL Server-databases, Oracle-databases, SQL Server Analysis Services-databases (in tabelvorm of multidimensionale) en SQL Server Reporting Services-servers.

## <a name="data-asset"></a>Gegevensasset
Gegevensassets zijn opgenomen in de gegevensbronnen die kunnen worden geregistreerd met de catalogus-objecten. Voorbeelden zijn SQL Server-tabellen en weergaven, Oracle-tabellen en weergaven, SQL Server Analysis Services metingen, dimensies en KPI's en rapporten van SQL Server Reporting Services.

## <a name="data-asset-location"></a>Locatie van de asset
De catalogus slaat de locatie van een gegevensbron of een gegevensasset, verbinding maken met de bron via een clienttoepassing kan worden gebruikt. De indeling en de details van de locatie variëren, afhankelijk van het gegevensbrontype. Een tabel met SQL Server kan bijvoorbeeld worden geïdentificeerd door de vierdelige naam – servernaam, databasenaam, de naam van schema, objectnaam – terwijl een SQL Server Reporting Services-rapport kan worden geïdentificeerd door de URL.

## <a name="structural-metadata"></a>Structurele metagegevens
Structurele metagegevens zijn de metagegevens die zijn opgehaald uit een gegevensbron die de structuur van een gegevensasset beschrijft. Dit omvat de locatie van de activa, de objectnaam en type en aanvullende typespecifieke kenmerken. De structurele metagegevens voor tabellen en weergaven bevat bijvoorbeeld de namen en gegevenstypen voor kolommen van het object.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens
Beschrijvende metagegevens zijn metagegevens die het doel of het doel van een gegevensasset beschrijft. Doorgaans beschrijvende metagegevens wordt toegevoegd door gebruikers van de catalogus met behulp van de Azure Data Catalog-portal, maar deze kan ook worden geëxtraheerd uit de gegevensbron tijdens de registratie. Bijvoorbeeld, het hulpprogramma Azure Data Catalog registratie beschrijvingen extraheren uit de eigenschap Description in SQL Server Analysis Services en SQL Server Reporting Services en vanaf de [ms_description uitgebreide eigenschap](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-databases, als deze eigenschappen zijn ingevuld met waarden.

## <a name="request-access"></a>Toegang aanvragen
Een gegevensasset beschrijvende metagegevens kan informatie over het aanvragen van toegang tot de gegevensasset of gegevensbron bevatten. Deze informatie wordt weergegeven met de locatie van asset en kan bevatten een of meer van de volgende opties:

* Het e-mailadres van de gebruiker of het team dat verantwoordelijk is voor het verlenen van toegang tot de gegevensbron.
* De URL van het gedocumenteerde proces dat gebruikers toegang krijgen tot de gegevensbron moeten volgen.
* De URL van een programma identiteit en toegang beheren (zoals Microsoft Identity Manager) die kan worden gebruikt voor toegang tot de gegevensbron.
* Een item met vrije tekst waarin wordt beschreven hoe gebruikers toegang kunnen krijgen tot de gegevensbron.

## <a name="preview"></a>Preview
Een voorbeeld van in Azure Data Catalog is een momentopname van maximaal 20 records die kunnen worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens van de asset gegevens. De preview kunt gebruikers die een gegevensasset detecteren beter begrip van de functie en het doel. Met andere woorden, ziet voorbeeldgegevens mag meer waard dan zien alleen de kolomnamen en gegevenstypen.
Voorbeelden worden alleen ondersteund voor tabellen en weergaven en moeten worden expliciet is geselecteerd door de gebruiker tijdens de registratie.

## <a name="data-profile"></a>Data-profiel
Een profiel van de gegevens in Azure Data Catalog is een momentopname van de tabel op gebruikersniveau en op kolomniveau metagegevens over een geregistreerde gegevensasset die kan worden opgehaald uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met de metagegevens van de asset gegevens. Het profiel van de gegevens kunt gebruikers die een gegevensasset detecteren beter begrip van de functie en het doel. Net als voorbeelden, profielen van de gegevens moeten worden expliciet is geselecteerd door de gebruiker tijdens de registratie.

> [!NOTE]
> Het uitpakken van een profiel van de gegevens kan kostbaar voor grote tabellen en weergaven, en kan de benodigde tijd voor een gegevensbron registreert aanzienlijk verhogen.
>
>

## <a name="user-perspective"></a>Gebruikersperspectief
In Azure Data Catalog, kan elke gebruiker beschrijvende metagegevens leveren voor een geregistreerde gegevensasset. Elke gebruiker heeft een afzonderlijke perspectief op de gegevens en het gebruik ervan. De beheerder die verantwoordelijk is voor een server kan bijvoorbeeld Geef details van de serviceovereenkomst (SLA) of de back-upvensters; een wereldburgers gegevens kan bepalen dat koppelingen naar documentatie voor het bedrijf verwerkt de gegevens worden ondersteund; en een analist kan Geef een beschrijving in de termen die meest relevant zijn voor andere analisten en wat het nuttigst voor gebruikers die u wilt detecteren en begrijpen van de gegevens kan zijn.

Elk van deze perspectieven zijn inherent waardevol en met Azure Data Catalog, krijgt elke gebruiker de informatie die relevant zijn, terwijl alle gebruikers kunnen deze informatie gebruiken om de gegevens en het doel te begrijpen.

## <a name="expert"></a>deskundige
Een expert is een gebruiker die is geïdentificeerd als een gefundeerde 'deskundige' perspectief voor een gegevensasset hebben. Elke gebruiker kan zichzelf of een andere gebruiker toevoegen als een expert voor een asset. Wordt vermeld als een expert extra bevoegdheden in Azure Data Catalog; niet overbrengen Hiermee kunt u gebruikers om gemakkelijk te vinden die perspectieven die waarschijnlijk nuttig zijn bij het controleren van de beschrijvende metagegevens van een actief zijn.

## <a name="owner"></a>Eigenaar
Een eigenaar is een gebruiker met aanvullende bevoegdheden voor het beheren van een gegevensasset in Azure Data Catalog. Gebruikers kunnen eigenaar worden van geregistreerde gegevensassets en eigenaren van andere gebruikers kunnen toevoegen als mede-eigenaren. Zie voor meer informatie [gegevensassets beheren](data-catalog-how-to-manage.md)  

> [!NOTE]
> Eigendom en het beheer zijn alleen beschikbaar in de Standard-editie van Azure Data Catalog.
>
>

## <a name="registration"></a>Registratie
Registratie is de handeling gegevens asset metagegevens extraheren uit een gegevensbron en kopiëren naar de service Azure Data Catalog. Gegevensassets die zijn geregistreerd, kunnen vervolgens aangetekend en gedetecteerd.

## <a name="see-also"></a>Zie ook
* [Wat is Azure Data Catalog?](data-catalog-what-is-data-catalog.md) -Dit artikel bevat een overzicht van de service Azure Data Catalog, de meerwaarde is en de scenario's ondersteund.
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) -in dit artikel biedt een end-to-end zelfstudie waarin u het gebruik van Azure Data Catalog voor detectie van gegevensbronnen.  
