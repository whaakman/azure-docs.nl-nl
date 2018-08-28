---
title: Azure Data Catalog-terminologie
description: In dit artikel bevat een inleiding tot de concepten en termen die worden gebruikt in de documentatie van Azure Data Catalog.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 70772ae07c4a8a6e87b4fa6f119acf2d51a5c23e
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053630"
---
# <a name="azure-data-catalog-terminology"></a>Azure Data Catalog-terminologie
## <a name="catalog"></a>Catalogus
De Azure Data Catalog is een cloud-gebaseerde metagegevens-opslagplaats in welke gegevens activa bronnen en gegevens kunnen worden geregistreerd. De catalogus fungeert als een centrale opslaglocatie voor de structurele metagegevens die zijn geëxtraheerd uit gegevensbronnen en beschrijvende metagegevens die zijn toegevoegd door gebruikers.

## <a name="data-source"></a>Gegevensbron
Een gegevensbron is een systeem of de container die wordt beheerd gegevensassets. Voorbeelden zijn onder meer SQL Server-databases, Oracle-databases, SQL Server Analysis Services-databases (in tabelvorm of multidimensionaal) en SQL Server Reporting Services-servers.

## <a name="data-asset"></a>Gegevensasset
Gegevensassets zijn opgenomen in de gegevensbronnen die kunnen worden geregistreerd met de catalog-objecten. Voorbeelden zijn onder meer SQL Server-tabellen en weergaven, Oracle-tabellen en weergaven, SQL Server Analysis Services metingen, dimensies en KPI's en rapporten van SQL Server Reporting Services.

## <a name="data-asset-location"></a>Asset-gegevenslocatie
De catalogus slaat de locatie van een gegevensbron of een gegevensasset, die kan worden gebruikt om verbinding maken met de gegevensbron met behulp van een clienttoepassing. De indeling en de details van de locatie variëren op basis van het gegevensbrontype. Een SQL Server-tabel kan bijvoorbeeld worden geïdentificeerd door de naam van de vier delen bestaande: servernaam, databasenaam, de naam van schema, objectnaam – terwijl een SQL Server Reporting Services-rapport kan worden geïdentificeerd door de URL.

## <a name="structural-metadata"></a>Structurele metagegevens
Structurele metagegevens zijn de metagegevens die zijn geëxtraheerd uit een gegevensbron die de structuur van een gegevensasset beschrijft. Dit omvat de locatie van de activa, de objectnaam en type en aanvullende typespecifieke kenmerken. De structurele metagegevens voor tabellen en weergaven bevat bijvoorbeeld de namen en gegevenstypen voor kolommen van het object.

