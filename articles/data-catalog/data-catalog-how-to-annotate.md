---
title: Aantekeningen toevoegen aan gegevensbronnen | Microsoft Docs
description: Hoe kan ik artikel aantekeningen toevoegen aan gegevensassets in Azure Data Catalog, met inbegrip van beschrijvende namen, tags, beschrijvingen en experts markeren.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: f9dd68380773e3cdb93cf1033ccd1b55d9f69305
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-annotate-data-sources"></a>Aantekeningen toevoegen aan gegevensbronnen
## <a name="introduction"></a>Inleiding
**Microsoft Azure Data Catalog** is een volledig beheerde cloudservice die als een systeem van registratie en systeem van detectie voor zakelijke gegevensbronnen fungeert. Data Catalog is met andere woorden, helpen bij het mensen detecteren, begrijpen en gebruiken van gegevensbronnen en helpt organisaties meerwaarde van hun bestaande gegevens ophalen. Als een gegevensbron is geregistreerd met Data Catalog, de metagegevens wordt gekopieerd en geïndexeerd door de service, maar het artikel er eindigt niet. Data Catalog kan gebruikers hun eigen beschrijvende metagegevens, zoals beschrijvingen en labels – ter aanvulling van de metagegevens die zijn geëxtraheerd uit de gegevensbron en de gegevensbron beter te begrijpen om aan te brengen meer mensen bieden.

## <a name="annotation-and-crowdsourcing"></a>Aantekening en crowdsourcing
Iedereen heeft advies. En dit is een goede prestaties.
Data Catalog herkent dat verschillende gebruikers verschillende perspectieven hebben voor zakelijke gegevensbronnen, en dat elk van deze perspectieven waardevol kan zijn. Houd rekening met het volgende scenario:

* De systeembeheerder kent de serviceovereenkomst voor servers of services die als host fungeren voor de gegevensbron.
* De databasebeheerder kent het back-upschema voor elke database en de maximale verwerkingstijd van de ETL-vensters.
* Eigenaar van het systeem, kent het proces voor gebruikers om toegang tot de gegevensbron te vragen.
* De gegevens wereldburgers weet hoe de elementen en kenmerken in de gegevensbron worden toegewezen aan het gegevensmodel enterprise.
* De analist weet hoe de gegevens wordt gebruikt in de context van de bedrijfsprocessen die hij ondersteunt.

Elk van deze perspectieven is waardevolle en Data Catalog gebruikt een benadering crowdsourcing van metagegevens waarmee elkaar kunnen worden vastgelegd en worden gebruikt voor het leveren van een volledig overzicht van geregistreerde gegevensbronnen. Met de Data Catalog-portal kan elke gebruiker toevoegen en bewerken van zijn of haar eigen aantekeningen kunnen aantekeningen die door andere gebruikers weergeven.

## <a name="different-types-of-annotations"></a>Verschillende soorten aantekeningen
Data Catalog ondersteunt de volgende soorten aantekeningen:

| Aantekening | Opmerkingen |
| --- | --- |
| Beschrijvende naam |Beschrijvende namen kunnen worden opgegeven op het niveau van de gegevens asset waarmee de gegevensassets gemakkelijker te begrijpen. Beschrijvende namen zijn handig als de naam van het onderliggende object cryptisch, afgekorte of anderszins niet relevant zijn voor gebruikers is. |
| Beschrijving |Beschrijvingen kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Beschrijvingen van aantekeningen korte tekst met een beschrijving van de gebruiker zijn perspectief op de gegevensasset of het gebruik ervan. |
| Labels (gebruiker tags) |Labels kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Gebruiker tags zijn aangepaste labels die kunnen worden gebruikt om te categoriseren gegevensassets of kenmerken. |
| Labels (verklarende woordenlijst tags) |Labels kunnen worden opgegeven tijdens de gegevensasset en het kenmerk / kolomniveaus. Verklarende woordenlijst tags zijn centraal gedefinieerd termen die kunnen worden gebruikt om te categoriseren gegevensassets of kenmerken met een gemeenschappelijke taxonomie voor bedrijven. Zie voor meer informatie [De zakelijke woordenlijst instellen voor Governed Tagging](data-catalog-how-to-business-glossary.md) |
| Experts |Experts kunnen worden opgegeven op het niveau van de asset gegevens. Experts kunnen gebruikers of groepen identificeren met deskundige perspectieven van de gegevens en fungeren als contactpunten voor gebruikers die de geregistreerde gegevensbronnen detecteren en vragen hebt die niet wordt beantwoord door bestaande aantekeningen. |
| Toegang aanvragen |Aanvraag voor toegang tot informatie kan worden opgegeven op het niveau van de asset gegevens. Deze informatie is voor gebruikers die een gegevensbron die ze hebt nog geen machtigingen om toegang te detecteren. Gebruikers het e-mailadres van de gebruiker of groep die verleent toegang tot de URL van het proces of het hulpprogramma dat gebruikers toegang moeten hebben, kunnen opgeven of het proces zelf kunnen invoeren als tekst. |
| Documentatie |Documentatie kan worden opgegeven op het niveau van de asset gegevens. Documentatie voor asset is RTF-informatie die koppelingen en afbeeldingen kunnen bevatten, en die informatie niet overgebracht via beschrijvingen en labels kunt bieden. |

## <a name="annotating-multiple-assets"></a>Meerdere assets aantekeningen maken
Als u meerdere gegevensassets in de portal voor Data Catalog, kunnen gebruikers aantekeningen toevoegen aan alle geselecteerde elementen in één bewerking. Aantekeningen van toepassing op alle geselecteerde activa, zodat u gemakkelijk kunt selecteren en geef een consistente beschrijving en sets met tags en experts voor bedrijfsmiddelen die met gerelateerde gegevens.

> [!NOTE]
> Tags en experts kunnen ook worden opgegeven wanneer registreren gegevensassets met behulp van de gegevens van Data Catalog bron hulpprogramma voor registratie.
>
>

Wanneer meerdere tabellen en weergaven, alleen kolommen dat alle gegevens activa gemeen hebben geselecteerd selecteren wordt weergegeven in de portal voor Data Catalog. Hiermee kunnen gebruikers voor labels en beschrijvingen voor alle kolommen met dezelfde naam voor alle geselecteerde activa.

## <a name="annotations-and-discovery"></a>Aantekeningen en detectie
Net als de metagegevens die zijn geëxtraheerd uit de gegevensbron tijdens de registratie wordt toegevoegd aan de search-index van Data Catalog, worden de gebruiker opgegeven metagegevens worden ook geïndexeerd. Dit betekent dat niet alleen aantekeningen maken het gemakkelijker voor gebruikers om te begrijpen van de gegevens die ze detecteren, aantekeningen ook gemakkelijker voor gebruikers om de aantekeningen gegevensassets door te zoeken met behulp van de voorwaarden die zinvol zijn om ze te detecteren.

## <a name="summary"></a>Samenvatting
Registreren van een gegevensbron met Data Catalog, maakt die gegevens kunnen worden gedetecteerd door structurele en beschrijvende metagegevens uit de gegevensbron kopiëren naar de catalogus-service. Zodra een gegevensbron is geregistreerd, kunnen gebruikers aantekeningen gemakkelijker te detecteren en begrijpen van de Data Catalog-portal opgeven.

## <a name="see-also"></a>Zie ook
* [Aan de slag met Azure Data Catalog](data-catalog-get-started.md) zelfstudie voor stapsgewijze informatie over het aantekeningen toevoegen aan gegevensbronnen.