## <a name="descriptive-metadata"></a>Beschrijvende metagegevens
Beschrijvende metagegevens is metagegevens met een beschrijving van het doel of het doel van een gegevensasset. Normaal gesproken beschrijvende metagegevens wordt toegevoegd door gebruikers van de catalogus met behulp van de Azure Data Catalog-portal, maar deze kan ook worden geëxtraheerd uit de gegevensbron tijdens de registratie. Bijvoorbeeld, het hulpprogramma voor registratie van Azure Data Catalog beschrijvingen extraheren uit de eigenschap Description in SQL Server Analysis Services en SQL Server Reporting Services, en van de [ms_description uitgebreide eigenschap](https://technet.microsoft.com/library/ms190243.aspx) in SQL Server-databases, als deze eigenschappen zijn ingevuld met waarden.

## <a name="request-access"></a>Toegang aanvragen
Beschrijvende metagegevens die een gegevensasset kan informatie over het aanvragen van toegang tot de gegevensasset of gegevensbron bevatten. Deze informatie wordt weergegeven met de locatie van de asset gegevens en kan bevatten een of meer van de volgende opties:

* Het e-mailadres van de gebruiker of team dat verantwoordelijk is voor het verlenen van toegang tot de gegevensbron.
* De URL van het beschreven proces dat gebruikers volgen moeten om toegang krijgen tot de gegevensbron.
* De URL van een identiteits- en toegangsbeheer management tool (zoals Microsoft Identity Manager) die kan worden gebruikt voor toegang tot de gegevensbron.
* Een vermelding voor vrije tekst waarin wordt beschreven hoe gebruikers kunnen toegang krijgen tot de gegevensbron.

## <a name="preview"></a>Preview
Een Preview-versie in Azure Data Catalog is een momentopname van maximaal 20 records die kunnen worden geëxtraheerd uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met metagegevens van de gegevens asset. De Preview-versie kunt u gebruikers die een gegevensasset te detecteren beter begrip van de functie en het doel. Met andere woorden, ziet voorbeeldgegevens mag waardevoller dan zien alleen de kolomnamen en gegevenstypen.
Previews worden alleen ondersteund voor tabellen en weergaven en moeten worden expliciet geselecteerd door de gebruiker tijdens de registratie.

## <a name="data-profile"></a>Gegevensprofiel
Een gegevensprofiel in Azure Data Catalog is een momentopname van de tabel- en kolomniveau metagegevens over een geregistreerde gegevensasset die kan worden geëxtraheerd uit de gegevensbron tijdens de registratie en opgeslagen in de catalogus met metagegevens van de gegevens asset. Het gegevensprofiel kunt gebruikers die een gegevensasset te detecteren beter begrip van de functie en het doel. Net als bij de Preview-versies, profielen van gegevens moeten worden expliciet geselecteerd door de gebruiker tijdens de registratie.

> [!NOTE]
> Een gegevensprofiel ophalen kan een dure bewerking voor grote tabellen en weergaven, en verhoogt de tijd die nodig is voor het registreren van een gegevensbron mogelijk aanzienlijk.
>
>

## <a name="user-perspective"></a>Perspectief van gebruiker
In Azure Data Catalog kan elke gebruiker beschrijvende metagegevens leveren voor een geregistreerde gegevensasset. Elke gebruiker heeft een unieke perspectief op de gegevens en het gebruik ervan. Bijvoorbeeld de details van de serviceovereenkomst (SLA) of de back-upvensters; door de beheerder die verantwoordelijk is voor een server kan worden opgeven een steward gegevens kan bepalen dat koppelingen naar documentatie voor het bedrijf verwerkt de gegevens worden ondersteund; en een analist kan Geef een beschrijving in de voorwaarden die zijn het meest relevant zijn voor andere analisten en waarmee het nuttigst voor gebruikers die u nodig hebt om te detecteren en begrijpen van de gegevens kunnen worden.

Elk van deze perspectieven zijn inherent waardevolle en met Azure Data Catalog kan elke gebruiker de informatie die zinvol is, terwijl alle gebruikers kunnen deze informatie gebruiken om te begrijpen van de gegevens en het doel leveren.

## <a name="expert"></a>Expert
Een expert is een gebruiker die is geïdentificeerd als een weloverwogen 'deskundige' perspectief voor een gegevensasset. Elke gebruiker kan zelf of een andere gebruiker toevoegen als een expert voor een asset. Wordt vermeld als een expert geen extra bevoegdheden in Azure Data Catalog; niet weergeeft Deze kan gebruikers eenvoudig vinden die perspectieven die waarschijnlijk om nog nuttig zijn bij het beoordelen van de beschrijvende metagegevens van een asset.

## <a name="owner"></a>Eigenaar
Een eigenaar is een gebruiker die extra bevoegdheden heeft voor het beheren van een gegevensasset in Azure Data Catalog. Gebruikers eigenaar worden van geregistreerde gegevensassets en eigenaren van andere gebruikers kunnen toevoegen als mede-eigenaren. Zie voor meer informatie [gegevensassets beheren](data-catalog-how-to-manage.md)  

> [!NOTE]
> Eigendom en het beheer zijn alleen beschikbaar in de Standard-editie van Azure Data Catalog.
>
>

## <a name="registration"></a>Registratie
De registratie is de handeling van het extraheren van metagegevens van de asset gegevens uit een gegevensbron en kopieert naar de service Azure Data Catalog. Gegevensassets die zijn geregistreerd, kunnen vervolgens van aantekeningen voorzien en gedetecteerd.

## <a name="see-also"></a>Zie ook
* [Wat is Azure Data Catalog?](data-catalog-what-is-data-catalog.md) -In dit artikel biedt een overzicht van de service Azure Data Catalog, de waarde die het biedt en de scenario's ondersteund.
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) -in dit artikel biedt een end-to-end zelfstudie waarin wordt uitgelegd hoe u met Azure Data Catalog voor detectie van gegevensbronnen.  
